---
title: Sledování vs. Sledování bez dotazy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 6c5d516fcb3950ae168860029660e1b1061546b8
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668775"
---
# <a name="tracking-vs-no-tracking-queries"></a>Sledování vs. Sledování bez dotazy

Sledování chování ovládacích prvků, zda Entity Framework Core uchovávat informace o instanci entity v jeho sledování změn. Pokud je sledována entity, všechny změny v entitě se ukládají do databáze během `SaveChanges()`. Entity Framework Core se také opravit navigačních vlastností mezi entitami, které jsou získávány z dotazu sledování a entity, které byly dříve načtena do DbContext instance.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="tracking-queries"></a>Sledování dotazy

Ve výchozím nastavení jsou sledování dotazy, které vracejí typy entit. To znamená, že provedete změny u těch instancí entit a tyto změny nebyla uložena ve `SaveChanges()`.

V následujícím příkladu, změna hodnocení blogy zjištěna a ukládají do databáze během `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Sledování bez dotazy

Žádné sledování dotazy jsou užitečné, když výsledky se používají ve scénáři jen pro čtení. Jsou rychlejší provést, protože není nutné k instalaci informace o sledování změn.

Jednotlivé dotazy se no-sledování, můžete zaměnit:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Můžete také změnit výchozí chování na úrovni instance kontextu sledování:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Žádné dotazy sledování stále zlepšovat řešení identit v rámci provádění dotazu. Pokud sada výsledků obsahuje více než jednou stejnou entitu, vrátí se stejnou instanci třídy entity u každého výskytu v sadě výsledků. Slabé odkazy se ale používají ke sledování entit, které již byly vráceny. Pokud předchozí výsledek se stejnou identitou dostane mimo rozsah a uvolňování paměti běží, může se zobrazit nová instance entity. Další informace najdete v tématu [jak funguje dotaz](overview.md).

## <a name="tracking-and-projections"></a>Sledování a projekce

I v případě, že typ výsledku dotazu není typ entity, pokud výsledek obsahuje typy entit se pořád sledovat ve výchozím nastavení. V následující dotaz, který vrátí anonymní typ instance `Blog` ve výsledku bude sledovat sadu.

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

Pokud sada výsledků neobsahuje žádné typy entit, se neprovádí žádné sledování. V následující dotaz, který vrátí anonymní typ s některými z hodnot z entity (ale bez instance typu skutečné entity), neexistuje žádné sledování provádět.

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
