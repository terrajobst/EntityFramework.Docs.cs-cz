---
title: Definování DbSets-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419092"
---
# <a name="defining-dbsets"></a><span data-ttu-id="2433c-102">Definování DbSets</span><span class="sxs-lookup"><span data-stu-id="2433c-102">Defining DbSets</span></span>
<span data-ttu-id="2433c-103">Při vývoji pomocí pracovního postupu Code First definujete odvozenou DbContext, která představuje vaši relaci s databází a zpřístupňuje Negenerickými pro každý typ v modelu.</span><span class="sxs-lookup"><span data-stu-id="2433c-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="2433c-104">Toto téma popisuje různé způsoby, jak můžete definovat vlastnosti Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="2433c-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="2433c-105">DbContext s vlastnostmi Negenerickými</span><span class="sxs-lookup"><span data-stu-id="2433c-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="2433c-106">Běžný případ zobrazený v Code First příklady je mít DbContext s veřejnými automatickými Negenerickými vlastnostmi pro typy entit vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="2433c-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="2433c-107">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2433c-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="2433c-108">Při použití v režimu Code First Tato akce nakonfiguruje Blogy a příspěvky jako typy entit a také nakonfiguruje další typy, které jsou z nich dosažitelné.</span><span class="sxs-lookup"><span data-stu-id="2433c-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="2433c-109">Kromě toho DbContext automaticky volá metodu setter pro každou z těchto vlastností pro nastavení instance příslušného Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="2433c-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="2433c-110">DbContext s vlastnostmi IDbSet</span><span class="sxs-lookup"><span data-stu-id="2433c-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="2433c-111">Existují situace, například při vytváření návrhů nebo napodobenin, kde je užitečnější k deklaraci vlastností sady pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2433c-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="2433c-112">V takových případech lze rozhraní IDbSet použít místo Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="2433c-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="2433c-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2433c-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="2433c-114">Tento kontext funguje naprosto stejným způsobem jako kontext, který používá třídu Negenerickými pro jeho vlastnosti set.</span><span class="sxs-lookup"><span data-stu-id="2433c-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="2433c-115">DbContext s vlastnostmi sady jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="2433c-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="2433c-116">Pokud nechcete zveřejnit veřejné metody setter pro vlastnosti Negenerickými nebo IDbSet, můžete místo toho vytvořit vlastnosti jen pro čtení a vytvořit instance sady sami.</span><span class="sxs-lookup"><span data-stu-id="2433c-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="2433c-117">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2433c-117">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

<span data-ttu-id="2433c-118">Všimněte si, že DbContext ukládá do mezipaměti instanci Negenerickými vrácenou z metody set, aby každá z těchto vlastností vrátila stejnou instanci pokaždé, když je volána.</span><span class="sxs-lookup"><span data-stu-id="2433c-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="2433c-119">Zjišťování typů entit pro Code First funguje stejným způsobem jako u vlastností s veřejnými metodami getter a setter.</span><span class="sxs-lookup"><span data-stu-id="2433c-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
