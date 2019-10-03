---
title: Vytvoření a vyřazení rozhraní API – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
ms.openlocfilehash: 88c1403d2fae740ad78bb7c41d404b0dd91e86ae
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813446"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="e25e9-102">Vytvoření a přemístění rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e25e9-102">Create and Drop APIs</span></span>

<span data-ttu-id="e25e9-103">Metody EnsureCreated a EnsureDeleted poskytují nezjednodušenou alternativu pro [migrace](migrations/index.md) pro správu schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="e25e9-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="e25e9-104">Tyto metody jsou užitečné ve scénářích, kdy jsou data přechodný a můžou být vyhozena při změně schématu.</span><span class="sxs-lookup"><span data-stu-id="e25e9-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="e25e9-105">Například při vytváření prototypů, v testech nebo místních mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="e25e9-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="e25e9-106">Někteří poskytovatelé (obzvláště nerelační) nepodporují migrace.</span><span class="sxs-lookup"><span data-stu-id="e25e9-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="e25e9-107">Pro tyto poskytovatele je EnsureCreated často nejjednodušší způsob, jak inicializovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="e25e9-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="e25e9-108">EnsureCreated a migrace nefungují dobře společně.</span><span class="sxs-lookup"><span data-stu-id="e25e9-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="e25e9-109">Pokud používáte migrace, nepoužívejte k inicializaci schématu EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="e25e9-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="e25e9-110">Přechod z EnsureCreated na migrace není bezproblémové prostředí.</span><span class="sxs-lookup"><span data-stu-id="e25e9-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="e25e9-111">Nejjednodušší způsob, jak to udělat, je vyřadit databázi a znovu ji vytvořit pomocí migrací.</span><span class="sxs-lookup"><span data-stu-id="e25e9-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="e25e9-112">Pokud předpokládáte použití migrace v budoucnu, je nejlepší začít s migracemi místo používání EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="e25e9-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="e25e9-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="e25e9-113">EnsureDeleted</span></span>

<span data-ttu-id="e25e9-114">Metoda EnsureDeleted vynechá databázi, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="e25e9-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="e25e9-115">Pokud nemáte příslušná oprávnění, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="e25e9-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="e25e9-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="e25e9-116">EnsureCreated</span></span>

<span data-ttu-id="e25e9-117">EnsureCreated vytvoří databázi, pokud neexistuje, a inicializuje schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="e25e9-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="e25e9-118">Pokud existují nějaké tabulky (včetně tabulek pro jinou třídu DbContext), schéma se neinicializuje.</span><span class="sxs-lookup"><span data-stu-id="e25e9-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="e25e9-119">K dispozici jsou také asynchronní verze těchto metod.</span><span class="sxs-lookup"><span data-stu-id="e25e9-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="e25e9-120">Skript SQL</span><span class="sxs-lookup"><span data-stu-id="e25e9-120">SQL Script</span></span>

<span data-ttu-id="e25e9-121">K získání SQL používaného v EnsureCreated můžete použít metodu GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="e25e9-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="e25e9-122">Více tříd DbContext</span><span class="sxs-lookup"><span data-stu-id="e25e9-122">Multiple DbContext classes</span></span>

<span data-ttu-id="e25e9-123">EnsureCreated funguje pouze v případě, že databáze neobsahuje žádné tabulky.</span><span class="sxs-lookup"><span data-stu-id="e25e9-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="e25e9-124">V případě potřeby můžete napsat vlastní kontrolu, abyste viděli, jestli se schéma musí inicializovat, a k inicializaci schématu použít základní službu IRelationalDatabaseCreator.</span><span class="sxs-lookup"><span data-stu-id="e25e9-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
