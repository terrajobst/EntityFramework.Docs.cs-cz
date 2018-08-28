---
title: Předvyplnění dat – EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994476"
---
# <a name="data-seeding"></a>Předvyplnění dat

> [!NOTE]  
> Tato funkce je nového v EF Core 2.1.

Předvyplnění dat umožňuje poskytovat počáteční data k naplnění databáze. Na rozdíl od v EF6 do EF Core předvyplnění dat souvisí s typem entity jako součást konfigurace modelu. Potom EF Core [migrace](xref:core/managing-schemas/migrations/index) dokáže automaticky výpočetní co vložit, aktualizovat nebo odstranit operací nutné použít při upgradu databáze na novou verzi modelu.

Jako příklad, můžete nakonfigurovat počáteční data `Blog` v `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Přidání entity, které mají hodnoty cizího klíče relace musí být zadána. Často jsou vlastnosti cizího klíče v stínové stavu, tak abyste mohli nastavit hodnoty anonymní třída by měla sloužit:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Po přidání entity, doporučuje se použít [migrace](xref:core/managing-schemas/migrations/index) změny projeví. 

Alternativně můžete použít `context.Database.EnsureCreated()` k vytvoření nové databáze obsahující počáteční hodnoty dat, jako je například testovací databázi nebo při použití zprostředkovatele v paměti. Všimněte si, že pokud databáze již existuje, `EnsureCreated()` ani aktualizuje schéma ani počáteční hodnoty data v databázi.
