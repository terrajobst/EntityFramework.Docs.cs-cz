---
title: Zobrazení mapování předem generovaného - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
caps.latest.revision: 3
ms.openlocfilehash: 9e74176d02afc424118219eec8e016843333cbb8
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914209"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="16b45-102">Zobrazení předem generovaného mapování</span><span class="sxs-lookup"><span data-stu-id="16b45-102">Pre-generated mapping views</span></span>
<span data-ttu-id="16b45-103">Entity Framework mohli spustit dotaz nebo uložit změny do zdroje dat, musíte vygenerovat sadu zobrazení mapování pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="16b45-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="16b45-104">Tato mapování zobrazení jsou sady Entity SQL příkazu, který abstraktní jak reprezentaci databáze a část metadata, která se uloží do mezipaměti pro doménu aplikace.</span><span class="sxs-lookup"><span data-stu-id="16b45-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="16b45-105">Pokud vytvoříte více instancí stejného kontextu ve stejné doméně aplikace, bude znovu použít zobrazení mapování ze metadata uložená v mezipaměti místo jejich obnovení.</span><span class="sxs-lookup"><span data-stu-id="16b45-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="16b45-106">Protože generování zobrazení mapování je podstatnou část celkové náklady na provedení první dotaz, Entity Framework umožňuje předběžně generovat zobrazení mapování a zahrnout je do kompilované projektu.</span><span class="sxs-lookup"><span data-stu-id="16b45-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="16b45-107">Další informace najdete v tématu [důležité informace o výkonu (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="16b45-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="16b45-108">Generuje se mapování zobrazení s EF Power Tools</span><span class="sxs-lookup"><span data-stu-id="16b45-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="16b45-109">Nejjednodušší způsob, jak předem vygenerovat zobrazení je použít [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span><span class="sxs-lookup"><span data-stu-id="16b45-109">The easiest way to pre-generate views is to use the [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="16b45-110">Jakmile budete mít nainstalované nástroje Power budete mít možnost nabídky generovat zobrazení, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="16b45-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="16b45-111">Pro **Code First** modelů klikněte pravým tlačítkem na soubor kódu, který obsahuje vaše třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="16b45-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="16b45-112">Pro **EF designeru** modelů klikněte pravým tlačítkem na soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="16b45-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generateViews](~/ef6/media/generateviews.png)

<span data-ttu-id="16b45-114">Po dokončení procesu budete mít podobný následujícímu generované třídy</span><span class="sxs-lookup"><span data-stu-id="16b45-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generatedViews](~/ef6/media/generatedviews.png)

<span data-ttu-id="16b45-116">Nyní při spuštění aplikace EF bude tato třída slouží k načtení zobrazení podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="16b45-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="16b45-117">Pokud se změny modelu a nejsou znovu generovány Tato třída EF vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="16b45-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="16b45-118">Generování zobrazení mapování z kódu – ef6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="16b45-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="16b45-119">Použití rozhraní API, která poskytuje EF je další způsob, jak vygenerovat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16b45-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="16b45-120">Při použití této metody budete moci svobodně k serializaci zobrazení, ale potřebujete, ale je také potřeba načíst zobrazení sami.</span><span class="sxs-lookup"><span data-stu-id="16b45-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="16b45-121">**EF6 a vyšší pouze** – API, které jsou uvedené v této části byly zavedeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="16b45-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="16b45-122">Pokud používáte starší verzi tyto informace se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="16b45-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="16b45-123">Generování zobrazení</span><span class="sxs-lookup"><span data-stu-id="16b45-123">Generating Views</span></span>

<span data-ttu-id="16b45-124">Rozhraní API pro generování zobrazení jsou ve třídě System.Data.Entity.Core.Mapping.StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="16b45-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="16b45-125">StorageMappingCollection pro kontext můžete načíst pomocí objektu MetadataWorkspace objektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="16b45-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="16b45-126">Pokud používáte rozhraní API novější kontext databáze. poté budete mít přístup s použitím IObjectContextAdapter podobná níže uvedenému příkladu, v tomto kódu máme instance vaší odvozené DbContext volá dbContext:</span><span class="sxs-lookup"><span data-stu-id="16b45-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="16b45-127">Jakmile budete mít objekt StorageMappingItemCollection můžete získat přístup k metodám GenerateViews a ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="16b45-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="16b45-128">První metoda vytvoří slovník s položkou pro každé zobrazení v mapování kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="16b45-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="16b45-129">Druhá metoda vypočítá hodnotu hash pro mapování jedné kontejnerů a je použita v době běhu k ověření, protože zobrazení byly předem generovaného nedošlo ke změně modelu.</span><span class="sxs-lookup"><span data-stu-id="16b45-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="16b45-130">Přepsání dvě metody jsou k dispozici pro komplexní scénáře zahrnující více mapování v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="16b45-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="16b45-131">Při generování zobrazení budete volat metodu GenerateViews a pak zapíše výsledný EntitySetBase a DbMappingView.</span><span class="sxs-lookup"><span data-stu-id="16b45-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="16b45-132">Musíte také uložit hodnotu hash pro generované metody ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="16b45-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="16b45-133">Načítání zobrazení</span><span class="sxs-lookup"><span data-stu-id="16b45-133">Loading Views</span></span>

<span data-ttu-id="16b45-134">Aby bylo možné načíst vzhled zobrazení vygenerovaných sadou GenerateViews metodu, zadáte EF s třídou, která dědí z abstraktní třídy DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="16b45-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="16b45-135">DbMappingViewCache určuje dvě metody, které je nutné implementovat:</span><span class="sxs-lookup"><span data-stu-id="16b45-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="16b45-136">Vlastnost MappingHashValue musí vracet-the-hash generovaných ComputeMappingHashValue metodou.</span><span class="sxs-lookup"><span data-stu-id="16b45-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="16b45-137">Když je EF tak má dotaz pro zobrazení nejprve generovat a srovnání hodnoty hash modelu pomocí hodnot hash, kterou tato vlastnost vrátí.</span><span class="sxs-lookup"><span data-stu-id="16b45-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="16b45-138">Pokud shodné nejsou EF vyvolá výjimku EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="16b45-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="16b45-139">Metoda GetView přijme EntitySetBase a je zapotřebí vrátit DbMappingVIew obsahující EntitySql, který byl vygenerován pro, který byl přidružen daný EntitySetBase ve slovníku vygenerovaná metoda GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="16b45-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="16b45-140">Pokud EF požádá o zobrazení, které nemají pak GetView by měl vrátit hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="16b45-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="16b45-141">Toto je výpis z DbMappingViewCache, který je generován pomocí nástroje pro výkon, jak je popsáno výše, v ní můžeme vidět jeden způsob, jak ukládat a načítat EntitySql vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="16b45-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

<span data-ttu-id="16b45-142">Pokud chcete, aby pomocí EF použijte vaše DbMappingViewCache přidáte DbMappingViewCacheTypeAttribute, kontext, který byl vytvořen pro zadání.</span><span class="sxs-lookup"><span data-stu-id="16b45-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="16b45-143">V následujícím kódu BlogContext přidružené k MyMappingViewCache třídy.</span><span class="sxs-lookup"><span data-stu-id="16b45-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="16b45-144">Pro složitější scénáře lze zadat mapování zobrazit mezipaměti instance tak, že určíte objekt mapování zobrazení mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16b45-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="16b45-145">To můžete udělat pomocí implementace abstraktní třídy System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="16b45-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="16b45-146">Instance objektu pro vytváření mezipaměti zobrazení mapování, která se používá můžete načíst nebo nastavit pomocí StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="16b45-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
