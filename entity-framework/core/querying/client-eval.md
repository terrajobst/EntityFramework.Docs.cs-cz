---
title: Klient vs. Vyhodnocení serveru – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: cb207d9e1b1004a4084dd6fc66712183b5bdd5dc
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921702"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="0cdd6-102">Klient vs. Vyhodnocení serveru</span><span class="sxs-lookup"><span data-stu-id="0cdd6-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="0cdd6-103">Entity Framework Core podporuje části dotazu vyhodnoceného na straně klienta a části, které se přidávají do databáze.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="0cdd6-104">Aby bylo možné zjistit, které části dotazu budou v databázi vyhodnocovány, je k poskytovateli databáze.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="0cdd6-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="0cdd6-106">Vyhodnocení klientů</span><span class="sxs-lookup"><span data-stu-id="0cdd6-106">Client evaluation</span></span>

<span data-ttu-id="0cdd6-107">V následujícím příkladu je pomocná metoda použita ke standardizaci adres URL pro Blogy, které se vracejí z databáze SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="0cdd6-108">Protože poskytovatel SQL Server nemá žádné informace o tom, jak je tato metoda implementována, není možné ji přeložit do jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="0cdd6-109">Všechny ostatní aspekty dotazu jsou vyhodnocovány v databázi, ale předávání vrácených `URL` prostřednictvím této metody je provedeno na klientovi.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs?highlight=6)] -->
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

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
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

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="0cdd6-110">Problémy s výkonem hodnocení klienta</span><span class="sxs-lookup"><span data-stu-id="0cdd6-110">Client evaluation performance issues</span></span>

<span data-ttu-id="0cdd6-111">I když může být hodnocení klienta velmi užitečné, může v některých případech dojít k špatnému výkonu.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="0cdd6-112">Vezměte v úvahu následující dotaz, ve kterém se nyní ve filtru používá pomocná metoda.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="0cdd6-113">Vzhledem k tomu, že to nelze provést v databázi, jsou všechna data načtena do paměti a filtr je použit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="0cdd6-114">V závislosti na množství dat a množství dat, která jsou odfiltrována, může to mít za následek špatný výkon.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="0cdd6-115">Protokolování vyhodnocení klientů</span><span class="sxs-lookup"><span data-stu-id="0cdd6-115">Client evaluation logging</span></span>

<span data-ttu-id="0cdd6-116">Ve výchozím nastavení EF Core zaznamená upozornění, když je provedeno vyhodnocení klienta.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="0cdd6-117">Další informace o zobrazení výstupu protokolování najdete v tématu [protokolování](../miscellaneous/logging.md) .</span><span class="sxs-lookup"><span data-stu-id="0cdd6-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="0cdd6-118">Volitelné chování: vyvolejte výjimku pro vyhodnocení klienta.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="0cdd6-119">Chování můžete změnit, když dojde k vyhodnocení klienta buď k vyvolání, nebo provedení akce.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="0cdd6-120">To se provádí při nastavování možností pro váš kontext – obvykle v `DbContext.OnConfiguring`nebo v `Startup.cs` případě, že používáte ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0cdd6-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
