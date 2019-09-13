---
title: Sledování vs. Žádné dotazy sledování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: d93be5c2b727d8fbaddd103f8f367c699ae80a7c
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921646"
---
# <a name="tracking-vs-no-tracking-queries"></a>Sledování vs. Žádné dotazy pro sledování

Chování při sledování Určuje, jestli Entity Framework Core v nástroji pro sledování změn budou uchovávat informace o instanci entity. Pokud je entita sledována, budou všechny změny zjištěné v entitě trvale uchovány v databázi `SaveChanges()`. Entity Framework Core budou také opravovat navigační vlastnosti mezi entitami získanými z sledovacího dotazu a entit, které byly dříve načteny do instance DbContext.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="tracking-queries"></a>Sledování dotazů

Ve výchozím nastavení jsou sledovány dotazy, které vracejí typy entit. To znamená, že můžete provádět změny těchto instancí entit a nechat tyto změny zachovat `SaveChanges()`.

V následujícím příkladu bude v průběhu `SaveChanges()`aplikace zjištěna změna hodnocení blogů a jejich uchování v databázi.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Žádné dotazy pro sledování

Žádné sledovací dotazy nejsou užitečné, pokud jsou výsledky použity ve scénáři jen pro čtení. Spouští se rychleji, protože není nutné nastavovat informace o sledování změn.

Jednotlivé dotazy můžete přepínat na možnost bez sledování:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Můžete také změnit výchozí chování při sledování na úrovni instance kontextu:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Žádné sledovací dotazy stále neprovádějí rozlišení identity v rámci spuštěného dotazu. Pokud sada výsledků obsahuje stejnou entitu vícekrát, bude pro každý výskyt v sadě výsledků vrácena stejná instance třídy entity. Slabé odkazy se však používají k udržení přehledu o entitách, které již byly vráceny. Pokud předchozí výsledek se stejnou identitou přejde mimo rozsah a spustí se uvolňování paměti, můžete získat novou instanci entity. Další informace najdete v tématu [Jak funguje dotaz](overview.md).

## <a name="tracking-and-projections"></a>Sledování a projekce

I v případě, že typ výsledku dotazu není typ entity, pokud výsledek obsahuje typy entit, které budou ve výchozím nastavení sledovány. V následujícím dotazu, který vrací anonymní typ, se budou sledovat instance `Blog` v sadě výsledků.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

Pokud sada výsledků neobsahuje žádné typy entit, sledování se neprovede. V následujícím dotazu, který vrátí anonymní typ s některými hodnotami z entity (ale žádné instance skutečného typu entity), se neprovede žádné sledování.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
