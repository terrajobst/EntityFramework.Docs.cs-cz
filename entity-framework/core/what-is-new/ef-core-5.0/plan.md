---
title: Plán pro Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125380"
---
# <a name="plan-for-entity-framework-core-50"></a>Plán pro Entity Framework Core 5,0

Jak je popsáno v [procesu plánování](../release-planning.md), shromáždili jsme vstup od zúčastněných stran do předběžného plánu pro vydání verze EF Core 5,0.

> [!IMPORTANT] 
> Tento plán je stále v průběhu práce. Tady není žádný závazek. Tento plán je výchozím bodem, který se bude vyvíjet, jakmile se dozvíte víc. Některé věci, které nejsou aktuálně plánovány pro 5,0, mohou být získány v. Některé věci, které jsou aktuálně plánovány pro 5,0, mohou punted.

### <a name="version-number-and-release-date"></a>Číslo verze a datum vydání

Verze EF Core 5,0 je aktuálně naplánována na vydání ve [stejnou dobu jako rozhraní .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Verze "5,0" byla zvolena pro zarovnání s .NET 5,0.

### <a name="supported-platforms"></a>Podporované platformy

EF Core 5,0 se plánuje běžet na jakékoli platformě .NET 5,0 na základě [sblížení těchto platforem až po .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/). To znamená, že v souvislosti s .NET Standard se stále definuje vlastní použité TFM.

EF Core 5,0 se na .NET Framework nespustí.

### <a name="breaking-changes"></a>Změny způsobující chyby

EF Core 5,0 bude obsahovat některé zásadní změny, ale bude to mnohem méně závažné, než bylo v případě EF Core 3,0. Naším cílem je dovolit, aby se velká většina aplikací aktualizovala bez přerušení.

Očekává se, že pro poskytovatele databáze budou zásadní změny, zejména pokud jde o podporu TPT. Očekáváme ale, že práce, která aktualizuje zprostředkovatele pro 5,0, bude menší, než se vyžaduje aktualizace pro 3,0.

## <a name="themes"></a>Motivy

Extrahovali jsme několik hlavních oblastí nebo motivů, které budou tvořit základ pro velké investice do EF Core 5,0.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Navigační vlastnosti m:n (a. k) pro přeskočení navigace (a. k)

Vývojářé olova: @smitpatel a @AndriySvyryd

Sledováno [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

Velikost trička pro T: L

Stav: probíhá

Na nevyřízených položkách GitHubu je nejužitečnější funkce m:n (přibližně 407 hlasy). Podpora vztahů m:n je možné rozdělit na tři hlavní oblasti:

* Přeskočit navigační vlastnosti To umožňuje, aby byl model použit pro dotazy atd. bez odkazů na podkladovou entitu JOIN Table.
* Typy entit kontejneru vlastností. Ty umožňují použití standardního typu CLR (např. `Dictionary`) pro instance entit tak, že pro každý typ entity není vyžadován explicitní typ CLR.
* Cukr pro jednoduchou konfiguraci relací m:n.

Věříme, že nejvýznamnější blokování pro ty, kteří vyžadují podporu m:n, nedokáže používat "přirozené" vztahy, a to bez odkazování na tabulku spojení v obchodní logice, jako jsou dotazy. Typ entity tabulky join může stále existovat, ale neměl by se zobrazovat v cestě k obchodní logice. To je důvod, proč jsme se rozhodli Přeskočit navigační vlastnosti pro 5,0.

V tuto chvíli se pro EF Core 5,0 sledují i další části m:n. To znamená, že nejsou v současnosti v plánu pro 5,0, ale pokud se o to dobře myslí, doufáme, že si je vyžádáme.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Mapování dědičnosti typu tabulka na typ (TPT)

Vedoucí vývojář: @AndriySvyryd

Sledováno [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

Velikost T-tričko: XL

Stav: Nezahájeno

Provádíme TPT, protože se jedná o vysoce požadovanou funkci (~ 254 hlasy, celkově třetí) a protože vyžaduje některé změny nízké úrovně, které jsou vhodné pro základní povahu celkového plánu .NET 5. Očekáváme, že dojde k zásadním změnám u zprostředkovatelů databází, i když by měly být mnohem méně závažné, než změny požadované pro 3,0.

## <a name="filtered-include"></a>Filtrované zahrnutí

Vedoucí vývojář: @maumar

Sledováno [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

Velikost T-tričko: M

Stav: Nezahájeno

Filtrované zahrnutí je vysoce vyžádaná funkce (~ 317 hlasy, druhá celková), která není obrovským objemem práce a že se domníváte, že bude odblokovat nebo dělat jednodušší mnoho scénářů, které aktuálně vyžadují filtry na úrovni modelu nebo složitější dotazy.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Racionalizovat ToTable, ToQuery, ToView, Z tabulek atd.

Vývojářé olova: @maumar a @smitpatel

Sledováno [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

Velikost trička pro T: L

Stav: Nezahájeno

V předchozích verzích jsme udělali postup k podpoře nezpracovaných SQL, bez klíčůch typů a souvisejících oblastí. Existují však mezery i nekonzistence, jak vše funguje dohromady jako celek. Cílem 5,0 je opravit tyto možnosti a vytvořit dobré prostředí pro definování, migraci a používání různých typů entit a jejich přidružených dotazů a artefaktů databáze. To může také zahrnovat aktualizace zkompilovaného rozhraní API pro dotazy.

Všimněte si, že tato položka může vést k nějakým změnám na úrovni aplikace, protože některé z funkcí, které aktuálně máme, jsou moc opravňující, takže můžou rychle vést k pitsi selhání. Budeme nejspíš zablokovat některé z těchto funkcí společně s pokyny k tomu, co dělat.

## <a name="general-query-enhancements"></a>Obecná vylepšení dotazů

Vývojářé olova: @smitpatel a @maumar

Sledováno [problémy s označením `area-query` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

Velikost T-tričko: XL

Stav: probíhá

Kód pro překlad dotazů byl rozsáhle přepsaný pro EF Core 3,0. Z tohoto důvodu je kód dotazu obecně v mnohem robustnějším stavu. Pro 5,0 neplánujeme provádět velké změny dotazů, mimo ty, které jsou potřeba k podpoře TPT a přeskočení navigačních vlastností. Nicméně stále významnou práci potřebují opravit určitou technickou pohledávku, která byla ponechána v 3,0 renovaci. Také plánujeme opravit spoustu chyb a implementovat malá vylepšení, aby bylo možné ještě víc zlepšit celkové prostředí dotazů.

## <a name="migrations-and-deployment-experience"></a>Migrace a prostředí nasazení

Vývojářé olova: @bricelam

Sledováno [#19587](https://github.com/dotnet/efcore/issues/19587)

Velikost trička pro T: L

Stav: probíhá

V současné době mnoho vývojářů migruje své databáze při spuštění aplikace. To je jednoduché, ale nedoporučuje se to z těchto důvodů:

* Více vláken/procesů/serverů se může pokusit o souběžnou migraci databáze
* Aplikace se můžou pokusit o přístup k nekonzistentnímu stavu, když se děje.
* Pro provádění aplikace by se obvykle neměla udělit oprávnění k databázi pro změnu schématu.
* Vrátit zpět do čistého stavu, pokud se něco pokazilo

Chceme doručovat toto lepší prostředí, které umožňuje snadný způsob migrace databáze v době nasazení. To by mělo:

* Práce v systémech Linux, Mac a Windows
* Dobré zkušenosti s příkazovým řádkem
* Scénáře podpory s kontejnery
* Práce s běžně používanými nástroji/toky pro reálné nasazení
* Integrace do alespoň sady Visual Studio

Výsledkem je pravděpodobně mnoho malých vylepšení EF Core (například lepší migrace na SQLite), spolu s pokyny a dlouhodobou spolupráci s ostatními týmy za účelem zlepšení kompletních prostředí, která překračují jenom EF.

## <a name="ef-core-platforms-experience"></a>Prostředí pro EF Core platformy 

Vývojářé olova: @roji a @bricelam

Sledováno [#19588](https://github.com/dotnet/efcore/issues/19588)

Velikost trička pro T: L

Stav: Nezahájeno

Máme dobré doprovodné materiály k používání EF Core v tradičních webových aplikacích podobných MVC. Pokyny pro jiné platformy a modely aplikací buď chybí, nebo jsou zastaralé. Pro EF Core 5,0 plánujeme prozkoumat, vylepšit a zdokumentovat prostředí používání EF Core pomocí:

* Blazor
* Xamarin, včetně scénáře použití AOT/linkeru
* WinForms/WPF/WinUI a případně jiné U.I. rozhraní

To je pravděpodobně mnoho malých vylepšení EF Core společně s pokyny a dlouhodobou spolupráci s ostatními týmy, aby bylo možné lépe využít ucelená prostředí, která překračují jenom EF.

Konkrétní oblasti, na které se plánujeme podívat:

* Nasazení, včetně prostředí pro použití nástrojů EF, například pro migrace
* Modely aplikací, včetně Xamarin a Blazor, a pravděpodobně dalších
* Prostředí SQLite, včetně prostorového prostředí a opětovného sestavování tabulek
* AOT a propojení prostředí
* Integrace diagnostiky, včetně čítačů výkonu

## <a name="performance"></a>Výkon

Vedoucí vývojář: @roji

Sledováno [problémy s označením `area-perf` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

Velikost trička pro T: L

Stav: probíhá

Pro EF Core plánujeme vylepšit naši sadu srovnávacích testů výkonu a zvýšit výkon na modul runtime. Kromě toho plánujeme dokončit nové rozhraní API pro dávkování ADO.NET, které bylo prototypem v průběhu cyklu vydávání 3,0. Ve vrstvě ADO.NET máme také k dispozici další vylepšení výkonu pro poskytovatele Npgsql.

V rámci této práce také plánujeme podle potřeby přidat čítače výkonu ADO.NET/EF Core a další diagnostiku.

## <a name="architecturalcontributor-documentation"></a>Dokumentace k architektuře/přispěvatelům

Vedoucí dokumentace: @ajcvickers

Sledováno [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)

Velikost trička pro T: L

Stav: Nezahájeno

Tady je postup, který usnadňuje pochopení toho, co se v vnitřních EF Corech prochází. To může být užitečné pro kohokoli, kdo používá EF Core, ale primární motivace usnadňuje externím lidem:

* Přispívání do kódu EF Core
* Vytvořit poskytovatele databáze
* Sestavení dalších rozšíření

## <a name="microsoftdatasqlite-documentation"></a>Dokumentace k Microsoft. data. sqlite

Vedoucí dokumentace: @bricelam

Sledováno [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)

Velikost T-tričko: M

Stav: dokončeno. Nová dokumentace je [živá na webu Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).

Tým EF také vlastní poskytovatele Microsoft. data. sqlite ADO.NET. V rámci vydání 5,0 plánujeme plně zdokumentovat tohoto poskytovatele.

## <a name="general-documentation"></a>Obecná dokumentace

Vedoucí dokumentace: @ajcvickers

Sledováno [problémy v úložišti dokumentů v milníku 5,0](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

Velikost trička pro T: L

Stav: probíhá

V průběhu aktualizace dokumentace pro vydání 3,0 a 3,1 už v dokumentaci. Pracujeme také na:
  * Změna dokumentů Začínáme, aby byly lépe přístupné/snazší
  * Reorganizace dokumentů, aby bylo snazší najít a přidat křížové odkazy
  * Přidání dalších podrobností a objasnění stávajících dokumentů
  * Aktualizace ukázek a přidání dalších příkladů

## <a name="fixing-bugs"></a>Oprava chyb

Sledováno [problémy s označením `type-bug` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

Vývojáři: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

Velikost trička pro T: L

Stav: probíhá

V době psaní máme 135 chyb, které jsou posouzené jako opravené ve verzi 5,0 (s již opravenou 62), ale výrazně se překrývají s výše uvedeným _obecným vylepšením dotazů_ .

Příchozí rychlost (problémy, které končí jako práce v milníku), probíhala přibližně 23 problémů za měsíc v průběhu vydání 3,0. Ne všechny tyto akce budou muset být opraveny v 5,0. V rámci hrubého odhadu plánujeme vyřešit další 150 problémů v časovém intervalu 5,0.

## <a name="small-enhancements"></a>Malá vylepšení

Sledováno [problémy s označením `type-enhancement` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

Vývojáři: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

Velikost trička pro T: L

Stav: probíhá

Kromě větších funkcí uvedených výše je také k dispozici mnoho menších vylepšení naplánovaných na 5,0, aby bylo možné opravit "papírové řezy". Všimněte si, že mnohé z těchto vylepšení se vztahují i na obecnější motivy popsané výše.

## <a name="below-the-line"></a>Pod řádkem

Sledováno [problémy s označením `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

Jedná se o opravy chyb a vylepšení, která **nejsou aktuálně** naplánovaná pro vydání 5,0, ale budeme se považovat za ambiciózní cíle v závislosti na pokroku v práci.

Kromě toho vždy vybereme [nejbezpečnější problémy](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) při plánování. Vyjmutí všech těchto problémů z verze je vždycky bolestivý, ale pro tyto prostředky potřebujeme realističtější plán.

## <a name="feedback"></a>Názor

Váš názor na plánování je důležitý. Nejlepším způsobem, jak určit důležitost problému, je hlasovat (palec) pro daný problém na GitHubu. Tato data se pak budou předávat do [procesu plánování](../release-planning.md) pro další vydání.
