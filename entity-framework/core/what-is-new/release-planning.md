---
title: Plánování vydání EF Core
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888054"
---
# <a name="release-planning-process"></a>Proces plánování vydaných verzí

Často se dostanete na to, jak si vybíráme konkrétní funkce, které se dají použít k přechodu do konkrétní verze.
Tento dokument popisuje proces, který používáme.
Proces se neustále vyvíjí, protože jsme našli lepší způsob plánování, ale obecné nápady zůstanou stejné.

## <a name="different-kinds-of-releases"></a>Různé druhy verzí

Různé druhy verzí obsahují různé druhy změn.
To znamená, že plánování vydaných verzí se liší od různých druhů vydání.

### <a name="patch-releases"></a>Verze oprav

Vydání oprav mění pouze část verze "patch".
Například EF Core 3,1. **1** je verze, která opraví problémy zjištěné v EF Core 3,1. **0**.

Verze oprav jsou určeny k opravě důležitých chyb zákazníků.
To znamená, že verze opravy nezahrnují nové funkce.
Změny rozhraní API nejsou ve verzích oprav povoleny, s výjimkou zvláštních okolností.

Pruh, který provede změnu ve vydání opravy, je velmi vysoký.
Důvodem je to, že verze opravy nezavádí nové chyby.
Proto proces rozhodování zvýrazní vysoké hodnoty a nízké riziko.

Je pravděpodobnější, že problém vyřešíte, pokud:
  * Má vliv na více zákazníků.
  * Jedná se o regresi z předchozí verze.
  * Selhání způsobuje poškození dat.

Je pravděpodobnější, že problém vyřešíte, pokud:
  * Existují přiměřená řešení
  * Oprava má vysoké riziko poškození nějakého jiného
  * Chyba je v rohovém případě.

Tento pruh se postupně zvyšuje po celou dobu životnosti vydaných verzí [LTS (Long-Term support)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) . Důvodem je to, že LTS releases zdůrazňuje stabilitu.

Konečné rozhodnutí o tom, jestli se má problém opravit, provedla ředitelé .NET v Microsoftu.

### <a name="minor-releases"></a>Dílčí verze

Dílčí verze mění pouze část "podverze".
Například EF Core 3. **1**0 je verze, která vylepšuje EF Core 3. **0**. 0.

Dílčí verze:
* Mají za cíl zlepšit kvalitu a funkce předchozí verze
* Obvykle obsahuje opravy chyb a nové funkce.
* Nezahrnovat úmyslné neúmyslné změny
* Nahrálo se několik verzí Preview předběžných verzí do NuGet.

### <a name="major-releases"></a>Hlavní verze

Hlavní verze mění číslo verze EF "hlavní".
Například EF Core **3**. 0,0 je hlavní verze, která představuje velký krok předávaného přes EF Core 2.2. x.

Hlavní verze:
* Mají za cíl zlepšit kvalitu a funkce předchozí verze
* Obvykle obsahuje opravy chyb a nové funkce.
  * Některé nové funkce můžou být zásadními změnami, jak EF Core fungují.
* Obvykle zahrnuje úmyslné průlomové změny.
  * Zásadní změny jsou nezbytné při vývoji EF Core, jak se naučíme
  * V důsledku potenciálního dopadu na zákazníka ale myslíme velmi pečlivě na provedení jakékoli zásadní změny. Můžeme být příliš agresivní s zásadními změnami v minulosti. Až dál, budeme se snažit minimalizovat změny, které přeruší aplikace, a snížit změny, které přeruší poskytovatele a rozšíření databáze.
* Máte mnoho předběžných verzí Preview, které jste posunuli do NuGet.

## <a name="planning-for-majorminor-releases"></a>Plánování pro hlavní a dílčí verze

### <a name="github-issue-tracking"></a>Sledování problémů GitHubu

GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) je zdrojem pravdy pro všechna EF Core plánování.

Problémy na GitHubu mají:

