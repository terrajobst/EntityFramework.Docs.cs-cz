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
# <a name="client-vs-server-evaluation"></a>Vyhodnocení klienta vs. Zkušební verze serveru

Entity Framework Core podporuje částí dotazu za vyhodnocovaný na klientovi a částí se nainstaluje do databáze. Je až poskytovatel databáze k určení, které části dotazu se vyhodnotí v databázi.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="client-evaluation"></a>Hodnocení klientů

V následujícím příkladu se používá pomocnou metodu ke standardizaci adresy URL pro blogy, které jsou vráceny z databáze serveru SQL Server. Protože zprostředkovatele SQL Server nemá žádné přehled o tom, jak tato metoda je implementována, není možné přeložit do SQL. Všechny ostatní aspekty dotazu jsou vyhodnocovány v databázi, ale předejte vrácené `URL` prostřednictvím této metody se provádí na straně klienta.

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

Hodnocení klientů mohou být velmi užitečné v některých případech může vést ke špatnému výkonu. Vezměte v úvahu následující dotaz, kde se teď používá pomocnou metodu ve filtru. Protože toto nelze provést v databázi, vše, co data se načítají do paměti a potom filtr platí na straně klienta. V závislosti na množství dat a jak velká část těchto dat je odfiltrována to mohlo způsobit snížení výkonu.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Ve výchozím nastavení bude EF Core zaprotokolovat upozornění při vyhodnocení klienta se provádí. Zobrazit [protokolování](../miscellaneous/logging.md) pro další informace o prohlížení uložit výstup protokolování. Můžete změnit chování, když dojde k hodnocení klientů provést operaci throw nebo Neprovádět žádnou akci. To se provádí při nastavování možnosti pro váš kontext – obvykle v `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
