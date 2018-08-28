---
title: EF Core a EF6 – které jedna je pro vás nejvhodnější
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 83ae754f899b624d322c48e8de8432b65519546e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993830"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core a EF6: který z nich je pro vás nejvhodnější

Následující informace vám pomohou při výběru mezi Entity Framework Core a Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Doporučení pro nové aplikace

EF Core můžete použít pro vývoj nových aplikací, pokud chcete využít všech možností, které EF Core poskytuje, a vaše aplikace nevyžaduje funkce, které ještě nejsou v EF Core implementované.

EF6 vyžaduje rozhraní .NET Framework 4.0 (nebo novější verzi) a je podporován pouze na Windows (to znamená, EF6 momentálně neběží v .NET Core a není podporována v jiných operačních systémech), ale stále je přijatelné volbou pro nové aplikace jsou dlouho těchto omezení přijatelné a aplikace nevyžaduje, aby nové funkce v EF Core, které nejsou k dispozici pro EF6.

Přečtěte si [článek s porovnáním funkcí](features.md), kde zjistíte, jestli je EF Core vhodná volba pro vaši aplikaci.

## <a name="guidance-for-existing-ef6-applications"></a>Doporučení pro existující aplikace s EF6

Z důvodu zásadních změn v EF Core nedoporučujeme migraci z EF6 na EF Core, pokud pro takovou změnu nemáte dobrý důvod. Pokud chcete migrovat stávající řešení na EF Core například kvůli novým funkcím, nejdříve se ujistěte, že jste si vědomi případných omezení EF Core. Přečtěte si [článek s porovnáním funkcí](features.md), kde zjistíte, jestli je EF Core vhodná volba pro vaši aplikaci.

**Přechod z EF6 na EF Core je potřeba chápat spíše jako migraci než jako upgrade.** Další informace najdete v [článku o přechodu z EF6 na EF Core](porting/index.md).
