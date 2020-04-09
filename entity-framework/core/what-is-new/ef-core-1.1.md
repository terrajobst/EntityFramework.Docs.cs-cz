---
title: Co je nového v EF Core 1.1 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417515"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="54774-102">Nové funkce v EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="54774-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="54774-103">Modelování</span><span class="sxs-lookup"><span data-stu-id="54774-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="54774-104">Mapování pole</span><span class="sxs-lookup"><span data-stu-id="54774-104">Field mapping</span></span>

<span data-ttu-id="54774-105">Umožňuje nakonfigurovat záložní pole pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="54774-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="54774-106">To může být užitečné pro vlastnosti jen pro čtení nebo data, která má Get/Set metody spíše než vlastnost.</span><span class="sxs-lookup"><span data-stu-id="54774-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="54774-107">Mapování na tabulky optimalizované pro paměť na serveru SQL Server</span><span class="sxs-lookup"><span data-stu-id="54774-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="54774-108">Můžete určit, že tabulka, na kterou je entita mapována, je optimalizovaná pro paměť.</span><span class="sxs-lookup"><span data-stu-id="54774-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="54774-109">Při použití EF Core vytvořit a udržovat databázi na základě `Database.EnsureCreated()`modelu (buď s migrací nebo ), bude vytvořena tabulka optimalizovaná pro paměť pro tyto entity.</span><span class="sxs-lookup"><span data-stu-id="54774-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="54774-110">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="54774-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="54774-111">Další api pro sledování změn z EF6</span><span class="sxs-lookup"><span data-stu-id="54774-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="54774-112">Jako `Reload`je `GetModifiedProperties` `GetDatabaseValues` , , atd.</span><span class="sxs-lookup"><span data-stu-id="54774-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="54774-113">Dotaz</span><span class="sxs-lookup"><span data-stu-id="54774-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="54774-114">Explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="54774-114">Explicit Loading</span></span>

<span data-ttu-id="54774-115">Umožňuje aktivovat plnění vlastnosti navigace na entitu, která byla dříve načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="54774-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="54774-116">DbSet.Najít</span><span class="sxs-lookup"><span data-stu-id="54774-116">DbSet.Find</span></span>

<span data-ttu-id="54774-117">Poskytuje snadný způsob, jak načíst entitu na základě její hodnoty primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="54774-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="54774-118">Ostatní</span><span class="sxs-lookup"><span data-stu-id="54774-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="54774-119">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="54774-119">Connection resiliency</span></span>

<span data-ttu-id="54774-120">Automaticky opakuje neúspěšné příkazy databáze.</span><span class="sxs-lookup"><span data-stu-id="54774-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="54774-121">To je užitečné zejména při připojení k SQL Azure, kde jsou běžné přechodné chyby.</span><span class="sxs-lookup"><span data-stu-id="54774-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="54774-122">Zjednodušená náhrada služby</span><span class="sxs-lookup"><span data-stu-id="54774-122">Simplified service replacement</span></span>

<span data-ttu-id="54774-123">Usnadňuje nahrazení interních služeb, které používá EF.</span><span class="sxs-lookup"><span data-stu-id="54774-123">Makes it easier to replace internal services that EF uses.</span></span>
