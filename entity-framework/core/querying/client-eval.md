---
title: Vyhodnocení klienta vs. Zkušební verze serveru – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 78f8d9576748a725634665f915def80b5a13820c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997874"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="d2b73-102">Vyhodnocení klienta vs. Zkušební verze serveru</span><span class="sxs-lookup"><span data-stu-id="d2b73-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="d2b73-103">Entity Framework Core podporuje částí dotazu za vyhodnocovaný na klientovi a částí se nainstaluje do databáze.</span><span class="sxs-lookup"><span data-stu-id="d2b73-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="d2b73-104">Je až poskytovatel databáze k určení, které části dotazu se vyhodnotí v databázi.</span><span class="sxs-lookup"><span data-stu-id="d2b73-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="d2b73-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d2b73-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="d2b73-106">Hodnocení klientů</span><span class="sxs-lookup"><span data-stu-id="d2b73-106">Client evaluation</span></span>

<span data-ttu-id="d2b73-107">V následujícím příkladu se používá pomocnou metodu ke standardizaci adresy URL pro blogy, které jsou vráceny z databáze serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d2b73-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="d2b73-108">Protože zprostředkovatele SQL Server nemá žádné přehled o tom, jak tato metoda je implementována, není možné přeložit do SQL.</span><span class="sxs-lookup"><span data-stu-id="d2b73-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="d2b73-109">Všechny ostatní aspekty dotazu jsou vyhodnocovány v databázi, ale předejte vrácené `URL` prostřednictvím této metody se provádí na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d2b73-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="disabling-client-evaluation"></a><span data-ttu-id="d2b73-110">Zakázání hodnocení klientů</span><span class="sxs-lookup"><span data-stu-id="d2b73-110">Disabling client evaluation</span></span>

<span data-ttu-id="d2b73-111">Hodnocení klientů mohou být velmi užitečné v některých případech může vést ke špatnému výkonu.</span><span class="sxs-lookup"><span data-stu-id="d2b73-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="d2b73-112">Vezměte v úvahu následující dotaz, kde se teď používá pomocnou metodu ve filtru.</span><span class="sxs-lookup"><span data-stu-id="d2b73-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="d2b73-113">Protože toto nelze provést v databázi, vše, co data se načítají do paměti a potom filtr platí na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d2b73-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="d2b73-114">V závislosti na množství dat a jak velká část těchto dat je odfiltrována to mohlo způsobit snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="d2b73-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="d2b73-115">Ve výchozím nastavení bude EF Core zaprotokolovat upozornění při vyhodnocení klienta se provádí.</span><span class="sxs-lookup"><span data-stu-id="d2b73-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="d2b73-116">Zobrazit [protokolování](../miscellaneous/logging.md) pro další informace o prohlížení uložit výstup protokolování.</span><span class="sxs-lookup"><span data-stu-id="d2b73-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="d2b73-117">Můžete změnit chování, když dojde k hodnocení klientů provést operaci throw nebo Neprovádět žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="d2b73-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="d2b73-118">To se provádí při nastavování možnosti pro váš kontext – obvykle v `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2b73-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
