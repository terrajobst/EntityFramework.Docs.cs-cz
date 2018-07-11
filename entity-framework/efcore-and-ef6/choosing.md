---
title: EF Core a EF6 – které jedna je pro vás nejvhodnější
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949137"
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
