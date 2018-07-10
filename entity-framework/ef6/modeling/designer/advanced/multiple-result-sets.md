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
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="efd8e-102">Uložené procedury s více sad výsledků dotazu</span><span class="sxs-lookup"><span data-stu-id="efd8e-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="efd8e-103">Někdy při použití uložených procedur, je potřeba vrátit více než jeden výsledek nastavit.</span><span class="sxs-lookup"><span data-stu-id="efd8e-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="efd8e-104">Tento scénář se často používá ke snížení počtu databáze má zpáteční převod vyžaduje k vytváření na jedné obrazovce.</span><span class="sxs-lookup"><span data-stu-id="efd8e-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="efd8e-105">Před EF5 Entity Framework by umožnilo uloženou proceduru, která se má volat, ale pouze vrátí první sadu výsledků do volajícího kódu.</span><span class="sxs-lookup"><span data-stu-id="efd8e-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="efd8e-106">Tento článek vám ukáže dva způsoby, které můžete použít pro přístup k více než jednu sadu výsledků z uložené procedury v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="efd8e-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="efd8e-107">Ten, který používá jenom kódu a pracuje s kód nejprve a EF designeru a ten, který pracuje pouze s EF designeru.</span><span class="sxs-lookup"><span data-stu-id="efd8e-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="efd8e-108">Nástroje a podpora rozhraní API pro to měl vylepšit v budoucnu verze Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="efd8e-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="efd8e-109">Model</span><span class="sxs-lookup"><span data-stu-id="efd8e-109">Model</span></span>

<span data-ttu-id="efd8e-110">V příkladech v tomto článku se používá základní blogu a příspěvky model, kdy blogu má mnoho příspěvky a příspěvek patří do jedné blogu.</span><span class="sxs-lookup"><span data-stu-id="efd8e-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="efd8e-111">Budeme používat uložené procedury v databázi, která vrátí všechny blogů a příspěvky podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="efd8e-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="efd8e-112">Přístup k více výsledku nastaví s kódem</span><span class="sxs-lookup"><span data-stu-id="efd8e-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="efd8e-113">Můžeme spouštět kód na použití k nezpracované SQL příkaz ke spuštění našich uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="efd8e-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="efd8e-114">Výhodou tohoto přístupu je, že pracuje s kód nejprve a EF designeru.</span><span class="sxs-lookup"><span data-stu-id="efd8e-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="efd8e-115">Pokud chcete získat více výsledku nastaví práci, kterou potřebujeme vyřadit ObjectContext rozhraní API pomocí rozhraní IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="efd8e-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="efd8e-116">Jakmile budeme mít objektu ObjectContext jsme přeložit výsledky našich uložené procedury do entity, které můžete sledovat a použít v EF jako za normálních okolností použijte metodu přeložit.</span><span class="sxs-lookup"><span data-stu-id="efd8e-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="efd8e-117">Následující příklad kódu ukazuje to v akci.</span><span class="sxs-lookup"><span data-stu-id="efd8e-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="efd8e-118">Metoda přeložit přijímá čtecí modul, který jsme dostali, když jsme spouštěli postup, název objektu EntitySet a MergeOption.</span><span class="sxs-lookup"><span data-stu-id="efd8e-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="efd8e-119">Název objektu EntitySet budou stejné jako vlastnost DbSet odvozené kontextu.</span><span class="sxs-lookup"><span data-stu-id="efd8e-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="efd8e-120">Výčet MergeOption řídí, jak se zpracovává výsledky, pokud již stejná entita existuje v paměti.</span><span class="sxs-lookup"><span data-stu-id="efd8e-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="efd8e-121">Tady jsme iteraci prostřednictvím kolekce blogy před říkáme NextResult, to je důležité, uvedené výše uvedený kód vzhledem k tomu, že první sada výsledek musí být využity ještě před přesunem do další sadu výsledků.</span><span class="sxs-lookup"><span data-stu-id="efd8e-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="efd8e-122">Po přeložit dvě metody jsou volány pak blogu a po entity jsou sledovány objektem EF stejným způsobem jako jiné entitě a proto se dají upravit nebo odstranit a uložit jako za normálních okolností.</span><span class="sxs-lookup"><span data-stu-id="efd8e-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="efd8e-123">EF nepřebírá žádné mapování v úvahu při vytváření entit pomocí metody přeložit.</span><span class="sxs-lookup"><span data-stu-id="efd8e-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="efd8e-124">Budou se jednoduše shodovat s názvy sloupců v sadě s názvy vlastností ve třídách výsledků.</span><span class="sxs-lookup"><span data-stu-id="efd8e-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="efd8e-125">Pokud máte opožděné načtení povoleno, přístup k vlastnosti příspěvky na jednom z blogu entity, které pak EF bude připojení k databázi laxně načíst všechny příspěvky, i v případě, že jsme všechny již načtena.</span><span class="sxs-lookup"><span data-stu-id="efd8e-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="efd8e-126">Je to proto EF nemůže vědět, jestli jste načetli všechny příspěvky nebo pokud existují další databáze.</span><span class="sxs-lookup"><span data-stu-id="efd8e-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="efd8e-127">Pokud chcete předejít, pak budete muset zakázat opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="efd8e-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="efd8e-128">Více sad výsledků dotazu pomocí nakonfigurovaného v EDMX</span><span class="sxs-lookup"><span data-stu-id="efd8e-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="efd8e-129">Je potřeba cílit rozhraní .NET Framework 4.5, abyste mohli nakonfigurovat více sad výsledků dotazu v EDMX.</span><span class="sxs-lookup"><span data-stu-id="efd8e-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="efd8e-130">Pokud se zaměřujete na rozhraní .NET 4.0, můžete použít metodu založený na kódu je znázorněno v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="efd8e-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="efd8e-131">Pokud používáte EF designeru, můžete také změnit váš model, tak, aby věděl o sadách jiné výsledky, které budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="efd8e-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="efd8e-132">Jednu věc, kterou potřebujete vědět, než je ručně, nástrojů není více výsledků nastavit vědět, takže budete muset ručně upravit soubor edmx.</span><span class="sxs-lookup"><span data-stu-id="efd8e-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="efd8e-133">Úpravy souboru edmx, jako je to bude fungovat, ale je také přeruší ověření modelu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="efd8e-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="efd8e-134">Proto pokud ověření modelu se vždy zobrazí chyby.</span><span class="sxs-lookup"><span data-stu-id="efd8e-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="efd8e-135">Pokud to chcete udělat, budete muset přidat uložené procedury do modelu, stejně jako jeden výsledek dotazu sady.</span><span class="sxs-lookup"><span data-stu-id="efd8e-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="efd8e-136">Jakmile budete mít, to je nutné klikněte pravým tlačítkem na model a vyberte **otevřít v programu...**</span><span class="sxs-lookup"><span data-stu-id="efd8e-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="efd8e-137">potom **Xml**</span><span class="sxs-lookup"><span data-stu-id="efd8e-137">then **Xml**</span></span>

    ![OpenAs](~/ef6/media/openas.png)

