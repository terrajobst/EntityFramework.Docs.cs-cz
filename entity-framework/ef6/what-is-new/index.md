---
title: Co je nového – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149115"
---
# <a name="whats-new-in-ef6"></a>Co je nového v EF6

Důrazně doporučujeme, abyste používali nejnovější vydanou verzi Entity Framework, abyste měli jistotu, že získáte nejnovější funkce a nejvyšší stabilitu.
Uvědomujeme si ale, že možná budete muset použít předchozí verzi nebo můžete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.
Chcete-li nainstalovat konkrétní verze EF, přečtěte si téma [Get Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

Modul runtime EF 6.3.0 byl vydán do NuGet v září 2019. Hlavním cílem této verze bylo usnadnit migraci stávajících aplikací, které používají EF 6 až .NET Core 3,0. Komunita také přispěla k několika opravám chyb a vylepšením. Podrobnosti najdete v tématu problémy uzavřené v jednotlivých 6.3.0 [milníku](https://github.com/aspnet/EntityFramework6/milestones?state=closed) . Tady jsou některé z důležitějších verzí:

- Podpora pro .NET Core 3,0
  - Balíček EntityFramework se teď zaměřuje .NET Standard 2,1, kromě .NET Framework 4. x.
  - Příkazy migrace byly přepsány, aby vytvářely mimo proces a pracovaly s projekty ve stylu sady SDK.
- Podpora SQL Server HierarchyId
- Vylepšená kompatibilita s Roslyn a NuGet PackageReference
- Přidání EF6. exe pro povolení, přidání, skriptování a instalaci migrace ze sestavení. Tím se nahradí soubor migrovat. exe.

## <a name="past-releases"></a>Minulé verze

Stránka [starší](past-releases.md) verze obsahuje archiv všech předchozích verzí EF a hlavní funkce, které byly představeny v každé vydané verzi.
