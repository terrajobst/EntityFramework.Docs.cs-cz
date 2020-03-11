---
title: Uložené procedury s více sadami výsledků – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418702"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Uložené procedury s více sadami výsledků
Při použití uložených procedur budete někdy potřebovat vrátit více sad výsledků. Tento scénář se běžně používá ke snížení počtu přenosů databáze potřebných k vytvoření jedné obrazovky. Před EF5 by Entity Framework umožnila volání uložené procedury, ale vrátila pouze první sadu výsledků na volající kód.

Tento článek vám ukáže dva způsoby, jak můžete použít pro přístup k více než jedné sadě výsledků z uložené procedury v Entity Framework. Ten, který používá pouze kód a pracuje s prvním kódem a návrhářem EF a jedním, který pracuje pouze s návrhářem EF. Podpora nástrojů a rozhraní API pro tuto verzi by se měla zlepšit v budoucích verzích Entity Framework.

## <a name="model"></a>Model

Příklady v tomto článku využívají základní blog a model příspěvků, kde blog obsahuje mnoho příspěvků a příspěvek patří do jednoho blogu. V databázi použijeme uloženou proceduru, která vrátí všechny blogy a příspěvky, třeba takto:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Přístup k více sadám výsledků s kódem

Ke spuštění naší uložené procedury můžeme použít kód pro vydání nezpracovaného příkazu SQL. Výhodou tohoto přístupu je, že funguje jak jako první, tak i v Návrháři EF.

Aby bylo možné získat více sad výsledků, potřebujeme vyřadit rozhraní ObjectContext API pomocí rozhraní IObjectContextAdapter.

Jakmile budeme mít ObjectContext, můžeme použít metodu překladu k překladu výsledků naší uložené procedury do entit, které lze sledovat a používat v EF jako normální. Následující příklad kódu ukazuje tuto v akci.

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

Metoda přeložit přijme čtenář, který jsme dostali, když jsme provedli proceduru, název EntitySet a MergeOption. Název objektu EntitySet bude stejný jako vlastnost Negenerickými v odvozeném kontextu. Výčet MergeOption řídí způsob zpracování výsledků, pokud už stejná entita existuje v paměti.

Tady procházíme kolekcí blogů před voláním NextResult, to je důležité pro výše uvedený kód, protože první sada výsledků musí být spotřebovaná před přechodem na další sadu výsledků.

Po volání dvou metod překladu jsou entity blog a post sledovány pomocí EF stejným způsobem jako u jakékoli jiné entity, a proto je lze upravit nebo odstranit a uložit jako normální.

>[!NOTE]
> EF nebere v úvahu žádné mapování při vytváření entit pomocí metody přeložit. Bude jednoduše odpovídat názvům sloupců v sadě výsledků s názvy vlastností ve třídách.

>[!NOTE]
> Pokud máte povolené opožděné načítání, budete mít přístup k vlastnosti příspěvky v jedné z entit blogu a potom se EF připojí k databázi, aby laxně vytvářená načíst všechny příspěvky, a to i v případě, že jsme je už načetli. Je to proto, že EF nemůže zjistit, zda jste načetli všechny příspěvky nebo zda jsou v databázi více. Pokud se k tomu chcete vyhnout, budete muset zakázat opožděné načítání.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Více sad výsledků s nakonfigurovaným ve EDMX

>[!NOTE]
> Aby bylo možné v EDMX nakonfigurovat více sad výsledků, je třeba cílit na .NET Framework 4,5. Pokud cílíte na rozhraní .NET 4,0, můžete použít metodu založenou na kódu uvedenou v předchozí části.

Pokud používáte návrháře EF, můžete také upravit model tak, aby znal o různých sadách výsledků, které budou vráceny. Jedna věc, kterou je třeba znát před ručním nastavením, je, že nástroj nepracuje s více sadami výsledků, takže budete muset ručně upravit soubor EDMX. Upravený soubor EDMX bude fungovat, ale bude také přerušit ověřování modelu v VS. Takže pokud ověříte svůj model, vždycky se zobrazí chyby.

-   Aby to bylo možné, musíte do modelu přidat uloženou proceduru, stejně jako pro jeden dotaz sady výsledků dotazu.
-   Až to budete mít, musíte kliknout pravým tlačítkem na model a vybrat **otevřít s...** pak **XML**

    ![Otevřít jako](~/ef6/media/openas.png)

Jakmile máte model otevřený jako XML, musíte provést následující kroky:

-   Najděte komplexní typ a import funkcí v modelu:

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
-   Aktualizujte import funkce tak, aby se namapovala na vaše entity, v našem případě bude vypadat jako v následujícím příkladu:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Tímto způsobem se dozvíte, že uložená procedura vrátí dvě kolekce, jednu z položek blogu a jednu z položek post.

-   Vyhledejte element mapování funkcí:

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

-   Nahraďte mapování výsledků jedním z těchto vrácených entit, například následující:

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

Je také možné namapovat sady výsledků na komplexní typy, jako je například ta vytvořená ve výchozím nastavení. Chcete-li to provést, vytvořte nový komplexní typ místo odebrání a používejte komplexní typy všude, kde jste používali názvy entit v předchozích příkladech.

Po změně těchto mapování můžete model Uložit a spustit následující kód pro použití uložené procedury:

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
> Pokud ručně upravíte soubor EDMX pro model, bude přepsán, pokud model někdy znovu vygenerujete z databáze.

## <a name="summary"></a>Souhrn

Tady jsme ukázali dvě různé metody přístupu k několika sadám výsledků pomocí Entity Framework. Oba z nich jsou stejně platné v závislosti na vaší situaci a preferencích a měli byste zvolit ten, který se pro vaše situace jeví jako nejvhodnější. Plánuje se, že podpora více sad výsledků bude v budoucích verzích Entity Framework vylepšená a že provedení kroků v tomto dokumentu už nebude nutné.