* Stav
  * [Otevřené](https://github.com/dotnet/efcore/issues) problémy se nevyřešily.
  * Byly vyřešeny [uzavřené](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) problémy.
    * Všechny vyřešené problémy jsou označené jako [uzavřené – pevné](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed). Problém označený pomocí uzavřeno-fixed je opraven a sloučen, ale pravděpodobně nebyl vydán.
    * Jiné popisky `closed-` ukazují další důvody pro uzavření problému. Například duplicity jsou označeny s uzavřenou duplicitou.
* Typ
  * [Chyby](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) reprezentují chyby.
  * [Vylepšení](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) představují nové funkce nebo lepší funkce v existujících funkcích.
* Milník
  * [Problémy bez milníku](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) se považují za tým. Rozhodnutí o tom, co dělat s problémem, se ještě neudělalo nebo se nebere v úvahu změna rozhodnutí.
  * [Problémy v milníku nevyřízených](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) položek jsou položky, na kterých tým EF v budoucí verzi bude uvažovat pracovat.
    * Problémy v nevyřízených položkách mohou být [označeny příznakem pro další verzi](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) , což znamená, že tato pracovní položka je v seznamu pro další vydání vysoká.
  * Otevřené problémy v milníku se správou verzí jsou položky, na kterých tým plánuje pracovat v této verzi. Jedná se například o [problémy, na kterých plánujeme pracovat EF Core 5,0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * Uzavřené problémy v milníku se správou verzí jsou problémy, které jsou pro tuto verzi splněné. Všimněte si, že verze ještě nemusí být vydaná. Jedná se například [o problémy, které jsou dokončené pro EF Core 3,0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* Hlas!
  * Hlasování je nejlepším způsobem, jak indikovat, že je pro vás důležitý problém.
  * Chcete-li hlasovat, stačí přidat k problému 👍 "palec". Jedná se například [o problémy s nejvyšším rizikem](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) .
  * Pokud se domníváte, že tato funkce přidává hodnotu, uveďte také komentář popisující konkrétní důvody, které tuto funkci potřebují. Komentář "+ 1" nebo podobný "nepřidává hodnotu.

### <a name="the-planning-process"></a>Proces plánování

Proces plánování je více zapojen, než jenom přebírání požadovaných funkcí z nevyřízených položek.
Důvodem je, že shromažďujeme zpětnou vazbu od několika zúčastněných stran několika způsoby.
Vydáváme vydanou verzi na základě těchto vlastností:

* Vstup od zákazníků
* Vstup od jiných zúčastněných stran
* Strategický směr
* Dostupné prostředky
* Plán

Některé z otázek, které požádáme:

1. **Kolik vývojářů se domníváme, že tuto funkci budou používat a kolik lepších funkcí bude mít aplikace nebo prostředí?** K zodpovězení této otázky shromažďujeme názory z mnoha zdrojů – komentáře a hlasy na problémech jsou jedním z těchto zdrojů. Konkrétní zapojení s důležitými zákazníky jsou jiné.

2. **Jaká řešení můžou lidé využít, pokud tuto funkci ještě neimplementujete?** Mnoho vývojářů může například namapovat spojovací tabulku tak, aby fungovala s nedostatečným množstvím nativních podpor. Zjevně ne všichni vývojáři to dělají, ale mnoho z nich se může v našem rozhodnutí počítat jako faktor.

3. **Implementuje Tato funkce architekturu EF Core tak, aby se nám přesunula blíž k implementaci dalších funkcí?** Snažíme se upřednostnit funkce, které fungují jako stavební bloky pro další funkce. Například entity kontejneru objektů a dat můžou přispět k přesunu na více než mnoho podpory a konstruktory entit mají povolené naše opožděné načítání.

4. **Je funkce bodem rozšiřitelnosti?** Doporučujeme upřednostnit body rozšiřitelnosti oproti normálním funkcím, protože umožňují vývojářům připojovat vlastní chování a kompenzovat všechny chybějící funkce.

5. **Jaká je součinnost funkce při použití v kombinaci s jinými produkty?** Dáváme jim funkce, které umožňují nebo významně vylepšit možnosti použití EF Core s jinými produkty, jako je například .NET Core, nejnovější verze sady Visual Studio, Microsoft Azure a tak dále.

6. **Jaké jsou dovednosti, které lidé k dispozici pro práci na funkci a jak můžeme tyto prostředky co nejlépe využít?** Každý člen týmu EF a naši Přispěvatelé komunity mají různé úrovně zkušeností v různých oblastech, takže je nutné odpovídajícím způsobem naplánovat. I v případě, že chceme mít "veškerou práci na palubě", která bude fungovat na konkrétní funkci, jako jsou překlady GroupBy, nebo m:n, což by nebylo praktické.
