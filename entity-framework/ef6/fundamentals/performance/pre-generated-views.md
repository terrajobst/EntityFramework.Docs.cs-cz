---
title: Předem vygenerovaná zobrazení mapování – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419389"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="4d991-102">Předem vygenerovaná zobrazení mapování</span><span class="sxs-lookup"><span data-stu-id="4d991-102">Pre-generated mapping views</span></span>
<span data-ttu-id="4d991-103">Předtím, než Entity Framework může spustit dotaz nebo uložit změny do zdroje dat, musí vygenerovat sadu zobrazení mapování pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="4d991-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="4d991-104">Tato zobrazení mapování jsou sada příkazů Entity SQL, které představují databázi abstraktním způsobem, a jsou součástí metadat, která jsou ukládána do mezipaměti na jednu doménu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d991-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="4d991-105">Pokud vytvoříte více instancí stejného kontextu ve stejné doméně aplikace, bude znovu použita mapování zobrazení z metadat v mezipaměti, nikoli znovu jejich generování.</span><span class="sxs-lookup"><span data-stu-id="4d991-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="4d991-106">Vzhledem k tomu, že generování zobrazení mapování je významnou součástí celkových nákladů na provedení prvního dotazu, Entity Framework umožňuje předem generovat zobrazení mapování a zahrnout je do zkompilovaného projektu.</span><span class="sxs-lookup"><span data-stu-id="4d991-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span><span data-ttu-id="4d991-107"> Další informace najdete v tématu  [požadavky na výkon (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="4d991-107"> For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="4d991-108">Generování zobrazení mapování pomocí edice EF Power Tools Community</span><span class="sxs-lookup"><span data-stu-id="4d991-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="4d991-109">Nejjednodušší způsob, jak předběžně generovat zobrazení, je použití [edice EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span><span class="sxs-lookup"><span data-stu-id="4d991-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="4d991-110">Po instalaci nástrojů Power Tools budete mít možnost nabídky pro generování zobrazení, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="4d991-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="4d991-111">U modelů **Code First** klikněte pravým tlačítkem myši na soubor kódu, který obsahuje vaši třídu DbContext.</span><span class="sxs-lookup"><span data-stu-id="4d991-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="4d991-112">Pro modely **Návrháře EF** klikněte pravým tlačítkem na soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="4d991-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generování zobrazení](~/ef6/media/generateviews.png)

<span data-ttu-id="4d991-114">Až se proces dokončí, budete mít třídu, která bude podobná následujícímu vygenerovanému.</span><span class="sxs-lookup"><span data-stu-id="4d991-114">Once the process is finished you will have a class similar to the following generated</span></span>

![vygenerovaná zobrazení](~/ef6/media/generatedviews.png)

<span data-ttu-id="4d991-116">Když teď spustíte aplikaci EF, použije se tato třída k načtení zobrazení podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4d991-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="4d991-117">Pokud se váš model změní a znovu negenerujete tuto třídu, EF EF vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="4d991-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="4d991-118">Generování zobrazení mapování z kódu – EF6 a vyšší</span><span class="sxs-lookup"><span data-stu-id="4d991-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="4d991-119">Dalším způsobem, jak vygenerovat zobrazení, je použití rozhraní API, které poskytuje EF.</span><span class="sxs-lookup"><span data-stu-id="4d991-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="4d991-120">Při použití této metody máte volnost v jejich serializaci, ale budete je muset také načíst sami.</span><span class="sxs-lookup"><span data-stu-id="4d991-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="4d991-121">**Jenom EF6** – rozhraní API uvedená v této části se zavedla v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="4d991-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="4d991-122">Pokud používáte starší verzi, tyto informace se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="4d991-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="4d991-123">Generování zobrazení</span><span class="sxs-lookup"><span data-stu-id="4d991-123">Generating Views</span></span>

