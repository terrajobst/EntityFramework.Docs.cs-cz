---
title: "Vlastnosti stínové – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
---
# <a name="shadow-properties"></a>Stínové vlastnosti

Vlastnosti stínové jsou vlastnosti, které nejsou definovány v třídě .NET entity, ale jsou definovány pro tento typ entity v modelu EF jádra. Hodnota a stav těchto vlastností se udržuje výhradně v modulu Sledování změn.

Vlastnosti stínové jsou užitečné, když je dat v databázi, která by neměl být odhalen na typy namapované entit. Nejčastěji se používají pro vlastnosti cizího klíče, kde je vztah mezi dvěma entitami reprezentována hodnotou cizí klíče v databázi, ale relace je spravován na typy entit pomocí vlastnosti navigace mezi typy entit.

Hodnoty vlastností stínové můžete získat a změnit prostřednictvím `ChangeTracker` rozhraní API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Vlastnosti stínové může být odkazováno v dotazech LINQ prostřednictvím `EF.Property` statickou metodu.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konvence

Vlastnosti stínové lze vytvořit pomocí konvence při zjištění vztahu však nalezen žádný vlastností cizího klíče ve třídě závislé entity. V takovém případě bude potřeba zavést stínové vlastností cizího klíče. Vlastnost cizího klíče stínové budou pojmenované `<navigation property name><principal key property name>` (navigaci na závislé entity, která odkazuje na objektu entity, se používá pro pojmenování). Pokud název hlavního klíče vlastnosti obsahuje název navigační vlastnosti, pak se chovat název `<principal key property name>`. Pokud není žádná vlastnost navigace na závislé entity, se používá název typ objektu zabezpečení na příslušné místo.

Například následující výpis kódu způsobí `BlogId` stínové vlastnost uvedena na `Post` entity.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Datových poznámek

Vlastnosti stínové nelze vytvořit pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Můžete konfigurovat vlastnosti stínové rozhraní Fluent API. Po volání přetížení řetězec `Property` můžete některé z konfigurace volání by pro ostatní vlastnosti řetězu.

Pokud je název zadaný pro `Property` metoda odpovídá názvu existující vlastnost (vlastnost stínové nebo definovaná na třídy entita) a potom kód bude nakonfigurujte tuto existující vlastnost nemuseli zavádět nové vlastnosti stínové.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
