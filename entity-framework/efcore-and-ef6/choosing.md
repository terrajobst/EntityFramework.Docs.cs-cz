---
title: "Základní EF a EF6 – což jeden je pro vás nejvhodnější"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>Základní EF a EF6: který z nich je pro vás nejvhodnější

Následující informace vám pomůže vybrat mezi Entity Framework Core a Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Pokyny pro nové aplikace

Protože základní EF je novém produktu a stále chybí některé důležité funkce O/RM, zůstanou EF6 nejvhodnější volbou pro mnoho aplikací.

**Toto jsou typy aplikací, které by doporučujeme používat jádro EF pro:**

* Nové aplikace, které nevyžadují funkce, které zatím nejsou implementované v EF jádra. Zkontrolujte [porovnání funkcí](features.md) chcete zobrazit, pokud základní EF může být vhodné volbu pro vaši aplikaci.

* Aplikace, které cílí na .NET Core, jako jsou například aplikace pro univerzální platformu Windows (UWP) a ASP.NET Core. Tyto aplikace nemůžou používat EF6, jak to vyžaduje rozhraní .NET Framework (tj. rozhraní .NET Framework 4.5).

## <a name="guidance-for-existing-ef6-applications"></a>Pokyny pro existující aplikace EF6

Z důvodu zásadní změny v EF základní nedoporučujeme pokusu o přesunutí EF6 aplikace do základní EF, pokud nemáte přesvědčivý důvod k provedení změny. Pokud chcete přesunout do základní EF nutné používat nové funkce, pak se ujistěte se, že jste si vědomi její omezení před zahájením. Zkontrolujte [porovnání funkcí](features.md) chcete zobrazit, pokud základní EF může být vhodné volbu pro vaši aplikaci.

**Přesunutí ze EF6 na jádro EF jako port místo upgradu by měl zobrazit.** V tématu [Portování ze EF6 na jádro EF](porting/index.md) Další informace.
