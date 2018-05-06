---
title: "Základní EF a EF6 – což jeden je pro vás nejvhodnější"
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
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core a EF6: který z nich je pro vás nejvhodnější

Následující informace vám pomohou vybrat mezi Entity Framework Core a Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Doporučení pro nové aplikace

EF Core zvažte použít pro vývoj nových aplikací, pokud chcete využít všech výhod EF Core a Vaše aplikace zároveň nevyžaduje funkce, které doposud nebyly implementovány v EF Core.

Entity Framework 6 vyžaduje .NET Framework 4.0 (nebo novější verzi) a je podporován pouze v systému Windows (nefunguje na .NET Core a není podporován v jiných operačních systémech). Stále je však vhodným řešením pro nové aplikace u nichž se předpokládá pouze běh v prostředí Windows.

[Porovnejte funkce](features.md) a ověřte si, zda může být EF Core pro vás vhodnou volbou.

## <a name="guidance-for-existing-ef6-applications"></a>Doporučení pro existující aplikace s EF6

Z důvodu zásadních změn v EF Core nedoporučujeme migraci z EF6 na EF Core, pokud pro takové rozhodnutí nemáte dobrý důvod. Pokud chcete zmigrovat stávající řešení na EF Core například kvůli novým funkcím, ujistěte se, že jste si vědomi případných omezení EF Core. [Porovnejte funkce](features.md) a ověřte si, zda může být EF Core pro vás vhodnou volbou.

**Portování z EF6 na EF Core je více rozepsáno** v tématu [Portování z EF6 na EF Core](porting/index.md).
