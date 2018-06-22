---
title: Synchronizace replik indexů data - EF jádra
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163197"
---
# <a name="data-seeding"></a>Data synchronizace replik indexů

> [!NOTE]  
> Tato funkce je nového v EF základní 2.1.

Synchronizace replik indexů dat umožňuje poskytovat počáteční data k naplnění databáze. Na rozdíl od v EF6 v EF základní synchronizace replik indexů dat je přidružen typ entity jako součást konfigurace modelu. Potom základní EF [migrace](xref:core/managing-schemas/migrations/index) můžete automaticky vypočítat co vložit, aktualizovat nebo odstranit potřeba operations použijí při upgradu databáze na novou verzi modelu.

Jako příklad, můžete to konfigurovat počáteční data pro `Blog` v `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Chcete-li přidat entit, které mají relaci cizího klíče hodnoty je potřeba zadat. Často jsou vlastnosti cizího klíče v stínové stavu, proto být schopni nastavit hodnoty anonymní třída by měl použít:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Po přidání entity, se doporučuje použít [migrace](xref:core/managing-schemas/migrations/index) chcete změny použít. 

Alternativně můžete použít `context.Database.EnsureCreated()` k vytvoření nové databáze obsahující počáteční hodnoty data, například pro testovací databáze nebo při použití zprostředkovatele v paměti. Všimněte si, že pokud databáze již existuje, `EnsureCreated()` bude ani aktualizovat schéma ani počáteční hodnoty data v databázi.
