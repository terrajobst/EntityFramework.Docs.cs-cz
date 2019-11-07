---
title: Minulé verze Entity Framework – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
uid: ef6/what-is-new/past-releases
ms.openlocfilehash: fada7740453cd9a55a1d0069236efcecbd9aa314
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656143"
---
# <a name="past-releases-of-entity-framework"></a>Minulé verze Entity Framework

První verze Entity Framework byla vydána v 2008, jako součást .NET Framework 3,5 SP1 a Visual Studio 2008 SP1.

Počínaje verzí EF 4.1 jsme dodali jako [balíček NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) – aktuálně jeden z nejoblíbenějších balíčků na NuGet.org.

V rámci verze 4,1 a 5,0 balíček NuGet EntityFramework rozšířila knihovny EF, které byly dodávány jako součást .NET Framework.

Počínaje verzí 6 se v EF stala otevřený zdrojový projekt a zároveň se .NET Framework úplně přesunula.
To znamená, že když přidáte balíček NuGet verze 6 EntityFramework do aplikace, získáte úplnou kopii knihovny EF, která nezávisí na bitech EF, které jsou dodávány jako součást .NET Framework.
To vám pomohlo trochu zrychlit vývoj a doručování nových funkcí.

V červnu 2016 jsme vydali EF Core 1,0. EF Core je založena na novém základu kódu a je navržena jako obtížnější a rozšiřitelná verze EF.
V současné době je EF Core hlavním soustředěním vývoje Entity Framework týmu v Microsoftu.
To znamená, že pro EF6 nejsou naplánovány žádné nové hlavní funkce. EF6 se ale pořád udržuje jako open source projekt a podporovaný produkt Microsoftu.

Tady je seznam minulých verzí v obráceném chronologickém pořadí s informacemi o nových funkcích, které byly představeny v jednotlivých vydáních.

## <a name="ef-tools-update-in-visual-studio-2017-157"></a>Aktualizace nástrojů EF v aplikaci Visual Studio 2017 15,7
V květnu 2018 jsme vydali aktualizovanou verzi nástrojů EF jako součást sady Visual Studio 2017 15,7.
Obsahuje vylepšení některých běžných častých bodů:

