---
title: "Nezpracovaná SQL dotazy – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 79894c7b9fd9e40cdf14460abf5d872ee2f4b9f0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="raw-sql-queries"></a>Nezpracovaná SQL dotazy

Entity Framework Core umožňuje rozevírací nabídku nezpracovaná dotazy SQL při práci s relační databáze. To může být užitečné, pokud je dotaz, který chcete provést nedají vyjádřit pomocí LINQ, nebo pokud pomocí dotazu LINQ je výsledkem neefektivní SQL odesílány do databáze.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="limitations"></a>Omezení

Existuje několik omezení vzít na vědomí při použití nezpracovaná dotazů SQL:
* Dotazy SQL slouží pouze k návratové typy entit, které jsou součástí modelu. Je vylepšení na našem nevyřízených položek do [povolit vrácení ad-hoc typy z nezpracovaná dotazy SQL](https://github.com/aspnet/EntityFramework/issues/1862).

* Příkaz jazyka SQL musí vracet data pro všechny vlastnosti typu entity.

* Názvy sloupců, které vlastnosti jsou namapované na musí shodovat s názvy sloupců v sadě výsledků dotazu. Všimněte si, že se to neliší od EF6 kde mapování vlastnost nebo sloupce bylo ignorováno pro nezpracovanou dotazy SQL a výsledek sady sloupců museli názvy shodovat s názvy vlastností.

* Příkaz jazyka SQL nemůže obsahovat související data. Ale v mnoha případech můžete vytvoříte nad pomocí dotazu `Include` operátor vrátit související data (najdete v části [včetně souvisejících dat](#including-related-data)).

* `SELECT`příkazy předaná této metodě by měl být obecně bez možnosti složení: Pokud základní EF musí vyhodnocování operátory další dotazu na serveru (například přeložit LINQ operátory použity po `FromSql`), zadaný SQL, budou považovány za poddotazu. To znamená, že SQL předán neměl obsahovat žádné znaky nebo možnosti, které nejsou platné v poddotazu, jako například:
  * Koncové středníkem
  * Na serveru SQL Server koncové úrovni dotazu pomocného parametru, například`OPTION (HASH JOIN)`
  * Na serveru SQL Server `ORDER BY` klauzule, který není uveden z `TOP 100 PERCENT` v `SELECT` – klauzule

* SQL příkazy jinak než `SELECT` , se rozpoznávají automaticky jako bez možnosti složení. V důsledku toho úplné výsledky uložené procedury jsou vždy vrácen do klienta a jakékoli LINQ operátory použity po `FromSql` se vyhodnotí v paměti. 

## <a name="basic-raw-sql-queries"></a>Základní nezpracovaná dotazy SQL

Můžete použít *FromSql* metoda rozšíření zahájíte dotazu LINQ na základě nezpracované dotazu SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Nezpracovaná dotazy SQL můžete použít ke spuštění uložené procedury.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Předávání parametrů

Stejně jako u jakéhokoli rozhraní API, který přijímá SQL, je důležité o parametrizaci žádné vstupem uživatele k ochraně proti útok prostřednictvím injektáže SQL. Můžete zahrnout zástupné symboly parametrů v řetězci dotazu SQL a potom zadejte hodnoty parametrů jako další argumenty. Všechny hodnoty parametrů můžete zadat budou automaticky převedeny na `DbParameter`.

Následující příklad předá jeden parametr uložené procedury. Když to bude vypadat jako `String.Format` syntaxe, zadaná hodnota je uzavřen do parametr a název vygenerovaný parametru vložit where `{0}` byl zadán zástupný symbol.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Jde o stejný dotaz, ale pomocí syntaxe interpolace řetězec, který není podporovaný v EF základní 2.0 a vyšší:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Můžete také vytvořit DbParameter a zadejte jako hodnotu parametru. To umožňuje používat pojmenované parametry v řetězci dotazu SQL

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Sestavování s dotazy LINQ

Pokud příkaz jazyka SQL se může skládat na v databázi, potom můžete vytvořit nad počáteční nezpracovaná SQL dotazu pomocí LINQ operátory. Dotazy SQL, které se dají sestavit na probíhá pomocí `SELECT` – klíčové slovo.

Následující příklad používá nezpracovaná dotaz SQL, který vybere z zavolat – funkce (TVF) a potom vytvoří na pomocí LINQ pro filtrování a řazení.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Včetně související data

Sestavování s operátory LINQ lze zahrnout související data v dotazu.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Vždy používat Parametrizace pro nezpracovanou dotazů SQL:** rozhraní API, které přijímají nezpracovaná SQL řetězec, jako `FromSql` a `ExecuteSqlCommand` povolit hodnoty, které se snadno předat jako parametry. Kromě ověřování uživatelského vstupu, vždy Parametrizace používejte pro všechny hodnoty používané v nezpracovaných dotazu nebo příkaz SQL. Pokud používáte zřetězení řetězců dynamicky vytvářet jakékoliv části řetězce dotazu, pak jste zodpovědní za ověření žádný vstup chránit před útoky Injektáž SQL.
