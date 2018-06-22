---
title: Základní EF a EF6 – což jeden je pro vás nejvhodnější
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002818"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core a EF6: který z nich je pro vás nejvhodnější

Následující informace vám pomohou při výběru mezi Entity Framework Core a Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Doporučení pro nové aplikace

EF Core můžete použít pro vývoj nových aplikací, pokud chcete využít všech možností, které EF Core poskytuje, a vaše aplikace nevyžaduje funkce, které ještě nejsou v EF Core implementované.

Rozhraní EF 6 vyžaduje .NET Framework 4.0 (nebo novější verzi) a je podporované jenom ve Windows (neběží na .NET Core a není podporované v jiných operačních systémech). Stále je však vhodným řešením pro nové aplikace, pokud jsou tato omezení přijatelná a aplikace nevyžaduje nové funkce EF Core, které nejsou v EF6 k dispozici.

Přečtěte si [článek s porovnáním funkcí](features.md), kde zjistíte, jestli je EF Core vhodná volba pro vaši aplikaci.

## <a name="guidance-for-existing-ef6-applications"></a>Doporučení pro existující aplikace s EF6

Z důvodu zásadních změn v EF Core nedoporučujeme migraci z EF6 na EF Core, pokud pro takovou změnu nemáte dobrý důvod. Pokud chcete migrovat stávající řešení na EF Core například kvůli novým funkcím, nejdříve se ujistěte, že jste si vědomi případných omezení EF Core. Přečtěte si [článek s porovnáním funkcí](features.md), kde zjistíte, jestli je EF Core vhodná volba pro vaši aplikaci.

**Přechod z EF6 na EF Core je potřeba chápat spíše jako migraci než jako upgrade.** Další informace najdete v [článku o přechodu z EF6 na EF Core](porting/index.md).
