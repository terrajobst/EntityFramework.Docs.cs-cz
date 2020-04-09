---
title: Ef Core release planning EF Core release planning EF Core release planning EF Core
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: 71045b8d49c319a73f74443612bedd84ee33ab8a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417334"
---
# <a name="release-planning-process"></a>Proces plánování vydaných verzí

Často dostáváme otázky o tom, jak si vybíráme konkrétní funkce, které se mají dostat do konkrétní verze.
Tento dokument popisuje proces, který používáme.
Proces se neustále vyvíjí, jak jsme najít lepší způsoby, jak plánovat, ale obecné myšlenky zůstávají stejné.

## <a name="different-kinds-of-releases"></a>Různé druhy verzí

Různé druhy vydání obsahují různé druhy změn.
To zase znamená, že plánování vydání se liší pro různé druhy vydání.

### <a name="patch-releases"></a>Verze oprav

Verze opravy mění pouze "patch" část verze.
Například EF Core 3.1. **1** je verze, která opravuje problémy nalezené v EF Core 3.1. **0**.

Verze oprav jsou určeny k opravě kritických chyb zákazníků.
To znamená, že verze oprav neobsahují nové funkce.
Změny rozhraní API nejsou povoleny ve verzích oprav s výjimkou zvláštních okolností.

Lišta pro změnu v uvolnění náplasti je velmi vysoká.
Důvodem je, že je důležité, aby verze opravy nezaváděly nové chyby.
Proto rozhodovací proces klade důraz na vysokou hodnotu a nízké riziko.

Je pravděpodobnější, že problém opravíme, pokud:
  * Má dopad na více zákazníků
  * Jedná se o regresi z předchozí verze
  * Selhání způsobuje poškození dat

Je méně pravděpodobné, že problém opravíme, pokud:
  * Existují rozumná řešení
  * Oprava má vysoké riziko porušení něco jiného
  * Chyba je v rohovém pouzdře

Tento pruh se postupně zvyšuje po celou dobu životnosti [vydání dlouhodobé podpory (LTS).](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) Důvodem je, že lts verze zdůrazňují stabilitu.

Konečné rozhodnutí o tom, zda opravit problém je provedeno .NET ředitelů společnosti Microsoft.

### <a name="minor-releases"></a>Menší verze

Menší verze změnit pouze "menší" část verze.
Například EF Core 3. **1**.0 je verze, která zlepšuje na EF Core 3. **0**.0.

Menší verze:
* Jsou určeny ke zlepšení kvality a vlastností předchozí verze
* Obvykle obsahují opravy chyb a nové funkce
* Nezahrnovat záměrné změny
* Několik předběžných verzí náhledů bylo posunuto do nugetu

### <a name="major-releases"></a>Hlavní verze

Hlavní verze změnit číslo verze EF "hlavní".
Například EF Core **3**.0.0 je hlavní verze, která dělá velký krok vpřed nad EF Core 2.2.x.

Hlavní verze:
* Jsou určeny ke zlepšení kvality a vlastností předchozí verze
* Obvykle obsahují opravy chyb a nové funkce
  * Některé z nových funkcí mohou být zásadní změny ve způsobu, jakým EF Core funguje
* Obvykle zahrnují záměrné změny
  * Nejnovější změny jsou nezbytnou součástí vyvíjejícího se EF Core, jak se učíme
  * Velmi pečlivě však přemýšlíme o tom, že bychom kvůli možnému dopadu na zákazníka udělali nějakou změnu. Možná jsme byli příliš agresivní na prolomení změn v minulosti. Do budoucna se budeme snažit minimalizovat změny, které přeruší aplikace, a snížit změny, které přeruší poskytovatele databází a rozšíření.
* Máte mnoho předběžných náhledů posunutých do NuGet

## <a name="planning-for-majorminor-releases"></a>Plánování hlavních/dílčích verzí

### <a name="github-issue-tracking"></a>Sledování problémů GitHubu

GitHub[https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)( ) je zdrojem pravdy pro veškeré plánování EF Core.

Problémy na GitHubu mají:

