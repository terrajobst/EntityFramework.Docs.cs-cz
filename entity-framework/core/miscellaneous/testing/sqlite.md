---
title: Testování pomocí EF Core SQLite
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417300"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="57a19-102">Testování s SQLite</span><span class="sxs-lookup"><span data-stu-id="57a19-102">Testing with SQLite</span></span>

<span data-ttu-id="57a19-103">SQLite má režim v paměti, který umožňuje použít SQLite k zápisu testů na relační databázi bez režie pro skutečné databázové operace.</span><span class="sxs-lookup"><span data-stu-id="57a19-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="57a19-104">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) najdete v tomto článku na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="57a19-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="57a19-105">Ukázkový scénář testování</span><span class="sxs-lookup"><span data-stu-id="57a19-105">Example testing scenario</span></span>

<span data-ttu-id="57a19-106">Vezměte v úvahu následující službu, která umožňuje, aby kód aplikace prováděl některé operace týkající se blogů.</span><span class="sxs-lookup"><span data-stu-id="57a19-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="57a19-107">Interně používá `DbContext`, které se připojují k databázi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="57a19-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="57a19-108">Pro připojení k databázi SQLite v paměti by bylo vhodné tento kontext prohodit, aby bylo možné zapsat efektivní testy pro tuto službu, aniž by bylo nutné kód upravovat, nebo udělat spoustu práce, aby bylo možné vytvořit test Double v kontextu.</span><span class="sxs-lookup"><span data-stu-id="57a19-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="57a19-109">Připravte svůj kontext</span><span class="sxs-lookup"><span data-stu-id="57a19-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="57a19-110">Nekonfigurují se dva poskytovatelé databáze.</span><span class="sxs-lookup"><span data-stu-id="57a19-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="57a19-111">V testech provedete externě konfiguraci kontextu pro použití poskytovatele inMemory.</span><span class="sxs-lookup"><span data-stu-id="57a19-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="57a19-112">Pokud konfigurujete poskytovatele databáze přepsáním `OnConfiguring` v kontextu, budete muset přidat nějaký podmíněný kód, abyste měli jistotu, že jste poskytovatele databáze nakonfigurovali jenom v případě, že ještě není nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="57a19-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="57a19-113">Pokud používáte ASP.NET Core, neměli byste tento kód potřebovat, protože váš poskytovatel databáze je nakonfigurovaný mimo kontext (v Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="57a19-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="57a19-114">Přidání konstruktoru pro testování</span><span class="sxs-lookup"><span data-stu-id="57a19-114">Add a constructor for testing</span></span>

<span data-ttu-id="57a19-115">Nejjednodušší způsob, jak povolit testování v jiné databázi, je upravit svůj kontext, aby vystavoval konstruktor, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="57a19-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="57a19-116">`DbContextOptions<TContext>` informuje kontext o všech nastaveních, například o tom, ke které databázi se chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="57a19-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="57a19-117">Jedná se o stejný objekt, který je sestaven spuštěním metody při konfiguraci v kontextu.</span><span class="sxs-lookup"><span data-stu-id="57a19-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="57a19-118">Zápis testů</span><span class="sxs-lookup"><span data-stu-id="57a19-118">Writing tests</span></span>

<span data-ttu-id="57a19-119">Klíč k testování pomocí tohoto poskytovatele je schopnost sdělit kontextu použití SQLite a řídit obor databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="57a19-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="57a19-120">Rozsah databáze je řízen otevřením a zavřením připojení.</span><span class="sxs-lookup"><span data-stu-id="57a19-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="57a19-121">Databáze je vymezena na dobu, po kterou je připojení otevřeno.</span><span class="sxs-lookup"><span data-stu-id="57a19-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="57a19-122">Obvykle požadujete pro každou testovací metodu čistou databázi.</span><span class="sxs-lookup"><span data-stu-id="57a19-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="57a19-123">Pokud chcete použít `SqliteConnection()` a metodu rozšíření `.UseSqlite()`, odkazujte na balíček NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="57a19-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
