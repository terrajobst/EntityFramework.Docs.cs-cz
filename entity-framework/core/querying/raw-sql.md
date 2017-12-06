---
title: "Nezpracovaná SQL dotazy – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="69eff-102">Nezpracovaná SQL dotazy</span><span class="sxs-lookup"><span data-stu-id="69eff-102">Raw SQL Queries</span></span>

<span data-ttu-id="69eff-103">Entity Framework Core umožňuje rozevírací nabídku nezpracovaná dotazy SQL při práci s relační databáze.</span><span class="sxs-lookup"><span data-stu-id="69eff-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="69eff-104">To může být užitečné, pokud je dotaz, který chcete provést nedají vyjádřit pomocí LINQ, nebo pokud pomocí dotazu LINQ je výsledkem neefektivní SQL odesílány do databáze.</span><span class="sxs-lookup"><span data-stu-id="69eff-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="69eff-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="69eff-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="69eff-106">Omezení</span><span class="sxs-lookup"><span data-stu-id="69eff-106">Limitations</span></span>

<span data-ttu-id="69eff-107">Existuje několik omezení vzít na vědomí při použití nezpracovaná dotazů SQL:</span><span class="sxs-lookup"><span data-stu-id="69eff-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="69eff-108">Dotazy SQL slouží pouze k návratové typy entit, které jsou součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="69eff-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="69eff-109">Je vylepšení na našem nevyřízených položek do [povolit vrácení ad-hoc typy z nezpracovaná dotazy SQL](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="69eff-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="69eff-110">Příkaz jazyka SQL musí vracet data pro všechny vlastnosti typu entity.</span><span class="sxs-lookup"><span data-stu-id="69eff-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="69eff-111">Názvy sloupců, které vlastnosti jsou namapované na musí shodovat s názvy sloupců v sadě výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="69eff-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="69eff-112">Všimněte si, že se to neliší od EF6 kde mapování vlastnost nebo sloupce bylo ignorováno pro nezpracovanou dotazy SQL a výsledek sady sloupců museli názvy shodovat s názvy vlastností.</span><span class="sxs-lookup"><span data-stu-id="69eff-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="69eff-113">Příkaz jazyka SQL nemůže obsahovat související data.</span><span class="sxs-lookup"><span data-stu-id="69eff-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="69eff-114">Ale v mnoha případech můžete vytvoříte nad pomocí dotazu `Include` operátor vrátit související data (najdete v části [včetně souvisejících dat](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="69eff-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="69eff-115">Základní nezpracovaná dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="69eff-115">Basic raw SQL queries</span></span>

<span data-ttu-id="69eff-116">Můžete použít *FromSql* metoda rozšíření zahájíte dotazu LINQ na základě nezpracované dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="69eff-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="69eff-117">Nezpracovaná dotazy SQL můžete použít ke spuštění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="69eff-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="69eff-118">Předávání parametrů</span><span class="sxs-lookup"><span data-stu-id="69eff-118">Passing parameters</span></span>

<span data-ttu-id="69eff-119">Stejně jako u jakéhokoli rozhraní API, který přijímá SQL, je důležité o parametrizaci žádné vstupem uživatele k ochraně proti útok prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="69eff-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="69eff-120">Můžete zahrnout zástupné symboly parametrů v řetězci dotazu SQL a potom zadejte hodnoty parametrů jako další argumenty.</span><span class="sxs-lookup"><span data-stu-id="69eff-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="69eff-121">Všechny hodnoty parametrů můžete zadat budou automaticky převedeny na `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="69eff-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="69eff-122">Následující příklad předá jeden parametr uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="69eff-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="69eff-123">Když to bude vypadat jako `String.Format` syntaxe, zadaná hodnota je uzavřen do parametr a název vygenerovaný parametru vložit where `{0}` byl zadán zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="69eff-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="69eff-124">Jde o stejný dotaz, ale pomocí syntaxe interpolace řetězec, který není podporovaný v EF základní 2.0 a vyšší:</span><span class="sxs-lookup"><span data-stu-id="69eff-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="69eff-125">Můžete také vytvořit DbParameter a zadejte jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="69eff-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="69eff-126">To umožňuje používat pojmenované parametry v řetězci dotazu SQL</span><span class="sxs-lookup"><span data-stu-id="69eff-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="69eff-127">Sestavování s dotazy LINQ</span><span class="sxs-lookup"><span data-stu-id="69eff-127">Composing with LINQ</span></span>

<span data-ttu-id="69eff-128">Pokud příkaz jazyka SQL se může skládat na v databázi, potom můžete vytvořit nad počáteční nezpracovaná SQL dotazu pomocí LINQ operátory.</span><span class="sxs-lookup"><span data-stu-id="69eff-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="69eff-129">Dotazy SQL, které se dají sestavit na probíhá pomocí `SELECT` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="69eff-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="69eff-130">Následující příklad používá nezpracovaná dotaz SQL, který vybere z zavolat – funkce (TVF) a potom vytvoří na pomocí LINQ pro filtrování a řazení.</span><span class="sxs-lookup"><span data-stu-id="69eff-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="69eff-131">Včetně související data</span><span class="sxs-lookup"><span data-stu-id="69eff-131">Including related data</span></span>

<span data-ttu-id="69eff-132">Sestavování s operátory LINQ lze zahrnout související data v dotazu.</span><span class="sxs-lookup"><span data-stu-id="69eff-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="69eff-133">**Vždy používat Parametrizace pro nezpracovanou dotazů SQL:** rozhraní API, které přijímají nezpracovaná SQL řetězec, jako `FromSql` a `ExecuteSqlCommand` povolit hodnoty, které se snadno předat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="69eff-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="69eff-134">Kromě ověřování uživatelského vstupu, vždy Parametrizace používejte pro všechny hodnoty používané v nezpracovaných dotazu nebo příkaz SQL.</span><span class="sxs-lookup"><span data-stu-id="69eff-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="69eff-135">Pokud používáte zřetězení řetězců dynamicky vytvářet jakékoliv části řetězce dotazu, pak jste zodpovědní za ověření žádný vstup chránit před útoky Injektáž SQL.</span><span class="sxs-lookup"><span data-stu-id="69eff-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
