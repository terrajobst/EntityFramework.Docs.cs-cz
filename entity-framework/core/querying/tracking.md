---
title: Sledování vs. Žádné dotazy sledování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 588dee012039ce5ecc83f0ecf263a4ea6ca38c29
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181989"
---
# <a name="tracking-vs-no-tracking-queries"></a>Sledování vs. Žádné dotazy pro sledování

Chování při sledování Určuje, jestli Entity Framework Core v nástroji pro sledování změn budou uchovávat informace o instanci entity. Pokud je entita sledována, budou všechny změny zjištěné v entitě trvale uchovány v databázi během `SaveChanges()`. Entity Framework Core budou také opravovat navigační vlastnosti mezi entitami získanými z sledovacího dotazu a entit, které byly dříve načteny do instance DbContext.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="tracking-queries"></a>Sledování dotazů

Ve výchozím nastavení jsou sledovány dotazy, které vracejí typy entit. To znamená, že můžete provádět změny těchto instancí entit a nechat tyto změny trvale `SaveChanges()`.

V následujícím příkladu bude v průběhu `SaveChanges()` zjištěna změna v hodnocení blogů a bude uložena do databáze.

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
> Žádné sledovací dotazy stále neprovádějí rozlišení identity v rámci spuštěného dotazu. Pokud sada výsledků obsahuje stejnou entitu vícekrát, bude pro každý výskyt v sadě výsledků vrácena stejná instance třídy entity. Slabé odkazy se však používají k udržení přehledu o entitách, které již byly vráceny. Pokud předchozí výsledek se stejnou identitou přejde mimo rozsah a spustí se uvolňování paměti, můžete získat novou instanci entity. Další informace najdete v tématu [Jak funguje dotaz](xref:core/querying/how-query-works).

## <a name="tracking-and-projections"></a>Sledování a projekce

I v případě, že typ výsledku dotazu není typ entity, pokud výsledek obsahuje typy entit, které budou ve výchozím nastavení sledovány. V následujícím dotazu, který vrací anonymní typ, budou sledovány instance `Blog` v sadě výsledků.

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
