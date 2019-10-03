---
title: Nezpracované dotazy SQL – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813600"
---
# <a name="raw-sql-queries"></a>Nezpracované dotazy SQL

Entity Framework Core umožňuje vyřadit z provozu nezpracované dotazy SQL při práci s relační databází. To může být užitečné v případě, že dotaz, který chcete provést, nelze vyjádřit pomocí LINQ, nebo pokud použití dotazu LINQ má za následek neefektivní dotaz SQL. Nezpracované dotazy SQL mohou vracet běžné typy entit nebo [typy entit bez klíčů](xref:core/modeling/keyless-entity-types) , které jsou součástí vašeho modelu.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) na Githubu.

## <a name="basic-raw-sql-queries"></a>Základní nezpracované dotazy SQL

Můžete použít `FromSqlRaw` metodu rozšíření k zahájení dotazu LINQ na základě nezpracovaného dotazu SQL.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

Nezpracované dotazy SQL lze použít ke spuštění uložené procedury.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Předávání parametrů

> [!WARNING]
> **Vždy používat parametrizace pro nezpracované dotazy SQL**
>
> Při zavádění jakýchkoli uživatelsky zadaných hodnot do nezpracovaného dotazu SQL je nutné dbát na to, aby se zabránilo útokům prostřednictvím injektáže SQL. Kromě ověření, že tyto hodnoty neobsahují neplatné znaky, vždy použijte Parametrizace, který odesílá hodnoty oddělené od textu SQL.
>
> Konkrétně nikdy nepředávejte zřetězený nebo interpolující řetězec (`$""`) s neověřenými uživatelem poskytnutými hodnotami do `FromSqlRaw` nebo `ExecuteSqlRaw`. Metody `FromSqlInterpolated` a`ExecuteSqlInterpolated` umožňují použití syntaxe řetězcové interpolace způsobem, který chrání před útoky prostřednictvím injektáže SQL.

Následující příklad předává jeden parametr uložené proceduře zahrnutím zástupného symbolu parametru do řetězce dotazu SQL a zadáním dalšího argumentu. Přestože to může vypadat jako `String.Format` syntaxe, zadaná hodnota je zabalena `DbParameter` v a a vygenerovaný název `{0}` parametru vložený, kde byl zadán zástupný symbol.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Jako alternativu k `FromSqlRaw`, můžete použít `FromSqlInterpolated` , který umožňuje bezpečné použití řetězcové interpolace. Stejně jako v předchozím příkladu je hodnota převedena na `DbParameter` a, proto není ohrožena vložením SQL:

> [!NOTE]
> Před verzí 3,0 `FromSqlRaw` a `FromSqlInterpolated` byly dvě přetížení s názvem `FromSql`. Další podrobnosti najdete v [části předchozí verze](#previous-versions) .

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Můžete také vytvořit DbParameter a dodat ji jako hodnotu parametru. Vzhledem k tomu, že je použit běžný zástupný symbol parametru SQL, nikoli zástupný `FromSqlRaw` symbol řetězce, lze bezpečně použít:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

To umožňuje použít pojmenované parametry v řetězci dotazu SQL, což je užitečné, pokud má uložená procedura volitelné parametry:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Vytváření pomocí LINQ

Pokud je možné dotaz SQL sestavit v databázi, můžete vytvořit počáteční nezpracovaný dotaz SQL pomocí operátorů LINQ. Dotazy SQL, které mohou být tvořeny na začátku `SELECT` pomocí klíčového slova.

V následujícím příkladu se používá nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a pak se na něj vytvoří pomocí LINQ k filtrování a řazení.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Tím se vytvoří následující dotaz SQL:

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Sledování změn

Dotazy, které používají `FromSql` metody, dodržují stejná pravidla sledování změn jako jakýkoli jiný dotaz LINQ v EF Core. Například pokud se jedná o typy entit dotazování, výsledky budou ve výchozím nastavení sledovány.

V následujícím příkladu je použit nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a potom zakáže sledování změn voláním `AsNoTracking`:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Včetně souvisejících dat

`Include` Metodu lze použít k zahrnutí souvisejících dat, stejně jako u jakéhokoli jiného dotazu LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Všimněte si, že to vyžaduje, aby byl nezpracovaný dotaz SQL sestavitelný; nebude to zejména fungovat s voláními uložených procedur. V části [omezení](#limitations)si přečtěte poznámky k možnosti vytváření.

## <a name="limitations"></a>Omezení

Při použití nezpracovaných dotazů SQL je potřeba vědět o několika omezeních:

* Dotaz SQL musí vracet data pro všechny vlastnosti typu entity.

* Názvy sloupců v sadě výsledků se musí shodovat s názvy sloupců, na které jsou vlastnosti namapovány. Všimněte si, že se liší od EF6, kde bylo mapování vlastností nebo sloupců pro nezpracované dotazy SQL ignorováno a názvy sloupců sady výsledků musely odpovídat názvům vlastností.

* Dotaz SQL nemůže obsahovat související data. V mnoha případech však můžete vytvořit dotaz nad dotazem pomocí `Include` operátoru, který vrátí související data (viz [zahrnutí souvisejících dat](#including-related-data)).

* `SELECT`příkazy předané do této metody by obecně měly být sestavitelné: Pokud EF Core potřebuje vyhodnotit další operátory pro dotazování na serveru (například převést operátory LINQ použité po `FromSql` metodách), bude zadaný SQL považován za poddotaz. To znamená, že předaný SQL by neměl obsahovat žádné znaky ani možnosti, které nejsou platné pro poddotaz, například:
  * koncový středník
  * Na SQL Server, na koncové doporučení na úrovni dotazu (například `OPTION (HASH JOIN)`)
  * V SQL Server, `ORDER BY` klauzule, která není doprovázena `OFFSET 0` `SELECT` klauzulí nebo `TOP 100 PERCENT` v klauzuli

* Všimněte si, že SQL Server nepovoluje sestavování prostřednictvím volání uložených procedur, takže případný pokus o použití dalších operátorů dotazu na takové volání způsobí neplatnost SQL. Operátory dotazů lze zavést po `AsEnumerable()` vyhodnocení klientů.

## <a name="previous-versions"></a>Předchozí verze

EF Core verze 2,2 a starší obsahovala dvě přetížení s názvem `FromSql` , která se chovají stejným způsobem jako novější `FromSqlRaw` a `FromSqlInterpolated`. To usnadňuje náhodné volání metody nezpracovaného řetězce, pokud by záměr byl zavolat metodu interpolované řetězce a druhým způsobem. To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.
