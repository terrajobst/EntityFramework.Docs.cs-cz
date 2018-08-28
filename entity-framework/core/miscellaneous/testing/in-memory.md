---
title: Testování s InMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 2754d1deba98fcee0eb88669293b2197545c8874
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997889"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="8425e-102">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="8425e-102">Testing with InMemory</span></span>

<span data-ttu-id="8425e-103">Zprostředkovatel InMemory je užitečné, když chcete testovat komponenty pomocí nějakého nástroje, které se blíží připojení k databázi skutečné bez režie skutečné databázových operací.</span><span class="sxs-lookup"><span data-stu-id="8425e-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="8425e-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="8425e-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="8425e-105">InMemory není relační databáze</span><span class="sxs-lookup"><span data-stu-id="8425e-105">InMemory is not a relational database</span></span>

<span data-ttu-id="8425e-106">Poskytovatelé databází EF Core nemají být relačních databází.</span><span class="sxs-lookup"><span data-stu-id="8425e-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="8425e-107">InMemory byla navržena jako univerzální databázi pro účely testování a není navržen tak, aby napodoboval relační databáze.</span><span class="sxs-lookup"><span data-stu-id="8425e-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="8425e-108">Příklady zahrnují:</span><span class="sxs-lookup"><span data-stu-id="8425e-108">Some examples of this include:</span></span>

* <span data-ttu-id="8425e-109">InMemory vám umožní uložit data, která by způsobila porušení omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="8425e-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="8425e-110">Pokud používáte DefaultValueSql(string) pro vlastnost v modelu, to je relační databáze rozhraní API a nebude mít žádný vliv, pokud provádějí InMemory.</span><span class="sxs-lookup"><span data-stu-id="8425e-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="8425e-111">[Souběžnost prostřednictvím verze časové razítko/řádku](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` nebo `IsRowVersion`) se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="8425e-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="8425e-112">Ne [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) bude vyvolána, pokud se provádí aktualizace pomocí staré tokenem souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="8425e-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="8425e-113">Pro mnoho testovací účely nebude důležité tyto rozdíly.</span><span class="sxs-lookup"><span data-stu-id="8425e-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="8425e-114">Nicméně, pokud chcete testování proti objektu, který se chová podobně jako true relační databáze, pak zvažte použití [režimu in-memory SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="8425e-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="8425e-115">Ukázkový scénář testování</span><span class="sxs-lookup"><span data-stu-id="8425e-115">Example testing scenario</span></span>

<span data-ttu-id="8425e-116">Vezměte v úvahu následující služba, která umožňuje provádět některé operace související s blogy kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="8425e-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="8425e-117">Interně používá `DbContext` , která se připojuje k databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="8425e-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="8425e-118">Bylo by užitečné přepínat tímto kontextem za účelem připojení k databázi InMemory tak, aby jsme zápisu efektivních testů pro tuto službu bez nutnosti upravovat kód, nebo dělat spoustu práce pro vytvoření testu double kontextu.</span><span class="sxs-lookup"><span data-stu-id="8425e-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="8425e-119">Připravte váš kontext</span><span class="sxs-lookup"><span data-stu-id="8425e-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="8425e-120">Nechcete konfigurovat dva poskytovatelé databází</span><span class="sxs-lookup"><span data-stu-id="8425e-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="8425e-121">Ve vašich testech se chystáte externě nakonfigurovat místní na použití poskytovatele InMemory.</span><span class="sxs-lookup"><span data-stu-id="8425e-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="8425e-122">Pokud konfigurujete poskytovatele databáze tak, že přepíšete `OnConfiguring` v kontextu, pak budete muset přidat některé podmíněný kód tak, aby, pokud ještě nebyl nakonfigurován pouze konfigurace poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="8425e-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="8425e-123">Pokud používáte ASP.NET Core, by neměla od svého poskytovatele databáze je již nakonfigurován mimo kontext (v souboru Startup.cs) musí tento kód.</span><span class="sxs-lookup"><span data-stu-id="8425e-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="8425e-124">Přidejte konstruktor pro účely testování</span><span class="sxs-lookup"><span data-stu-id="8425e-124">Add a constructor for testing</span></span>

<span data-ttu-id="8425e-125">Nejjednodušší způsob, jak povolit testování na jinou databázi je upravit kontext k vystavení konstruktor, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="8425e-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="8425e-126">`DbContextOptions<TContext>` říká kontextu veškeré jeho nastavení, jako je například pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="8425e-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="8425e-127">Toto je stejný objekt, který je sestavený používané metody OnConfiguring váš kontext.</span><span class="sxs-lookup"><span data-stu-id="8425e-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="8425e-128">Zápis testů</span><span class="sxs-lookup"><span data-stu-id="8425e-128">Writing tests</span></span>

<span data-ttu-id="8425e-129">Klíčem k testování s tímto poskytovatelem je možnost předat kontext, který má používat poskytovatele InMemory a řízení rozsahu databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="8425e-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="8425e-130">Obvykle chcete vyčistit databázi pro každou metodu testu.</span><span class="sxs-lookup"><span data-stu-id="8425e-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="8425e-131">Tady je příklad testovací třídy, která používá databázi InMemory.</span><span class="sxs-lookup"><span data-stu-id="8425e-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="8425e-132">Každá testovací metoda Určuje jedinečný název databáze, což znamená, že každá z metod má svou vlastní databázi InMemory.</span><span class="sxs-lookup"><span data-stu-id="8425e-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="8425e-133">Použít `.UseInMemoryDatabase()` metody rozšíření, odkaz na balíček NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="8425e-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
