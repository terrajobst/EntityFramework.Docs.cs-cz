---
title: Předchozími verzemi nástroje Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
caps.latest.revision: 4
ms.openlocfilehash: 64cf28b858ad364243447a529f26475d449cf73e
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913451"
---
# <a name="past-releases-of-entity-framework"></a>Předchozími verzemi nástroje Entity Framework

První verze Entity Framework byla vydána v roce 2008 jako součást rozhraní .NET Framework 3.5 SP1 a Visual Studio 2008 SP1.

Od verze EF4.1 byla odeslaná jako [balíček NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) – právě jeden z nejoblíbenějších balíčků na NuGet.org.

Balíček EntityFramework NuGet mezi verze 4.1 a 5.0, rozšířené knihovny EF, která jsou dodávána jako součást rozhraní .NET Framework.   

Od verze 6, EF stala projekt open source a také přesune zcela ze vzdálené formuláře rozhraní .NET Framework.
To znamená, že při přidání balíčku NuGet EntityFramework verze 6 do aplikace se zobrazuje úplnou kopii EF knihovnu, která nezávisí na EF bitů, které se dodávají jako součást rozhraní .NET Framework.
Díky tomu trochu zrychlení tempu vývoje a doručování nových funkcí.

V červnu 2016 jsme vydali EF Core 1.0. EF Core je založené na nové základu kódu a je navržena jako jednoduchý a rozšiřitelný verze EF.
EF Core je aktuálně hlavní fokus vývoje pro Entity Framework tým společnosti Microsoft.
To znamená, že neexistují žádné nové hlavní funkce plánujeme přidat EF6. Stále je ale EF6 nakládat jako s projekt open source a podporovaných produktů společnosti Microsoft.

Tady je seznam minulých vydaných verzích v obráceném chronologickém pořadí s informacemi o nové funkce, které byly zavedeny v jednotlivých verzích.

## <a name="ef-613"></a>EF 6.1.3
EF 6.1.3 runtime byla vydána NuGet října 2015.
Tato verze obsahuje pouze opravy chyb s vysokou prioritou a regrese v 6.1.2 vydání.
Tyto opravy patří:

- Dotaz: Regrese v EF 6.1.2: operátoru OUTER APPLY zavedené a složitější dotazy pro relace 1:1 a "let" – klauzule
- TPT problém s skrytí vlastnost základní třídy ve zděděné třídě
- DbMigration.Sql selže, pokud je slovo "go" je obsažen v textu
- Vytvoření kompatibility příznak pro UnionAll a Intersect podpora vyrovnání
- Dotaz s více zahrnuje nefunguje v 6.1.2 (funguje v 6.1.1)
- Výjimka "Máte chybu v syntaxi SQL" po provedení upgradu z EF 6.1.1 k 6.1.2

## <a name="ef-612"></a>EF 6.1.2
EF 6.1.2 runtime byla vydána NuGet v prosinci 2014.
Tato verze je většinou o opravy chyb. Můžeme také povoleny několik zajímavosti změn od členů komunity:
- **Dá se parametry dotazu mezipaměť ze souboru app/web.configuration**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **Metody SqlFile a SqlResource na DbMigration** umožňují provozovat SQL skriptu uložen jako soubor nebo vložený zdroj.

## <a name="ef-611"></a>EF 6.1.1
EF 6.1.1 runtime byla vydána NuGet v červnu 2014.
Tato verze obsahuje opravy problémů, o kterých došlo k počet lidí. Mimo jiné:
- Návrhář: Chyba při otevírání EF5 edmx s počet desetinných míst v Návrháři EF6
- Výchozí instance detekce logiku pro LocalDB nefunguje s SQL serverem 2014

## <a name="ef-610"></a>EF 6.1.0
EF 6.1.0 runtime byla vydána NuGet v březnu 2014.
Tento dílčí aktualizace obsahuje velký počet nových funkcí:

