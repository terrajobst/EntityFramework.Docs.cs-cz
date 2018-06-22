---
title: Testování s InMemory - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
ms.locfileid: "27995584"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="25f12-102">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="25f12-102">Testing with InMemory</span></span>

<span data-ttu-id="25f12-103">Zprostředkovatel InMemory je užitečné, když chcete testovat komponent pomocí něco, co blíží připojení k databázi skutečné bez režie skutečné databázových operací.</span><span class="sxs-lookup"><span data-stu-id="25f12-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="25f12-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="25f12-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="25f12-105">InMemory není relační databáze</span><span class="sxs-lookup"><span data-stu-id="25f12-105">InMemory is not a relational database</span></span>

<span data-ttu-id="25f12-106">EF základní databáze zprostředkovatelé nemusí být relačních databází.</span><span class="sxs-lookup"><span data-stu-id="25f12-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="25f12-107">InMemory je navržený jako databázi obecné účely pro testování a není navržen tak, aby napodoboval relační databáze.</span><span class="sxs-lookup"><span data-stu-id="25f12-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="25f12-108">Některé příklady tohoto:</span><span class="sxs-lookup"><span data-stu-id="25f12-108">Some examples of this include:</span></span>
* <span data-ttu-id="25f12-109">InMemory vám umožní uložit data, která by způsobila porušení omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="25f12-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="25f12-110">Pokud používáte DefaultValueSql(string) pro vlastnost v modelu, to je rozhraní API, relační databáze a nebude mít žádný vliv, při spuštění proti InMemory.</span><span class="sxs-lookup"><span data-stu-id="25f12-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="25f12-111">Pro mnoho testovací účely nebude podstatné tyto rozdíly.</span><span class="sxs-lookup"><span data-stu-id="25f12-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="25f12-112">Ale pokud chcete k testování proti něco, co se chová podobně jako hodnota true, relační databáze, pak zvažte použití [SQLite v paměti režimu](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="25f12-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="25f12-113">Příklad scénáře testování</span><span class="sxs-lookup"><span data-stu-id="25f12-113">Example testing scenario</span></span>

<span data-ttu-id="25f12-114">Vezměte v úvahu následující služby, který umožňuje aplikaci provádět některé operace související s blogy.</span><span class="sxs-lookup"><span data-stu-id="25f12-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="25f12-115">Interně používá `DbContext` která se připojuje k databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="25f12-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="25f12-116">Je užitečné odkládacího souboru tímto kontextem za účelem připojení k databázi InMemory, aby jsme můžete zapsat efektivní testy pro tuto službu bez nutnosti změnit kód, nebo můžete provést spoustu práce vytvoření testu dvojité kontextu.</span><span class="sxs-lookup"><span data-stu-id="25f12-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="25f12-117">Příprava váš kontext</span><span class="sxs-lookup"><span data-stu-id="25f12-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="25f12-118">Vyhnout konfiguraci dva poskytovatelé databáze</span><span class="sxs-lookup"><span data-stu-id="25f12-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="25f12-119">Ve vašich testech budete externě nakonfigurovat kontext pro použití poskytovatele InMemory.</span><span class="sxs-lookup"><span data-stu-id="25f12-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="25f12-120">Pokud konfigurujete poskytovatele databáze přepsáním `OnConfiguring` v kontextu, budete muset přidat podmíněného kód, který Ujistěte se, pokud nebylo bylo nakonfigurováno pouze konfiguraci poskytovatele za databáze.</span><span class="sxs-lookup"><span data-stu-id="25f12-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="25f12-121">Pokud používáte ASP.NET Core, neměli vzhledem k tomu, že poskytovatel databáze je již nakonfigurována mimo kontext (v souboru Startup.cs) potřebovat tento kód.</span><span class="sxs-lookup"><span data-stu-id="25f12-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="25f12-122">Přidejte konstruktor pro testování</span><span class="sxs-lookup"><span data-stu-id="25f12-122">Add a constructor for testing</span></span>

<span data-ttu-id="25f12-123">Nejjednodušší způsob, jak povolit testování proti do jiné databáze je cílem upravit váš kontext vystavit konstruktor, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="25f12-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="25f12-124">`DbContextOptions<TContext>`informuje kontextu veškeré jeho nastavení, jako je například databázi, ke které připojit k.</span><span class="sxs-lookup"><span data-stu-id="25f12-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="25f12-125">Toto je stejný objekt, který je sestavena spuštění metody OnConfiguring ve vašem kontextu.</span><span class="sxs-lookup"><span data-stu-id="25f12-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="25f12-126">Zápis testů</span><span class="sxs-lookup"><span data-stu-id="25f12-126">Writing tests</span></span>

<span data-ttu-id="25f12-127">Klíč k testování s tímto poskytovatelem přístup je schopnost říct kontext, který má použít poskytovatele InMemory a řízení rozsahu databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="25f12-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="25f12-128">Obvykle budete chtít vyčištění databáze pro každou metodu test.</span><span class="sxs-lookup"><span data-stu-id="25f12-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="25f12-129">Tady je příklad testovací třídu, která používá databázi InMemory.</span><span class="sxs-lookup"><span data-stu-id="25f12-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="25f12-130">Každá metoda testovací Určuje jedinečný název databáze, což znamená, že každá z metod má svou vlastní databázi InMemory.</span><span class="sxs-lookup"><span data-stu-id="25f12-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="25f12-131">Chcete-li použít `.UseInMemoryDatabase()` metoda rozšíření, odkaz na balíček NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="25f12-131">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