<span data-ttu-id="4d991-124">Rozhraní API pro generování zobrazení jsou na třídě System. data. entity. Core. Mapping. StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="4d991-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="4d991-125">Můžete načíst StorageMappingCollection pro kontext pomocí objektu MetadataWorkspace objektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="4d991-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="4d991-126">Pokud používáte novější rozhraní API DbContext, můžete k tomu získat přístup pomocí IObjectContextAdapter, jak je uvedené níže. v tomto kódu máme instanci odvozeného DbContextu s názvem dbContext:</span><span class="sxs-lookup"><span data-stu-id="4d991-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="4d991-127">Jakmile budete mít StorageMappingItemCollection, můžete získat přístup k metodám GenerateViews a ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="4d991-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="4d991-128">První metoda vytvoří slovník s položkou pro každé zobrazení v mapování kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4d991-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="4d991-129">Druhá metoda vypočítá hodnotu hash pro jedno mapování kontejneru a používá se za běhu k ověření, že se model nezměnil od chvíle, kdy byla zobrazení předem vygenerována.</span><span class="sxs-lookup"><span data-stu-id="4d991-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="4d991-130">Přepsání těchto dvou metod jsou k dispozici pro složité scénáře zahrnující několik mapování kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="4d991-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="4d991-131">Při generování zobrazení zavoláte metodu GenerateViews a pak vypíšete výsledné EntitySetBase a DbMappingView.</span><span class="sxs-lookup"><span data-stu-id="4d991-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="4d991-132">Také budete muset uložit hodnotu hash generovanou metodou ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="4d991-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="4d991-133">Načítání zobrazení</span><span class="sxs-lookup"><span data-stu-id="4d991-133">Loading Views</span></span>

<span data-ttu-id="4d991-134">Aby bylo možné načíst zobrazení generovaná metodou GenerateViews, můžete pro EF poskytnout třídu, která dědí z abstraktní třídy DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="4d991-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="4d991-135">DbMappingViewCache určuje dvě metody, které je nutné implementovat:</span><span class="sxs-lookup"><span data-stu-id="4d991-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="4d991-136">Vlastnost MappingHashValue musí vracet hodnotu hash generovanou metodou ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="4d991-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="4d991-137">Když EF bude vyžadovat pro zobrazení, nejprve vygeneruje a porovná hodnotu hash modelu s hodnotou hash vrácenou touto vlastností.</span><span class="sxs-lookup"><span data-stu-id="4d991-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="4d991-138">Pokud se neshodují, EF EF vyvolá výjimku EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="4d991-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="4d991-139">Metoda GetView přijme EntitySetBase a musíte vrátit DbMappingVIew obsahující EntitySql, které bylo vytvořeno pro, které bylo přidruženo k dané EntitySetBase ve slovníku generovaném metodou GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="4d991-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="4d991-140">Pokud EF požádá o zobrazení, které nemáte, měla by GetView vracet hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="4d991-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="4d991-141">Následuje extrakce z DbMappingViewCache, která je generována pomocí nástrojů Power Tools, jak je popsáno výše, v této části vidíte jeden způsob, jak uložit a načíst požadovaný EntitySql.</span><span class="sxs-lookup"><span data-stu-id="4d991-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="4d991-142">Chcete-li, aby EF používal vaše DbMappingViewCache, použijte DbMappingViewCacheTypeAttribute a určete tak kontext, pro který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="4d991-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="4d991-143">V následujícím kódu přiřadíme BlogContext ke třídě MyMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="4d991-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="4d991-144">U složitějších scénářů je možné zadat mapování instancí zobrazení mapování zadáním objektu pro vytváření mezipaměti pro zobrazení mapování.</span><span class="sxs-lookup"><span data-stu-id="4d991-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="4d991-145">To lze provést implementací abstraktní třídy System. data. entity. Infrastructure. MappingViews. DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="4d991-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="4d991-146">Instance objektu pro vytváření mezipaměti zobrazení mapování, která se používá, se dá načíst nebo nastavit pomocí StorageMappingItemCollection. MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="4d991-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