- **Nástroje konsolidace** poskytuje konzistentní způsob, jak vytvořit nový model EF. Tato funkce [rozšiřuje průvodce datový Model Entity ADO.NET pro podporu vytváření modelů Code First](~/ef6/modeling/code-first/workflows/existing-database.md), včetně zpětná analýza z existující databáze. Tyto funkce byly dříve k dispozici v beta verzi kvality v EF Power Tools.
- **[Zpracování chyb potvrzení transakce](~/ef6/fundamentals/connection-resiliency/commit-failures.md) ** poskytuje CommitFailureHandler, která využívá nově zavedená možnost zachycení operace transakce. CommitFailureHandler umožňuje automatické obnovení v případě selhání připojení, zatímco potvrzení transakce.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md) ** umožňuje indexů možné zadat tak, že `[Index]` atributu pro vlastnost (nebo vlastnosti) v modelu Code First. Kód nejprve pak vytvoří odpovídající index v databázi.
- **Mapování veřejného rozhraní API** poskytuje přístup k informacím EF má na typy a vlastnosti zpřístupněných sloupců a tabulek v databázi. V minulých verzích tohoto rozhraní API byla interní.
- **[Možnost konfigurace sběrače prostřednictvím souboru App/Web.config](~/ef6/fundamentals/configuring/config-file.md) ** umožňuje sběrače přidávané bez opětovné kompilace aplikace.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**je nový sběrač, se kterou snadno protokolovat všechny operace databáze do souboru. V kombinaci s předchozí funkce, můžete tak snadno [zapnout protokolování databázové operace pro nasazenou aplikaci](~/ef6/fundamentals/configuring/config-file.md), aniž by bylo nutné znovu zkompilovat.
- **Detekce změn modelu migrace** jsme vylepšili tak, aby byly přesnější; výkon samotného procesu zjišťování změn je taky Vylepšená vygenerované migrace.
- **Vylepšení výkonu** včetně nižší databázové operace při inicializaci, optimalizace pro porovnání rovnosti null v dotazech LINQ, rychleji zobrazit. generace (vytvoření modelu) ve více scénářích, přehledné a efektivnější materializace sledované entit s několika přidružení.

## <a name="ef-602"></a>EF 6.0.2
EF 6.0.2 runtime byla vydána NuGet v prosinci 2013.
Tato verze je omezená na řešení problémů, které byly zavedeny v EF6 verzi (regrese výkonu a chování od EF5).

## <a name="ef-601"></a>EF 6.0.1
EF 6.0.1 runtime byla vydána NuGet v říjnu 2013 současně s EF 6.0.0, protože byl vložen ve verzi sady Visual Studio, který měl uzamčen před několika měsíci.
Tato verze je omezená na řešení problémů, které byly zavedeny v EF6 verzi (regrese výkonu a chování od EF5).
Nejdůležitější změny byly opravit některé problémy s výkonem při zahřívání EF modelů.
To je důležité při zahřívání, šlo to oblast fokus v EF6 a tyto problémy se negace některé z dalších zvýšení výkonu v EF6.

## <a name="ef-60"></a>EF 6.0
EF 6.0.0 runtime byla vydána NuGet v říjnu 2013.
Toto je první verze, ve které je součástí dokončení modulu runtime EF [balíček NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) která nezávisí na EF bitů, které jsou součástí rozhraní .NET Framework.
Přesun zbývajících součástí modulu runtime na balíček NuGet vyžaduje celou řadou zásadní změny pro existující kód.
Naleznete v části [upgrade na Entity Framework 6](upgrading-to-ef6.md) podrobné informace o ruční kroky nutné k upgradu.

Tato verze obsahuje mnoho nových funkcí.
Tyto funkce fungují pro modely vytvořené pomocí Code First nebo EF designeru:

