---
title: Co je nového – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 01dc618954da5dbd12fbd37c2c47701ce251be92
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271441"
---
# <a name="whats-new-in-ef6"></a>Co je nového v EF6

Důrazně doporučujeme, abyste používali nejnovější vydanou verzi Entity Framework, abyste měli jistotu, že získáte nejnovější funkce a nejvyšší stabilitu.
Uvědomujeme si ale, že možná budete muset použít předchozí verzi nebo můžete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.
Chcete-li nainstalovat konkrétní verze EF, přečtěte si téma [Get Entity Framework](~/ef6/fundamentals/install.md).

Tato stránka obsahuje funkce, které jsou součástí každé nové vydané verze.

## <a name="recent-releases"></a>Poslední verze

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Aktualizace nástrojů EF v aplikaci Visual Studio 2017 15,7

V květnu 2018 jsme vydali aktualizovanou verzi nástrojů EF jako součást sady Visual Studio 2017 15,7.
Obsahuje vylepšení některých běžných častých bodů:

- Opravy pro několik chyb usnadnění uživatelského rozhraní
- Alternativní řešení pro SQL Server regresi výkonu při generování modelů z existujících databází [#4](https://github.com/aspnet/entityframework6/issues/4)
- Podpora pro aktualizace modelů pro větší modely na SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Dalším vylepšením v této nové verzi nástrojů EF je, že při vytváření modelu v novém projektu nainstaluje modul runtime EF 6,2. Se staršími verzemi sady Visual Studio je možné použít modul runtime EF 6,2 (stejně jako všechny dřívější verze EF) instalací odpovídající verze balíčku NuGet.

### <a name="ef-62-runtime"></a>Modul runtime EF 6,2

Modul runtime EF 6,2 byl vydán do NuGet v říjnu 2017.
Děkujeme, že v naší komunitě otevřených přispěvatelů Open sources je naše komunita, EF 6,2 obsahuje řadu [chybových oprav](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) a [vylepšení produktů](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Tady je stručný seznam nejdůležitějších změn, které mají vliv na modul runtime EF 6,2:

- Snižte čas spuštění načtením dokončeného kódu první modely z trvalé mezipaměti [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Rozhraní Fluent API k definování indexů [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions. like () pro povolení psaní dotazů LINQ, které se překládají jako v SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migruje. exe teď podporuje-možnost skriptu [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 může nyní pracovat s hodnotami klíčů generovanými sekvencí v SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Aktualizace seznamu přechodných chyb pro SQL Azure strategii provádění [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Problém Opakované pokusy dotazů nebo příkazů SQL selžou s "rozhraní SqlParameter je již obsaženo v jiném SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Problém Vyhodnocování DbQuery. ToString () je v ladicím programu často vyprší [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Budoucí verze

Informace o budoucích verzích EF6 najdete [v našem plánu](roadmap.md).

## <a name="past-releases"></a>Minulé verze

Stránka [starší](past-releases.md) verze obsahuje archiv všech předchozích verzí EF a hlavní funkce, které byly představeny v každé vydané verzi.
