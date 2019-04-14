---
title: Testování s SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: e8ff204a09d50064b4f0d4376f02b05c8681ac25
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562530"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="9eee7-102">Testování s SQLite</span><span class="sxs-lookup"><span data-stu-id="9eee7-102">Testing with SQLite</span></span>

<span data-ttu-id="9eee7-103">SQLite má režim v paměti, která umožňuje používat k psaní testů pro relační databázi, bez režie skutečné databázových operací SQLite.</span><span class="sxs-lookup"><span data-stu-id="9eee7-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="9eee7-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) na Githubu</span><span class="sxs-lookup"><span data-stu-id="9eee7-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="9eee7-105">Ukázkový scénář testování</span><span class="sxs-lookup"><span data-stu-id="9eee7-105">Example testing scenario</span></span>

<span data-ttu-id="9eee7-106">Vezměte v úvahu následující služba, která umožňuje provádět některé operace související s blogy kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="9eee7-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="9eee7-107">Interně používá `DbContext` , která se připojuje k databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="9eee7-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="9eee7-108">Bylo by užitečné přepínat tímto kontextem za účelem připojení k databázi SQLite v paměti tak, aby jsme zápisu efektivních testů pro tuto službu bez nutnosti upravovat kód, nebo dělat spoustu práce pro vytvoření testu double kontextu.</span><span class="sxs-lookup"><span data-stu-id="9eee7-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="9eee7-109">Připravte váš kontext</span><span class="sxs-lookup"><span data-stu-id="9eee7-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="9eee7-110">Nechcete konfigurovat dva poskytovatelé databází</span><span class="sxs-lookup"><span data-stu-id="9eee7-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="9eee7-111">Ve vašich testech se chystáte externě nakonfigurovat místní na použití poskytovatele InMemory.</span><span class="sxs-lookup"><span data-stu-id="9eee7-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="9eee7-112">Pokud konfigurujete poskytovatele databáze tak, že přepíšete `OnConfiguring` v kontextu, pak budete muset přidat některé podmíněný kód tak, aby, pokud ještě nebyl nakonfigurován pouze konfigurace poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="9eee7-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="9eee7-113">Pokud používáte ASP.NET Core, by neměla od svého poskytovatele databáze je nakonfigurovaný mimo kontext (v souboru Startup.cs) musí tento kód.</span><span class="sxs-lookup"><span data-stu-id="9eee7-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="9eee7-114">Přidejte konstruktor pro účely testování</span><span class="sxs-lookup"><span data-stu-id="9eee7-114">Add a constructor for testing</span></span>

<span data-ttu-id="9eee7-115">Nejjednodušší způsob, jak povolit testování na jinou databázi je upravit kontext k vystavení konstruktor, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="9eee7-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="9eee7-116">`DbContextOptions<TContext>` říká kontextu veškeré jeho nastavení, jako je například pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="9eee7-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="9eee7-117">Toto je stejný objekt, který je sestavený používané metody OnConfiguring váš kontext.</span><span class="sxs-lookup"><span data-stu-id="9eee7-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="9eee7-118">Zápis testů</span><span class="sxs-lookup"><span data-stu-id="9eee7-118">Writing tests</span></span>

<span data-ttu-id="9eee7-119">Klíčem k testování s tímto poskytovatelem je možnost předat kontext, který má použít SQLite a řízení rozsahu databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="9eee7-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="9eee7-120">Obor databáze je řízena otevírání a zavírání připojení.</span><span class="sxs-lookup"><span data-stu-id="9eee7-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="9eee7-121">Databáze je vymezen na dobu, po kterou je připojení otevřeno.</span><span class="sxs-lookup"><span data-stu-id="9eee7-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="9eee7-122">Obvykle chcete vyčistit databázi pro každou metodu testu.</span><span class="sxs-lookup"><span data-stu-id="9eee7-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="9eee7-123">Chcete-li použít `SqliteConnection()` a `.UseSqlite()` metody rozšíření, odkaz na balíček NuGet [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="9eee7-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
