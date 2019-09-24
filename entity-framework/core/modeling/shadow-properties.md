---
title: Vlastnosti stínu – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197700"
---
# <a name="shadow-properties"></a><span data-ttu-id="bb606-102">Stínové vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bb606-102">Shadow Properties</span></span>

<span data-ttu-id="bb606-103">Vlastnosti stínu jsou vlastnosti, které nejsou definovány ve vaší třídě entity .NET, ale jsou definovány pro daný typ entity v modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="bb606-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="bb606-104">Hodnota a stav těchto vlastností se v sledování změn uchovávají čistě.</span><span class="sxs-lookup"><span data-stu-id="bb606-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="bb606-105">Vlastnosti stínu jsou užitečné, pokud jsou v databázi data, která by neměla být vystavena na mapovaných typech entit.</span><span class="sxs-lookup"><span data-stu-id="bb606-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="bb606-106">Nejčastěji se používají pro vlastnosti cizího klíče, ve kterých je vztah mezi dvěma entitami reprezentován hodnotou cizího klíče v databázi, ale vztah je spravován na typech entit pomocí navigačních vlastností mezi typy entit.</span><span class="sxs-lookup"><span data-stu-id="bb606-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="bb606-107">Hodnoty vlastností stínů se dají získat a změnit přes `ChangeTracker` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bb606-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="bb606-108">Na vlastnosti stínu lze odkazovat v dotazech LINQ prostřednictvím `EF.Property` statické metody.</span><span class="sxs-lookup"><span data-stu-id="bb606-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="bb606-109">Konvence</span><span class="sxs-lookup"><span data-stu-id="bb606-109">Conventions</span></span>

<span data-ttu-id="bb606-110">Vlastnosti stínu lze vytvořit podle konvence při zjištění relace, ale v třídě závislé entity není nalezena žádná vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="bb606-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="bb606-111">V tomto případě se zavede vlastnost stínového cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="bb606-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="bb606-112">Vlastnost stínového cizího klíče bude pojmenována `<navigation property name><principal key property name>` (navigace na závislé entitě, která odkazuje na hlavní entitu, se používá pro pojmenování).</span><span class="sxs-lookup"><span data-stu-id="bb606-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="bb606-113">Pokud název vlastnosti klíč objektu zabezpečení obsahuje název vlastnosti navigace, bude název pouze `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="bb606-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="bb606-114">Pokud není k dispozici žádná navigační vlastnost závislá entita, bude na svém místě použit název typu objektu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bb606-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="bb606-115">Například následující výpis kódu bude mít za následek `BlogId` zavedení vlastnosti Shadow `Post` do entity.</span><span class="sxs-lookup"><span data-stu-id="bb606-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="bb606-116">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="bb606-116">Data Annotations</span></span>

<span data-ttu-id="bb606-117">Vlastnosti stínu nelze vytvořit s datovými poznámkami.</span><span class="sxs-lookup"><span data-stu-id="bb606-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="bb606-118">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="bb606-118">Fluent API</span></span>

<span data-ttu-id="bb606-119">Ke konfiguraci vlastností stínu můžete použít rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="bb606-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="bb606-120">Po volání přetížení `Property` řetězce pro můžete zřetězit libovolné volání konfigurace, které byste měli pro jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bb606-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="bb606-121">Pokud se název zadaný `Property` metodě shoduje s názvem existující vlastnosti (vlastnost Shadow nebo jedna definovaná na třídě entity), pak kód bude konfigurovat tuto existující vlastnost místo zavedení nové vlastnosti stínu.</span><span class="sxs-lookup"><span data-stu-id="bb606-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