* Stav
  * [Otevřené](https://github.com/dotnet/efcore/issues) problémy nebyly vyřešeny.
  * [Byly](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) vyřešeny uzavřené problémy.
    * Všechny problémy, které byly opraveny, jsou [označeny uzavřenou osvojte](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed). Problém označený uzavřenou opevněnou je opraven a sloučen, ale pravděpodobně nebyl vydán.
    * Jiné `closed-` popisky označují další důvody pro uzavření problému. Například duplikáty jsou označeny uzavřeným duplikátem.
* Typ
  * [Chyby](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) představují chyby.
  * [Vylepšení](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) představují nové funkce nebo lepší funkce ve stávajících funkcích.
* Milník
  * Tým zvažuje [problémy bez milníku.](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) Rozhodnutí o tom, co s touto otázkou učinit, dosud nebylo učiněno nebo se zvažuje změna rozhodnutí.
  * [Problémy v milníku Nevyřízené položky](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) jsou položky, které tým EF zváží práci v budoucí verzi
    * Problémy v nevyřízených položkách mohou být [označeny zvážit pro další vydání](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) označující, že tato pracovní položka je vysoko v seznamu pro další verzi.
  * Otevřené problémy v milníku s verzí jsou položky, na kterých má tým v této verzi v plánu pracovat. Například [se jedná o problémy, na kterých plánujeme pracovat pro EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).
  * Uzavřené problémy v milníku s verzí jsou problémy, které jsou pro tuto verzi dokončeny. Všimněte si, že verze ještě nebyla vydána. Jedná se například [o problémy dokončené pro EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).
* Hlasů!
  * Hlasování je nejlepší způsob, jak naznačit, že problém je pro vás důležitý.
  * Chcete-li hlasovat, stačí přidat 👍 "palec nahoru" k problému. Jedná se například [o nejlépe hlasované otázky](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)
  * Prosím také komentář popisující konkrétní důvody, které potřebujete funkci, pokud máte pocit, že to přináší přidanou hodnotu. Komentování "+1" nebo podobné nepřidává hodnotu.

### <a name="the-planning-process"></a>Proces plánování

Proces plánování je více než jen s top-nejžádanější funkce z nevyřízených položek.
Je to proto, že shromažďujeme zpětnou vazbu od více zúčastněných stran několika způsoby.
Poté utváříme vydání na základě:

* Vstup od zákazníků
* Podněty od ostatních zúčastněných stran
* Strategický směr
* Dostupné zdroje
* Plán

Některé z otázek, které klademe, jsou:

1. **Kolik vývojářů si myslíme, že bude používat funkci a o kolik lepší to bude dělat jejich aplikace nebo zkušenosti?** Chcete-li odpovědět na tuto otázku, shromažďujeme zpětnou vazbu z mnoha zdrojů - Komentáře a hlasování o otázkách je jedním z těchto zdrojů. Další jsou specifické vztahy s důležitými zákazníky.

2. **Jaká řešení mohou uživatelé použít, pokud tuto funkci ještě neimplementujeme?** Mnoho vývojářů může například mapovat tabulku spojení, aby se vyřešilnedostatek nativní podpory N:N. Je zřejmé, že ne všichni vývojáři chtějí udělat, ale mnozí mohou, a to se počítá jako faktor v našem rozhodnutí.

3. **Vyvíjí implementace této funkce architekturu EF Core tak, že nás posune blíže k implementaci dalších funkcí?** Máme tendenci upřednostňovat funkce, které fungují jako stavební kameny pro další funkce. Například entity vak vlastností nám mohou pomoci přejít na podporu n:n a konstruktory entit povolily naši podporu opožděného načítání.

4. **Je funkce bodem rozšiřitelnosti?** Máme tendenci upřednostňovat body rozšiřitelnosti před normálními funkcemi, protože umožňují vývojářům připojit své vlastní chování a kompenzovat chybějící funkce.

5. **Jaká je synergie funkce při použití v kombinaci s jinými produkty?** Upřednostňujeme funkce, které umožňují nebo výrazně zlepšují možnosti používání EF Core s jinými produkty, jako je například .NET Core, nejnovější verze Sady Visual Studio, Microsoft Azure a tak dále.

6. **Jaké jsou dovednosti lidí, kteří jsou k dispozici pro práci na funkci, a jak můžeme nejlépe využít tyto zdroje?** Každý člen ef týmu a naši komunitní přispěvatelé mají různé úrovně zkušeností v různých oblastech, takže musíme podle toho plánovat. I kdybychom chtěli mít "všechny ruce na palubě", abychom pracovali na konkrétní funkci, jako jsou překlady GroupBy nebo mnoho-k-mnoho, nebylo by to praktické.