<span data-ttu-id="efd8e-139">Jakmile budete mít modelu otevřít ve formátu XML, je nutné provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="efd8e-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="efd8e-140">V modelu najdete komplexní typ a funkci importu:</span><span class="sxs-lookup"><span data-stu-id="efd8e-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="efd8e-141">Odebrat komplexní typ</span><span class="sxs-lookup"><span data-stu-id="efd8e-141">Remove the complex type</span></span>
-   <span data-ttu-id="efd8e-142">Aktualizace importované funkce tak, aby je namapován na entity, v našem případě to bude vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="efd8e-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="efd8e-143">Říká modelu, že dvě kolekce, jeden z článků blogu a jeden příspěvek položek, které se vrátí uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="efd8e-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="efd8e-144">Najděte prvek mapování funkce:</span><span class="sxs-lookup"><span data-stu-id="efd8e-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="efd8e-145">Nahraďte mapování výsledku s jednou pro každou entitu, se vrací, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="efd8e-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="efd8e-146">Je také možné mapovat sad výsledků pro komplexní typy, jako jsou vytvořeny ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="efd8e-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="efd8e-147">Provedete to tak můžete vytvořit nový komplexní typ, místo aby odebrala a používat komplexní typy všude, měli používat názvy entit ve výše uvedených příkladech.</span><span class="sxs-lookup"><span data-stu-id="efd8e-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="efd8e-148">Jakmile tato mapování se změnily, můžete uložit model a spusťte následující kód pro použití uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="efd8e-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="efd8e-149">Když ručně upravíte soubor edmx pro váš model ho budou přepsány, pokud někdy znovu generovat model z databáze.</span><span class="sxs-lookup"><span data-stu-id="efd8e-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="efd8e-150">Souhrn</span><span class="sxs-lookup"><span data-stu-id="efd8e-150">Summary</span></span>

<span data-ttu-id="efd8e-151">Tady jsme ukázalo dva různé způsoby přístupu k několika výsledku nastaví používající nástroj Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="efd8e-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="efd8e-152">Obou z nich jsou rovněž platné v závislosti na vaší situaci a předvolby a můžete zvolit ten, který se zdá být nejvhodnější pro vaše okolností.</span><span class="sxs-lookup"><span data-stu-id="efd8e-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="efd8e-153">Je naplánovaná, podporu pro více výsledků, bude sady vylepšené v budoucích verzích rozhraní Entity Framework a že provedením kroků v tomto dokumentu už bude nutné.</span><span class="sxs-lookup"><span data-stu-id="efd8e-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
