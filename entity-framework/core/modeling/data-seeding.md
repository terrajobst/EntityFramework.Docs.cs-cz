---
title: "Synchronizace replik indexů data - EF jádra"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a><span data-ttu-id="093d9-102">Data synchronizace replik indexů</span><span class="sxs-lookup"><span data-stu-id="093d9-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="093d9-103">Tato funkce je nového v EF základní 2.1.</span><span class="sxs-lookup"><span data-stu-id="093d9-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="093d9-104">Synchronizace replik indexů dat umožňuje poskytovat počáteční data k naplnění databáze.</span><span class="sxs-lookup"><span data-stu-id="093d9-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="093d9-105">Na rozdíl od v EF6 v EF základní synchronizace replik indexů dat je přidružen typ entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="093d9-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="093d9-106">Potom základní EF migrace můžete vypočítat automaticky, co vložit, aktualizovat nebo odstranit potřeba operations použijí při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="093d9-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="093d9-107">Jako příklad, můžete to konfigurovat počáteční data pro `Blog` v `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="093d9-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="093d9-108">Chcete-li přidat entit, které mají relaci cizího klíče hodnoty je potřeba zadat.</span><span class="sxs-lookup"><span data-stu-id="093d9-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="093d9-109">Často jsou vlastnosti cizího klíče v stínové stavu, proto být schopni nastavit hodnoty anonymní třída by měl použít:</span><span class="sxs-lookup"><span data-stu-id="093d9-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
