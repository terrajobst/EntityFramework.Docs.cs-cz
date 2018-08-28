---
title: Práce s proxy - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 7b82dd370e67d1622fc00ff5e5275721d0fc4fe1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997200"
---
# <a name="working-with-proxies"></a><span data-ttu-id="5a594-102">Práce s proxy servery</span><span class="sxs-lookup"><span data-stu-id="5a594-102">Working with proxies</span></span>
<span data-ttu-id="5a594-103">Při vytváření instancí typů entit POCO, Entity Framework často vytváří instance dynamicky generované odvozeného typu, který funguje jako proxy pro entitu.</span><span class="sxs-lookup"><span data-stu-id="5a594-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="5a594-104">Tento proxy server přepíše některé virtuální vlastnosti entity, chcete-li vložit háky pro provádění akcí automaticky při přístupu k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5a594-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="5a594-105">Tento mechanismus je například použít pro podporu opožděné načtení vztahů.</span><span class="sxs-lookup"><span data-stu-id="5a594-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="5a594-106">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="5a594-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="5a594-107">Zakazuje vytváření proxy serveru</span><span class="sxs-lookup"><span data-stu-id="5a594-107">Disabling proxy creation</span></span>  

<span data-ttu-id="5a594-108">Někdy je užitečné pro Entity Framework zabránit ve vytváření instancí serveru proxy.</span><span class="sxs-lookup"><span data-stu-id="5a594-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="5a594-109">Například serializaci instancí bez proxy serveru je mnohem jednodušší než serializaci instancí proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="5a594-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="5a594-110">Vytváření proxy server vypnete tím, že zrušíte ProxyCreationEnabled příznak.</span><span class="sxs-lookup"><span data-stu-id="5a594-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="5a594-111">V konstruktoru kontext je pohromadě, můžete to udělat.</span><span class="sxs-lookup"><span data-stu-id="5a594-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="5a594-112">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5a594-112">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="5a594-113">Všimněte si, že EF nevytvoří proxy pro typy tam, kde není nic pro proxy server provést.</span><span class="sxs-lookup"><span data-stu-id="5a594-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="5a594-114">To znamená, že se také můžete vyhnout proxy servery tak, že typy, které jsou zapečetěné a/nebo mít žádné virtuální vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5a594-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="5a594-115">Explicitní vytvoření instance serveru proxy</span><span class="sxs-lookup"><span data-stu-id="5a594-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="5a594-116">Instance serveru proxy se nevytvoří, pokud vytvoříte instanci entity pomocí operátoru new.</span><span class="sxs-lookup"><span data-stu-id="5a594-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="5a594-117">Nemusí se jednat o problém, ale pokud budete muset vytvořit instanci proxy serveru (například tak, aby opožděné načtení nebo proxy server sledování změn bude fungovat) pak můžete tak učinit pomocí metody Create DbSet.</span><span class="sxs-lookup"><span data-stu-id="5a594-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="5a594-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5a594-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="5a594-119">Obecné verzi vytvořit lze použít, pokud chcete vytvořit instanci typu odvozené entity.</span><span class="sxs-lookup"><span data-stu-id="5a594-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="5a594-120">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5a594-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="5a594-121">Všimněte si, že metody Create přidat nebo připojit ke kontextu vytvořené entity.</span><span class="sxs-lookup"><span data-stu-id="5a594-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="5a594-122">Všimněte si, že metody Create pouze vytvoří instance samotného typu entity při vytváření typ proxy entity by nemají žádnou hodnotu, protože nic nedělali.</span><span class="sxs-lookup"><span data-stu-id="5a594-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="5a594-123">Například pokud typ entity je zapečetěná a/nebo nemá žádné virtuální vlastnosti pak vytvořit pouze vytvoří instanci typu entity.</span><span class="sxs-lookup"><span data-stu-id="5a594-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="5a594-124">Získávání skutečné entity typu z typu proxy</span><span class="sxs-lookup"><span data-stu-id="5a594-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="5a594-125">Proxy server typů mají názvy, které vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="5a594-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="5a594-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="5a594-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="5a594-127">Najít typ entity pro tento typ proxy serveru pomocí metody Metoda GetObjectType z objektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="5a594-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="5a594-128">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5a594-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="5a594-129">Upozorňujeme, že pokud typ předaný Metoda GetObjectType, vrátí se stále instance typu entity, která není typu proxy potom je typ entity.</span><span class="sxs-lookup"><span data-stu-id="5a594-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="5a594-130">To znamená, že použijete tuto metodu můžete vždy získat typ skutečné entity bez jakékoli další kontroly a zjistěte, jestli typ je typ proxy serveru, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="5a594-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
