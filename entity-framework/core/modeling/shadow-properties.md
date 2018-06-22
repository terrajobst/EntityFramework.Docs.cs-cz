---
title: Vlastnosti stínové – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054562"
---
# <a name="shadow-properties"></a><span data-ttu-id="d0a98-102">Stínové vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d0a98-102">Shadow Properties</span></span>

<span data-ttu-id="d0a98-103">Vlastnosti stínové jsou vlastnosti, které nejsou definovány v třídě .NET entity, ale jsou definovány pro tento typ entity v modelu EF jádra.</span><span class="sxs-lookup"><span data-stu-id="d0a98-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="d0a98-104">Hodnota a stav těchto vlastností se udržuje výhradně v modulu Sledování změn.</span><span class="sxs-lookup"><span data-stu-id="d0a98-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="d0a98-105">Vlastnosti stínové jsou užitečné, když je dat v databázi, která by neměl být odhalen na typy namapované entit.</span><span class="sxs-lookup"><span data-stu-id="d0a98-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="d0a98-106">Nejčastěji se používají pro vlastnosti cizího klíče, kde je vztah mezi dvěma entitami reprezentována hodnotou cizí klíče v databázi, ale relace je spravován na typy entit pomocí vlastnosti navigace mezi typy entit.</span><span class="sxs-lookup"><span data-stu-id="d0a98-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="d0a98-107">Hodnoty vlastností stínové můžete získat a změnit prostřednictvím `ChangeTracker` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d0a98-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="d0a98-108">Vlastnosti stínové může být odkazováno v dotazech LINQ prostřednictvím `EF.Property` statickou metodu.</span><span class="sxs-lookup"><span data-stu-id="d0a98-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="d0a98-109">Konvence</span><span class="sxs-lookup"><span data-stu-id="d0a98-109">Conventions</span></span>

<span data-ttu-id="d0a98-110">Vlastnosti stínové lze vytvořit pomocí konvence při zjištění vztahu však nalezen žádný vlastností cizího klíče ve třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="d0a98-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="d0a98-111">V takovém případě bude potřeba zavést stínové vlastností cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="d0a98-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="d0a98-112">Vlastnost cizího klíče stínové budou pojmenované `<navigation property name><principal key property name>` (navigaci na závislé entity, která odkazuje na objektu entity, se používá pro pojmenování).</span><span class="sxs-lookup"><span data-stu-id="d0a98-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="d0a98-113">Pokud název hlavního klíče vlastnosti obsahuje název navigační vlastnosti, pak se chovat název `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="d0a98-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="d0a98-114">Pokud není žádná vlastnost navigace na závislé entity, se používá název typ objektu zabezpečení na příslušné místo.</span><span class="sxs-lookup"><span data-stu-id="d0a98-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="d0a98-115">Například následující výpis kódu způsobí `BlogId` stínové vlastnost uvedena na `Post` entity.</span><span class="sxs-lookup"><span data-stu-id="d0a98-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="d0a98-116">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="d0a98-116">Data Annotations</span></span>

<span data-ttu-id="d0a98-117">Vlastnosti stínové nelze vytvořit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="d0a98-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d0a98-118">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="d0a98-118">Fluent API</span></span>

<span data-ttu-id="d0a98-119">Můžete konfigurovat vlastnosti stínové rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="d0a98-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="d0a98-120">Po volání přetížení řetězec `Property` můžete některé z konfigurace volání by pro ostatní vlastnosti řetězu.</span><span class="sxs-lookup"><span data-stu-id="d0a98-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="d0a98-121">Pokud je název zadaný pro `Property` metoda odpovídá názvu existující vlastnost (vlastnost stínové nebo definovaná na třídy entita) a potom kód bude nakonfigurujte tuto existující vlastnost nemuseli zavádět nové vlastnosti stínové.</span><span class="sxs-lookup"><span data-stu-id="d0a98-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
