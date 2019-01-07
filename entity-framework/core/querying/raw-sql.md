---
title: Nezpracované dotazy SQL – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 5bddddfbc2fe8d0ba99914f03b28bde4076fae42
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058707"
---
# <a name="raw-sql-queries"></a>Nezpracované dotazy SQL

Entity Framework Core umožňuje rozevírací seznam pro nezpracované dotazy SQL při práci s relační databáze. To může být užitečné, pokud dotaz, který chcete provést nelze vyjádřen pomocí jazyka LINQ, nebo pomocí dotazu LINQ je výsledkem neefektivní dotazy SQL. Nezpracované dotazy SQL může vrátit typy entit nebo, počínaje EF Core 2.1 [typy dotazů](xref:core/modeling/query-types) , které jsou součástí modelu.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="limitations"></a>Omezení

Existuje několik omezení, která je potřeba při používání nezpracované dotazy SQL:

* Příkaz jazyka SQL musí vracet data pro všechny vlastnosti typu entity nebo dotazu.

* Názvy sloupců v sadě výsledků musí shodovat s názvy sloupců, které vlastnosti jsou mapovány na. Všimněte si, že se liší od EF6, ve kterém byl ignorován, vlastnost nebo sloupec mapování pro nezpracované dotazy SQL a názvy musí shodovat s názvy vlastností, které sloupce sady výsledků dotazu.

* Příkaz jazyka SQL nemůže obsahovat související data. Ale v mnoha případech můžete vytvářet nad pomocí dotazu `Include` operátor vrátí související data (naleznete v tématu [včetně souvisejících dat](#including-related-data)).

* `SELECT` příkazy předaný této metodě by měl být obecně sestavitelný: Pokud EF Core potřebuje k vyhodnocení operátorů další dotazu na serveru (například pro převod operátory LINQ použity po `FromSql`), zadaný SQL bude zacházeno jako s poddotaz. To znamená, že SQL předán nesmí obsahovat žádné znaky nebo možnosti, které nejsou platné v poddotazu, jako například:
  * koncovou středníkem
  * Na serveru SQL Server, koncové pomocný parametr dotazu úrovni (třeba `OPTION (HASH JOIN)`)
  * Na serveru SQL Server `ORDER BY` klauzuli, která se připojí z `TOP 100 PERCENT` v `SELECT` – klauzule

* SQL příkazy jiných než `SELECT` jsou automaticky uznáváni jako bez možnosti složení. V důsledku toho úplné výsledky uložené procedury jsou vždy vrácen do klienta a jakékoli operátory LINQ použity po `FromSql` jsou vyhodnocené jako v paměti.

## <a name="basic-raw-sql-queries"></a>Základní nezpracované dotazy SQL

Můžete použít *FromSql* metodu rozšíření k začátku dotazu LINQ na základě nezpracované dotazu SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Nezpracované dotazy SQL můžete použít ke spuštění uložené procedury.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Předávání parametrů

Stejně jako u jakékoli rozhraní API, které přijímá SQL, je potřeba parametrizovat libovolný uživatelský vstup k ochraně proti útoku prostřednictvím injektáže SQL. Můžete zahrnout parametr zástupné symboly v řetězci dotazu SQL a pak zadejte hodnoty parametrů jako další argumenty. Všechny hodnoty parametrů, které zadáte, se automaticky převedou na `DbParameter`.

Následující příklad předá jeden parametr uložené procedury. Přestože to může vypadat, jako jsou `String.Format` syntaxe, zadaná hodnota je zabalena v parametru a název vygenerovaný parametru vložen where `{0}` byl určen zástupný.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Jde o stejný dotaz, ale pomocí syntaxe interpolace řetězce, který není podporovaný v EF Core 2.0 a vyšší:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Můžete také sestavit DbParameter a zadat jako hodnotu parametru. To umožňuje použití pojmenovaných parametrů v řetězci dotazu SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Sestavování s jazykem LINQ

Pokud se dotaz SQL se může skládat na v databázi, potom můžete vytvářet nad rámec počáteční neupraveného dotazu SQL pomocí operátorů LINQ. Dotazy SQL, které mohou být složené na začínat `SELECT` – klíčové slovo.

Následující příklad používá nezpracovaná dotaz SQL, který vybere z Table-Valued – funkce (TVF) a pak vytvoří v něm pomocí jazyka LINQ k provedení filtrování a řazení.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Včetně souvisejících dat

Sestavování s operátory LINQ, je možné zahrnout související data v dotazu.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Vždy používejte Parametrizace pro nezpracované dotazy SQL:** Rozhraní API, které přijímají nezpracovaná SQL, jako řetězec `FromSql` a `ExecuteSqlCommand` povolit hodnot do snadno předat jako parametry. Kromě ověřování uživatelského vstupu, vždy používejte Parametrizace pro všechny hodnoty použité v nezpracované dotazu nebo příkaz SQL. Pokud používáte zřetězení řetězců dynamicky vytvářet všechny části řetězce dotazu, pak budete muset ověřování jakékoli vstupu pro ochranu před útoky prostřednictvím injektáže SQL.
