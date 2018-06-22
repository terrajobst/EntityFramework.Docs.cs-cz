---
title: Co je nového v EF základní 1.1 - EF jádra
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 74c1033cab2704bdbb9fa4d3ce111df1f1c29418
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054286"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="252ae-102">Nové funkce v EF základní 1.1</span><span class="sxs-lookup"><span data-stu-id="252ae-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="252ae-103">Modelování</span><span class="sxs-lookup"><span data-stu-id="252ae-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="252ae-104">Mapování polí</span><span class="sxs-lookup"><span data-stu-id="252ae-104">Field mapping</span></span>
<span data-ttu-id="252ae-105">Umožňuje nakonfigurovat základní pole pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="252ae-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="252ae-106">To může být užitečné pro vlastnosti jen pro čtení nebo data, která má metody Get/Set než vlastnost.</span><span class="sxs-lookup"><span data-stu-id="252ae-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="252ae-107">Mapování pro paměťově optimalizované tabulky v systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="252ae-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="252ae-108">Můžete určit, zda je v tabulce, entita je namapovaná na paměťově optimalizované.</span><span class="sxs-lookup"><span data-stu-id="252ae-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="252ae-109">Při použití EF jádra pro vytváření a údržbu databáze založené na modelu (buď pomocí migrace nebo `Database.EnsureCreated()`), vytvoří se paměťově optimalizované tabulky pro tyto entity.</span><span class="sxs-lookup"><span data-stu-id="252ae-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="252ae-110">sledování změn</span><span class="sxs-lookup"><span data-stu-id="252ae-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="252ae-111">Sledování rozhraní API z EF6 dalších změn.</span><span class="sxs-lookup"><span data-stu-id="252ae-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="252ae-112">Například `Reload`, `GetModifiedProperties`, `GetDatabaseValues` atd.</span><span class="sxs-lookup"><span data-stu-id="252ae-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="252ae-113">Dotazy</span><span class="sxs-lookup"><span data-stu-id="252ae-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="252ae-114">Explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="252ae-114">Explicit Loading</span></span>
<span data-ttu-id="252ae-115">Umožňuje aktivovat naplnění navigační vlastnost u entity, která byla dříve načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="252ae-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="252ae-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="252ae-116">DbSet.Find</span></span>
<span data-ttu-id="252ae-117">Poskytuje snadný způsob, jak načíst podle jeho hodnotu primárního klíče entity.</span><span class="sxs-lookup"><span data-stu-id="252ae-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="252ae-118">Ostatní</span><span class="sxs-lookup"><span data-stu-id="252ae-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="252ae-119">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="252ae-119">Connection resiliency</span></span>
<span data-ttu-id="252ae-120">Opakované pokusy byly neúspěšné automaticky příkazy databáze.</span><span class="sxs-lookup"><span data-stu-id="252ae-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="252ae-121">To je obzvláště užitečná při připojení k Azure SQL, které jsou společné přechodných chyb.</span><span class="sxs-lookup"><span data-stu-id="252ae-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="252ae-122">Zjednodušená služba nahrazení</span><span class="sxs-lookup"><span data-stu-id="252ae-122">Simplified service replacement</span></span>
<span data-ttu-id="252ae-123">Usnadňuje nahradit interních služeb, které používá EF.</span><span class="sxs-lookup"><span data-stu-id="252ae-123">Makes it easier to replace internal services that EF uses.</span></span>
