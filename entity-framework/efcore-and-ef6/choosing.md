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
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>Základní EF a EF6: který z nich je pro vás nejvhodnější

Následující informace vám pomůže vybrat mezi Entity Framework Core a Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Pokyny pro nové aplikace

Zvažte použití EF jádra pro nové aplikace, pokud chcete využít výhod všechny funkce EF jádra a aplikace nevyžaduje žádné funkce, které zatím nejsou implementované v EF jádra.

EF6 vyžaduje rozhraní .NET Framework 4.0 (nebo novější verzi) a je podporován pouze v systému Windows (tj, že nefunguje na .NET Core a není podporováno v jiných operačních systémech), ale je stále vhodným řešením pro nové aplikace jsou přijatelné dlouho těchto omezení a aplikačních nevyžaduje nové funkce v EF jádra, které nejsou k dispozici pro EF6.

Zkontrolujte [porovnání funkcí](features.md) chcete zobrazit, pokud základní EF může být vhodné volbu pro vaši aplikaci.

## <a name="guidance-for-existing-ef6-applications"></a>Pokyny pro existující aplikace EF6

Z důvodu zásadní změny v EF základní nedoporučujeme pokusu o přesunutí EF6 aplikace do základní EF, pokud nemáte přesvědčivý důvod k provedení změny. Pokud chcete přesunout do základní EF nutné používat nové funkce, pak se ujistěte se, že jste si vědomi její omezení před zahájením. Zkontrolujte [porovnání funkcí](features.md) chcete zobrazit, pokud základní EF může být vhodné volbu pro vaši aplikaci.

**Přesunutí ze EF6 na jádro EF jako port místo upgradu by měl zobrazit.** V tématu [Portování ze EF6 na jádro EF](porting/index.md) Další informace.