- Opravy pro několik chyb usnadnění uživatelského rozhraní
- Alternativní řešení pro SQL Server regresi výkonu při generování modelů z existujících databází [#4](https://github.com/aspnet/entityframework6/issues/4)
- Podpora pro aktualizace modelů pro větší modely na SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Dalším vylepšením v této nové verzi nástrojů EF je, že při vytváření modelu v novém projektu nainstaluje modul runtime EF 6,2. Se staršími verzemi sady Visual Studio je možné použít modul runtime EF 6,2 (stejně jako všechny dřívější verze EF) instalací odpovídající verze balíčku NuGet.

## <a name="ef-620"></a>EF 6.2.0
Modul runtime EF 6,2 byl vydán do NuGet v říjnu 2017.
Děkujeme, že v naší komunitě otevřených přispěvatelů Open sources je naše komunita, EF 6,2 obsahuje řadu [chybových oprav](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) a [vylepšení produktů](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Tady je stručný seznam nejdůležitějších změn, které mají vliv na modul runtime EF 6,2:

- Snižte čas spuštění načtením dokončeného kódu první modely z trvalé mezipaměti [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Rozhraní Fluent API k definování indexů [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions. like () pro povolení psaní dotazů LINQ, které se překládají jako v SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migruje. exe teď podporuje-možnost skriptu [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 může nyní pracovat s hodnotami klíčů generovanými sekvencí v SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Aktualizace seznamu přechodných chyb pro SQL Azure strategii provádění [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Chyba: opakované pokusy dotazů nebo příkazů SQL selžou s "rozhraní SqlParameter je již obsaženo v jiném SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Chyba: časový limit vyhodnocení DbQuery. ToString () v ladicím programu [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="ef-613"></a>EF 6.1.3
Modul runtime EF 6.1.3 byl vydaný do NuGet v říjnu od 2015.
Tato verze obsahuje jenom opravy chyb s vysokou prioritou a regrese nahlášené ve verzi 6.1.2.
Tyto opravy zahrnují:

- Dotaz: regrese v EF – 6.1.2: vnější použití zavedeno a složitější dotazy pro vztahy 1:1 a klauzuli let
- TPT problém s skrytím vlastnosti základní třídy v zděděné třídě
- DbMigration. SQL se nezdařila, pokud je slovo ' přejít ' obsaženo v textu
- Vytvořit příznak kompatibility pro podporu UnionAll a Intersecting pro sloučení
- Dotaz s vícenásobnými Zahrnutími nefunguje v bodu 6.1.2 (práce v 6.1.1).
- "Při pokusu o provedení upgradu z EF 6.1.1 na 6.1.2 se zobrazí chyba ve vaší syntaxi SQL" výjimka ".

## <a name="ef-612"></a>EF 6.1.2
Modul runtime EF 6.1.2 byl vydán do NuGet v prosinci 2014.
Tato verze je převážně o opravách chyb. Také jsme přijali několik zajímavosti změn od členů komunity:
- **Parametry mezipaměti dotazů je možné nakonfigurovat ze souboru app/web. Configuration.**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **SQLFile a SqlResource metody v DbMigration** umožňují spustit skript SQL uložený jako soubor nebo vložený prostředek.

## <a name="ef-611"></a>EF 6.1.1
Modul runtime EF 6.1.1 byl vydán do NuGet v červnu 2014.
Tato verze obsahuje opravy pro problémy, ke kterým došlo v řadě lidí. Mimo jiné:
- Návrhář: Chyba při otevírání EF5 EDMX s desítkovou přesností v Návrháři EF6
- Výchozí logika rozpoznávání instancí pro LocalDB nefunguje s SQL Server 2014

## <a name="ef-610"></a>EF 6.1.0
Modul runtime EF 6.1.0 byl vydán do NuGet v březnu 2014.
Tato dílčí aktualizace obsahuje velký počet nových funkcí:

- **Konsolidace nástrojů** nabízí jednotný způsob, jak vytvořit nový model EF. Tato funkce [rozšiřuje průvodce model EDM (Entity Data Model) ADO.NET, aby podporoval vytváření Code Firstch modelů](~/ef6/modeling/code-first/workflows/existing-database.md), včetně zpětné analýzy z existující databáze. Tyto funkce byly dříve dostupné v kvalitě beta verze nástroje EF Power Tools.
- **[Zpracování chyb potvrzení transakce](~/ef6/fundamentals/connection-resiliency/commit-failures.md)** poskytuje CommitFailureHandler, který využívá nově zavedenou schopnost zachytit transakční operace. CommitFailureHandler umožňuje automatické obnovení při selhání připojení a zároveň potvrzení transakce.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md)** umožňuje zadat indexy umístěním atributu `[Index]` do vlastnosti (nebo vlastností) ve vašem modelu Code First. Code First pak vytvoří odpovídající index v databázi.
- **Rozhraní API pro veřejné mapování** poskytuje přístup k informacím EF, které se týkají způsobu mapování vlastností a typů na sloupce a tabulky v databázi. V dřívějších verzích bylo toto rozhraní API interní.
- **[Možnost konfigurovat zachycení prostřednictvím souboru app/web. config](~/ef6/fundamentals/configuring/config-file.md)** umožní, aby se přichytily k přidání bez opětovné kompilace aplikace.
- **System. data. entity. Infrastructure. Intercept. DatabaseLogger**je nový zachytávací nástroj, který usnadňuje protokolování všech operací databáze do souboru. V kombinaci s předchozí funkcí vám to umožňuje snadno [Přepnout na protokolování operací databáze pro nasazenou aplikaci](~/ef6/fundamentals/configuring/config-file.md), aniž by bylo nutné znovu kompilovat.
- Bylo vylepšeno **zjišťování změn modelů migrace** , aby bylo možné zajistit přesnější a migrační migrace. byl vylepšen i výkon procesu zjišťování změn.
- **Vylepšení výkonu** , včetně méně databázových operací během inicializace, optimalizace pro porovnání rovnosti null v dotazech LINQ, rychlejší generování zobrazení (vytváření modelů) ve více scénářích a efektivnější materializace sledované entity s více přidruženími.

## <a name="ef-602"></a>EF 6.0.2
Modul runtime EF 6.0.2 byl vydán do NuGet v prosinci 2013.
Tato verze opravy je omezená na opravy problémů, které byly představené ve verzi EF6 (regrese v výkonu nebo chování od EF5).

## <a name="ef-601"></a>EF 6.0.1
Běhový modul 6.0.1 EF byl vydán do NuGet v říjnu 2013 současně s EF 6.0.0, protože byl vložen ve verzi sady Visual Studio, která byla dříve uzamčena několika měsíci.
Tato verze opravy je omezená na opravy problémů, které byly představené ve verzi EF6 (regrese v výkonu nebo chování od EF5).
Nejvýznamnější změny byly opravit některé problémy s výkonem během zahřívání pro modely EF.
To je důležité, protože teplový výkon byl oblastí fokusu v EF6 a tyto problémy byly na konci některých dalších nárůstů výkonu provedených v EF6.

## <a name="ef-60"></a>EF 6,0
Modul runtime EF 6.0.0 byl vydán do NuGet v říjnu od 2013.
Toto je první verze, ve které je kompletní běhový modul EF zahrnutý v [balíčku NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) , který nezávisí na bitech EF, které jsou součástí .NET Framework.
Přesunutí zbývajících částí modulu runtime do balíčku NuGet vyžadovalo určitý počet zásadních změn pro existující kód.
Další podrobnosti o ručních krocích požadovaných k upgradu najdete v části věnované [upgradu na Entity Framework 6](upgrading-to-ef6.md) .

Tato verze zahrnuje mnoho nových funkcí.
Následující funkce fungují pro modely vytvořené pomocí Code First nebo návrháře EF:

- **[Asynchronní dotazování a ukládání](~/ef6/fundamentals/async.md)** přidává podporu pro asynchronní vzory založené na úlohách, které byly představeny v rozhraní .NET 4,5.
- **[Odolnost připojení](~/ef6/fundamentals/connection-resiliency/retry-logic.md)** umožňuje automatické obnovení při přechodných selháních připojení.
- **[Konfigurace na základě kódu](~/ef6/fundamentals/configuring/code-based.md)** poskytuje možnost provádět konfiguraci – to se tradičně provádí v konfiguračním souboru – v kódu.
- **[Řešení závislostí](~/ef6/fundamentals/configuring/dependency-resolution.md)** zavádí podporu pro vzor lokátoru služby a jsme vyhodnotili některé z funkcí, které se dají nahradit vlastními implementacemi.
- **[Zachytávání/protokolování SQL](~/ef6/fundamentals/logging-and-interception.md)** poskytuje špičkové stavební bloky pro zachycení operací EF s jednoduchým protokolováním SQL postaveným na nejvyšší úrovni.
- **Vylepšení testování** usnadňuje vytváření dvojitých testů pro DbContext a negenerickými při použití napodobné [architektury](~/ef6/fundamentals/testing/mocking.md) nebo [psaní vlastních testů](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[DbContext se teď dají vytvořit pomocí DbConnection, který už je otevřený,](~/ef6/fundamentals/connection-management.md)** což umožňuje scénáře, kde by bylo užitečné, pokud by bylo možné otevřít připojení při vytváření kontextu (například sdílení propojení mezi součástmi, které nemůžete zaručit. stav připojení.
- **[Vylepšená podpora transakcí](~/ef6/saving/transactions.md)** poskytuje podporu pro transakci mimo rámec a také vylepšené způsoby vytváření transakcí v rámci rozhraní.
- **Výčty, prostorové a lepší výkon na platformě .net 4,0** – přesunutím základních komponent, které se používají v .NET Framework do balíčku NuGet NuGet, teď můžeme nabízet podporu výčtu, prostorové datové typy a vylepšení výkonu z EF5 v .NET 4,0.
- **Vylepšený výkon vyčíslitelné. obsahuje v dotazech LINQ**.
- **Zvýšila se doba zahřívání (zobrazení generace)** , zejména pro velké modely.
- **Služba jednotného připojování &amp; v množném čísle**
- Nyní jsou podporovány **vlastní implementace Equals nebo GetHashCode** na třídy entit.
- **Negenerickými. AddRange/RemoveRange** poskytuje optimalizovaný způsob, jak přidat nebo odebrat více entit ze sady.
- **DbChangeTracker. HasChanges** poskytuje snadný a efektivní způsob, jak zjistit, jestli jsou v databázi uložené nějaké nedokončené změny.
- **SqlCeFunctions** poskytuje příkaz SQL Compact ekvivalentní k SqlFunctions.

Následující funkce platí jenom pro Code First:

- **[Vlastní konvence Code First](~/ef6/modeling/code-first/conventions/custom.md)** umožňují psát vlastní konvence, které vám pomůžou se vyhnout opakující se konfiguraci. Poskytujeme jednoduché rozhraní API pro zjednodušené konvence i některé složitější stavební bloky, které vám umožní vytvářet složitější konvence.
- V tuto chvíli se podporuje **[mapování Code First uložených procedur pro vložení, aktualizaci nebo odstranění](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)** .
- **[Idempotentní migrace skriptů](~/ef6/modeling/code-first/migrations/index.md)** vám umožní vygenerovat skript SQL, který může upgradovat databázi v libovolné verzi na nejnovější verzi.
- **[Konfigurovatelná tabulka historie migrace](~/ef6/modeling/code-first/migrations/history-customization.md)** umožňuje přizpůsobit definici tabulky historie migrace. To je užitečné hlavně pro poskytovatele databází, kteří vyžadují příslušné datové typy atd. pro správné fungování tabulky historie migrace.
- **Více kontextů na databázi** při použití migrace odebere předchozí omezení jednoho Code First modelu na databázi nebo když Code First automaticky vytvořila databáze za vás.
- **[DbModelBuilder. HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)** je nové rozhraní API Code First, které umožňuje nakonfigurovat výchozí schéma databáze pro model Code First na jednom místě. Předchozí schéma Code First bylo pevně zakódováno pro &quot;dbo&quot; a jediným způsobem, jak nakonfigurovat schéma, ke kterému byla tabulka patřila prostřednictvím rozhraní API ToTable.
- **DbModelBuilder. configurations. AddFromAssembly metoda** umožňuje snadno přidat všechny třídy konfigurace definované v sestavení při použití tříd konfigurace s rozhraním Code First Fluent API.
- **[Vlastní operace migrace](https://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)** umožňují přidat další operace, které se mají použít v rámci migrace na základě kódu.
- **Výchozí úroveň izolace transakce se změní na READ_COMMITTED_SNAPSHOT** pro databáze vytvořené pomocí Code First, což umožňuje větší škálovatelnost a menší počet zablokování.
- **Entity a komplexní typy teď můžou být nestedinside třídy**.

## <a name="ef-50"></a>EF 5,0
Modul runtime EF 5.0.0 byl vydán do NuGet v srpnu od 2012.
Tato verze zavádí některé nové funkce, včetně podpory výčtu, funkcí vracející tabulky, prostorových datových typů a různých vylepšení výkonu.

Entity Framework Designer v aplikaci Visual Studio 2012 také zavádí podporu pro více diagramů na model, vybarvení tvarů na návrhové ploše a dávkového importu uložených procedur.

Tady je seznam obsahu, který jsme sepravili speciálně pro vydání EF 5:

-   [Příspěvek k vydání EF 5](https://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Nové funkce v EF5
    -   [Podpora výčtu v Code First](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Podpora výčtu v Návrháři EF](~/ef6/modeling/designer/data-types/enums.md)
    -   [Typy prostorových dat v Code First](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Typy prostorových dat v Návrháři EF](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Podpora poskytovatelů prostorových typů](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Funkce vracející tabulku](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Více diagramů na model](~/ef6/modeling/designer/multiple-diagrams.md)
-   Nastavení modelu
    -   [Vytvoření modelu](~/ef6/modeling/index.md)
    -   [Připojení a modely](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Faktory ovlivňující výkon](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Práce s Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Nastavení konfiguračního souboru](~/ef6/fundamentals/configuring/config-file.md)
    -   [Glosář](~/ef6/resources/glossary.md)
    -   Code First
        -   [Code First k nové databázi (návod a video)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First do existující databáze (návod a video)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Konvence](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Datové poznámky](~/ef6/modeling/code-first/data-annotations.md)
        -   [Rozhraní Fluent API – konfigurace/mapování vlastností & typů](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [Rozhraní Fluent API – konfigurace relací](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [Rozhraní Fluent API s VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migrace Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Automatické Migrace Code First](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrace. exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Definování DbSets](~/ef6/modeling/code-first/dbsets.md)
    -   Návrhář EF
        -   [Model First (návod a video)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (návod a video)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Komplexní typy](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Přidružení/vztahy](~/ef6/modeling/designer/relationships.md)
        -   [Vzor dědičnosti TPT](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Vzor dědičnosti TPH](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Dotaz s uloženými procedurami](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Uložené procedury s více sadami výsledků](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Vložení, aktualizace & odstranění s uloženými procedurami](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Mapování entity na více tabulek (rozdělování entit)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Mapování více entit na jednu tabulku (rozdělení tabulky)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definování dotazů](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Šablony pro generování kódu](~/ef6/modeling/designer/codegen/index.md)
        -   [Návrat k objektu ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Používání modelu
    -   [Práce s DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Dotazování na entity a jejich hledání](~/ef6/querying/index.md)
    -   [Práce s relacemi](~/ef6/fundamentals/relationships.md)
    -   [Načítají se související entity.](~/ef6/querying/related-data.md)
    -   [Práce s místními daty](~/ef6/querying/local-data.md)
    -   [N-vrstvé aplikace](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Nezpracované dotazy SQL](~/ef6/querying/raw-sql.md)
    -   [Vzorce optimistické souběžnosti](~/ef6/saving/concurrency.md)
    -   [Práce s proxy servery](~/ef6/fundamentals/proxies.md)
    -   [Automaticky zjišťovat změny](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Žádné dotazy pro sledování](~/ef6/querying/no-tracking.md)
    -   [Metoda Load](~/ef6/querying/load-method.md)
    -   [Přidání/připojení a stavy entity](~/ef6/saving/change-tracking/entity-state.md)
    -   [Práce s hodnotami vlastností](~/ef6/saving/change-tracking/property-values.md)
    -   [Datová vazba s WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Datová vazba s WinForms (model Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
Modul runtime EF 4.3.1 byl vydaný do NuGet v únoru 2012 krátce po 4.3.0 EF.
Tato oprava obsahuje některé opravy chyb pro vydání EF 4,3 a zavedla lepší podporu LocalDB pro zákazníky, kteří používají EF 4,3 s Visual Studio 2012.

Tady je seznam obsahu, který jsme společně zadali pro vydání EF 4.3.1. většina obsahu poskytovaného pro EF 4,1 se ale vztahuje i na EF 4,3.

-   [Blogový příspěvek k vydání EF 4.3.1](https://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4,3
Modul runtime EF 4.3.0 byl vydán do NuGet v únoru z 2012.
Tato verze zahrnovala novou funkci Migrace Code First, která umožňuje přírůstkově měnit databázi vytvořené pomocí Code First při vývojovém modelu Code First.

Tady je seznam obsahu, který jsme společně zadali pro vydání EF 4,3. většina obsahu poskytovaného pro EF 4,1 se ale vztahuje i na EF 4,3.
-   [Příspěvek k vydání EF 4,3](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Návod k migraci na základě kódu EF 4,3](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Návod pro automatické migrace EF 4,3](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4,2
Modul runtime EF 4.2.0 byl vydán do NuGet v listopadu 2011.
Tato verze zahrnuje opravy chyb pro vydání EF 4.1.1.
Vzhledem k tomu, že tato verze obsahuje jenom opravy chyb, může se jednat o verzi produktu EF 4.1.2, ale zjistili jsme, že jsme přešli na 4,2, abychom mohli opustit číslo verze opravy založené na datu, které jsme použili ve verzích 4.1. x a přijali standard [sémantických verzí](https://semver.org) pro s. Správa verzí emantic.

Tady je seznam obsahu, který je určený speciálně pro vydání EF 4,2. obsah poskytovaný pro EF 4,1 se přesto vztahuje i na EF 4,2:

-   [Příspěvek k vydání EF 4,2](https://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Návod Code First](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Návod pro Database First & modelu](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
Modul runtime EF 4.1.10715 byl vydán do NuGet v červenci 2011.
Kromě oprav chyb v tomto vydání opravy zavedlo některé komponenty, které usnadňují práci s nástrojem pro vytváření Code Firstho modelu.
Tyto komponenty používá Migrace Code First (zahrnuté v EF 4,3) a nástroje EF Power Tools.

Všimněte si, že číslo podivné verze 4.1.10715 balíčku.
Předtím, než jsme se rozhodli použít [sémantickou správu verzí](https://semver.org), jsme použili k používání verzí oprav na základě data.
Tuto verzi si můžete představit jako EF 4,1 s aktualizací 1 (nebo EF 4.1.1).

Tady je seznam obsahu, který jsme povedli pro vydání 4.1.1:

-   [Verze EF 4.1.1 – post](https://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4,1
Modul runtime EF 4.1.10331 byl první publikovaný v NuGet v dubnu 2011.
Tato verze zahrnovala zjednodušené rozhraní DbContext API a pracovní postup Code First.

Všimněte si čísla podivné verze, 4.1.10331, který by měl být skutečně 4,1. Kromě toho je k dispozici verze 4.1.10311, která by měla být 4.1.0-RC (' RC ' představuje ' Release Candidate ').
Předtím, než jsme se rozhodli použít [sémantickou správu verzí](https://semver.org), jsme použili k používání verzí oprav na základě data.

Tady je seznam obsahu, který jsme sepravili pro vydání 4,1. Mnohé z nich se stále vztahují na pozdější verze Entity Framework:

-   [Příspěvek k vydání EF 4,1](https://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Návod Code First](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Návod pro Database First & modelu](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azureé federace a Entity Framework](https://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4,0
Tato verze byla součástí .NET Framework 4 a Visual Studio 2010 v dubnu 2010.
Důležité nové funkce v této verzi zahrnují podporu POCO, mapování cizích klíčů, opožděné načítání, vylepšení testování, přizpůsobitelnou generaci kódu a Model First pracovní postup.

I když se jednalo o druhou verzi Entity Framework, měla název EF 4, která je v souladu s verzí .NET Framework, se kterou dodal.
Po této verzi jsme začali vytvářet Entity Framework k dispozici na NuGetu a přijali sémantickou správu verzí, protože už nejste vázáni na .NET Framework verzi.

Všimněte si, že některé další verze .NET Framework byly dodávány s významnými aktualizacemi zahrnutých bitů EF.
Mnoho nových funkcí EF 5,0 bylo ve skutečnosti implementováno jako vylepšení těchto bitů.
Aby se však racionalizovat příběh verzí pro EF, i nadále odkazujeme na bity EF, které jsou součástí .NET Framework jako modul runtime EF 4,0, zatímco všechny novější verze sestávají z [balíčku NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).

## <a name="ef-35"></a>EF 3,5
Počáteční verze Entity Framework byla součástí sady .NET 3,5 Service Pack 1 a Visual Studio 2008 SP1 vydané v srpnu 2008.
Tato verze poskytuje podporu Basic O/RM pomocí pracovního postupu Database First.
