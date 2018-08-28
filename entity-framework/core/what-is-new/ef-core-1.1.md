---
title: Novinky v EF Core 1.1 – EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995432"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="622d7-102">Novinky v EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="622d7-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="622d7-103">Modelování</span><span class="sxs-lookup"><span data-stu-id="622d7-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="622d7-104">Mapování polí</span><span class="sxs-lookup"><span data-stu-id="622d7-104">Field mapping</span></span>
<span data-ttu-id="622d7-105">Umožňuje nakonfigurovat pomocné pole pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="622d7-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="622d7-106">To může být užitečné pro vlastnosti jen pro čtení nebo data, která má metody Get/Set, nikoli vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="622d7-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="622d7-107">Mapování na paměťově optimalizovaných tabulek v SQL serveru</span><span class="sxs-lookup"><span data-stu-id="622d7-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="622d7-108">Můžete určit, že je tabulka entita je namapována na paměťově optimalizovaná.</span><span class="sxs-lookup"><span data-stu-id="622d7-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="622d7-109">Když pomocí EF Core pro vytváření a údržbu databáze na základě modelu (pomocí migrace nebo `Database.EnsureCreated()`), paměťově optimalizované tabulky se vytvoří pro tyto entity.</span><span class="sxs-lookup"><span data-stu-id="622d7-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="622d7-110">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="622d7-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="622d7-111">Další řešení change tracking rozhraní API z EF6</span><span class="sxs-lookup"><span data-stu-id="622d7-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="622d7-112">Například `Reload`, `GetModifiedProperties`, `GetDatabaseValues` atd.</span><span class="sxs-lookup"><span data-stu-id="622d7-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="622d7-113">Dotazy</span><span class="sxs-lookup"><span data-stu-id="622d7-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="622d7-114">Explicitní načtení</span><span class="sxs-lookup"><span data-stu-id="622d7-114">Explicit Loading</span></span>
<span data-ttu-id="622d7-115">Umožňuje aktivovat naplnění navigační vlastnost s entitou, která byla dříve načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="622d7-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="622d7-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="622d7-116">DbSet.Find</span></span>
<span data-ttu-id="622d7-117">Poskytuje snadný způsob, jak načíst podle jeho hodnotu primárního klíče entity.</span><span class="sxs-lookup"><span data-stu-id="622d7-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="622d7-118">Ostatní</span><span class="sxs-lookup"><span data-stu-id="622d7-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="622d7-119">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="622d7-119">Connection resiliency</span></span>
<span data-ttu-id="622d7-120">Automatické opakované pokusy byly neúspěšné databázových příkazů.</span><span class="sxs-lookup"><span data-stu-id="622d7-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="622d7-121">To je užitečné hlavně při připojení k SQL Azure, kde jsou běžné přechodná selhání.</span><span class="sxs-lookup"><span data-stu-id="622d7-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="622d7-122">Zjednodušená služba nahrazení</span><span class="sxs-lookup"><span data-stu-id="622d7-122">Simplified service replacement</span></span>
<span data-ttu-id="622d7-123">Usnadňuje nahradit interních služeb, které EF používá.</span><span class="sxs-lookup"><span data-stu-id="622d7-123">Makes it easier to replace internal services that EF uses.</span></span>
