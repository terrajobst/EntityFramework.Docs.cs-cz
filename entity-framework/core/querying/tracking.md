---
title: Sledování vs. Ne sledování dotazy – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>Sledování vs. Ne sledování dotazy

Ovládací prvky chování sledování, zda Entity Framework Core uchovávat informace o instanci entity v jeho sledování změn. Pokud je sledovat entitu, všechny změny v entitě bude natrvalo k databázi během `SaveChanges()`. Entity Framework Core bude také opravu up navigační vlastnosti mezi entit, které jsou získány z dotazu sledování a entitami, které byly dříve načteny do DbContext instance.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="tracking-queries"></a>Dotazy pro sledování

Ve výchozím nastavení jsou sledování dotazů, které vrátí typy entit. To znamená, můžete provádět změny těchto instancí entit a tyto změny ve uchováván `SaveChanges()`.

V následujícím příkladu bude zjistil změnu hodnocení blogy a trvalé k databázi během `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Ne sledování dotazy

Žádné dotazy sledování jsou užitečné, pokud výsledky se používá ve scénáři jen pro čtení. Jsou rychlejší provést, protože není nutné k instalaci informace o sledování změn.

Konkrétní dotaz na být ne sledování, můžete zaměnit:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Můžete také změnit výchozí nastavení sledování chování na úrovni instance kontextu:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Žádné dotazy sledování i nadále provádět řešení identit v rámci dotaz spustit. Pokud sadu výsledků dotazu obsahuje stejné entity vícekrát, stejnou instanci třídy entita se vrátí pro každý výskyt v sadě výsledků dotazu. Slabé odkazy jsou však použít ke sledování entit, které již byly vráceny. Pokud předchozí výsledek se stejnou identitou ocitne mimo obor, a uvolňování paměti běží, zobrazí se nové instance entity. Další informace najdete v tématu [jak funguje dotazu](overview.md).

## <a name="tracking-and-projections"></a>Sledování a projekce

I když výsledný typ dotazu není typ entity, pokud výsledek obsahuje typy entit se stále sledovat ve výchozím nastavení. V následující dotaz, který vrátí instanci anonymního typu instance `Blog` ve výsledku se bude sledovat sady.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
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

Pokud sada výsledků neobsahuje žádné typy entit, je provedena bez sledování. V následující dotaz, který vrátí anonymní typ s některé hodnoty z entity (ale žádné instance skutečný typ entity), neexistuje žádný sledování provádět.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
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
