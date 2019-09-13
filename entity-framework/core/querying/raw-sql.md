---
title: Nezpracované dotazy SQL – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 7a0df6fb656be58103971f45b9e12e9f1383311f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921718"
---
# <a name="raw-sql-queries"></a>Nezpracované dotazy SQL

Entity Framework Core umožňuje vyřadit z provozu nezpracované dotazy SQL při práci s relační databází. To může být užitečné v případě, že dotaz, který chcete provést, nelze vyjádřit pomocí LINQ, nebo pokud použití dotazu LINQ má za následek neefektivní dotazy SQL. Nezpracované dotazy SQL mohou vracet typy entit nebo, počínaje EF Core 2,1, [typy dotazů](xref:core/modeling/query-types) , které jsou součástí vašeho modelu.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="basic-raw-sql-queries"></a>Základní nezpracované dotazy SQL

Metodu rozšíření *z tabulek* můžete použít k zahájení dotazu LINQ na základě nezpracovaného dotazu SQL.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Nezpracované dotazy SQL lze použít ke spuštění uložené procedury.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Předávání parametrů

Stejně jako u libovolného rozhraní API, které podporuje SQL, je důležité parametrizovat libovolný vstup uživatele, aby chránil proti útoku prostřednictvím injektáže SQL. Do řetězce dotazu SQL můžete zahrnout zástupné symboly parametrů a pak zadat hodnoty parametrů jako další argumenty. Všechny hodnoty parametrů, které zadáte, budou automaticky převedeny `DbParameter`na.

Následující příklad předává jeden parametr uložené proceduře. I když to může vypadat `String.Format` jako syntaxe, zadaná hodnota je zabalena v parametru a vygenerovaný název parametru vložený, `{0}` kde byl zadán zástupný symbol.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Jedná se o stejný dotaz, ale používá syntaxi řetězcové interpolace, která je podporovaná v EF Core 2,0 a novějších verzích:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Můžete také vytvořit DbParameter a dodat ji jako hodnotu parametru:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

To umožňuje použít pojmenované parametry v řetězci dotazu SQL, což je užitečné, pokud má uložená procedura volitelné parametry:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Vytváření pomocí LINQ

Pokud je možné dotaz SQL sestavit v databázi, můžete vytvořit počáteční nezpracovaný dotaz SQL pomocí operátorů LINQ. Dotazy SQL, které mohou být tvořeny na začátku `SELECT` pomocí klíčového slova.

V následujícím příkladu se používá nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a pak se na něj vytvoří pomocí LINQ k filtrování a řazení.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>Sledování změn

Dotazy, které používají `FromSql()` přesná stejná pravidla sledování změn jako všechny ostatní dotazy LINQ v EF Core. Například pokud se jedná o typy entit dotazování, výsledky budou ve výchozím nastavení sledovány.  

V následujícím příkladu je použit nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a potom zakáže sledování změn s voláním. AsNoTracking():

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Včetně souvisejících dat

`Include()` Metodu lze použít k zahrnutí souvisejících dat, stejně jako u jakéhokoli jiného dotazu LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Omezení

Při použití nezpracovaných dotazů SQL je potřeba vědět o několika omezeních:

* Dotaz SQL musí vracet data pro všechny vlastnosti entity nebo typu dotazu.

* Názvy sloupců v sadě výsledků se musí shodovat s názvy sloupců, na které jsou vlastnosti namapovány. Všimněte si, že se liší od EF6, kde bylo mapování vlastností nebo sloupců pro nezpracované dotazy SQL ignorováno a názvy sloupců sady výsledků musely odpovídat názvům vlastností.

* Dotaz SQL nemůže obsahovat související data. V mnoha případech však můžete vytvořit dotaz nad dotazem pomocí `Include` operátoru, který vrátí související data (viz [zahrnutí souvisejících dat](#including-related-data)).

* `SELECT`příkazy předané do této metody by obecně měly být sestavitelné: Pokud EF Core potřebuje vyhodnotit další operátory pro dotazování na serveru (například převést operátory LINQ použité po `FromSql`), bude zadaný SQL považován za poddotaz. To znamená, že předaný SQL by neměl obsahovat žádné znaky ani možnosti, které nejsou platné pro poddotaz, například:
  * koncový středník
  * Na SQL Server, na koncové doporučení na úrovni dotazu (například `OPTION (HASH JOIN)`)
  * V SQL Server, `ORDER BY` klauzule, která není doprovázena `OFFSET 0` `SELECT` klauzulí nebo `TOP 100 PERCENT` v klauzuli

* Příkazy jazyka SQL jiné `SELECT` než jsou rozpoznány automaticky jako nevyhovující. V důsledku toho jsou všechny výsledky uložených procedur vždycky vraceny klientovi a všechny operátory LINQ použité po `FromSql` jsou vyhodnocovány v paměti.

> [!WARNING]  
> **Vždy používat parametrizace pro nezpracované dotazy SQL:** Kromě ověřování vstupu uživatele vždy použijte parametrizace pro všechny hodnoty používané v nezpracovaných dotazech nebo příkazech jazyka SQL. Rozhraní API, která přijímají Nezpracovaný řetězec `FromSql` SQL `ExecuteSqlCommand` jako a umožňují snadné předání hodnot jako parametrů. Přetížení `FromSql` a`ExecuteSqlCommand` přijímající FormattableString také umožňují použití syntaxe řetězcové interpolace způsobem, který pomáhá chránit před útoky prostřednictvím injektáže SQL. 
> 
> Pokud používáte zřetězení řetězců nebo interpolace k dynamickému sestavení jakékoli části řetězce dotazu nebo předání vstupu uživatele k příkazům nebo uloženým procedurám, které mohou spustit tyto vstupy jako dynamické SQL, pak zodpovídáte za ověření jakéhokoli vstupu na Chraňte proti útokům prostřednictvím injektáže SQL.
