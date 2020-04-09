---
title: Plán pro jádro rámce entity 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136221"
---
# <a name="plan-for-entity-framework-core-50"></a>Plán pro jádro rámce entity 5.0

Jak je popsáno v [procesu plánování](../release-planning.md), shromáždili jsme podněty od zúčastněných stran do předběžného plánu pro vydání EF Core 5.0.

> [!IMPORTANT] 
> Tento plán je stále nedokončená. Nic tady není závazek. Tento plán je výchozím bodem, který se bude vyvíjet, jak se dozvíme více. Některé věci, které nejsou v současné době plánovány na 5,0 může dostat vytáhl palců Některé věci v současné době plánováno na 5,0 může dostat punted ven.

### <a name="version-number-and-release-date"></a>Číslo verze a datum vydání.

Ef Core 5.0 je aktuálně naplánováno na vydání [ve stejnou dobu jako .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Verze "5.0" byla zvolena tak, aby byla zarovnána s rozhraním .NET 5.0.

### <a name="supported-platforms"></a>Podporované platformy

EF Core 5.0 je plánováno spuštění na libovolné platformě .NET 5.0 založené na [konvergenci těchto platforem na .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Co to znamená z hlediska .NET Standard a skutečné TFM použité je stále TBD.

EF Core 5.0 nebude spuštěna v rozhraní .NET Framework.

### <a name="breaking-changes"></a>Změny způsobující chyby

EF Core 5.0 bude obsahovat některé zásadní změny, ale ty budou mnohem méně závažné, než tomu bylo v případě EF Core 3.0. Naším cílem je umožnit, aby se drtivá většina aplikací aktualizovala bez porušení.

Očekává se, že dojde k určitým zásadním změnám pro poskytovatele databází, zejména pokud jde o podporu TPT. Očekáváme však, že práce na aktualizaci zprostředkovatele pro 5.0 bude menší, než bylo požadováno k aktualizaci pro 3.0.

## <a name="themes"></a>Motivy

Získali jsme několik hlavních oblastí nebo témat, které budou základem pro velké investice do EF Core 5.0.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Vlastnosti navigace N:N (také mj. "přeskočit navigace")

Vedoucí vývojáři: @smitpatel a@AndriySvyryd

Sledováno [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

Velikost trička: L

Stav: Probíhá

N-to-n je [nejžádanější funkce](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~ 407 hlasů) na nevyřízených položkách GitHub.

Podpora vztahů n:N v plném rozsahu je sledována jako [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508). To lze rozdělit do tří hlavních oblastí:

* Přeskočit navigační vlastnosti. Ty umožňují model, který má být použit pro dotazy atd. ([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))
* Typy entit vak vlastností. Ty umožňují standardní typ CLR (např. `Dictionary`) použít pro instance entity tak, že explicitní typ CLR není potřeba pro každý typ entity. (Roztáhnout pro 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)
* Cukr pro snadnou konfiguraci vztahů n:n. (Roztáhnout pro 5.0.)

Věříme, že nejvýznamnější blokátor pro ty, kteří chtějí podporu N:N, není schopen používat "přirozené" vztahy, aniž by odkazoval na tabulku join, v obchodní logice, jako jsou dotazy. Typ entity tabulky spojení může stále existovat, ale neměl by se dostat do cesty obchodní logiky. Proto jsme se rozhodli řešit vlastnosti přeskočit navigaci pro 5.0.

V současné době jsou ostatní části n-to-many sledovány jako cíl pro EF Core 5.0. To znamená, že nejsou v současné době v plánu na 5,0, ale pokud to půjde dobře doufáme, že vytáhnout je palců

## <a name="table-per-type-tpt-inheritance-mapping"></a>Mapování dědičnosti typu tpt (Table-per-type)

Vedoucí vývojář:@AndriySvyryd

Sledováno [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

Velikost trička: XL

Stav: Probíhá

Děláme TPT, protože je to jak vysoce žádaná funkce (~ 254 hlasů, 3. celkově), tak proto, že vyžaduje některé změny na nízké úrovni, které považujeme za vhodné pro základní povahu celkového plánu .NET 5. Očekáváme, že to povede k přerušení změny pro zprostředkovatele databáze, i když by měly být mnohem méně závažné než změny požadované pro 3.0.

## <a name="filtered-include"></a>Filtrované zahrnutí

Vedoucí vývojář:@maumar

Sledováno [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

Velikost trička: M

Stav: Probíhá

Filtrované zahrnout je vysoce žádaná funkce (~ 317 hlasů, 2nd celkově), která není obrovské množství práce, a že věříme, že odblokuje nebo usnadnit mnoho scénářů, které v současné době vyžadují filtry na úrovni modelu nebo složitější dotazy.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Racionalizovat ToTable, ToQuery, ToView, FromSql, atd.

Vedoucí vývojáři: @maumar a@smitpatel

Sledováno [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

Velikost trička: L

Stav: Probíhá

V předchozích verzích jsme dosáhli pokroku směrem k podpoře nezpracovaných typů SQL, bezklíčových typů a souvisejících oblastí. Existují však mezery i nesrovnalosti ve způsobu, jakým vše funguje společně jako celek. Cílem pro 5.0 je opravit tyto a vytvořit dobré prostředí pro definování, migraci a používání různých typů entit a jejich přidružené dotazy a databázové artefakty. To může také zahrnovat aktualizace zkompilovaného rozhraní API dotazu.

Všimněte si, že tato položka může mít za následek některé změny na úrovni aplikace, protože některé funkce, které máme aktuálně, jsou příliš tolerantní, takže může rychle vést lidi do jam selhání. Pravděpodobně nakonec blokujeme některé z těchto funkcí spolu s pokyny, co dělat místo toho.

## <a name="general-query-enhancements"></a>Obecná vylepšení dotazů

Vedoucí vývojáři: @smitpatel a@maumar

Sledováno [problémy označenými `area-query` v milníku 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

Velikost trička: XL

Stav: Probíhá

Kód překladu dotazu byl rozsáhle přepsán pro EF Core 3.0. Kód dotazu je obecně v mnohem robustnějším stavu z důvodu tohoto. Pro 5.0 neplánujeme provádět hlavní změny dotazů mimo ty, které jsou potřebné pro podporu TPT a přeskočit navigační vlastnosti. Nicméně, tam je ještě značné práce potřebné k opravě některých technických dluhů zbylo z 3,0 opravy. Plánujeme také opravit mnoho chyb a implementovat malá vylepšení pro další zlepšení celkového prostředí dotazu.

## <a name="migrations-and-deployment-experience"></a>Migrace a možnosti nasazení

Vedoucí vývojáři:@bricelam

Sledováno [#19587](https://github.com/dotnet/efcore/issues/19587)

Velikost trička: L

Stav: Probíhá

V současné době mnoho vývojářů migruje své databáze v době spuštění aplikace. To je snadné, ale nedoporučuje se, protože:

* Souběžně se může pokoušet o migraci databáze více vláken/procesů/serverů.
* Aplikace se mohou pokusit o přístup k nekonzistentnímu stavu, zatímco se to děje
* Obvykle by neměla být udělena oprávnění databáze k úpravě schématu pro spuštění aplikace
* Je těžké vrátit se zpět do čistého stavu, pokud se něco pokazí

Chceme zde poskytovat lepší prostředí, které umožňuje snadný způsob migrace databáze v době nasazení. To by mělo:

* Práce na Linuxu, Macu a Windows
* Buďte dobrou zkušeností na příkazovém řádku
* Scénáře podpory s kontejnery
* Práce s běžně používanými nástroji/toky pro nasazení v reálném světě
* Integrace alespoň do sady Visual Studio

Výsledkem bude pravděpodobně mnoho malých vylepšení ef core (například lepší migrace na SQLite), spolu s pokyny a dlouhodobější spolupráce s jinými týmy s cílem zlepšit komplexní prostředí, které přesahuje pouze EF.

## <a name="ef-core-platforms-experience"></a>Zkušenosti s platformami EF Core 

Vedoucí vývojáři: @roji a@bricelam

Sledováno [#19588](https://github.com/dotnet/efcore/issues/19588)

Velikost trička: L

Stav: Nespuštěno

Máme dobré pokyny pro použití EF Core v tradičních webových aplikacích podobných MVC. Pokyny pro jiné platformy a modely aplikací chybí nebo jsou zastaralé. Pro EF Core 5.0 plánujeme prozkoumat, zlepšit a zdokumentovat zkušenosti s používáním EF Core s:

* Blazor
* Xamarin, včetně použití příběhu AOT/linker
* WinForms/WPF/WinUI a případně další u.i. Rámců

To bude pravděpodobně mnoho malých zlepšení v EF Core, spolu s pokyny a dlouhodobější spolupráce s jinými týmy s cílem zlepšit komplexní prostředí, které přesahují pouze EF.

Konkrétní oblasti, na které se chceme podívat, jsou:

* Nasazení, včetně zkušeností s používáním nástrojů EF, například pro migrace
* Aplikační modely, včetně Xamarin a Blazor, a pravděpodobně i další
* SQLite zkušenosti, včetně prostorových zkušeností a tabulky obnovuje
* AOT a propojení zkušeností
* Integrace diagnostiky, včetně čítačů perf

## <a name="performance"></a>Výkon

Vedoucí vývojář:@roji

Sledováno [problémy označenými `area-perf` v milníku 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

Velikost trička: L

Stav: Probíhá

Pro EF Core plánujeme vylepšit naši sadu výkonnostních měřítek a provést cílená vylepšení výkonu běhu. Kromě toho plánujeme dokončit nový ADO.NET dávkování API, který byl prototypběhem 3.0 cyklu vydání. Také na ADO.NET vrstvě plánujeme další vylepšení výkonu pro poskytovatele Npgsql.

V rámci této práce také plánujeme přidat ADO.NET/EF čítače výkonu Core a další diagnostiku podle potřeby.

## <a name="architecturalcontributor-documentation"></a>Architektonická/přispěvatelka

Vedoucí dokumentátor:@ajcvickers

Sledováno [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)

Velikost trička: L

Stav: Probíhá

Cílem je usnadnit pochopení toho, co se děje ve vnitřních částí EF Core. To může být užitečné pro každého, kdo používá EF Core, ale primární motivací je usnadnit externím lidem:

* Přispívat do základního kódu EF
* Vytvořit zprostředkovatele databáze
* Sestavení dalších rozšíření

## <a name="microsoftdatasqlite-documentation"></a>Dokumentace k microsoftu.Data.Sqlite

Vedoucí dokumentátor:@bricelam

Sledováno [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)

Velikost trička: M

Stav: Dokončeno. Nová dokumentace je [aktivní na webu dokumentů společnosti Microsoft](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).

Ef tým také vlastní Microsoft.Data.Sqlite ADO.NET zprostředkovatele. Plánujeme plně dokumentovat tohoto poskytovatele jako součást verze 5.0.

## <a name="general-documentation"></a>Obecná dokumentace

Vedoucí dokumentátor:@ajcvickers

Sledováno [problémy v repo dokumentů v milníku 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

Velikost trička: L

Stav: Probíhá

Jsme již v procesu aktualizace dokumentace pro 3.0 a 3.1 verze. Pracujeme také na:
  * Přepracování začínádocs, aby byly přístupnější / snadněji sledovat
  * Reorganizace dokumentů, které usnadňují hledání a přidávání křížových odkazů
  * Přidání dalších podrobností a vysvětlení ke stávajícím dokumentům
  * Aktualizace ukázek a přidání dalších příkladů

## <a name="fixing-bugs"></a>Oprava chyb

Sledováno [problémy označenými `type-bug` v milníku 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

@rojiVývojáři: @maumar @bricelam, @smitpatel @AndriySvyryd, , , ,@ajcvickers

Velikost trička: L

Stav: Probíhá

V době psaní máme 135 chyb, které mají být opraveny ve verzi 5.0 (s již opraveným 62), ale existuje významné překrývání s výše uvedenou částí _Obecné vylepšení dotazů._

Příchozí sazba (problémy, které skončí jako práce v milníku) byla asi 23 otázek za měsíc v průběhu vydání 3.0. Ne všechny z nich bude třeba stanovit v 5.0. Jako hrubý odhad plánujeme opravit dalších 150 problémů v časovém rámci 5.0.

## <a name="small-enhancements"></a>Malá vylepšení

Sledováno [problémy označenými `type-enhancement` v milníku 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

@rojiVývojáři: @maumar @bricelam, @smitpatel @AndriySvyryd, , , ,@ajcvickers

Velikost trička: L

Stav: Probíhá

Kromě větších funkcí popsaných výše, máme také mnoho menších vylepšení naplánováno na 5.0 opravit "papír-kusy". Všimněte si, že mnoho z těchto vylepšení jsou také zahrnuty obecnější témata jsou popsány výše.

## <a name="below-the-line"></a>Pod čarou

Sledováno [problémy označenými `consider-for-next-release` ](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

Jedná se o opravy chyb a vylepšení, které **nejsou** v současné době naplánovány na verzi 5.0, ale podíváme se na jako cíle úseku v závislosti na pokroku dosaženém v práci výše.

Kromě toho při plánování vždy zvažujeme [nejvíce hlasovali.](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) Řezání některé z těchto otázek z uvolnění je vždy bolestivé, ale potřebujeme realistický plán pro zdroje, které máme.

## <a name="feedback"></a>Váš názor

Vaše zpětná vazba na plánování je důležitá. Nejlepší způsob, jak označit důležitost problému, je hlasovat (palec nahoru) pro tuto otázku na GitHubu. Tato data se pak budou připojuji k [procesu plánování](../release-planning.md) pro další verzi.
