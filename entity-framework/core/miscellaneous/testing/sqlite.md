---
title: Testování pomocí SQLite - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054172"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="5d03a-102">Testování pomocí SQLite</span><span class="sxs-lookup"><span data-stu-id="5d03a-102">Testing with SQLite</span></span>

<span data-ttu-id="5d03a-103">SQLite má režim v paměti, která umožňuje zápis testů na relační databázi, bez nutnosti skutečné databázových operací pomocí SQLite.</span><span class="sxs-lookup"><span data-stu-id="5d03a-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="5d03a-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) na Githubu</span><span class="sxs-lookup"><span data-stu-id="5d03a-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="5d03a-105">Příklad scénáře testování</span><span class="sxs-lookup"><span data-stu-id="5d03a-105">Example testing scenario</span></span>

<span data-ttu-id="5d03a-106">Vezměte v úvahu následující služby, který umožňuje aplikaci provádět některé operace související s blogy.</span><span class="sxs-lookup"><span data-stu-id="5d03a-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="5d03a-107">Interně používá `DbContext` která se připojuje k databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5d03a-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="5d03a-108">Je užitečné odkládacího souboru tímto kontextem za účelem připojení k databázi v paměti SQLite tak, aby jsme můžete zapsat efektivní testy pro tuto službu bez nutnosti změnit kód, nebo můžete provést spoustu práce vytvoření testu dvojité kontextu.</span><span class="sxs-lookup"><span data-stu-id="5d03a-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="5d03a-109">Příprava váš kontext</span><span class="sxs-lookup"><span data-stu-id="5d03a-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="5d03a-110">Vyhnout konfiguraci dva poskytovatelé databáze</span><span class="sxs-lookup"><span data-stu-id="5d03a-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="5d03a-111">Ve vašich testech budete externě nakonfigurovat kontext pro použití poskytovatele InMemory.</span><span class="sxs-lookup"><span data-stu-id="5d03a-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="5d03a-112">Pokud konfigurujete poskytovatele databáze přepsáním `OnConfiguring` v kontextu, budete muset přidat podmíněného kód, který Ujistěte se, pokud nebylo bylo nakonfigurováno pouze konfiguraci poskytovatele za databáze.</span><span class="sxs-lookup"><span data-stu-id="5d03a-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="5d03a-113">Pokud používáte ASP.NET Core, neměli vzhledem k tomu, že poskytovatel databáze nastaven mimo kontext (v souboru Startup.cs) potřebovat tento kód.</span><span class="sxs-lookup"><span data-stu-id="5d03a-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="5d03a-114">Přidejte konstruktor pro testování</span><span class="sxs-lookup"><span data-stu-id="5d03a-114">Add a constructor for testing</span></span>

<span data-ttu-id="5d03a-115">Nejjednodušší způsob, jak povolit testování proti do jiné databáze je cílem upravit váš kontext vystavit konstruktor, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="5d03a-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="5d03a-116">`DbContextOptions<TContext>`informuje kontextu veškeré jeho nastavení, jako je například databázi, ke které připojit k.</span><span class="sxs-lookup"><span data-stu-id="5d03a-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="5d03a-117">Toto je stejný objekt, který je sestavena spuštění metody OnConfiguring ve vašem kontextu.</span><span class="sxs-lookup"><span data-stu-id="5d03a-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="5d03a-118">Zápis testů</span><span class="sxs-lookup"><span data-stu-id="5d03a-118">Writing tests</span></span>

<span data-ttu-id="5d03a-119">Klíč k testování s tímto poskytovatelem přístup je schopnost říct kontext, který má používat SQLite a řízení rozsahu databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="5d03a-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="5d03a-120">Obor databáze je řízena otvírání a zavírání připojení.</span><span class="sxs-lookup"><span data-stu-id="5d03a-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="5d03a-121">Databáze je vymezen na dobu trvání připojení je otevřeno.</span><span class="sxs-lookup"><span data-stu-id="5d03a-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="5d03a-122">Obvykle budete chtít vyčištění databáze pro každou metodu test.</span><span class="sxs-lookup"><span data-stu-id="5d03a-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
