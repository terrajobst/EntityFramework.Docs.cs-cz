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
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="caa36-102">Uložené procedury s více sadami výsledků</span><span class="sxs-lookup"><span data-stu-id="caa36-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="caa36-103">Při použití uložených procedur budete někdy potřebovat vrátit více sad výsledků.</span><span class="sxs-lookup"><span data-stu-id="caa36-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="caa36-104">Tento scénář se běžně používá ke snížení počtu přenosů databáze potřebných k vytvoření jedné obrazovky.</span><span class="sxs-lookup"><span data-stu-id="caa36-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span><span data-ttu-id="caa36-105"> Před EF5 by Entity Framework umožnila volání uložené procedury, ale vrátila pouze první sadu výsledků na volající kód.</span><span class="sxs-lookup"><span data-stu-id="caa36-105"> Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="caa36-106">Tento článek vám ukáže dva způsoby, jak můžete použít pro přístup k více než jedné sadě výsledků z uložené procedury v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="caa36-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="caa36-107">Ten, který používá pouze kód a pracuje s prvním kódem a návrhářem EF a jedním, který pracuje pouze s návrhářem EF.</span><span class="sxs-lookup"><span data-stu-id="caa36-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="caa36-108">Podpora nástrojů a rozhraní API pro tuto verzi by se měla zlepšit v budoucích verzích Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="caa36-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="caa36-109">Model</span><span class="sxs-lookup"><span data-stu-id="caa36-109">Model</span></span>

<span data-ttu-id="caa36-110">Příklady v tomto článku využívají základní blog a model příspěvků, kde blog obsahuje mnoho příspěvků a příspěvek patří do jednoho blogu.</span><span class="sxs-lookup"><span data-stu-id="caa36-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="caa36-111">V databázi použijeme uloženou proceduru, která vrátí všechny blogy a příspěvky, třeba takto:</span><span class="sxs-lookup"><span data-stu-id="caa36-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="caa36-112">Přístup k více sadám výsledků s kódem</span><span class="sxs-lookup"><span data-stu-id="caa36-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="caa36-113">Ke spuštění naší uložené procedury můžeme použít kód pro vydání nezpracovaného příkazu SQL.</span><span class="sxs-lookup"><span data-stu-id="caa36-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="caa36-114">Výhodou tohoto přístupu je, že funguje jak jako první, tak i v Návrháři EF.</span><span class="sxs-lookup"><span data-stu-id="caa36-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="caa36-115">Aby bylo možné získat více sad výsledků, potřebujeme vyřadit rozhraní ObjectContext API pomocí rozhraní IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="caa36-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="caa36-116">Jakmile budeme mít ObjectContext, můžeme použít metodu překladu k překladu výsledků naší uložené procedury do entit, které lze sledovat a používat v EF jako normální.</span><span class="sxs-lookup"><span data-stu-id="caa36-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="caa36-117">Následující příklad kódu ukazuje tuto v akci.</span><span class="sxs-lookup"><span data-stu-id="caa36-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="caa36-118">Metoda přeložit přijme čtenář, který jsme dostali, když jsme provedli proceduru, název EntitySet a MergeOption.</span><span class="sxs-lookup"><span data-stu-id="caa36-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="caa36-119">Název objektu EntitySet bude stejný jako vlastnost Negenerickými v odvozeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="caa36-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="caa36-120">Výčet MergeOption řídí způsob zpracování výsledků, pokud už stejná entita existuje v paměti.</span><span class="sxs-lookup"><span data-stu-id="caa36-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="caa36-121">Tady procházíme kolekcí blogů před voláním NextResult, to je důležité pro výše uvedený kód, protože první sada výsledků musí být spotřebovaná před přechodem na další sadu výsledků.</span><span class="sxs-lookup"><span data-stu-id="caa36-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="caa36-122">Po volání dvou metod překladu jsou entity blog a post sledovány pomocí EF stejným způsobem jako u jakékoli jiné entity, a proto je lze upravit nebo odstranit a uložit jako normální.</span><span class="sxs-lookup"><span data-stu-id="caa36-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="caa36-123">EF nebere v úvahu žádné mapování při vytváření entit pomocí metody přeložit.</span><span class="sxs-lookup"><span data-stu-id="caa36-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="caa36-124">Bude jednoduše odpovídat názvům sloupců v sadě výsledků s názvy vlastností ve třídách.</span><span class="sxs-lookup"><span data-stu-id="caa36-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="caa36-125">Pokud máte povolené opožděné načítání, budete mít přístup k vlastnosti příspěvky v jedné z entit blogu a potom se EF připojí k databázi, aby laxně vytvářená načíst všechny příspěvky, a to i v případě, že jsme je už načetli.</span><span class="sxs-lookup"><span data-stu-id="caa36-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="caa36-126">Je to proto, že EF nemůže zjistit, zda jste načetli všechny příspěvky nebo zda jsou v databázi více.</span><span class="sxs-lookup"><span data-stu-id="caa36-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="caa36-127">Pokud se k tomu chcete vyhnout, budete muset zakázat opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="caa36-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="caa36-128">Více sad výsledků s nakonfigurovaným ve EDMX</span><span class="sxs-lookup"><span data-stu-id="caa36-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="caa36-129">Aby bylo možné v EDMX nakonfigurovat více sad výsledků, je třeba cílit na .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="caa36-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="caa36-130">Pokud cílíte na rozhraní .NET 4,0, můžete použít metodu založenou na kódu uvedenou v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="caa36-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="caa36-131">Pokud používáte návrháře EF, můžete také upravit model tak, aby znal o různých sadách výsledků, které budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="caa36-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="caa36-132">Jedna věc, kterou je třeba znát před ručním nastavením, je, že nástroj nepracuje s více sadami výsledků, takže budete muset ručně upravit soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="caa36-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="caa36-133">Upravený soubor EDMX bude fungovat, ale bude také přerušit ověřování modelu v VS.</span><span class="sxs-lookup"><span data-stu-id="caa36-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="caa36-134">Takže pokud ověříte svůj model, vždycky se zobrazí chyby.</span><span class="sxs-lookup"><span data-stu-id="caa36-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="caa36-135">Aby to bylo možné, musíte do modelu přidat uloženou proceduru, stejně jako pro jeden dotaz sady výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="caa36-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="caa36-136">Až to budete mít, musíte kliknout pravým tlačítkem na model a vybrat **otevřít s...**</span><span class="sxs-lookup"><span data-stu-id="caa36-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="caa36-137">pak **XML**</span><span class="sxs-lookup"><span data-stu-id="caa36-137">then **Xml**</span></span>

    ![Otevřít jako](~/ef6/media/openas.png)

