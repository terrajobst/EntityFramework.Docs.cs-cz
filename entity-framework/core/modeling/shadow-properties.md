---
title: Stínové vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993799"
---
# <a name="shadow-properties"></a><span data-ttu-id="7b6aa-102">Stínové vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7b6aa-102">Shadow Properties</span></span>

<span data-ttu-id="7b6aa-103">Stínové vlastnosti jsou vlastnosti, které nejsou definovány ve třídě .NET entity, ale jsou definovány pro daný typ entity v modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="7b6aa-104">Hodnota a stav těchto vlastností je udržován čistě v nástroji Sledování změn.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="7b6aa-105">Stínové vlastnosti jsou užitečné, když data v databázi, kterou by neměly být vystaveny pro typy entity pro mapovanou.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="7b6aa-106">Nejčastěji se používají pro vlastnosti cizího klíče, kde je vztah mezi dvěma entitami reprezentována hodnoty cizího klíče v databázi, ale vztah je spravovat na typy entit pomocí vlastnosti navigace mezi typy entit.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="7b6aa-107">Hodnoty vlastností stínové můžete získat a změnit prostřednictvím `ChangeTracker` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="7b6aa-108">Stínové vlastnosti lze odkazovat v dotazech LINQ prostřednictvím `EF.Property` statické metody.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="7b6aa-109">Konvence</span><span class="sxs-lookup"><span data-stu-id="7b6aa-109">Conventions</span></span>

<span data-ttu-id="7b6aa-110">Stínové vlastnosti lze vytvořit podle konvence při zjištění relace, ale nebyla nalezena žádná vlastnost cizího klíče ve třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="7b6aa-111">V takovém případě se bude zavádět stínové vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="7b6aa-112">Bude mít vlastnost cizího klíče stínové `<navigation property name><principal key property name>` (navigaci na závislé entity, která odkazuje na základní entitu, se používá pro pojmenování).</span><span class="sxs-lookup"><span data-stu-id="7b6aa-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="7b6aa-113">Pokud název navigační vlastnosti obsahuje název instančního objektu klíčová vlastnost a potom název bude právě `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="7b6aa-114">Pokud není žádná vlastnost navigace u entity závislé, se používá název instančního objektu typu na jeho místo.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="7b6aa-115">Například, bude výsledkem následující výpis kódu `BlogId` stínové vlastnosti se seznámili s `Post` entity.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="7b6aa-116">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="7b6aa-116">Data Annotations</span></span>

<span data-ttu-id="7b6aa-117">Stínové vlastnosti nelze vytvořit s anotacemi dat.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7b6aa-118">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="7b6aa-118">Fluent API</span></span>

<span data-ttu-id="7b6aa-119">Rozhraní Fluent API můžete použít ke konfiguraci stínové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="7b6aa-120">Po volání přetížení řetězce `Property` můžete zřetězit některý z konfigurace volání byste to udělali pro jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="7b6aa-121">Pokud název `Property` metody odpovídá názvu existující vlastnosti (vlastnosti stínové nebo definovaná ve třídě entity) a pak kód nakonfiguruje existující vlastnosti a nemuseli zavádět nové stínové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b6aa-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
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