- **[Asynchronní dotazy a uložit](~/ef6/fundamentals/async.md) ** přidává podporu pro založené na úlohách asynchronních vzorech, které byly zavedeny v rozhraní .NET 4.5.
- **[Odolnost připojení](~/ef6/fundamentals/connection-resiliency/retry-logic.md) ** umožňuje automatické obnovení v případě selhání přechodné připojení.
- **[Konfigurace založená na kódu](~/ef6/fundamentals/configuring/code-based.md) ** získáte možnost provádět konfiguraci – které tradičně bylo provedeno v konfiguračním souboru – v kódu.
- **[Řešení závislostí](~/ef6/fundamentals/configuring/dependency-resolution.md) ** zavádí podporu pro lokátoru služeb vzor a My jsme dostaneme si některé části funkcí, které je možné nahradit vlastní implementace.
- **[Zachycení/SQL protokolování](~/ef6/fundamentals/logging-and-interception.md) ** poskytuje nízké úrovně stavební bloky pro zachycení EF operace s jednoduchou protokolování SQL vytvořené v horní části.
- **Zlepšení testovatelnosti** usnadňují vytvoření testu zdvojnásobí pro kontext databáze a DbSet při [pomocí napodobování framework](~/ef6/fundamentals/testing/mocking.md) nebo [sestavování vlastních testu zdvojnásobí](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[Kontext databáze. teď se dají vytvářet pomocí DbConnection, který je již otevřen](~/ef6/fundamentals/connection-management.md) ** která umožňuje scénáře, ve kterém by bylo užitečné, pokud připojení může být otevřený, při vytváření kontextu (například sdílení připojení mezi komponentami kde je nelze zaručit stav připojení).
- **[Vylepšená podpora transakce](~/ef6/saving/transactions.md) ** poskytuje podporu pro externí rozhraní framework a také vylepšené možnosti vytváření transakcí v rámci transakce.
- **Výčty, Spatial a lepší výkon v .NET 4.0** – přesunutím základní součásti, které mají být používány v rozhraní .NET Framework do balíčku EF NuGet jsme schopní teď nabízí podporu výčtu, typy prostorových dat a vylepšení výkonu z EF5 na rozhraní .NET 4.0.
- **Vylepšený výkon Enumerable.Contains v dotazech LINQ**.
- **Vylepšené záložním čas (generování zobrazení)**, zejména u velkých modelů.
- **Modulární Pluralizace &amp; Singularization služby**.
- **Vlastní implementace Equals a GetHashCode** u entity třídy jsou nyní podporovány.
- **DbSet.AddRange/RemoveRange** poskytuje optimalizované způsob, jak přidat nebo odebrat více entit ze sady.
- **DbChangeTracker.HasChanges** poskytuje snadný a efektivní způsob, jestli jsou všechny probíhající změny uložit do databáze.
- **SqlCeFunctions** poskytuje SQL Compact odpovídá SqlFunctions.

Tyto funkce použít jenom na Code First:

- **[Vlastní první konvence kódu](~/ef6/modeling/code-first/conventions/custom.md) ** umožňují napsat vlastní zásady odvíjející zabránit tomu, aby opakované konfigurace. Zajišťuje jednoduché rozhraní API pro zjednodušené vytváření názvů, jakož i nějakou složitější stavební bloky a umožňuje tak vytvářet složitější konvence.
- **[Kód první mapování na uložené procedury Insert/Update/Delete](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md) ** se teď podporuje.
- **[Skripty migrace Idempotentní](~/ef6/modeling/code-first/migrations/index.md) ** mohli generovat skript SQL, který můžete upgradovat databáze v libovolné verze na nejnovější verzi.
- **[Tabulky historie konfigurovatelné migrace](~/ef6/modeling/code-first/migrations/history-customization.md) ** vám umožní přizpůsobit definici tabulky historie migrace. To je užitečné hlavně pro poskytovatelé databází, které vyžadují příslušné datové typy atd. pro tabulku historie migrace mohly fungovat správně.
- **Více kontexty na databázi** odebere předchozí omezení jednoho Code First modelu na databázi při použití migrace nebo Code First automaticky vytvoří databáze za vás.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md) ** je nový kód prvního rozhraní API, která umožňuje výchozí schéma databáze modelu Code First v jednom místě. Dříve byl Code First výchozí schéma pevně zakódované &quot;dbo&quot; a byl jediný způsob, jak nakonfigurovat schématu, do které tabulky patřil přes ToTable rozhraní API.
- **Metoda DbModelBuilder.Configurations.AddFromAssembly** umožňuje snadno přidat všechny třídy konfigurace definované v sestavení, při použití třídy pro konfiguraci s první Fluent API kódu.
- **[Vlastní operace migrace](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/) ** povolená, můžete přidat další operace se používá v migrací založený na kódu.
- **Výchozí úroveň izolace transakce se změní na možnost READ_COMMITTED_SNAPSHOT** pro databáze vytvořené využitím Code First, umožňuje další škálovatelnost a méně zablokování.
- **Entity a komplexní typy může být nyní nestedinside třídy**. |

## <a name="ef-50"></a>EF 5.0
EF 5.0.0 runtime byla vydána NuGet v srpnu 2012.
Tato verze přináší několik nových funkcí, včetně podpory výčtu, funkce vracející tabulku, typy prostorových dat a různá vylepšení výkonu.

