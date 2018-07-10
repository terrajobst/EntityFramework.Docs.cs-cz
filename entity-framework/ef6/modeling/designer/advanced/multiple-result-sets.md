---
title: Uložené procedury s více sad výsledků dotazu - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
caps.latest.revision: 3
ms.openlocfilehash: 68d544b0c553868ad1ff36cd24db19cff08db073
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912856"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Uložené procedury s více sad výsledků dotazu
Někdy při použití uložených procedur, je potřeba vrátit více než jeden výsledek nastavit. Tento scénář se často používá ke snížení počtu databáze má zpáteční převod vyžaduje k vytváření na jedné obrazovce. Před EF5 Entity Framework by umožnilo uloženou proceduru, která se má volat, ale pouze vrátí první sadu výsledků do volajícího kódu.

Tento článek vám ukáže dva způsoby, které můžete použít pro přístup k více než jednu sadu výsledků z uložené procedury v Entity Framework. Ten, který používá jenom kódu a pracuje s kód nejprve a EF designeru a ten, který pracuje pouze s EF designeru. Nástroje a podpora rozhraní API pro to měl vylepšit v budoucnu verze Entity Framework.

## <a name="model"></a>Model

V příkladech v tomto článku se používá základní blogu a příspěvky model, kdy blogu má mnoho příspěvky a příspěvek patří do jedné blogu. Budeme používat uložené procedury v databázi, která vrátí všechny blogů a příspěvky podobný následujícímu:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Přístup k více výsledku nastaví s kódem

Můžeme spouštět kód na použití k nezpracované SQL příkaz ke spuštění našich uložené procedury. Výhodou tohoto přístupu je, že pracuje s kód nejprve a EF designeru.

Pokud chcete získat více výsledku nastaví práci, kterou potřebujeme vyřadit ObjectContext rozhraní API pomocí rozhraní IObjectContextAdapter.

Jakmile budeme mít objektu ObjectContext jsme přeložit výsledky našich uložené procedury do entity, které můžete sledovat a použít v EF jako za normálních okolností použijte metodu přeložit. Následující příklad kódu ukazuje to v akci.

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

Metoda přeložit přijímá čtecí modul, který jsme dostali, když jsme spouštěli postup, název objektu EntitySet a MergeOption. Název objektu EntitySet budou stejné jako vlastnost DbSet odvozené kontextu. Výčet MergeOption řídí, jak se zpracovává výsledky, pokud již stejná entita existuje v paměti.

Tady jsme iteraci prostřednictvím kolekce blogy před říkáme NextResult, to je důležité, uvedené výše uvedený kód vzhledem k tomu, že první sada výsledek musí být využity ještě před přesunem do další sadu výsledků.

Po přeložit dvě metody jsou volány pak blogu a po entity jsou sledovány objektem EF stejným způsobem jako jiné entitě a proto se dají upravit nebo odstranit a uložit jako za normálních okolností.

>[!NOTE]
> EF nepřebírá žádné mapování v úvahu při vytváření entit pomocí metody přeložit. Budou se jednoduše shodovat s názvy sloupců v sadě s názvy vlastností ve třídách výsledků.

>[!NOTE]
> Pokud máte opožděné načtení povoleno, přístup k vlastnosti příspěvky na jednom z blogu entity, které pak EF bude připojení k databázi laxně načíst všechny příspěvky, i v případě, že jsme všechny již načtena. Je to proto EF nemůže vědět, jestli jste načetli všechny příspěvky nebo pokud existují další databáze. Pokud chcete předejít, pak budete muset zakázat opožděné načtení.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Více sad výsledků dotazu pomocí nakonfigurovaného v EDMX

>[!NOTE]
> Je potřeba cílit rozhraní .NET Framework 4.5, abyste mohli nakonfigurovat více sad výsledků dotazu v EDMX. Pokud se zaměřujete na rozhraní .NET 4.0, můžete použít metodu založený na kódu je znázorněno v předchozí části.

Pokud používáte EF designeru, můžete také změnit váš model, tak, aby věděl o sadách jiné výsledky, které budou vráceny. Jednu věc, kterou potřebujete vědět, než je ručně, nástrojů není více výsledků nastavit vědět, takže budete muset ručně upravit soubor edmx. Úpravy souboru edmx, jako je to bude fungovat, ale je také přeruší ověření modelu v sadě Visual Studio. Proto pokud ověření modelu se vždy zobrazí chyby.

-   Pokud to chcete udělat, budete muset přidat uložené procedury do modelu, stejně jako jeden výsledek dotazu sady.
-   Jakmile budete mít, to je nutné klikněte pravým tlačítkem na model a vyberte **otevřít v programu...** potom **Xml**

    ![OpenAs](~/ef6/media/openas.png)

Jakmile budete mít modelu otevřít ve formátu XML, je nutné provést následující kroky:

-   V modelu najdete komplexní typ a funkci importu:

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   Odebrat komplexní typ
-   Aktualizace importované funkce tak, aby je namapován na entity, v našem případě to bude vypadat nějak takto:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Říká modelu, že dvě kolekce, jeden z článků blogu a jeden příspěvek položek, které se vrátí uloženou proceduru.

-   Najděte prvek mapování funkce:

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   Nahraďte mapování výsledku s jednou pro každou entitu, se vrací, jako je následující:

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

Je také možné mapovat sad výsledků pro komplexní typy, jako jsou vytvořeny ve výchozím nastavení. Provedete to tak můžete vytvořit nový komplexní typ, místo aby odebrala a používat komplexní typy všude, měli používat názvy entit ve výše uvedených příkladech.

Jakmile tato mapování se změnily, můžete uložit model a spusťte následující kód pro použití uložené procedury:

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> Když ručně upravíte soubor edmx pro váš model ho budou přepsány, pokud někdy znovu generovat model z databáze.

## <a name="summary"></a>Souhrn

Tady jsme ukázalo dva různé způsoby přístupu k několika výsledku nastaví používající nástroj Entity Framework. Obou z nich jsou rovněž platné v závislosti na vaší situaci a předvolby a můžete zvolit ten, který se zdá být nejvhodnější pro vaše okolností. Je naplánovaná, podporu pro více výsledků, bude sady vylepšené v budoucích verzích rozhraní Entity Framework a že provedením kroků v tomto dokumentu už bude nutné.
