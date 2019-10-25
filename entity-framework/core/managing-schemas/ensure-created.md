---
title: Vytvoření a vyřazení rozhraní API – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811783"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="d0401-102">Vytvoření a přemístění rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d0401-102">Create and Drop APIs</span></span>

<span data-ttu-id="d0401-103">Metody EnsureCreated a EnsureDeleted poskytují nezjednodušenou alternativu pro [migrace](migrations/index.md) pro správu schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="d0401-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="d0401-104">Tyto metody jsou užitečné ve scénářích, kdy jsou data přechodný a můžou být vyhozena při změně schématu.</span><span class="sxs-lookup"><span data-stu-id="d0401-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="d0401-105">Například při vytváření prototypů, v testech nebo místních mezipamětí.</span><span class="sxs-lookup"><span data-stu-id="d0401-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="d0401-106">Někteří poskytovatelé (obzvláště nerelační) nepodporují migrace.</span><span class="sxs-lookup"><span data-stu-id="d0401-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="d0401-107">Pro tyto poskytovatele je EnsureCreated často nejjednodušší způsob, jak inicializovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="d0401-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="d0401-108">EnsureCreated a migrace nefungují dobře společně.</span><span class="sxs-lookup"><span data-stu-id="d0401-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="d0401-109">Pokud používáte migrace, nepoužívejte k inicializaci schématu EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="d0401-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="d0401-110">Přechod z EnsureCreated na migrace není bezproblémové prostředí.</span><span class="sxs-lookup"><span data-stu-id="d0401-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="d0401-111">Nejjednodušší způsob, jak to udělat, je vyřadit databázi a znovu ji vytvořit pomocí migrací.</span><span class="sxs-lookup"><span data-stu-id="d0401-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="d0401-112">Pokud předpokládáte použití migrace v budoucnu, je nejlepší začít s migracemi místo používání EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="d0401-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="d0401-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="d0401-113">EnsureDeleted</span></span>

<span data-ttu-id="d0401-114">Metoda EnsureDeleted vynechá databázi, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="d0401-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="d0401-115">Pokud nemáte příslušná oprávnění, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="d0401-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="d0401-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="d0401-116">EnsureCreated</span></span>

<span data-ttu-id="d0401-117">EnsureCreated vytvoří databázi, pokud neexistuje, a inicializuje schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="d0401-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="d0401-118">Pokud existují nějaké tabulky (včetně tabulek pro jinou třídu DbContext), schéma se neinicializuje.</span><span class="sxs-lookup"><span data-stu-id="d0401-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="d0401-119">K dispozici jsou také asynchronní verze těchto metod.</span><span class="sxs-lookup"><span data-stu-id="d0401-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="d0401-120">Skript SQL</span><span class="sxs-lookup"><span data-stu-id="d0401-120">SQL Script</span></span>

<span data-ttu-id="d0401-121">K získání SQL používaného v EnsureCreated můžete použít metodu GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="d0401-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="d0401-122">Více tříd DbContext</span><span class="sxs-lookup"><span data-stu-id="d0401-122">Multiple DbContext classes</span></span>

<span data-ttu-id="d0401-123">EnsureCreated funguje pouze v případě, že databáze neobsahuje žádné tabulky.</span><span class="sxs-lookup"><span data-stu-id="d0401-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="d0401-124">V případě potřeby můžete napsat vlastní kontrolu, abyste viděli, jestli se schéma musí inicializovat, a k inicializaci schématu použít základní službu IRelationalDatabaseCreator.</span><span class="sxs-lookup"><span data-stu-id="d0401-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
