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
# <a name="data-seeding"></a>Data synchronizace replik indexů

> [!NOTE]  
> Tato funkce je nového v EF základní 2.1.

Synchronizace replik indexů dat umožňuje poskytovat počáteční data k naplnění databáze. Na rozdíl od v EF6 v EF základní synchronizace replik indexů dat je přidružen typ entity jako součást konfigurace modelu. Potom základní EF migrace můžete vypočítat automaticky, co vložit, aktualizovat nebo odstranit potřeba operations použijí při upgradu databáze na novou verzi modelu.

Jako příklad, můžete to konfigurovat počáteční data pro `Blog` v `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Chcete-li přidat entit, které mají relaci cizího klíče hodnoty je potřeba zadat. Často jsou vlastnosti cizího klíče v stínové stavu, proto být schopni nastavit hodnoty anonymní třída by měl použít:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
