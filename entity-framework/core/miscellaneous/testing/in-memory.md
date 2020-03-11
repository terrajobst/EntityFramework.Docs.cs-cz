---
title: Testování s pamětí EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416507"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="a34f8-102">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="a34f8-102">Testing with InMemory</span></span>

<span data-ttu-id="a34f8-103">Zprostředkovatel inMemory je užitečný v případě, že chcete testovat komponenty pomocí objektu, který se blíží k reálné databázi, bez režie pro skutečné databázové operace.</span><span class="sxs-lookup"><span data-stu-id="a34f8-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="a34f8-104">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="a34f8-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="a34f8-105">Nepaměť není relační databáze.</span><span class="sxs-lookup"><span data-stu-id="a34f8-105">InMemory is not a relational database</span></span>

<span data-ttu-id="a34f8-106">Poskytovatelé databáze EF Core nemusí být relační databáze.</span><span class="sxs-lookup"><span data-stu-id="a34f8-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="a34f8-107">InMemory je navržen jako databáze pro obecné účely testování a není navržena pro napodobení relační databázi.</span><span class="sxs-lookup"><span data-stu-id="a34f8-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="a34f8-108">Mezi příklady patří:</span><span class="sxs-lookup"><span data-stu-id="a34f8-108">Some examples of this include:</span></span>

* <span data-ttu-id="a34f8-109">InMemory vám umožní ukládat data, která by v relační databázi porušila omezení referenční integrity.</span><span class="sxs-lookup"><span data-stu-id="a34f8-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="a34f8-110">Pokud pro vlastnost v modelu používáte DefaultValueSql (String), jedná se o rozhraní API relační databáze a nebude mít žádný vliv, pokud je spuštěný proti paměti.</span><span class="sxs-lookup"><span data-stu-id="a34f8-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="a34f8-111">[Souběžnost přes verzi časového razítka nebo řádku](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` nebo `IsRowVersion`) není podporována.</span><span class="sxs-lookup"><span data-stu-id="a34f8-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="a34f8-112">Pokud je aktualizace provedena pomocí starého tokenu souběžnosti, nebude vyvolána žádná [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) .</span><span class="sxs-lookup"><span data-stu-id="a34f8-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="a34f8-113">Pro mnoho testovacích účelů tyto rozdíly nezáleží.</span><span class="sxs-lookup"><span data-stu-id="a34f8-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="a34f8-114">Pokud však chcete testovat proti nějakému objektu, který se chová podobně jako skutečná relační databáze, zvažte použití [režimu v paměti SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="a34f8-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="a34f8-115">Ukázkový scénář testování</span><span class="sxs-lookup"><span data-stu-id="a34f8-115">Example testing scenario</span></span>

<span data-ttu-id="a34f8-116">Vezměte v úvahu následující službu, která umožňuje, aby kód aplikace prováděl některé operace týkající se blogů.</span><span class="sxs-lookup"><span data-stu-id="a34f8-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="a34f8-117">Interně používá `DbContext`, které se připojují k databázi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a34f8-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="a34f8-118">Je vhodné tento kontext prohodit, aby se připojil k databázi inMemory, aby bylo možné zapsat efektivní testy pro tuto službu, aniž byste museli upravovat kód, nebo udělat spoustu práce pro vytvoření dvojitě funkčního testu v kontextu.</span><span class="sxs-lookup"><span data-stu-id="a34f8-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="a34f8-119">Připravte svůj kontext</span><span class="sxs-lookup"><span data-stu-id="a34f8-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="a34f8-120">Nekonfigurují se dva poskytovatelé databáze.</span><span class="sxs-lookup"><span data-stu-id="a34f8-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="a34f8-121">V testech provedete externě konfiguraci kontextu pro použití poskytovatele inMemory.</span><span class="sxs-lookup"><span data-stu-id="a34f8-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="a34f8-122">Pokud konfigurujete poskytovatele databáze přepsáním `OnConfiguring` v kontextu, budete muset přidat nějaký podmíněný kód, abyste měli jistotu, že jste poskytovatele databáze nakonfigurovali jenom v případě, že ještě není nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="a34f8-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="a34f8-123">Pokud používáte ASP.NET Core, neměli byste tento kód potřebovat, protože váš poskytovatel databáze je už nakonfigurovaný mimo kontext (v Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="a34f8-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="a34f8-124">Přidání konstruktoru pro testování</span><span class="sxs-lookup"><span data-stu-id="a34f8-124">Add a constructor for testing</span></span>

<span data-ttu-id="a34f8-125">Nejjednodušší způsob, jak povolit testování v jiné databázi, je upravit svůj kontext, aby vystavoval konstruktor, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="a34f8-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="a34f8-126">`DbContextOptions<TContext>` informuje kontext o všech nastaveních, například o tom, ke které databázi se chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="a34f8-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="a34f8-127">Jedná se o stejný objekt, který je sestaven spuštěním metody při konfiguraci v kontextu.</span><span class="sxs-lookup"><span data-stu-id="a34f8-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="a34f8-128">Zápis testů</span><span class="sxs-lookup"><span data-stu-id="a34f8-128">Writing tests</span></span>

<span data-ttu-id="a34f8-129">Klíčem k testování pomocí tohoto poskytovatele je schopnost sdělit kontextu použití poskytovatele inMemory a řídit obor databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="a34f8-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="a34f8-130">Obvykle požadujete pro každou testovací metodu čistou databázi.</span><span class="sxs-lookup"><span data-stu-id="a34f8-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="a34f8-131">Zde je příklad třídy testu, která používá databázi inMemory.</span><span class="sxs-lookup"><span data-stu-id="a34f8-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="a34f8-132">Každá metoda testu určuje jedinečný název databáze, což znamená, že každá metoda má svou vlastní databázi inMemory.</span><span class="sxs-lookup"><span data-stu-id="a34f8-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="a34f8-133">Chcete-li použít metodu rozšíření `.UseInMemoryDatabase()`, odkázat na balíček NuGet [Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="a34f8-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