<span data-ttu-id="caa36-139">Jakmile máte model otevřený jako XML, musíte provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="caa36-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="caa36-140">Najděte komplexní typ a import funkcí v modelu:</span><span class="sxs-lookup"><span data-stu-id="caa36-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="caa36-141">Odebrat komplexní typ</span><span class="sxs-lookup"><span data-stu-id="caa36-141">Remove the complex type</span></span>
-   <span data-ttu-id="caa36-142">Aktualizujte import funkce tak, aby se namapovala na vaše entity, v našem případě bude vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="caa36-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="caa36-143">Tímto způsobem se dozvíte, že uložená procedura vrátí dvě kolekce, jednu z položek blogu a jednu z položek post.</span><span class="sxs-lookup"><span data-stu-id="caa36-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="caa36-144">Vyhledejte element mapování funkcí:</span><span class="sxs-lookup"><span data-stu-id="caa36-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="caa36-145">Nahraďte mapování výsledků jedním z těchto vrácených entit, například následující:</span><span class="sxs-lookup"><span data-stu-id="caa36-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="caa36-146">Je také možné namapovat sady výsledků na komplexní typy, jako je například ta vytvořená ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="caa36-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="caa36-147">Chcete-li to provést, vytvořte nový komplexní typ místo odebrání a používejte komplexní typy všude, kde jste používali názvy entit v předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="caa36-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="caa36-148">Po změně těchto mapování můžete model Uložit a spustit následující kód pro použití uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="caa36-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="caa36-149">Pokud ručně upravíte soubor EDMX pro model, bude přepsán, pokud model někdy znovu vygenerujete z databáze.</span><span class="sxs-lookup"><span data-stu-id="caa36-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="caa36-150">Souhrn</span><span class="sxs-lookup"><span data-stu-id="caa36-150">Summary</span></span>

<span data-ttu-id="caa36-151">Tady jsme ukázali dvě různé metody přístupu k několika sadám výsledků pomocí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="caa36-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="caa36-152">Oba z nich jsou stejně platné v závislosti na vaší situaci a preferencích a měli byste zvolit ten, který se pro vaše situace jeví jako nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="caa36-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="caa36-153">Plánuje se, že podpora více sad výsledků bude v budoucích verzích Entity Framework vylepšená a že provedení kroků v tomto dokumentu už nebude nutné.</span><span class="sxs-lookup"><span data-stu-id="caa36-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
