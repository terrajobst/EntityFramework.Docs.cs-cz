---
title: Co je nového v EF Core 1,1-EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417515"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="78f23-102">Nové funkce v EF Core 1,1</span><span class="sxs-lookup"><span data-stu-id="78f23-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="78f23-103">Modelování</span><span class="sxs-lookup"><span data-stu-id="78f23-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="78f23-104">Mapování polí</span><span class="sxs-lookup"><span data-stu-id="78f23-104">Field mapping</span></span>

<span data-ttu-id="78f23-105">Umožňuje konfigurovat pole pro zálohování pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="78f23-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="78f23-106">To může být užitečné pro vlastnosti, které jsou jen pro čtení, nebo data, která mají metody get/set spíše než vlastnost.</span><span class="sxs-lookup"><span data-stu-id="78f23-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="78f23-107">Mapování na paměťově optimalizované tabulky v SQL Server</span><span class="sxs-lookup"><span data-stu-id="78f23-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="78f23-108">Můžete určit, že tabulka, na kterou je entita mapována, je optimalizovaná pro paměť.</span><span class="sxs-lookup"><span data-stu-id="78f23-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="78f23-109">Při použití EF Core k vytvoření a údržbě databáze založené na modelu (buď s migrací nebo `Database.EnsureCreated()`) se pro tyto entity vytvoří paměťově optimalizovaná tabulka.</span><span class="sxs-lookup"><span data-stu-id="78f23-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="78f23-110">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="78f23-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="78f23-111">Další rozhraní API pro sledování změn z EF6</span><span class="sxs-lookup"><span data-stu-id="78f23-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="78f23-112">Například `Reload`, `GetModifiedProperties``GetDatabaseValues` atd.</span><span class="sxs-lookup"><span data-stu-id="78f23-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="78f23-113">Dotaz</span><span class="sxs-lookup"><span data-stu-id="78f23-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="78f23-114">Explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="78f23-114">Explicit Loading</span></span>

<span data-ttu-id="78f23-115">Umožňuje aktivovat naplnění vlastnosti navigace u entity, která byla dříve načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="78f23-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="78f23-116">Negenerickými. Find</span><span class="sxs-lookup"><span data-stu-id="78f23-116">DbSet.Find</span></span>

<span data-ttu-id="78f23-117">Poskytuje snadný způsob, jak načíst entitu na základě její hodnoty primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="78f23-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="78f23-118">Ostatní</span><span class="sxs-lookup"><span data-stu-id="78f23-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="78f23-119">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="78f23-119">Connection resiliency</span></span>

<span data-ttu-id="78f23-120">Automaticky opakuje neúspěšné příkazy databáze.</span><span class="sxs-lookup"><span data-stu-id="78f23-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="78f23-121">To je užitečné hlavně v případě připojení k SQL Azure, kde jsou časté přechodné chyby.</span><span class="sxs-lookup"><span data-stu-id="78f23-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="78f23-122">Zjednodušené nahrazení služby</span><span class="sxs-lookup"><span data-stu-id="78f23-122">Simplified service replacement</span></span>

<span data-ttu-id="78f23-123">Usnadňuje nahrazení interních služeb, které používá EF.</span><span class="sxs-lookup"><span data-stu-id="78f23-123">Makes it easier to replace internal services that EF uses.</span></span>
