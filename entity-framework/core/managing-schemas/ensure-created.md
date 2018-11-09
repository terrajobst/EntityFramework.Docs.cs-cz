---
title: Vytvoření a přemístění rozhraní API – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285636"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="7dfed-102">Vytvoření a přemístění rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7dfed-102">Create and Drop APIs</span></span>

<span data-ttu-id="7dfed-103">Metody EnsureCreated a EnsureDeleted poskytují jednoduchý alternativou k [migrace](migrations/index.md) pro správu schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7dfed-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="7dfed-104">To je užitečné v situacích, když data je přechodná a můžete vyřadit, když se změní schéma.</span><span class="sxs-lookup"><span data-stu-id="7dfed-104">This is useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="7dfed-105">Například při vytváření prototypů, v testech, nebo pro místní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7dfed-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="7dfed-106">Někteří poskytovatelé (zvlášť ty nerelačních) nepodporují migrace.</span><span class="sxs-lookup"><span data-stu-id="7dfed-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="7dfed-107">Pro tyto EnsureCreated je často nejjednodušší způsob, jak inicializovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7dfed-107">For these, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="7dfed-108">Migrace a EnsureCreated nefungují dobře spolupracovaly.</span><span class="sxs-lookup"><span data-stu-id="7dfed-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="7dfed-109">Pokud používáte migraci, nepoužívejte EnsureCreated inicializovat schématu.</span><span class="sxs-lookup"><span data-stu-id="7dfed-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="7dfed-110">Přechod z EnsureCreated pro migrace není bezproblémové prostředí.</span><span class="sxs-lookup"><span data-stu-id="7dfed-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="7dfed-111">Je simpelest způsob, jak toho dosáhnout, je vyřaďte databázi a znovu vytvořit pomocí migrace.</span><span class="sxs-lookup"><span data-stu-id="7dfed-111">The simpelest way to achieve this is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="7dfed-112">Pokud očekáváte, že v budoucnu pomocí migrace, je vhodné pouze začít s migrací namísto použití EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="7dfed-112">If you anticipate using Migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="7dfed-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="7dfed-113">EnsureDeleted</span></span>

<span data-ttu-id="7dfed-114">Metoda EnsureDeleted bude vymazání databáze, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="7dfed-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="7dfed-115">Pokud nemáte odpovídající oprávnění, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="7dfed-115">If you don't have the appropiate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="7dfed-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="7dfed-116">EnsureCreated</span></span>

<span data-ttu-id="7dfed-117">EnsureCreated vytvoří databázi, pokud ho neexistuje a inicializovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7dfed-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="7dfed-118">Pokud neexistují žádné tabulky (včetně tabulek pro jiné třídy DbContext), schéma nelze inicializovat.</span><span class="sxs-lookup"><span data-stu-id="7dfed-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="7dfed-119">Asynchronní verze těchto metod jsou také k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7dfed-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="7dfed-120">Skript SQL</span><span class="sxs-lookup"><span data-stu-id="7dfed-120">SQL Script</span></span>

<span data-ttu-id="7dfed-121">SQL používané EnsureCreated získáte můžete použít metodu GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="7dfed-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="7dfed-122">Vícenásobné třídy DbContext</span><span class="sxs-lookup"><span data-stu-id="7dfed-122">Multiple DbContext classes</span></span>

<span data-ttu-id="7dfed-123">EnsureCreated funguje pouze v případě v databázi nejsou žádné tabulky.</span><span class="sxs-lookup"><span data-stu-id="7dfed-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="7dfed-124">V případě potřeby můžete napsat vlastní zkontrolujte, jestli je potřeba inicializovat schématu a použijte základní službě IRelationalDatabaseCreator inicializovat schématu.</span><span class="sxs-lookup"><span data-stu-id="7dfed-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
