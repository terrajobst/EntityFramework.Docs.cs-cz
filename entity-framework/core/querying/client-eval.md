---
title: "Klient vs. Zkušební verze serveru – EF jádra"
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
# <a name="client-vs-server-evaluation"></a>Klient vs. Zkušební verze serveru

Entity Framework Core podporuje části dotazu vyhodnocovaný na klientovi a částí se nainstaluje do databáze. Je zprostředkovatel databáze k určení části dotazů, které se vyhodnotí v databázi.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="client-evaluation"></a>Hodnocení klientů

V následujícím příkladu metoda helper slouží pro standardizaci adresy URL pro blogy, které jsou vráceny z databáze systému SQL Server. Vzhledem k tomu, že má poskytovatel nástroje SQL Server bez přehled o tom, jak je tato metoda implementována, není možné přeloží ji do SQL. Vyhodnocují se všechny aspekty dotazu v databázi, ale předávání vrácený `URL` prostřednictvím této metody se provádí na straně klienta.

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

## <a name="disabling-client-evaluation"></a>Zakázání hodnocení klientů

Při hodnocení klientů může být velmi užitečná, v některých případech se může vést k nižšímu výkonu. Vezměte v úvahu následující dotaz, kde se teď používá metodu helper ve filtru. Protože to není možné v databázi, všechny, které jsou získávány na paměti a potom filtr je použita na straně klienta. V závislosti na množství dat a kolik dat je odfiltrovat může to vést k nižšímu výkonu.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Ve výchozím nastavení bude EF základní zaprotokolovat upozornění při hodnocení klientů se provádí. V tématu [protokolování](../miscellaneous/logging.md) Další informace o zobrazení výstupu protokolování. Chování lze změnit, pokud dojde k vyhodnocení klienta throw nebo nic nestane. To se provádí při nastavování možnosti pro váš kontext – obvykle ve `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
