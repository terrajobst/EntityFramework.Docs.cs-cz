---
title: Klient vs. Zkušební verze serveru – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="01831-102">Klient vs. Zkušební verze serveru</span><span class="sxs-lookup"><span data-stu-id="01831-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="01831-103">Entity Framework Core podporuje části dotazu vyhodnocovaný na klientovi a částí se nainstaluje do databáze.</span><span class="sxs-lookup"><span data-stu-id="01831-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="01831-104">Je zprostředkovatel databáze k určení části dotazů, které se vyhodnotí v databázi.</span><span class="sxs-lookup"><span data-stu-id="01831-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="01831-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="01831-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="01831-106">Hodnocení klientů</span><span class="sxs-lookup"><span data-stu-id="01831-106">Client evaluation</span></span>

<span data-ttu-id="01831-107">V následujícím příkladu metoda helper slouží pro standardizaci adresy URL pro blogy, které jsou vráceny z databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="01831-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="01831-108">Vzhledem k tomu, že má poskytovatel nástroje SQL Server bez přehled o tom, jak je tato metoda implementována, není možné přeloží ji do SQL.</span><span class="sxs-lookup"><span data-stu-id="01831-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="01831-109">Vyhodnocují se všechny aspekty dotazu v databázi, ale předávání vrácený `URL` prostřednictvím této metody se provádí na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="01831-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="01831-110">Zakázání hodnocení klientů</span><span class="sxs-lookup"><span data-stu-id="01831-110">Disabling client evaluation</span></span>

<span data-ttu-id="01831-111">Při hodnocení klientů může být velmi užitečná, v některých případech se může vést k nižšímu výkonu.</span><span class="sxs-lookup"><span data-stu-id="01831-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="01831-112">Vezměte v úvahu následující dotaz, kde se teď používá metodu helper ve filtru.</span><span class="sxs-lookup"><span data-stu-id="01831-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="01831-113">Protože to není možné v databázi, všechny, které jsou získávány na paměti a potom filtr je použita na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="01831-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="01831-114">V závislosti na množství dat a kolik dat je odfiltrovat může to vést k nižšímu výkonu.</span><span class="sxs-lookup"><span data-stu-id="01831-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="01831-115">Ve výchozím nastavení bude EF základní zaprotokolovat upozornění při hodnocení klientů se provádí.</span><span class="sxs-lookup"><span data-stu-id="01831-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="01831-116">V tématu [protokolování](../miscellaneous/logging.md) Další informace o zobrazení výstupu protokolování.</span><span class="sxs-lookup"><span data-stu-id="01831-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="01831-117">Chování lze změnit, pokud dojde k vyhodnocení klienta throw nebo nic nestane.</span><span class="sxs-lookup"><span data-stu-id="01831-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="01831-118">To se provádí při nastavování možnosti pro váš kontext – obvykle ve `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01831-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