Entity Framework Designer v sadě Visual Studio 2012 také zavádí podporu pro více diagramů na model a obarvení tvarů při návrhu plochu a batch importu uložené procedury.

Tady je seznam obsahu, který jsme připravili speciálně pro verze EF 5.

-   [EF 5 Release příspěvku](http://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Nové funkce v EF5
    -   [Výčet podporují v kódu nejprve](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Podpora výčtu v EF designeru](~/ef6/modeling/designer/data-types/enums.md)
    -   [Prostorové datové typy v kódu nejprve](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Typy prostorových dat v EF designeru](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Podpora zprostředkovatele pro prostorové typy](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Funkce vracející tabulku](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Více diagramů podle modelu](~/ef6/modeling/designer/multiple-diagrams.md)
-   Nastavení modelu
    -   [Vytvoření modelu](~/ef6/modeling/index.md)
    -   [Připojení a modelů](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Důležité informace o výkonu](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Práce s Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Nastavení konfiguračního souboru](~/ef6/fundamentals/configuring/config-file.md)
    -   [Glosář](~/ef6/resources/glossary.md)
    -   Kód nejprve
        -   [Kód nejprve pro novou databázi (návod a video)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Kód nejprve k existující databázi (návod a video)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Konvence](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Datové poznámky](~/ef6/modeling/code-first/data-annotations.md)
        -   [Rozhraní Fluent API – konfigurace nebo mapování vlastnosti & typy](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [Rozhraní Fluent API – konfigurace relace](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [Rozhraní Fluent API s VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migrace Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Migrace automatické Code First](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Definování DbSets](~/ef6/modeling/code-first/dbsets.md)
    -   EF designeru
        -   [Model First (návod a video)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Databáze první (návod a video)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Komplexní typy](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Přidružení/relace](~/ef6/modeling/designer/relationships.md)
        -   [Vzor TPT dědičnosti](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Vzor TPH dědičnosti](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Dotaz s uloženými procedurami](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Uložené procedury s více sad výsledků dotazu](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Vložení, aktualizace a odstranění s uloženými procedurami](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Mapovat Entity k několika tabulkám (rozdělení Entity)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Mapování více entit na jedné tabulce (rozdělení tabulky)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definice dotazů](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Šablony pro generování kódu](~/ef6/modeling/designer/codegen/index.md)
        -   [Návrat k objektu ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Pomocí vašeho modelu
    -   [Práce s DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Dotazování a hledání entit](~/ef6/querying/index.md)
    -   [Práce se vztahy](~/ef6/fundamentals/relationships.md)
    -   [Načítají se související entity](~/ef6/querying/related-data.md)
    -   [Práce s místními daty](~/ef6/querying/local-data.md)
    -   [N-vrstvé aplikace](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Nezpracované dotazy SQL](~/ef6/querying/raw-sql.md)
    -   [Vzory optimistického řízení souběžnosti](~/ef6/saving/concurrency.md)
    -   [Práce s proxy servery](~/ef6/fundamentals/proxies.md)
    -   [Automatické zjištění změn](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Sledování bez dotazy](~/ef6/querying/no-tracking.md)
    -   [Metoda Load](~/ef6/querying/load-method.md)
    -   [Přidat nebo připojit a stavy Entity](~/ef6/saving/change-tracking/entity-state.md)
    -   [Práce s hodnotami vlastností](~/ef6/saving/change-tracking/property-values.md)
    -   [Datové vazby pomocí WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Datové vazby s WinForms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
EF 4.3.1 runtime byla vydána NuGet v únoru 2012 krátce po EF 4.3.0.
Tato verze opravy zahrnuté opravám některých chyb na verzi EF 4.3 a zavést lepší LocalDB podporu pro zákazníky, kteří používají EF 4.3 pomocí sady Visual Studio 2012.

Tady je seznam obsahu, který jsme připravili speciálně pro verzi EF 4.3.1, většina obsahu k dispozici pro EF 4.1 stále platí pro EF 4.3 také.

-   [EF 4.3.1 Release Blogový příspěvek](http://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
EF 4.3.0 runtime byla vydána NuGet v únoru 2012.
Tato verze součástí nové funkce migrace Code First, který umožňuje databáze vytvoří ji Code First postupně změnit, protože váš model Code First vyvíjí.

Tady je seznam obsahu, který jsme připravili speciálně pro verze EF 4.3, většina obsahu k dispozici pro EF 4.1 stále platí pro EF 4.3 také:
-   [EF verze 4.3 příspěvku](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Návod EF 4.3 založený na kódu migrace](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Návod 4.3 Automatické migrace EF](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4.2
EF 4.2.0 runtime byla vydána NuGet v Listopad 2011.
Tato verze obsahuje opravy chyb EF 4.1.1 release.
Vzhledem k tomu, že tato verze zahrnuty pouze oprava to mohlo být EF 4.1.2 opravy chyb vydání ale jsme se rozhodli pro přesun do 4.2 pro umožní se přesunout od data na základě čísla verze opravy jsme použili v 4.1.x uvolní a přijmout [sémantické Versionsing](https://semver.org) úrovně standard pro sémantické správy verzí.

Tady je seznam obsahu, který jsme připravili speciálně pro verze EF 4.2, obsah zadaný u EF 4.1 stále platí pro EF 4.2 také.

-   [EF verze 4.2 příspěvku](http://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [První původce kódem](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Návod první model & databáze](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
EF 4.1.10715 runtime byla vydána NuGet v z července 2011.
Kromě opravených chyb této vydané verzi opravy zavést některé součásti, aby bylo snazší pro návrhové nástroje pro práci s modelem Code First.
Tyto součásti jsou používány migrace Code First (zahrnutý v EF 4.3) a EF Power Tools.

Můžete si všimnout, že číslo neobvyklé verze 4.1.10715 balíčku.
Použili jsme používat data na základě opravy verze předtím, než jsme se rozhodli přijmout [Semantic Versioning](https://semver.org).
Tato verze si můžete představit jako oprava EF 4.1 1 (tj. 4.1.1).

Tady je seznam obsahu, sestavili jsme pro 4.1.1 verze:

-   [EF 4.1.1 Release příspěvku](http://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4.1
EF 4.1.10331 runtime byl první k publikování na webu NuGet, dubna 2011.
Tato verze obsahovala zjednodušené rozhraní API DbContext a Code First pracovního postupu.

Všimnete si číslo verze neobvyklé 4.1.10331, který by měl mít ve skutečnosti byl 4.1. Kromě toho existuje 4.1.10311 verze, která by byla 4.1.0-rc ("rc" znamená "release candidate").
Použili jsme používat data na základě opravy verze předtím, než jsme se rozhodli přijmout [Semantic Versioning](https://semver.org).

Tady je seznam obsahu, který jsme připravili pro verze 4.1. Většina článku stále platí pro novější verze rozhraní Entity Framework:

-   [EF verze 4.1 příspěvku](http://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [První původce kódem](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Návod první model & databáze](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure federace a Entity Framework](http://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
Tato verze byla zahrnuta v rozhraní .NET Framework 4 a Visual Studio 2010, v dubnu 2010.
Důležité nové funkce v této verzi zahrnuté POCO podpory, cizí klíče mapování, opožděné načtení, zlepšení testovatelnosti, generování přizpůsobitelné kódu a Model prvního pracovního postupu.

I když bylo druhou verzi Entity Frameworku, jmenovalo EF 4, aby bylo v souladu s verzí rozhraní .NET Framework, která je dodávána s prostředím.
Po tomto vydání začali jsme zpřístupnění Entity Frameworku na webu NuGet a přijaté sémantické správy verzí, protože jsme se už není svázaný na verzi rozhraní .NET Framework.

Všimněte si, že některé další verze rozhraní .NET Framework byly dodány s významné aktualizace na zahrnuté EF bitů.
Ve skutečnosti mnoho nových funkcí EF 5.0 byly implementovány jako vylepšení na tyto služby bits.
Ale, aby bylo možné racionalizovat scénáře správy verzí pro EF, jsme i dál odkazovat na EF bitů, které jsou součástí rozhraní .NET Framework jako modul runtime EF 4.0, zatímco všechny novější verze se skládají z [balíček NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).         

## <a name="ef-35"></a>EF 3.5
Počáteční verze Entity Framework byla zahrnuta v rozhraní .NET 3.5 Service Pack 1 a Visual Studio 2008 SP1, vydané v srpnu 2008.
Tato verze poskytuje podpora na úrovni basic O/RM pomocí databáze prvního pracovního postupu.
