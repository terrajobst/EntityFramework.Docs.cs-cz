---
title: Co je nového – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: fcd6339f67a1512dd66220c59537d12cf0b22620
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490295"
---
# <a name="whats-new-in-ef6"></a>Co je nového v EF6

Důrazně doporučujeme používat nejnovější vydanou verzi sady Entity Framework k zajištění, že máte k dispozici nejnovější funkce a nejvyšší stabilitu.
Uvědomujeme si však, že budete možná muset použít předchozí verze, nebo můžete experimentovat s nových vylepšeních v nejnovější předběžnou verzi.
Pokud chcete nainstalovat konkrétní verze EF, naleznete v tématu [získat Entity Framework](~/ef6/fundamentals/install.md).

Tato stránka dokumenty funkce, které jsou zahrnuty v každé nové verze.

## <a name="recent-releases"></a>Nedávno vydané aktualizace

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Aktualizace nástroje EF v sadě Visual Studio 2017 15.7

V květnu 2018 jsme vydali aktualizovanou verzi nástroje EF jako součást Visual Studio 2017 15.7.
Obsahuje vylepšení pro některé běžné problémové body:

- Opravy několika chyb usnadnění přístupu uživatelského rozhraní
- Alternativní řešení pro systém SQL Server snížení výkonu při generování modely z existujících databází [#4](https://github.com/aspnet/entityframework6/issues/4)
- Podporu aktualizace modelů pro větší modely na serveru SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Další vylepšení v tomto této nové verzi nástroje EF je, že se nainstaluje modul runtime EF 6.2 při vytváření modelu v novém projektu. Ve starších verzích sady Visual Studio je možné pomocí nainstalovat odpovídající verzi balíčku NuGet EF 6.2 runtime (stejně jako všechny poslední verze EF).

### <a name="ef-62-runtime"></a>EF 6.2 Runtime

NuGet EF 6.2 runtime byla vydána. října 2017.
Děkujeme vám ve velké části úsilí naší komunity open source přispěvatele, EF 6.2 zahrnuje mnoho [opravy chyb](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) a [vylepšení produktu](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Tady je stručný seznam vašich nejdůležitějších změn by to ovlivnilo EF 6.2 runtime:

- Načítají se snižuje doba spuštění dokončení první modely kódu z trvalá mezipaměť [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Rozhraní Fluent API definovat indexy [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions.Like() povolit zápis dotazů LINQ, které přeložit jako v SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migrate.exe teď podporuje – možnost skript [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 teď můžete pracovat s hodnoty klíče, které jsou generovány podle pořadí v systému SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Aktualizace seznamu přechodných chyb strategie provádění SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Chyba: Opakování dotazy nebo příkazy SQL selže s "SqlParameter je již obsažen v jiné SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Chyba: Vyhodnocení DbQuery.ToString() často vyprší časový limit v ladicím programu [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Budoucí verze

Informace v budoucí verzi systému EF6, podívejte se na naše [plán](roadmap.md).

## <a name="past-releases"></a>Minulých verzích

[Minulých vydaných verzích](past-releases.md) stránka obsahuje archiv všechny předchozí verze EF a hlavní funkce, které byly zavedeny v jednotlivých verzích.
