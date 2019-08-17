---
title: Přerušující změny v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 884cc6611b986fb213d99d3d2fc69d7bebe34aa2
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565327"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>Přerušující změny zahrnuté v EF Core 3,0 (aktuálně ve verzi Preview)

> [!IMPORTANT]
> Počítejte s tím, že sady funkcí a plány budoucích verzí se vždycky mění, a i když se pokusíme tuto stránku uchovávat v aktuálním stavu, nemusí se po celou dobu projevit naše nejnovější plány.

Následující změny rozhraní API a chování mají možnost rušit aplikace vyvinuté pro EF Core 2.2. x při jejich upgradu na 3.0.0.
Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](../../providers/provider-log.md).
Přerušení nových funkcí zavedených z jedné verze 3,0 Preview do jiné 3,0 verze Preview zde nejsou dokumentovány.

## <a name="summary"></a>Souhrn

| **Zásadní změna**                                                                                               | **Dopad** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [Dotazy LINQ již nejsou vyhodnocovány na klientovi.](#linq-queries-are-no-longer-evaluated-on-the-client)         | Vysoká       |
| [EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0](#netstandard21) | Vysoká      |
| [EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK](#dotnet-ef) | Vysoká      |
| [Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.](#fromsql) | Vysoká      |
| [Typy dotazů jsou konsolidovány s typy entit](#qt) | Vysoká      |
| [Entity Framework Core už není součástí sdílené ASP.NET Core architektury.](#no-longer) | Střední      |
| [Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.](#cascade) | Střední      |
| [DeleteBehavior. restrict má sémantiku čištění.](#deletebehavior) | Střední      |
| [Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.](#config) | Střední      |
| [Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.](#each) | Střední      |
| [Žádné dotazy pro sledování neprovádějí překlad identity](#notrackingresolution) | Střední      |
| [Změny rozhraní API pro metadata](#metadata-api-changes) | Střední      |
| [Změny rozhraní API pro konkrétního zprostředkovatele](#provider) | Střední      |
| [UseRowNumberForPaging se odebral.](#urn) | Střední      |
| [Metody Z tabulek se dají zadat jenom v kořenech dotazů.](#fromsql) | Nízká      |
| [~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit](#qe) | Nízká      |
| [Dočasné hodnoty klíčů už nejsou nastavené na instance entit.](#tkv) | Nízká      |
| [DetectChanges respektuje hodnoty klíčů generované úložištěm.](#dc) | Nízká      |
| [Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.](#de) | Nízká      |
| [Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.](#aes) | Nízká      |
| [Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.](#ip) | Nízká      |
| [Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.](#fkp) | Nízká      |
| [Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.](#dbc) | Nízká      |
| [Ve výchozím nastavení se používají pole pro zálohování.](#backing-fields-are-used-by-default) | Nízká      |
| [Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí](#throw-if-multiple-compatible-backing-fields-are-found) | Nízká      |
| [Názvy vlastností pouze pro pole se musí shodovat s názvem pole.](#field-only-property-names-should-match-the-field-name) | Nízká      |
| [AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.](#adddbc) | Nízká      |
| [DbContext. entry teď provádí místní DetectChanges.](#dbe) | Nízká      |
| [Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.](#string-and-byte-array-keys-are-not-client-generated-by-default) | Nízká      |
| [ILoggerFactory je teď služba s vymezeným oborem.](#ilf) | Nízká      |
| [Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | Nízká      |
| [Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | Nízká      |
| [Nové chování pro HasOne/HasMany se volá s jedním řetězcem.](#nbh) | Nízká      |
| [Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask](#rtnt) | Nízká      |
| [Relační: anotace TypeMapping je nyní pouze TypeMapping](#rtt) | Nízká      |
| [ToTable na odvozeném typu vyvolá výjimku.](#totable-on-a-derived-type-throws-an-exception) | Nízká      |
| [EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.](#pragma) | Nízká      |
| [Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3](#sqlite3) | Nízká      |
| [Hodnoty GUID se teď ukládají jako TEXT na SQLite.](#guid) | Nízká      |
| [Hodnoty char se teď ukládají jako TEXT na SQLite.](#char) | Nízká      |
| [ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.](#migid) | Nízká      |
| [Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.](#xinfo) | Nízká      |
| [LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.](#lqpe) | Nízká      |
| [Vysvětlení rozhraní API pro názvy omezení cizího klíče](#clarify) | Nízká      |
| [IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.](#irdc2) | Nízká      |
| [Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.](#dip) | Nízká      |
| [SQLitePCL. Raw aktualizováno na verzi 2.0.0](#SQLitePCL) | Nízká      |
| [NetTopologySuite aktualizace na verzi 2.0.0](#NetTopologySuite) | Nízká      |
| [Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.](#mersa) | Nízká      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Dotazy LINQ již nejsou vyhodnocovány na klientovi.

[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[taky zobrazit problémy #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.
Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.

**Nové chování**

Počínaje 3,0 EF Core v rámci projekce nejvyšší úrovně (posledního `Select()` volání v dotazu) jenom výrazy vyhodnotit na straně klienta.
Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.

**Proč**

Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.
Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.
Například podmínka ve `Where()` volání, která se nedá přeložit, může způsobit přenesení všech řádků z databázového serveru a filtr, který se má použít na straně klienta.
Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.
Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.

Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.

**Hrozeb**

Pokud dotaz nelze plně přeložit, pak buď Přepište dotaz do formuláře, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`nebo podobným způsobem explicitně přeneste data zpět do klienta, kde lze následně dále zpracovávat pomocí LINQ-to-Objects.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0

[Sledování problému #15498](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

Tato změna je představena ve verzi EF Core 3,0-Preview 7.

**Staré chování**

Před 3,0 EF Core cílené .NET Standard 2,0 a spustí se na všech platformách, které podporují tento standard, včetně .NET Framework.

**Nové chování**

Počínaje 3,0 se EF Core cíle .NET Standard 2,1 a spustí se na všech platformách, které podporují tento standard. To nezahrnuje .NET Framework.

**Proč**

Toto je součást strategického rozhodnutí napříč technologiemi .NET a zaměřuje se na energii na platformě .NET Core a dalších moderních platformách .NET, jako je Xamarin.

**Hrozeb**

Zvažte přechod na moderní platformu .NET. Pokud to není možné, pak pokračujte v používání EF Core 2,1 nebo EF Core 2,2, které podporují .NET Framework.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core už není součástí sdílené ASP.NET Core architektury.

[Sledování oznámení o problémech # 325](https://github.com/aspnet/Announcements/issues/325)

Tato změna je představena ve verzi ASP.NET Core 3,0-Preview 1. 

**Staré chování**

Před ASP.NET Core 3,0 se při přidání odkazu na balíček do `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`zahrnovala EF Core a někteří poskytovatelé EF Core dat jako poskytovatel SQL Server.

**Nové chování**

Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.

**Proč**

Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server. Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.

V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.
Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.

**Hrozeb**

Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK

[Sledování problému #14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

Tato změna je představena v EF Core 3,0-Preview 4 a odpovídající verzi .NET Core SDK.

**Staré chování**

Před 3,0 `dotnet ef` byl nástroj součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků. 

**Nové chování**

Počínaje 3,0 sada .NET SDK nezahrnuje `dotnet ef` nástroj, takže před tím, než ho budete moct použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj. 

**Proč**

Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako běžný nástroj rozhraní .NET CLI v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.

**Hrozeb**

Aby bylo možné spravovat migrace nebo uživatelské rozhraní a `DbContext`, nainstalujte `dotnet-ef` nástroj jako globální nástroj:

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.

[Sledování problému #10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.

**Nové chování**

Počínaje EF Core 3,0, použijte `FromSqlRaw`, `ExecuteSqlRaw`a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.
Příklad:

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Použijte `FromSqlInterpolated`, `ExecuteSqlInterpolated` a`ExecuteSqlInterpolatedAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány jako součást interpolované řetězce dotazu.
Příklad:

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.

**Proč**

Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.
To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.

**Hrozeb**

Přepněte na použití nových názvů metod.

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>Metody Z tabulek se dají zadat jenom v kořenech dotazů.

[Sledování problému #15704](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

Tato změna je představena ve verzi EF Core 3,0-Preview 6.

**Staré chování**

Před EF Core 3,0 `FromSql` lze metodu zadat kdekoli v dotazu.

**Nové chování**

Počínaje EF Core `FromSqlRaw` 3,0 lze nové metody a `FromSqlInterpolated` (které nahradí `FromSql`) zadat pouze v kořenech `DbSet<>`dotazu, tj. přímo na. Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.

**Proč**

Určení `FromSql` kdekoli jinde než u a `DbSet` neobsahovalo žádné přidané významy nebo přidání hodnoty a může v některých scénářích způsobit nejednoznačnost.

**Hrozeb**

`FromSql`volání by se měla přesunout přímo na, `DbSet` na které se vztahují.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>Žádné dotazy pro sledování neprovádějí překlad identity

[Sledování problému #13518](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

Tato změna je představena ve verzi EF Core 3,0-Preview 6.

**Staré chování**

Před EF Core 3,0 se stejná instance entity používá pro všechny výskyty entity se zadaným typem a ID. To odpovídá chování sledovacích dotazů. Například tento dotaz:

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
vrátí stejnou `Category` instanci pro každý `Product` , který je spojen s danou kategorií.

**Nové chování**

Počínaje EF Core 3,0 budou vytvořeny různé instance entit při výskytu entity se zadaným typem a ID na různých místech vráceného grafu. Například dotaz výše bude nyní vracet novou `Category` instanci pro každou `Product` , i když jsou ke stejné kategorii přidruženy dva produkty.

**Proč**

Překlad identity (to znamená, že určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a režii paměti. Obvykle se spustí čítač, aby se v prvním místě nepoužily žádné dotazy na sledování. I když může být v některých případech užitečný překlad identity, není potřeba, pokud se entity mají serializovat a odeslat klientovi, což je běžné pro žádné dotazy pro sledování.

**Hrozeb**

Pokud je vyžadováno rozlišení identity, použijte dotaz sledování.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit

[Sledování problému #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Tato změna se vrátí v EF Core 3,0-Preview 7.

Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací. Například chcete-li přepnout protokolování SQL na `Debug`, explicitně nakonfigurovat úroveň v `OnConfiguring` nebo `AddDbContext`:
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Dočasné hodnoty klíčů už nejsou nastavené na instance entit.

[Sledování problému #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

Tato změna je představena ve verzi EF Core 3,0-Preview 2.

**Staré chování**

Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.
Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.

**Nové chování**

Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.

**Proč**

Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována pomocí nějaké `DbContext` instance, je přesunuta do jiné `DbContext` instance. 

**Hrozeb**

Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve `Added` stavu.
K tomu je možné se vyhnout:
* Nepoužívejte klíče generované úložištěm.
* Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.
* Získá aktuální dočasné hodnoty klíče z informací o sledování entity.
Například vrátí dočasnou hodnotu, `context.Entry(blog).Property(e => e.Id).CurrentValue` i když `blog.Id` nebyla nastavena.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges respektuje hodnoty klíčů generované úložištěm.

[Sledování problému #14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Předtím, než EF Core 3,0, nalezené `DetectChanges` Nesledované entity by byly sledovány `Added` ve stavu a vloženy jako nový řádek při `SaveChanges` volání.

**Nové chování**

Počínaje EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve `Modified` stavu.
To znamená, že se předpokládá, že řádek pro entitu existuje a že bude při `SaveChanges` volání aktualizována.
Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude se nová entita dál sledovat jako `Added` v předchozích verzích.

**Proč**

Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.

**Hrozeb**

Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.
Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.
Například s rozhraním API Fluent:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Nebo s poznámkami k datům:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.

[Sledování problému #10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.

**Nové chování**

Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.
Například volání `context.Remove()` na odstranění objektu zabezpečení bude mít za následek, že jsou všechny sledované požadované závislé položky nastaveny také na `Deleted` hodnotu okamžitě.

**Proč**

Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou _před_ `SaveChanges` voláním odstraněny.

**Hrozeb**

Předchozí chování lze obnovit pomocí nastavení v `context.ChangedTracker`.
Příklad:

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior. restrict má sémantiku čištění.

[Sledování problému #12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

Tato změna je představena ve verzi EF Core 3,0-Preview 5.

**Staré chování**

Před 3,0 se `DeleteBehavior.Restrict` v databázi vytvořily cizí klíče se `Restrict` sémantikou, ale zároveň se nezřetelně změnila interní oprava.

**Nové chování**

Počínaje 3,0 `DeleteBehavior.Restrict` zajistí, že se cizí klíče vytvoří s `Restrict` sémantikou – to znamená bez kaskády; porušení omezení throw--bez vlivu na interní opravu EF.

**Proč**

Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem, a to bez neočekávaných vedlejších účinků.

**Hrozeb**

Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>Typy dotazů jsou konsolidovány s typy entit

[Sledování problému #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/query-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.
To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).

**Nové chování**

Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.
Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.

**Proč**

Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.
Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.
Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.

**Hrozeb**

Následující části rozhraní API jsou teď zastaralé:
* **`ModelBuilder.Query<>()`** – Místo `ModelBuilder.Entity<>().HasNoKey()` toho je nutné volat k označení typu entity, protože neobsahují žádné klíče.
Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.
* **`DbQuery<>`** – Místo `DbSet<>` toho by se mělo použít.
* **`DbContext.Query<>()`** – Místo `DbContext.Set<>()` toho by se mělo použít.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.

[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14153) problému

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před EF Core 3,0 byla konfigurace vztahu vlastníka provedena přímo po `OwnsOne` volání nebo. `OwnsMany` 

**Nové chování**

Od EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.
Příklad:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena `WithOwner()` podobně jako konfigurace ostatních vztahů.
I když se konfigurace samotného vlastního typu bude i nadále zřetězit za `OwnsOne()/OwnsMany()`.
Příklad:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

Kromě toho `Entity()`, `HasOne()`že volání `Set()` ,, nebo s cílem cílového typu, nyní vyvolá výjimku.

**Proč**

Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.
Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako `HasForeignKey`je.

**Hrozeb**

Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.

[Sledování problému #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Vezměte v úvahu následující model:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
Pokud `OrderDetails` je před EF Core 3,0 ve `Order` vlastnictví nebo explicitně namapována na stejnou tabulku `OrderDetails` , byla při přidávání nové `Order`instance vždy vyžadována instance.


**Nové chování**

Počínaje 3,0 EF Core umožňuje přidat `Order` `OrderDetails` bez `OrderDetails` a mapování všech vlastností kromě primárního klíče na sloupce s možnou hodnotou null.
Při dotazování EF Core sady `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.

**Hrozeb**

Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale u navigace, na kterou se odkazuje, se neočekává `null` , že by aplikace měla být upravena tak, aby zpracovávala případy, kdy je `null`navigace. Pokud to není možné, měla by být do daného typu entity přidána požadovaná vlastnost, nebo alespoň jedna vlastnost by k ní měla mít`null` přiřazenou hodnotu bez hodnoty.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.

[Sledování problému #14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Vezměte v úvahu následující model:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
Pokud `OrderDetails` je před EF Core 3,0, ve `Order` vlastnictví nebo explicitně namapovaných na `OrderDetails` stejnou tabulku, pak aktualizace nebude aktualizovat `Version` hodnotu u klienta a další aktualizace selže.


**Nové chování**

Počínaje 3,0 EF Core rozšíří novou `Version` `Order` hodnotu, pokud je vlastní `OrderDetails`. V opačném případě je při ověřování modelu vyvolána výjimka.

**Proč**

Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.

**Hrozeb**

Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti. Je možné, že ho vytvoříte ve stínovém stavu:
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.

[Sledování problému #13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Vezměte v úvahu následující model:
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

Před EF Core 3,0 `ShippingAddress` bude vlastnost namapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.

**Nové chování**

Počínaje 3,0 se EF Core pro `ShippingAddress`nedá vytvořit jenom jeden sloupec.

**Proč**

Starý behavoir nebyl očekáván.

**Hrozeb**

Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.

[Sledování problému #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Vezměte v úvahu následující model:
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
Před EF Core 3,0 `CustomerId` se vlastnost používá pro cizí klíč podle konvence.
Nicméně pokud `Order` je vlastněný typ, pak by to vedlo `CustomerId` také k tomu, že primární klíč a to není obvykle očekávané.

**Nové chování**

Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.
Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.
Příklad:

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Proč**

Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.

**Hrozeb**

Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.

[Sledování problému #14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Pokud kontext otevře během EF Core 3,0 připojení uvnitř `TransactionScope`, zůstane připojení otevřené, zatímco aktuální `TransactionScope` aktivní je.

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

**Nové chování**

Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.

**Proč**

Tato změna umožňuje použít více kontextů současně `TransactionScope`. Nové chování se také shoduje s EF6.

**Hrozeb**

Pokud připojení potřebuje zůstat otevřeným explicitním voláním `OpenConnection()` , zajistí, že EF Core neuzavře předčasně:

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.

[Sledování problému #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.

**Nové chování**

Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.
Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.

**Proč**

Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.

**Hrozeb**

To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.
Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.

### <a name="backing-fields-are-used-by-default"></a>Ve výchozím nastavení se používají pole pro zálohování.

[Sledování problému #12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

Tato změna je představena ve verzi EF Core 3,0-Preview 2.

**Staré chování**

Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.
Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.

**Nové chování**

Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.
To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.

**Proč**

Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.

**Hrozeb**

Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti zapnuto `ModelBuilder`.
Příklad:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí

[Sledování problému #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.
To může způsobit, že se nesprávné pole použije v nejednoznačných případech.

**Nové chování**

Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.

**Proč**

Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.

**Hrozeb**

Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.
Například pomocí rozhraní Fluent API:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>Názvy vlastností pouze pro pole se musí shodovat s názvem pole.

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu CLR nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Nové chování**

Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Proč**

Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.

**Hrozeb**

Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.
V novější verzi EF Core 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti:

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.

[Sledování problému #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před EF Core 3,0, volání `AddDbContext` nebo `AddDbContextPool` by také registrovaly služby protokolování a ukládání do mezipaměti v paměti pomocí D. I přes volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Nové chování**

Počínaje EF Core 3,0 `AddDbContext` a `AddDbContextPool` nadále se nebudou registrovat tyto služby se vkládáním závislostí (di).

**Proč**

EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI. Pokud `ILoggerFactory` je však v kontejneru aplikace zaregistrován, bude nadále používána EF Core.

**Hrozeb**

Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext. entry teď provádí místní DetectChanges.

[Sledování problému #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.
Tím je zajištěno, že stav zpřístupněno v `EntityEntry` nástroji byl aktuální.

**Nové chování**

Počínaje EF Core 3,0 se nyní volání `DbContext.Entry` pokusí zjistit změny v dané entitě a všechny sledované hlavní entity, které se k ní vztahují.
To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.

Všimněte si, `ChangeTracker.AutoDetectChangesEnabled` že pokud je `false` nastaveno na, tak i toto místní zjišťování změn bude zakázáno.

Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`---stále zapříčiní plné `DetectChanges` ze všech sledovaných entit.

**Proč**

Tato změna byla provedena za účelem zlepšení výchozího výkonu použití `context.Entry`.

**Hrozeb**

Před `ChgangeTracker.DetectChanges()` voláním `Entry` zajistěte explicitní volání, aby bylo zajištěno chování před 3,0.

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.

[Sledování problému #14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před EF Core 3,0 `string` a `byte[]` vlastnosti klíče lze použít bez explicitního nastavení hodnoty, která není null.
V takovém případě se hodnota klíče vygeneruje na klientovi jako identifikátor GUID, který je serializovaný na bajty pro `byte[]`.

**Nové chování**

Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.

**Proč**

Tato změna byla provedena, protože obecně nejsou `string` užitečné hodnoty generované / `byte[]` klientem a výchozí chování způsobilo, že je obtížné vygenerovat hodnoty klíčů běžným způsobem.

**Hrozeb**

Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.
Například s rozhraním API Fluent:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Nebo s poznámkami k datům:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory je teď služba s vymezeným oborem.

[Sledování problému #14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před EF Core 3,0 `ILoggerFactory` byl zaregistrován jako služba typu singleton.

**Nové chování**

Počínaje EF Core 3,0 `ILoggerFactory` je nyní registrováno jako obor.

**Proč**

Tato změna byla provedena tak, aby povolovala přidružení protokolovacího `DbContext` nástroje s instancí, která umožňuje další funkce a odebírá některé případy patologického chování, jako je například rozpad interních poskytovatelů služeb.

**Hrozeb**

Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.
To není běžné.
V těchto případech bude většina věcí pořád fungovat, ale jakákoli služba typu Singleton, která byla v závislosti `ILoggerFactory` na tom, bude muset změnit tak, `ILoggerFactory` aby získala jiný způsob.

Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core modul pro sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak `ILoggerFactory` ho používáte, abychom mohli lépe pochopit, jak to v budoucnu Nerušit.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.

[Sledování problému #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Předtím, než EF Core 3,0, `DbContext` neexistuje žádný způsob, jak zjistit, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo ne.
Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.
V těchto případech by byl pokus o opožděné načtení no-op.

**Nové chování**

Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.
To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.
Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.
Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.

**Proč**

Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou `DbContext` instanci bylo chování konzistentní a správné.

**Hrozeb**

Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.

[Sledování problému #10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.

**Nové chování**

Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka. 

**Proč**

Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.

**Hrozeb**

Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.
Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.
Příklad:

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Nové chování pro HasOne/HasMany se volá s jedním řetězcem.

[Sledování problému #9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před EF Core 3,0, volání `HasOne` kódu nebo `HasMany` s jedním řetězcem bylo interpretováno matoucím způsobem.
Příklad:
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Kód vypadá jako v `Samurai` `Entrance` souvislosti s jiným typem entity pomocí navigační vlastnosti, která může být soukromá.

Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez navigační vlastnosti.

**Nové chování**

Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.

**Proč**

Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.

**Hrozeb**

Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.
To není běžné.
Předchozí chování lze získat pomocí explicitního předání `null` názvu vlastnosti navigace.
Příklad:

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask

[Sledování problému #15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Následující asynchronní metody dříve vrátily `Task<T>`:

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()`(a odvozování tříd)

**Nové chování**

Výše uvedené metody nyní vrací stejnou `ValueTask<T>` `T` hodnotu jako předtím.

**Proč**

Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.

**Hrozeb**

Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.
Složitější využití (například předání vráceného `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby bylo vráceno `ValueTask<T>` převedené na a `Task<T>` voláním `AsTask()` .
Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>Relační: anotace TypeMapping je nyní pouze TypeMapping

[Sledování problému #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

Tato změna je představena ve verzi EF Core 3,0-Preview 2.

**Staré chování**

Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".

**Nové chování**

Název poznámky pro mapování typů je nyní "TypeMapping".

**Proč**

Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.

**Hrozeb**

Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.
Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable na odvozeném typu vyvolá výjimku. 

[Sledování problému #11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před EF Core 3,0, `ToTable()` který je volán na odvozeném typu, by byl ignorován, protože pouze dědění mapování dědičnosti bylo typu TPH, který není platný. 

**Nové chování**

Počínaje EF Core 3,0 a přípravou pro přidání podpory TPT a TPC v novější verzi, která se `ToTable()` volá na odvozeném typu, teď vyvolá výjimku, aby se předešlo neočekávané změně mapování v budoucnu.

**Proč**

V současné době není platný pro mapování odvozeného typu na jinou tabulku.
Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.

**Hrozeb**

Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex nahrazeno HasIndex 

[Sledování problému #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před EF Core 3,0 jste `ForSqlServerHasIndex().ForSqlServerInclude()` získali způsob, jak nakonfigurovat sloupce používané pomocí `INCLUDE`nástroje.

**Nové chování**

Počínaje EF Core 3,0 se teď použití `Include` na indexu podporuje na relační úrovni.
Použijte `HasIndex().ForSqlServerInclude()`.

**Proč**

Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy `Include` na jednom místě pro všechny poskytovatele databáze.

**Hrozeb**

Použijte nové rozhraní API, jak vidíte výše.

### <a name="metadata-api-changes"></a>Změny rozhraní API pro metadata

[Sledování problému #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Nové chování**

Následující vlastnosti byly převedeny na rozšiřující metody:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Proč**

Tato změna zjednodušuje implementaci výše uvedených rozhraní.

**Hrozeb**

Použijte nové metody rozšíření.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>Změny rozhraní API pro konkrétního zprostředkovatele

[Sledování problému #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

Tato změna je představena ve verzi EF Core 3,0-Preview 6.

**Nové chování**

Metody rozšíření specifické pro poskytovatele budou shrnuty:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**Proč**

Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.

**Hrozeb**

Použijte nové metody rozšíření.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.

[Sledování problému #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

Tato změna je představena ve verzi EF Core 3,0-Preview 3.

**Staré chování**

Před EF Core 3,0 EF Core odeslat `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.

**Nové chování**

Počínaje EF Core 3,0 EF Core již neposílá `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.

**Proč**

Tato změna byla provedena, protože EF Core `SQLitePCLRaw.bundle_e_sqlite3` používá ve výchozím nastavení, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.

**Hrozeb**

Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.
V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3

**Staré chování**

Před EF Core 3,0 se používá `SQLitePCLRaw.bundle_green`EF Core.

**Nové chování**

Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.

**Proč**

Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.

**Hrozeb**

Pokud chcete použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` , aby používala `SQLitePCLRaw` jiný svazek.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Hodnoty GUID se teď ukládají jako TEXT na SQLite.

[Sledování problému #15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.

**Nové chování**

Hodnoty GUID jsou nyní uloženy jako TEXT.

**Proč**

Binární formát identifikátorů GUID není standardizovaný. Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.

**Hrozeb**

Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Hodnoty char se teď ukládají jako TEXT na SQLite.

[Sledování problému #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite. Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.

**Nové chování**

Hodnoty typu char jsou nyní uloženy jako TEXT.

**Proč**

Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.

**Hrozeb**

Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.

[Sledování problému #12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.

**Nové chování**

ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).

**Proč**

Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování. Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.

**Hrozeb**

Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář). Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.

ID migrace najdete v atributu migrace v souborech návrháře migrace.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Také je potřeba aktualizovat tabulku historie migrace.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging se odebral.

[Sledování problému #16400](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

Tato změna je představena ve verzi EF Core 3,0-Preview 6.

**Staré chování**

Před EF Core 3,0 `UseRowNumberForPaging` lze použít k vygenerování SQL pro stránkování, které je kompatibilní s SQL Server 2008.

**Nové chování**

Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server. 

**Proč**

Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.

**Hrozeb**

Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován. To znamená, že pokud to nemůžete udělat, [komentář k problému](https://github.com/aspnet/EntityFrameworkCore/issues/16400) s sledováním najdete v podrobnostech. Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.

[Sledování problému #16119](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

Tato změna je představena ve verzi EF Core 3,0-Preview 7.

**Staré chování**

`IDbContextOptionsExtension`obsažené metody pro poskytování metadat o rozšíření.

**Nové chování**

Tyto metody byly přesunuty na novou `DbContextOptionsExtensionInfo` abstraktní základní třídu, která je vrácena z nové `IDbContextOptionsExtension.Info` vlastnosti.

**Proč**

V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.
Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.

**Hrozeb**

Aktualizovat rozšíření tak, aby následovala nový vzor.
Příklady naleznete v mnoha implementacích `IDbContextOptionsExtension` pro různé druhy rozšíření ve zdrojovém kódu EF Core.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.

[Sledování problému #10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Mění**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`byla přejmenována `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na.

**Proč**

Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.

**Hrozeb**

Použijte nový název. (Všimněte si, že číslo ID události se nezměnilo.)

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>Vysvětlení rozhraní API pro názvy omezení cizího klíče

[Sledování problému #10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název". Příklad:

```C#
var constraintName = myForeignKey.Name;
```

**Nové chování**

Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení". Příklad:

```C#
var constraintName = myForeignKey.ConstraintName;
```

**Proč**

Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.

**Hrozeb**

Použijte nový název.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.

[Sledování problému #15997](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

Tato změna je představena ve verzi EF Core 3,0-Preview 7.

**Staré chování**

Před EF Core 3,0 byly tyto metody chráněné.

**Nové chování**

Počínaje EF Core 3,0 jsou tyto metody veřejné.

**Proč**

Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná. To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.

**Hrozeb**

Změňte přístupnost všech přepsání.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.

[Sledování problému #11506](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

Tato změna je představena ve verzi EF Core 3,0-Preview 4.

**Staré chování**

Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.

**Nové chování**

Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency. To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již nemůžete ve výchozím nastavení odkazovat na své sestavení.

**Proč**

Tento balíček se má použít jenom v době návrhu. Nasazené aplikace by neměli na ni odkazovat. Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.

**Hrozeb**

Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky aktualizovat PackageReference v projektu. Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL. Raw aktualizováno na verzi 2.0.0

[Sledování problému #14824](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

Tato změna je představena ve verzi EF Core 3,0-Preview 7.

**Staré chování**

Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.

**Nové chování**

Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.

**Proč**

2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0. Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.

**Hrozeb**

SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny. Podrobnosti najdete v poznámkách k [verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>NetTopologySuite aktualizace na verzi 2.0.0

[Sledování problému #14825](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

Tato změna je představena ve verzi EF Core 3,0-Preview 7.

**Staré chování**

Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.

**Nové chování**

Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.

**Proč**

2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.

**Hrozeb**

NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny. Podrobnosti najdete v poznámkách k [verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe. 

[Sledování problému #13573](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

Tato změna je představena ve verzi EF Core 3,0-Preview 6.

**Staré chování**

Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah. Příklad:

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

**Nové chování**

Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.

**Proč**

Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.

**Hrozeb**

Použijte úplnou konfiguraci relace. Příklad:

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```
