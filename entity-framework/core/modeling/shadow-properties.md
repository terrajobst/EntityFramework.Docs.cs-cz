---
title: Stínové vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 4029539f3642f539a427f5901577d4df96c00f30
ms.sourcegitcommit: 119058fefd7f35952048f783ada68be9aa612256
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749706"
---
# <a name="shadow-properties"></a>Stínové vlastnosti

Stínové vlastnosti jsou vlastnosti, které nejsou definovány ve třídě .NET entity, ale jsou definovány pro daný typ entity v modelu EF Core. Hodnota a stav těchto vlastností je udržován čistě v nástroji Sledování změn.

Stínové vlastnosti jsou užitečné, když data v databázi, kterou by neměly být vystaveny pro typy entity pro mapovanou. Nejčastěji se používají pro vlastnosti cizího klíče, kde je vztah mezi dvěma entitami reprezentována hodnoty cizího klíče v databázi, ale vztah je spravovat na typy entit pomocí vlastnosti navigace mezi typy entit.

Hodnoty vlastností stínové můžete získat a změnit prostřednictvím `ChangeTracker` rozhraní API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Stínové vlastnosti lze odkazovat v dotazech LINQ prostřednictvím `EF.Property` statické metody.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konvence

Stínové vlastnosti lze vytvořit podle konvence při zjištění relace, ale nebyla nalezena žádná vlastnost cizího klíče ve třídě závislé entity. V takovém případě se bude zavádět stínové vlastnost cizího klíče. Bude mít vlastnost cizího klíče stínové `<navigation property name><principal key property name>` (navigaci na závislé entity, která odkazuje na základní entitu, se používá pro pojmenování). Pokud název navigační vlastnosti obsahuje název instančního objektu klíčová vlastnost a potom název bude právě `<principal key property name>`. Pokud není žádná vlastnost navigace u entity závislé, se používá název instančního objektu typu na jeho místo.

Například, bude výsledkem následující výpis kódu `BlogId` stínové vlastnosti se seznámili s `Post` entity.

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

## <a name="data-annotations"></a>Datové poznámky

Stínové vlastnosti nelze vytvořit s anotacemi dat.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci stínové vlastnosti. Po volání přetížení řetězce `Property` můžete zřetězit některý z konfigurace volání byste to udělali pro jiné vlastnosti.

Pokud název `Property` metody odpovídá názvu existující vlastnosti (vlastnosti stínové nebo definovaná ve třídě entity) a pak kód nakonfiguruje existující vlastnosti a nemuseli zavádět nové stínové vlastnosti.

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
