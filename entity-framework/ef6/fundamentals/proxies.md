---
title: Práce s proxy servery – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419337"
---
# <a name="working-with-proxies"></a><span data-ttu-id="818f5-102">Práce s proxy servery</span><span class="sxs-lookup"><span data-stu-id="818f5-102">Working with proxies</span></span>
<span data-ttu-id="818f5-103">Při vytváření instancí Entity Framework typů entit POCO často vytváří instance dynamicky generovaného odvozeného typu, který funguje jako proxy pro entitu.</span><span class="sxs-lookup"><span data-stu-id="818f5-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="818f5-104">Tento proxy Přepisuje některé virtuální vlastnosti entity, aby mohl vkládat háky pro provádění akcí automaticky při získání k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="818f5-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="818f5-105">Například tento mechanismus slouží k podpoře opožděného načítání vztahů.</span><span class="sxs-lookup"><span data-stu-id="818f5-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="818f5-106">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="818f5-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="818f5-107">Zákaz vytváření proxy serveru</span><span class="sxs-lookup"><span data-stu-id="818f5-107">Disabling proxy creation</span></span>  

<span data-ttu-id="818f5-108">Někdy je užitečné zabránit Entity Framework vytváření proxy instancí.</span><span class="sxs-lookup"><span data-stu-id="818f5-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="818f5-109">Například serializace instancí, které nejsou proxy, je výrazně snazší než serializace instancí proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="818f5-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="818f5-110">Vytvoření proxy serveru může být vypnuté zrušením příznaku ProxyCreationEnabled.</span><span class="sxs-lookup"><span data-stu-id="818f5-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="818f5-111">Jedno místo, které lze provést, je v konstruktoru vašeho kontextu.</span><span class="sxs-lookup"><span data-stu-id="818f5-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="818f5-112">Příklad:</span><span class="sxs-lookup"><span data-stu-id="818f5-112">For example:</span></span>  

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

<span data-ttu-id="818f5-113">Všimněte si, že EF nevytvoří proxy pro typy, kde není nic pro to, aby mohl proxy dělat.</span><span class="sxs-lookup"><span data-stu-id="818f5-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="818f5-114">To znamená, že se můžete také vyhnout PROXYm typům, které mají zapečetěné nebo nemají žádné virtuální vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="818f5-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="818f5-115">Explicitní vytvoření instance proxy</span><span class="sxs-lookup"><span data-stu-id="818f5-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="818f5-116">Instance proxy nebude vytvořena, pokud vytvoříte instanci entity pomocí operátoru new.</span><span class="sxs-lookup"><span data-stu-id="818f5-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="818f5-117">Nemusí se jednat o problém, ale pokud potřebujete vytvořit proxy instanci (například tak, aby fungovalo opožděné načítání nebo sledování změn proxy), můžete tak učinit pomocí metody Create Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="818f5-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="818f5-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="818f5-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="818f5-119">Obecnou verzi objektu Create lze použít, pokud chcete vytvořit instanci odvozeného typu entity.</span><span class="sxs-lookup"><span data-stu-id="818f5-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="818f5-120">Příklad:</span><span class="sxs-lookup"><span data-stu-id="818f5-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="818f5-121">Všimněte si, že metoda Create nepřidá do kontextu vytvořenou entitu ani ji nepřipojí.</span><span class="sxs-lookup"><span data-stu-id="818f5-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="818f5-122">Všimněte si, že metoda Create vytvoří pouze instanci samotného typu entity, pokud vytvoření typu proxy pro entitu nemá žádnou hodnotu, protože by nedošlo k žádným chybám.</span><span class="sxs-lookup"><span data-stu-id="818f5-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="818f5-123">Například pokud je typ entity zapečetěný nebo nemá žádné virtuální vlastnosti, pak vytvořit vytvoří instanci typu entity.</span><span class="sxs-lookup"><span data-stu-id="818f5-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="818f5-124">Získání skutečného typu entity z typu proxy</span><span class="sxs-lookup"><span data-stu-id="818f5-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="818f5-125">Typy proxy mají názvy, které vypadají přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="818f5-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="818f5-126">System. data. entity. DynamicProxies. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="818f5-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="818f5-127">Typ entity pro tento typ proxy serveru můžete najít pomocí metody GetObjectType z ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="818f5-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="818f5-128">Příklad:</span><span class="sxs-lookup"><span data-stu-id="818f5-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="818f5-129">Všimněte si, že pokud typ předaný metodě GetObjectType je instancí typu entity, který není typem proxy serveru, typ entity se pořád vrátí.</span><span class="sxs-lookup"><span data-stu-id="818f5-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="818f5-130">To znamená, že tuto metodu můžete vždy použít k získání skutečného typu entity bez jakékoli jiné kontroly, aby se zobrazilo, zda typ je typ proxy serveru nebo ne.</span><span class="sxs-lookup"><span data-stu-id="818f5-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
