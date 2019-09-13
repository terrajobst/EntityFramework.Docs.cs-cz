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
# <a name="client-vs-server-evaluation"></a>Klient vs. Vyhodnocení serveru

Entity Framework Core podporuje části dotazu vyhodnoceného na straně klienta a části, které se přidávají do databáze. Aby bylo možné zjistit, které části dotazu budou v databázi vyhodnocovány, je k poskytovateli databáze.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="client-evaluation"></a>Vyhodnocení klientů

V následujícím příkladu je pomocná metoda použita ke standardizaci adres URL pro Blogy, které se vracejí z databáze SQL Server. Protože poskytovatel SQL Server nemá žádné informace o tom, jak je tato metoda implementována, není možné ji přeložit do jazyka SQL. Všechny ostatní aspekty dotazu jsou vyhodnocovány v databázi, ale předávání vrácených `URL` prostřednictvím této metody je provedeno na klientovi.

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

## <a name="client-evaluation-performance-issues"></a>Problémy s výkonem hodnocení klienta

I když může být hodnocení klienta velmi užitečné, může v některých případech dojít k špatnému výkonu. Vezměte v úvahu následující dotaz, ve kterém se nyní ve filtru používá pomocná metoda. Vzhledem k tomu, že to nelze provést v databázi, jsou všechna data načtena do paměti a filtr je použit na straně klienta. V závislosti na množství dat a množství dat, která jsou odfiltrována, může to mít za následek špatný výkon.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>Protokolování vyhodnocení klientů

Ve výchozím nastavení EF Core zaznamená upozornění, když je provedeno vyhodnocení klienta. Další informace o zobrazení výstupu protokolování najdete v tématu [protokolování](../miscellaneous/logging.md) . 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>Volitelné chování: vyvolejte výjimku pro vyhodnocení klienta.

Chování můžete změnit, když dojde k vyhodnocení klienta buď k vyvolání, nebo provedení akce. To se provádí při nastavování možností pro váš kontext – obvykle v `DbContext.OnConfiguring`nebo v `Startup.cs` případě, že používáte ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
