---
title: Vytvoření a přemístění rozhraní API – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688626"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="7331b-102">Vytvoření a přemístění rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7331b-102">Create and Drop APIs</span></span>

<span data-ttu-id="7331b-103">Metody EnsureCreated a EnsureDeleted poskytují jednoduchý alternativou k [migrace](migrations/index.md) pro správu schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7331b-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="7331b-104">Tyto metody jsou užitečné v situacích, když je přechodná data a můžete vyřadit, když se změní schéma.</span><span class="sxs-lookup"><span data-stu-id="7331b-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="7331b-105">Například při vytváření prototypů, v testech, nebo pro místní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7331b-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="7331b-106">Někteří poskytovatelé (zvlášť ty nerelačních) nepodporují migrace.</span><span class="sxs-lookup"><span data-stu-id="7331b-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="7331b-107">U těchto poskytovatelů je EnsureCreated často nejjednodušší způsob, jak inicializovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7331b-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="7331b-108">Migrace a EnsureCreated nefungují dobře spolupracovaly.</span><span class="sxs-lookup"><span data-stu-id="7331b-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="7331b-109">Pokud používáte migraci, nepoužívejte EnsureCreated inicializovat schématu.</span><span class="sxs-lookup"><span data-stu-id="7331b-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="7331b-110">Přechod z EnsureCreated pro migrace není bezproblémové prostředí.</span><span class="sxs-lookup"><span data-stu-id="7331b-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="7331b-111">Nejjednodušší způsob, jak to udělat, je vyřaďte databázi a znovu vytvořit pomocí migrace.</span><span class="sxs-lookup"><span data-stu-id="7331b-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="7331b-112">Pokud očekáváte, že v budoucnu pomocí migrace, je vhodné pouze začít s migrací namísto použití EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="7331b-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="7331b-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="7331b-113">EnsureDeleted</span></span>

<span data-ttu-id="7331b-114">Metoda EnsureDeleted bude vymazání databáze, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="7331b-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="7331b-115">Pokud nemáte příslušná oprávnění, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="7331b-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="7331b-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="7331b-116">EnsureCreated</span></span>

<span data-ttu-id="7331b-117">EnsureCreated vytvoří databázi, pokud ho neexistuje a inicializovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7331b-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="7331b-118">Pokud neexistují žádné tabulky (včetně tabulek pro jiné třídy DbContext), schéma nelze inicializovat.</span><span class="sxs-lookup"><span data-stu-id="7331b-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="7331b-119">Asynchronní verze těchto metod jsou také k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7331b-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="7331b-120">Skript SQL</span><span class="sxs-lookup"><span data-stu-id="7331b-120">SQL Script</span></span>

<span data-ttu-id="7331b-121">SQL používané EnsureCreated získáte můžete použít metodu GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="7331b-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="7331b-122">Vícenásobné třídy DbContext</span><span class="sxs-lookup"><span data-stu-id="7331b-122">Multiple DbContext classes</span></span>

<span data-ttu-id="7331b-123">EnsureCreated funguje pouze v případě v databázi nejsou žádné tabulky.</span><span class="sxs-lookup"><span data-stu-id="7331b-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="7331b-124">V případě potřeby můžete napsat vlastní zkontrolujte, jestli je potřeba inicializovat schématu a použijte základní službě IRelationalDatabaseCreator inicializovat schématu.</span><span class="sxs-lookup"><span data-stu-id="7331b-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
