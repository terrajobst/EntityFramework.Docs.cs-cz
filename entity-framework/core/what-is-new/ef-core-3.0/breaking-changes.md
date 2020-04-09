---
title: Nejnovější změny v EF Core 3.0 - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417459"
---
# <a name="breaking-changes-included-in-ef-core-30"></a>Nejnovější změny zahrnuté v EF Core 3.0

Následující změny rozhraní API a chování mají potenciál přerušit existující aplikace při jejich upgradu na 3.0.0.
Změny, které očekáváme, že ovlivní pouze zprostředkovatele databáze, jsou dokumentovány v rámci [změn zprostředkovatele](xref:core/providers/provider-log).

## <a name="summary"></a>Souhrn

| **Narušující změny**                                                                                               | **Dopad** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [Dotazy LINQ již nejsou vyhodnocovány na straně klienta.](#linq-queries-are-no-longer-evaluated-on-the-client)         | Vysoká       |
| [Cíle EF Core 3.0 .NET Standard 2.1 spíše než .NET Standard 2.0](#netstandard21) | Vysoká      |
| [Nástroj příkazového řádku EF Core, dotnet ef, již není součástí sady .NET Core SDK](#dotnet-ef) | Vysoká      |
| [DetectChanges respektuje hodnoty klíče generované úložištěm](#dc) | Vysoká      |
| [FromSql, ExecuteSql a ExecuteSqlAsync byly přejmenovány](#fromsql) | Vysoká      |
| [Typy dotazů jsou konsolidovány s typy entit.](#qt) | Vysoká      |
| [Core rámce účetních údajů již není součástí sdíleného rámce ASP.NET Core](#no-longer) | Střednědobé používání      |
| [Kaskádové odstranění se nyní ve výchozím nastavení děje okamžitě](#cascade) | Střednědobé používání      |
| [Eager načítání souvisejících entit se nyní děje v jednom dotazu](#eager-loading-single-query) | Střednědobé používání      |
| [DeleteBehavior.Restrict má čistší sémantiku.](#deletebehavior) | Střednědobé používání      |
| [Konfigurační rozhraní API pro vztahy vlastněných typů bylo změněno.](#config) | Střednědobé používání      |
| [Každá vlastnost používá nezávislé generování celého klíče v paměti](#each) | Střednědobé používání      |
| [Dotazy bez sledování již neprovádějí rozlišení identity](#notrackingresolution) | Střednědobé používání      |
| [Změny rozhraní API metadat](#metadata-api-changes) | Střednědobé používání      |
| [Změny rozhraní METADAT SPECIFICKÉ Pro zprostředkovatele](#provider) | Střednědobé používání      |
| [UseRowNumberForPaging byl odebrán.](#urn) | Střednědobé používání      |
| [FromSql metodu při použití s uloženou proceduru nelze skládat](#fromsqlsproc) | Střednědobé používání      |
| [FromSql metody lze zadat pouze na kořeny dotazu](#fromsql) | Nízká      |
| [~~Spuštění dotazu je zaznamenáno na úrovni ladění.~~ Vráceny](#qe) | Nízká      |
| [Hodnoty dočasného klíče již nejsou nastaveny na instance entit.](#tkv) | Nízká      |
| [Závislé entity sdílející tabulku s objektem zabezpečení jsou nyní volitelné.](#de) | Nízká      |
| [Všechny entity sdílející tabulku se sloupcem tokenu souběžnosti musí mapovat na vlastnost](#aes) | Nízká      |
| [Vlastněné entity nelze dotazovat bez vlastníka pomocí sledovacího dotazu.](#owned-query) | Nízká      |
| [Zděděné vlastnosti z nemapovaných typů jsou nyní mapovány na jeden sloupec pro všechny odvozené typy.](#ip) | Nízká      |
| [Konvence vlastností cizího klíče již neodpovídá stejnému názvu jako hlavní vlastnost.](#fkp) | Nízká      |
| [Připojení databáze je nyní uzavřena, pokud již není použit a před dokončením TransactionScope](#dbc) | Nízká      |
| [Ve výchozím nastavení se používají záložní pole.](#backing-fields-are-used-by-default) | Nízká      |
| [Vyvolání, pokud je nalezeno více kompatibilních doprovodových polí](#throw-if-multiple-compatible-backing-fields-are-found) | Nízká      |
| [Názvy vlastností pouze pro pole by měly odpovídat názvu pole.](#field-only-property-names-should-match-the-field-name) | Nízká      |
| [AddDbContext/AddDbContextPool již nevolá addlogging a addmemorycache](#adddbc) | Nízká      |
| [AddEntityFramework* přidá IMemoryCache s limitem velikosti](#addentityframework-adds-imemorycache-with-a-size-limit) | Nízká      |
| [DbContext.Entry nyní provádí místní DetectChanges](#dbe) | Nízká      |
| [Klíče pole řetězce a bajtů nejsou ve výchozím nastavení generovány klientem.](#string-and-byte-array-keys-are-not-client-generated-by-default) | Nízká      |
| [ILoggerFactory je nyní oborová služba](#ilf) | Nízká      |
| [Opožděné načítání proxy servery již předpokládat, navigační vlastnosti jsou plně načteny](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | Nízká      |
| [Nadměrné vytváření interních poskytovatelů služeb je nyní ve výchozím nastavení chybou](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | Nízká      |
| [Nové chování pro HasOne/HasMany volána s jedním řetězcem](#nbh) | Nízká      |
| [Návratový typ pro několik asynchronních metod byl změněn z Task na ValueTask.](#rtnt) | Nízká      |
| [Relační a textová poznámka je nyní pouze typemapping](#rtt) | Nízká      |
| [ToTable na odvozený typ vyvolá výjimku](#totable-on-a-derived-type-throws-an-exception) | Nízká      |
| [EF Core již neodesílá pragma pro vynucení SQLite FK](#pragma) | Nízká      |
| [Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3](#sqlite3) | Nízká      |
| [Hodnoty guid jsou nyní uloženy jako TEXT na SQLite](#guid) | Nízká      |
| [Hodnoty Char jsou nyní uloženy jako TEXT na SQLite](#char) | Nízká      |
| [ID migrace jsou nyní generovány pomocí kalendáře invariantní jazykové verze](#migid) | Nízká      |
| [Informace/metadata rozšíření byly odebrány z rozšíření IDbContextOptionsExtension.](#xinfo) | Nízká      |
| [LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.](#lqpe) | Nízká      |
| [Objasnit rozhraní API pro názvy omezení cizího klíče](#clarify) | Nízká      |
| [IRelationalDatabaseCreator.HasTables/HasTablesAsync byly zveřejněny](#irdc2) | Nízká      |
| [Microsoft.EntityFrameworkCore.Design je nyní balíček DevelopmentDependency](#dip) | Nízká      |
| [SQLitePCL.raw aktualizován na verzi 2.0.0](#SQLitePCL) | Nízká      |
| [NetTopologySuite aktualizován na verzi 2.0.0](#NetTopologySuite) | Nízká      |
| [Místo klienta System.Data.SqlClient se používá místo klienta System.Data.SqlClient](#SqlClient) | Nízká      |
| [Musí být nakonfigurováno více nejednoznačných vztahů, které samy odkazují na sebe.](#mersa) | Nízká      |
| [DbFunction.Schema je null nebo prázdný řetězec nakonfiguruje, aby byl ve výchozím schématu modelu](#udf-empty-string) | Nízká      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Dotazy LINQ již nejsou vyhodnocovány na straně klienta.

[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Také zobrazit #12795 problému](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

**Staré chování**

Před 3.0, když EF Core nelze převést výraz, který byl součástí dotazu buď SQL nebo parametr, automaticky vyhodnocenvýrazv klientovi.
Ve výchozím nastavení klienthodnocení potenciálně nákladné výrazy pouze spustila upozornění.

**Nové chování**

Počínaje 3.0 EF Core umožňuje pouze výrazy v projekci nejvyšší úrovně (poslední `Select()` volání v dotazu) vyhodnoceny na straně klienta.
Pokud výrazy v jakékoli jiné části dotazu nelze převést na SQL nebo parametr, je vyvolána výjimka.

**Proč**

Automatické vyhodnocení dotazů klienta umožňuje mnoho dotazů, které mají být provedeny i v případě, že důležité části z nich nelze přeložit.
Toto chování může mít za následek neočekávané a potenciálně škodlivé chování, které může být zřejmé pouze v produkčním prostředí.
Například podmínka ve `Where()` volání, které nelze přeložit, může způsobit přenos všech řádků z tabulky z databázového serveru a filtru, který má být použit na klienta.
Tato situace může snadno přejít nezjištěný, pokud tabulka obsahuje pouze několik řádků ve vývoji, ale tvrdě přístupů při aplikaci přesune do produkčního prostředí, kde tabulka může obsahovat miliony řádků.
Upozornění na vyhodnocení klienta se také ukázala jako příliš snadno ignorovat během vývoje.

Kromě toho automatické vyhodnocení klienta může vést k problémům, ve kterých zlepšení překladu dotazů pro konkrétní výrazy způsobily nechtěné změny mezi verzemi.

**Omezení rizik**

Pokud dotaz nelze plně přeložit, přepište dotaz ve formuláři, který lze přeložit, `AsEnumerable()` `ToList()`nebo použijte , nebo podobně jako explicitně převést data zpět do klienta, kde je pak možné dále zpracovat pomocí LINQ-to-Objects.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>Cíle EF Core 3.0 .NET Standard 2.1 spíše než .NET Standard 2.0

[Sledování #15498 problému](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> EF Core 3.1 opět cílí na standard .NET Standard 2.0. To přináší zpět podporu pro rozhraní .NET Framework.

**Staré chování**

Před 3.0 EF Core cílené .NET Standard 2.0 a bude fungovat na všech platformách, které podporují tento standard, včetně rozhraní .NET Framework.

**Nové chování**

Počínaje 3.0, EF Core cíle .NET Standard 2.1 a bude spuštěna na všech platformách, které podporují tento standard. To nezahrnuje rozhraní .NET Framework.

**Proč**

Toto je součástí strategického rozhodnutí napříč technologiemi .NET zaměřit energii na .NET Core a další moderní .NET platformy, jako je například Xamarin.

**Omezení rizik**

Použijte EF Core 3.1.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Core rámce účetních údajů již není součástí sdíleného rámce ASP.NET Core

[Sledování oznámení o problému#325](https://github.com/aspnet/Announcements/issues/325)

**Staré chování**

Před ASP.NET Core 3.0, když jste `Microsoft.AspNetCore.App` `Microsoft.AspNetCore.All`přidali odkaz na balíček nebo , by to zahrnovalo EF Core a některé zprostředkovatele dat EF Core, jako je poskytovatel serveru SQL Server.

**Nové chování**

Počínaje 3.0 ASP.NET základní sdílené rozhraní nezahrnuje EF Core nebo zprostředkovatele dat EF Core.

**Proč**

Před touto změnou, získání EF Core vyžaduje různé kroky v závislosti na tom, zda aplikace cílené ASP.NET Core a SQL Server nebo ne. Také inovace ASP.NET Core vynutila upgrade EF Core a zprostředkovatele SQL Server, což není vždy žádoucí.

S touto změnou je prostředí získávání EF Core stejné napříč všemi poskytovateli, podporovanými implementacemi rozhraní .NET a typy aplikací.
Vývojáři teď taky můžou přesně řídit, kdy jsou upgradováni zprostředkovatelé dat EF Core a EF Core.

**Omezení rizik**

Chcete-li použít EF Core v ASP.NET core 3.0 aplikace nebo jiné podporované aplikace, explicitně přidejte odkaz na balíček zprostředkovatele databáze EF Core, který bude vaše aplikace používat.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>Nástroj příkazového řádku EF Core, dotnet ef, již není součástí sady .NET Core SDK

[#14016 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

**Staré chování**

Před 3.0 `dotnet ef` byl nástroj zahrnut do sady .NET Core SDK a byl snadno dostupný z příkazového řádku z libovolného projektu bez nutnosti dalších kroků. 

**Nové chování**

Počínaje 3.0 sada .NET SDK neobsahuje `dotnet ef` nástroj, takže před použitím jej musíte explicitně nainstalovat jako místní nebo globální nástroj. 

**Proč**

Tato změna nám umožňuje `dotnet ef` distribuovat a aktualizovat jako běžný nástroj .NET CLI na NuGet, konzistentní se skutečností, že EF Core 3.0 je také vždy distribuován jako balíček NuGet.

**Omezení rizik**

Chcete-li spravovat migrace nebo lešení `DbContext` `dotnet-ef` a , nainstalujte jako globální nástroj:

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

Můžete také získat místní nástroj při obnovení závislostí projektu, který deklaruje jako závislost nástroje pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>FromSql, ExecuteSql a ExecuteSqlAsync byly přejmenovány

[#10996 sledování problému](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

**Staré chování**

Před EF Core 3.0 byly tyto názvy metod přetíženy, aby pracovaly s normálním řetězcem nebo řetězcem, který by měl být interpolován do SQL a parametrů.

**Nové chování**

Počínaje EF Core 3.0, `ExecuteSqlRaw`použijte `ExecuteSqlRawAsync` `FromSqlRaw`, a vytvořit parametrizovaný dotaz, kde jsou parametry předány odděleně od řetězce dotazu.
Příklad:

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Použijte `FromSqlInterpolated` `ExecuteSqlInterpolated`, `ExecuteSqlInterpolatedAsync` a vytvořte parametrizovaný dotaz, kde jsou parametry předány jako součást řetězce interpolovaného dotazu.
Příklad:

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Všimněte si, že oba výše uvedené dotazy vytvoří stejné parametrizované SQL se stejnými parametry SQL.

**Proč**

Přetížení metody, jako je tento, usnadňují náhodné volání metody raw string, když záměrem bylo volání interpolované metody řetězce a naopak.
To může mít za následek dotazy nejsou parametrizovány, když by měly být.

**Omezení rizik**

Přepněte, abyste použili nové názvy metod.

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a>FromSql metodu při použití s uloženou proceduru nelze skládat

[Sledování #15392 problému](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

**Staré chování**

Před EF Core 3.0, FromSql metoda se pokusil zjistit, pokud předané SQL lze skládat na. Provedla vyhodnocení klienta, když sql byl non-composable jako uložená procedura. Následující dotaz fungoval spuštěním uložené procedury na serveru a provedením FirstOrDefault na straně klienta.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

**Nové chování**

Počínaje EF Core 3.0 EF Core se nepokusí analyzovat SQL. Takže pokud jste skládání po FromSqlRaw/FromSqlInterpolated, pak EF Core bude skládat SQL tím, že způsobí dílčí dotaz. Takže pokud používáte uloženou proceduru s kompozicí, pak získáte výjimku pro neplatnou syntaxi SQL.

**Proč**

EF Core 3.0 nepodporuje automatické hodnocení klienta, protože byl náchylný k chybám, jak je vysvětleno [zde](#linq-queries-are-no-longer-evaluated-on-the-client).

**Omezení rizik**

Pokud používáte uloženou proceduru v FromSqlRaw/FromSqlInterpolated, víte, že ji nelze skládat, takže můžete přidat __AsEnumerable/AsAsyncEnumerable__ hned po volání metody FromSql, abyste se vyhnuli jakékoli kompozici na straně serveru.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>FromSql metody lze zadat pouze na kořeny dotazu

[#15704 sledování problému](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

**Staré chování**

Před EF Core `FromSql` 3.0, metoda může být zadán kdekoli v dotazu.

**Nové chování**

Počínaje EF Core 3.0, `FromSqlRaw` `FromSqlInterpolated` nové a `FromSql`metody (které nahrazují ) lze zadat pouze na `DbSet<>`kořeny dotazu, tj. Pokus o jejich zadání kdekoli jinde bude mít za následek chybu kompilace.

**Proč**

Zadání `FromSql` kdekoli jinde než `DbSet` na neměl žádný význam nebo přidanou hodnotu a může způsobit nejednoznačnost v určitých scénářích.

**Omezení rizik**

`FromSql`by měla být přesunuta tak, `DbSet` aby byla přímo na základě toho, na které se vztahují.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>Dotazy bez sledování již neprovádějí rozlišení identity

[Sledování #13518 problému](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

**Staré chování**

Před EF Core 3.0 by se stejná instance entity použila pro každý výskyt entity s daným typem a ID. To odpovídá chování sledování dotazů. Například tento dotaz:

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
vrátí stejnou `Category` instanci `Product` pro každou, která je přidružena k dané kategorii.

**Nové chování**

Počínaje EF Core 3.0, budou vytvořeny různé instance entity, když se na různých místech vráceného grafu objeví entita s daným typem a ID. Například výše uvedený dotaz nyní `Category` vrátí novou `Product` instanci pro každou z nich i v případě, že dva produkty jsou přidruženy ke stejné kategorii.

**Proč**

Rozlišení identity (to znamená určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a nároky na paměť. To obvykle běží v rozporu s tím, proč žádné sledování dotazy jsou používány na prvním místě. Také zatímco překlad identity může být někdy užitečné, není potřeba, pokud entity mají být serializovány a odeslány klientovi, což je běžné pro dotazy bez sledování.

**Omezení rizik**

Pokud je vyžadováno rozlišení identity, použijte sledovací dotaz.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~Spuštění dotazu je zaznamenáno na úrovni ladění.~~ Vráceny

[#14523 sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Tuto změnu jsme vrátili zpět, protože nová konfigurace v EF Core 3.0 umožňuje úroveň protokolu pro každou událost, která má být určena aplikací. Chcete-li například přepnout `Debug`protokolování sql `OnConfiguring` na `AddDbContext`, explicitně nakonfigurujte úroveň v nebo :
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Hodnoty dočasného klíče již nejsou nastaveny na instance entit.

[#12378 sledování problému](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

**Staré chování**

Před EF Core 3.0 dočasné hodnoty byly přiřazeny ke všem vlastnostem klíče, které by později mít reálnou hodnotu generovanou databází.
Obvykle tyto dočasné hodnoty byly velké záporná čísla.

**Nové chování**

Počínaje 3.0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá samotnou vlastnost klíče beze změny.

**Proč**

Tato změna byla provedena, aby se zabránilo chybně dočasným hodnotám klíče, aby `DbContext` se chybně staly `DbContext` trvalými, když je entita, která byla dříve sledována nějakou instancí, přesunuta do jiné instance. 

**Omezení rizik**

Aplikace, které přiřazují hodnoty primárního klíče cizím klíčům k vytvoření přidružení mezi entitami, `Added` mohou záviset na starém chování, pokud jsou primární klíče generovány v úložišti a patří k entitám ve státě.
Tomu se lze vyhnout:
* Nepoužíváte klíče generované úložištěm.
* Nastavení navigačních vlastností na vytvoření relací namísto nastavení hodnot cizího klíče.
* Získejte skutečné hodnoty dočasného klíče z informací o sledování entity.
Například `context.Entry(blog).Property(e => e.Id).CurrentValue` vrátí dočasnou hodnotu, i když `blog.Id` sama nebyla nastavena.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges respektuje hodnoty klíče generované úložištěm

[#14616 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

**Staré chování**

Před EF Core 3.0, nesledované `DetectChanges` entity nalezeny by `Added` být sledovány ve stavu `SaveChanges` a vloženy jako nový řádek, když je volána.

**Nové chování**

Počínaje EF Core 3.0, pokud entita používá generované hodnoty klíče a je nastavena některá hodnota klíče, bude entita sledována ve `Modified` stavu.
To znamená, že se předpokládá, že existuje řádek `SaveChanges` pro entitu a bude aktualizován, když je volána.
Pokud není nastavena hodnota klíče nebo pokud typ entity nepoužívá generované klíče, bude nová entita stále sledována jako `Added` v předchozích verzích.

**Proč**

Tato změna byla provedena, aby bylo snazší a konzistentnější pracovat s grafy odpojených entit při použití klíčů generovaných úložištěm.

**Omezení rizik**

Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurován pro použití generovaných klíčů, ale hodnoty klíčů jsou explicitně nastaveny pro nové instance.
Oprava je explicitně nakonfigurovat vlastnosti klíče nepoužívat generované hodnoty.
Například s fluent API:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Nebo s poznámkami o datech:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>Kaskádové odstranění se nyní ve výchozím nastavení děje okamžitě

[Sledování #10114 problému](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

**Staré chování**

Před 3.0 EF Core použít kaskádové akce (odstranění závislých entit při odstranění požadované ho objektu zabezpečení nebo při přerušeném vztahu k požadované jistiny) nedošlo, dokud SaveChanges byla volána.

**Nové chování**

Počínaje 3.0, EF Core použije kaskádové akce, jakmile je zjištěna spouštěcí podmínka.
Například volání `context.Remove()` k odstranění hlavní entity bude mít za následek všechny `Deleted` sledované související požadované závislé osoby také nastavena na okamžitě.

**Proč**

Tato změna byla provedena ke zlepšení prostředí pro data vazby a auditování scénáře, kde je důležité pochopit, které entity budou odstraněny _před_ `SaveChanges` nazývá.

**Omezení rizik**

Předchozí chování lze obnovit pomocí `context.ChangeTracker`nastavení na .
Příklad:

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a>Eager načítání souvisejících entit se nyní děje v jednom dotazu

[Sledování #18022 problému](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

**Staré chování**

Před 3.0 dychtivě načítání kolekce `Include` navigace prostřednictvím operátorů způsobil více dotazů, které mají být generovány na relační databáze, jeden pro každý typ související entity.

**Nové chování**

Počínaje 3.0, EF Core generuje jeden dotaz s JOINs na relační databáze.

**Proč**

Vydávání více dotazů k implementaci jednoho dotazu LINQ způsobil řadu problémů, včetně negativní výkon jako více databázových roundtrips byly nezbytné a problémy s koherence dat jako každý dotaz mohl sledovat jiný stav databáze.

**Omezení rizik**

I když technicky to není narušující změny, může mít značný vliv na výkon `Include` aplikace, pokud jeden dotaz obsahuje velký počet operátorna navigační služby kolekce. Další informace a přepisování dotazů efektivněji naleznete v [tomto komentáři.](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085)

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior.Restrict má čistší sémantiku.

[Sledování #12661 problému](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

**Staré chování**

Před 3.0, `DeleteBehavior.Restrict` vytvořil cizí klíče `Restrict` v databázi s sémantiku, ale také změnil vnitřní opravu v non-zřejmým způsobem.

**Nové chování**

Počínaje 3.0 `DeleteBehavior.Restrict` zajišťuje, že cizí klíče `Restrict` jsou vytvořeny sémantikou -- to znamená, že žádné kaskády; vyvolat porušení omezení – bez dopadu také EF interní opravy.

**Proč**

Tato změna byla provedena s `DeleteBehavior` cílem zlepšit zkušenosti pro použití intuitivním způsobem, bez neočekávaných vedlejších účinků.

**Omezení rizik**

Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`aplikace .

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>Typy dotazů jsou konsolidovány s typy entit.

[Sledování #14194 problému](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

**Staré chování**

Před EF Core 3.0, [typy dotazů](xref:core/modeling/keyless-entity-types) byly prostředkem pro dotazování dat, která nedefinuje primární klíč strukturovaným způsobem.
To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (spíše ze zobrazení, ale případně z tabulky), zatímco normální typ entity byl použit, když byl k dispozici klíč (pravděpodobně z tabulky, ale případně ze zobrazení).

**Nové chování**

Typ dotazu se nyní stane pouze typem entity bez primárního klíče.
Typy bezklíčových entit mají stejné funkce jako typy dotazů v předchozích verzích.

**Proč**

Tato změna byla provedena snížit nejasnosti kolem účelu typů dotazů.
Konkrétně jsou bezklíčové typy entit a jsou ze své podstaty jen pro čtení z tohoto důvodu, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.
Podobně jsou často mapovány na zobrazení, ale je to jen proto, že zobrazení často nedefinují klíče.

**Omezení rizik**

Následující části rozhraní API jsou nyní zastaralé:
* **`ModelBuilder.Query<>()`**- `ModelBuilder.Entity<>().HasNoKey()` Místo toho je třeba volat označit typ entity jako bez klíčů.
To by stále není nakonfigurován podle konvence, aby se zabránilo chybné konfiguraci, když je očekáván primární klíč, ale neodpovídá konvenci.
* **`DbQuery<>`**- `DbSet<>` Místo toho by měl být použit.
* **`DbContext.Query<>()`**- `DbContext.Set<>()` Místo toho by měl být použit.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>Konfigurační rozhraní API pro vztahy vlastněných typů bylo změněno.

[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

**Staré chování**

Před EF Core 3.0 konfigurace vztahu vlastněné `OwnsOne` byla `OwnsMany` provedena přímo po nebo volání. 

**Nové chování**

Počínaje EF Core 3.0, je nyní fluent ROZHRANÍ API pro `WithOwner()`konfiguraci navigační vlastnost vlastníka pomocí .
Příklad:

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

Konfigurace související se vztahem mezi vlastníkem a vlastněným by nyní měla být zřetězena podobně jako `WithOwner()` ostatní vztahy.
Zatímco konfigurace pro vlastněný typ sám by `OwnsOne()/OwnsMany()`stále být zřetězené po .
Příklad:

```csharp
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

Navíc volání `Entity()` `HasOne()`, `Set()` nebo s cílem typu vlastněného nyní vyvolá výjimku.

**Proč**

Tato změna byla provedena k vytvoření čistší oddělení mezi konfigurací vlastněného typu samotného a _vztah k_ vlastněného typu.
To zase odstraňuje nejednoznačnost a `HasForeignKey`zmatek kolem metod, jako je .

**Omezení rizik**

Změňte konfiguraci vztahů vlastněných typů tak, aby používaly nový povrch rozhraní API, jak je znázorněno ve výše uvedeném příkladu.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Závislé entity sdílející tabulku s objektem zabezpečení jsou nyní volitelné.

[Sledování #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

**Staré chování**

Zvažte následující model:
```csharp
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
Před EF Core `OrderDetails` 3.0, `Order` pokud je vlastněna nebo explicitně mapována na stejnou tabulku, byla `OrderDetails` instance vždy vyžadována při přidávání nového `Order`.


**Nové chování**

Počínaje 3.0, EF Core umožňuje `Order` přidat `OrderDetails` bez a `OrderDetails` mapuje všechny vlastnosti s výjimkou primárního klíče na sloupce s možnou hodnotou null.
Při dotazování EF `OrderDetails` `null` Core sady, pokud některý z jeho požadovaných vlastností nemá hodnotu nebo pokud `null`nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou .

**Omezení rizik**

Pokud váš model má tabulka sdílení závislé se všemi volitelnými sloupci, ale navigace ukazující na něj se neočekává, že bude `null` pak aplikace by měla být upravena tak, aby zpracování případů, kdy navigace je `null`. Pokud to není možné, požadovaná vlastnost by měla být přidána k`null` typu entity nebo alespoň jedna vlastnost by měla mít non-hodnota přiřazena k němu.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Všechny entity sdílející tabulku se sloupcem tokenu souběžnosti musí mapovat na vlastnost

[#14154 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

**Staré chování**

Zvažte následující model:
```csharp
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
Před EF Core `OrderDetails` 3.0, `Order` pokud je vlastněna nebo explicitně `OrderDetails` mapována na stejnou tabulku, pak aktualizace prostě nebude aktualizovat `Version` hodnotu na straně klienta a další aktualizace se nezdaří.


**Nové chování**

Počínaje 3.0, EF Core rozšíří `Version` novou `Order` hodnotu, `OrderDetails`pokud vlastní . V opačném případě je vyvolána výjimka během ověřování modelu.

**Proč**

Tato změna byla provedena, aby se zabránilo zatuchlé hodnota tokenu souběžnosti při aktualizaci pouze jedné entity mapované na stejnou tabulku.

**Omezení rizik**

Všechny entity sdílející tabulku musí obsahovat vlastnost, která je mapována na sloupec tokenu souběžnosti. Je možné, že vytvořit jeden ve stínovém stavu:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a>Vlastněné entity nelze dotazovat bez vlastníka pomocí sledovacího dotazu.

[Sledování #18876 problému](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

**Staré chování**

Před EF Core 3.0, vlastněné entity mohou být dotazovánjako jakékoli jiné navigace.

```csharp
context.People.Select(p => p.Address);
```

**Nové chování**

Počínaje 3.0 EF Core vyvolá, pokud dotaz sledování projekty vlastněné entity bez vlastníka.

**Proč**

S vlastněnými entitami nelze manipulovat bez vlastníka, takže v převážné většině případů je dotazování tímto způsobem chybou.

**Omezení rizik**

Pokud by měla být vlastněná entita sledována, aby byla později změněna, měl by být do dotazu zahrnut vlastník.

V opačném `AsNoTracking()` případě přidejte volání:

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Zděděné vlastnosti z nemapovaných typů jsou nyní mapovány na jeden sloupec pro všechny odvozené typy.

[Sledování #13998 problému](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

**Staré chování**

Zvažte následující model:
```csharp
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

Před EF Core 3.0 by `ShippingAddress` vlastnost byla mapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.

**Nové chování**

Počínaje 3.0, EF Core vytvoří `ShippingAddress`pouze jeden sloupec pro .

**Proč**

Starý behavoir byl nečekaný.

**Omezení rizik**

Vlastnost může být stále explicitně mapována na samostatný sloupec na odvozených typech:

```csharp
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Konvence vlastností cizího klíče již neodpovídá stejnému názvu jako hlavní vlastnost.

[#13274 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

**Staré chování**

Zvažte následující model:
```csharp
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
Před EF Core 3.0, `CustomerId` vlastnost by být použity pro cizí klíč podle konvence.
Pokud `Order` je však vlastněný typ, pak `CustomerId` by to také primární klíč, a to není obvykle očekávání.

**Nové chování**

Počínaje 3.0 EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako hlavní vlastnost.
Název hlavního typu spojený s názvem hlavní vlastnosti a název navigace spojený se vzory názvů hlavní vlastnosti jsou stále spárovány.
Příklad:

```csharp
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

```csharp
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

Tato změna byla provedena, aby se zabránilo chybné definování vlastnosti primárního klíče na vlastněném typu.

**Omezení rizik**

Pokud vlastnost byla určena jako cizí klíč a proto součástí primárního klíče, pak explicitně nakonfigurovat jako takové.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>Připojení databáze je nyní uzavřena, pokud již není použit a před dokončením TransactionScope

[Sledování #14218 problému](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

**Staré chování**

Před EF Core 3.0, pokud kontext `TransactionScope`otevře připojení uvnitř , `TransactionScope` připojení zůstane otevřené, zatímco aktuální je aktivní.

```csharp
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

Počínaje 3.0 EF Core ukončí připojení, jakmile je to hotovo.

**Proč**

Tato změna umožňuje použít více kontextů ve stejném `TransactionScope`. Nové chování také odpovídá EF6.

**Omezení rizik**

Pokud připojení musí zůstat otevřené `OpenConnection()` explicitní volání zajistí, že EF Core nezavře předčasně:

```csharp
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Každá vlastnost používá nezávislé generování celého klíče v paměti

[#6872 sledování problému](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

**Staré chování**

Před EF Core 3.0 jeden generátor sdílených hodnot byl použit pro všechny vlastnosti klíče v paměti celé číslo.

**Nové chování**

Počínaje EF Core 3.0, každá vlastnost celočíselný klíč získá vlastní generátor hodnot při použití databáze v paměti.
Také pokud je databáze odstraněna, generování klíčů se resetuje pro všechny tabulky.

**Proč**

Tato změna byla provedena za účelem těsnějšího zarovnání generování klíče v paměti s generováním skutečného klíče databáze a zlepšením možnosti izolovat testy od sebe navzájem při použití databáze v paměti.

**Omezení rizik**

To může přerušit aplikaci, která je závislá na konkrétní hodnoty klíče v paměti, které mají být nastaveny.
Zvažte místo toho není spoléhat na konkrétní hodnoty klíče nebo aktualizace tak, aby odpovídaly nové chování.

### <a name="backing-fields-are-used-by-default"></a>Ve výchozím nastavení se používají záložní pole.

[Sledování #12430 problému](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

**Staré chování**

Před 3.0, i v případě, že bylo známo záložní pole pro vlastnost, EF Core by stále ve výchozím nastavení číst a zapisovat hodnotu vlastnosti pomocí metody getter vlastnost a setter.
Výjimkou bylo spuštění dotazu, kde by bylo záložní pole nastaveno přímo, pokud je známo.

**Nové chování**

Počínaje EF Core 3.0, pokud je známé záložní pole pro vlastnost, pak EF Core bude vždy číst a zapisovat tuto vlastnost pomocí záložního pole.
To může způsobit přerušení aplikace, pokud aplikace spoléhá na další chování kódované do metody getter nebo setter.

**Proč**

Tato změna byla provedena, aby se zabránilo EF Core z chybně spouštění obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnujících entity.

**Omezení rizik**

Chování pre-3.0 lze obnovit prostřednictvím konfigurace režimu `ModelBuilder`přístupu vlastnosti na .
Příklad:

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Vyvolání, pokud je nalezeno více kompatibilních doprovodových polí

[#12523 sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

**Staré chování**

Před EF Core 3.0, pokud více polí uzavřeno pravidla pro hledání záložní pole vlastnosti, pak jedno pole by bylo vybráno na základě některé pořadí priorit.
To může způsobit nesprávné pole, které mají být použity v nejednoznačných případech.

**Nové chování**

Počínaje EF Core 3.0, pokud více polí jsou spárovány se stejnou vlastností, pak je vyvolána výjimka.

**Proč**

Tato změna byla provedena, aby se zabránilo tichépoužití jednoho pole přes jiné, když pouze jeden může být správné.

**Omezení rizik**

Vlastnosti s nejednoznačnými opěrnými poli musí mít zadané pole, které má být explicitně určeno.
Například pomocí fluent API:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>Názvy vlastností pouze pro pole by měly odpovídat názvu pole.

**Staré chování**

Před EF Core 3.0, vlastnost může být určena řetězcovou hodnotou a pokud žádná vlastnost s tímto názvem byla nalezena na typu .NET pak EF Core by se pokusit porovnat s polem pomocí pravidel konvence.

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Nové chování**

Počínaje EF Core 3.0, vlastnost pouze pole musí přesně odpovídat názvu pole.

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Proč**

Tato změna byla provedena, aby se zabránilo použití stejné pole pro dvě vlastnosti pojmenované podobně, také umožňuje odpovídající pravidla pro vlastnosti pouze pole stejné jako pro vlastnosti mapované na clr vlastnosti.

**Omezení rizik**

Vlastnosti pouze pro pole musí být pojmenovány stejně jako pole, na které jsou mapovány.
V budoucí verzi EF Core po 3.0 plánujeme znovu povolit explicitně konfiguraci názvu pole, který se liší od názvu vlastnosti (viz problém [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool již nevolá addlogging a addmemorycache

[#14756 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

**Staré chování**

Před EF Core `AddDbContext` 3.0, volání nebo `AddDbContextPool` by také zaregistrovat protokolování a ukládání do mezipaměti služby s DI prostřednictvím volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Nové chování**

Počínaje EF Core `AddDbContext` 3.0 `AddDbContextPool` a již nebude registrovat tyto služby s vkládání závislostí (DI).

**Proč**

EF Core 3.0 nevyžaduje, aby tyto služby jsou v kontejneru DI aplikace. Však `ILoggerFactory` pokud je registrována v kontejneru DI aplikace, pak bude stále používán EF Core.

**Omezení rizik**

Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a>AddEntityFramework* přidá IMemoryCache s limitem velikosti

[#12905 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

**Staré chování**

Před EF Core 3.0 by volající `AddEntityFramework*` metody také zaregistrovat služby ukládání do mezipaměti paměti s DI bez omezení velikosti.

**Nové chování**

Počínaje EF Core 3.0, `AddEntityFramework*` zaregistruje službu IMemoryCache s limitem velikosti. Pokud některé další služby přidané později závisí na IMemoryCache mohou rychle dosáhnout výchozího limitu způsobuje výjimky nebo snížený výkon.

**Proč**

Použití IMemoryCache bez omezení může mít za následek nekontrolované využití paměti, pokud je chyba v dotazu ukládání do mezipaměti logiky nebo dotazy jsou generovány dynamicky. Výchozí limit zmírňuje potenciální útok DoS.

**Omezení rizik**

Ve většině `AddEntityFramework*` případů volání `AddDbContext` není `AddDbContextPool` nutné, pokud nebo je volána také. Proto nejlepší zmírnění je odebrat `AddEntityFramework*` volání.

Pokud vaše aplikace potřebuje tyto služby, zaregistrujte implementaci IMemoryCache explicitně s kontejnerem DI předem pomocí [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry nyní provádí místní DetectChanges

[Sledování #13552 problému](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

**Staré chování**

Před EF Core 3.0 volání `DbContext.Entry` by způsobit změny, které mají být zjištěny pro všechny sledované entity.
To zajistilo, že `EntityEntry` stav vystavený v aktuálním stavu.

**Nové chování**

Počínaje EF Core 3.0 `DbContext.Entry` volání se nyní pokusí pouze zjistit změny v dané entitě a všechny sledované hlavní entity s ním související.
To znamená, že změny jinde nemusí být zjištěny voláním této metody, což může mít vliv na stav aplikace.

Všimněte `ChangeTracker.AutoDetectChangesEnabled` si, `false` že pokud je nastavena na pak i toto místní zjišťování změn bude zakázáno.

Jiné metody, které způsobují `ChangeTracker.Entries` zjišťování `SaveChanges`změn-- například a --stále způsobují plné `DetectChanges` všech sledovaných entit.

**Proč**

Tato změna byla provedena ke zlepšení `context.Entry`výchozího výkonu použití .

**Omezení rizik**

Volání `ChangeTracker.DetectChanges()` explicitně `Entry` před voláním zajistit pre-3.0 chování.

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Klíče pole řetězce a bajtů nejsou ve výchozím nastavení generovány klientem.

[Sledování #14617 problému](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

**Staré chování**

Před EF Core 3.0 `string` a `byte[]` vlastnosti klíče lze použít bez explicitního nastavení hodnoty bez null.
V takovém případě by hodnota klíče byla generována na straně klienta jako identifikátor `byte[]`GUID serializovaný na bajty pro .

**Nové chování**

Počínaje EF Core 3.0 bude vyvolána výjimka označující, že nebyla nastavena žádná hodnota klíče.

**Proč**

Tato změna byla provedena, `string` / `byte[]` protože klient generované hodnoty obecně nejsou užitečné a výchozí chování ztěžovalo důvod o generovaných hodnotklíče běžným způsobem.

**Omezení rizik**

Chování pre-3.0 lze získat explicitním zadáním, že vlastnosti klíče by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota bez nuly.
Například s fluent API:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Nebo s poznámkami o datech:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory je nyní oborová služba

[#14698 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

**Staré chování**

Před EF Core 3.0, `ILoggerFactory` byl registrován jako singleton služby.

**Nové chování**

Počínaje EF Core 3.0, `ILoggerFactory` je nyní registrována jako vymezené.

**Proč**

Tato změna byla provedena tak, aby `DbContext` přidružení protokolování s instancí, která umožňuje další funkce a odstraňuje některé případy patologického chování, jako je například rozpad interních poskytovatelů služeb.

**Omezení rizik**

Tato změna by neměla mít vliv na kód aplikace, pokud není registrace a používání vlastních služeb na zprostředkovatele interních služeb EF Core.
To není běžné.
V těchto případech bude většina věcí stále fungovat, ale `ILoggerFactory` jakákoli služba singleton, `ILoggerFactory` která byla závislá na, bude muset být změněna, aby získala jiným způsobem.

Pokud narazíte na situace, jako je tato, podejte prosím problém na EF Core `ILoggerFactory` [GitHub problém tracker,](https://github.com/aspnet/EntityFrameworkCore/issues) dejte nám vědět, jak používáte tak, že můžeme lépe pochopit, jak to v budoucnu znovu nerozbít.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Opožděné načítání proxy servery již předpokládat, navigační vlastnosti jsou plně načteny

[Sledování #12780 problému](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

**Staré chování**

Před EF Core 3.0, jakmile `DbContext` byl vyřazen neexistuje žádný způsob, jak zjistit, zda dané navigační vlastnost na entitu získané z tohoto kontextu byla plně načtena nebo ne.
Proxy servery by místo toho předpokládat, že navigace odkazu je načten, pokud má hodnotu non-null a že navigace kolekce je načten, pokud není prázdný.
V těchto případech pokus o opožděné zatížení by bez operace.

**Nové chování**

Počínaje EF Core 3.0 proxy sledovat, zda je načten navigační vlastnost.
To znamená, že pokus o přístup k navigační vlastnosti, která je načtena po zpřístupnění kontextu bude vždy no-op, i když načtená navigace je prázdná nebo null.
Naopak pokus o přístup k navigační vlastnost, která není načten vyvolá výjimku, pokud je uvolněn kontext i v případě, že navigační vlastnost je neprázdná kolekce.
Pokud k této situaci nastane, znamená to, že kód aplikace se pokouší použít opožděné načítání v neplatný čas a aplikace by měla být změněna tak, aby to neudělala.

**Proč**

Tato změna byla provedena tak, aby chování konzistentní a správné při `DbContext` pokusu opožděné zatížení na vyřazené instance.

**Omezení rizik**

Aktualizujte kód aplikace tak, aby se nepokoušel opožděné načítání s vyřazeným kontextem, nebo nakonfigurujte tento kód jako no-op, jak je popsáno ve zprávě o výjimce.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>Nadměrné vytváření interních poskytovatelů služeb je nyní ve výchozím nastavení chybou

[Sledování #10236 problému](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

**Staré chování**

Před EF Core 3.0 by bylo zaznamenáno upozornění pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb.

**Nové chování**

Počínaje EF Core 3.0, toto upozornění je nyní považováno za a je vyvolána chyba a je vyvolána výjimka. 

**Proč**

Tato změna byla provedena řídit lepší kód aplikace prostřednictvím vystavení tohoto patologického případu explicitněji.

**Omezení rizik**

Nejvhodnější příčinou akce při výskytu této chyby je pochopit hlavní příčinu a zastavit vytváření tolik interních poskytovatelů služeb.
Chybu však lze převést zpět na upozornění (nebo ignorovány) prostřednictvím konfigurace na `DbContextOptionsBuilder`.
Příklad:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Nové chování pro HasOne/HasMany volána s jedním řetězcem

[Sledování #9171 problému](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

**Staré chování**

Před EF Core 3.0, volání `HasOne` kódu nebo `HasMany` s jedním řetězcem byla interpretována matoucím způsobem.
Příklad:
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Kód vypadá, že se `Samurai` týká některého jiného `Entrance` typu entity pomocí navigační vlastnosti, která může být soukromá.

Ve skutečnosti se tento kód pokusí vytvořit vztah `Entrance` k nějakému typu entity volané bez navigační vlastnosti.

**Nové chování**

Počínaje EF Core 3.0, kód výše nyní dělá to, co to vypadalo, že by měl dělat dříve.

**Proč**

Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.

**Omezení rizik**

Tím se přeruší pouze aplikace, které explicitně konfigurují vztahy pomocí řetězců pro názvy typů a bez explicitního zadání vlastnosti navigace.
To není běžné.
Předchozí chování lze získat prostřednictvím explicitní předávání `null` pro název vlastnosti navigace.
Příklad:

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Návratový typ pro několik asynchronních metod byl změněn z Task na ValueTask.

[Sledování #15184 problému](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

**Staré chování**

Následující asynchronní metody `Task<T>`dříve vrátily :

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()`(a odvozené třídy)

**Nové chování**

Výše uvedené metody nyní vrátit `ValueTask<T>` více než `T` stejné jako dříve.

**Proč**

Tato změna snižuje počet přidělení haldy vzniklé při vyvolání těchto metod, zlepšení obecného výkonu.

**Omezení rizik**

Aplikace, které jednoduše čekají na výše uvedená api, je třeba pouze překompilovat - nejsou nutné žádné změny zdroje.
Složitější použití (např. předání vrácené `Task` `Task.WhenAny()`do) obvykle vyžaduje, `ValueTask<T>` aby vrácené `Task<T>` převedeny na volání `AsTask()` na něj.
Všimněte si, že to neguje snížení přidělení, které tato změna přináší.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>Relační a textová poznámka je nyní pouze typemapping

[Sledování #9913 problému](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

**Staré chování**

Název poznámky pro poznámky mapování typů byl "Relational:TypeMapping".

**Nové chování**

Název poznámky pro poznámky mapování typů je nyní "TypeMapping".

**Proč**

Mapování typů se nyní používá pro více než jen zprostředkovatelé relační databáze.

**Omezení rizik**

Tím se pouze poruší aplikace, které přistupují k mapování typů přímo jako poznámku, která není běžná.
Nejvhodnější akcí k opravě je použití povrchu rozhraní API pro přístup k mapování typu, nikoli k přímému použití poznámky.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable na odvozený typ vyvolá výjimku 

[Sledování #11811 problému](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

**Staré chování**

Před EF Core 3.0, `ToTable()` volal na odvozený typ by být ignorována, protože pouze mapování dědičnosti strategie byla TPH, kde to není platný. 

**Nové chování**

Počínaje EF Core 3.0 a v rámci přípravy na přidání `ToTable()` podpory TPT a TPC v novější verzi, volal na odvozený typ bude nyní vyvolat výjimku, aby se zabránilo neočekávané změny mapování v budoucnu.

**Proč**

V současné době není platný mapovat odvozený typ do jiné tabulky.
Tato změna se zabrání lámání v budoucnu, když se stane platnou věc udělat.

**Omezení rizik**

Odeberte všechny pokusy o mapování odvozených typů do jiných tabulek.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex nahrazen HasIndex 

[Sledování #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

**Staré chování**

Před EF Core 3.0, za předpokladu, že způsob, `ForSqlServerHasIndex().ForSqlServerInclude()` jak nakonfigurovat sloupce používané s `INCLUDE`.

**Nové chování**

Počínaje EF Core 3.0, pomocí `Include` na indexu je nyní podporována na relační úrovni.
Použijte `HasIndex().ForSqlServerInclude()`.

**Proč**

Tato změna byla provedena ke konsolidaci `Include` rozhraní API pro indexy s na jednom místě pro všechny poskytovatele databáze.

**Omezení rizik**

Použijte nové rozhraní API, jak je znázorněno výše.

### <a name="metadata-api-changes"></a>Změny rozhraní API metadat

[Sledování #214 problému](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Nové chování**

Následující vlastnosti byly převedeny na metody rozšíření:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Proč**

Tato změna zjednodušuje implementaci výše uvedených rozhraní.

**Omezení rizik**

Použijte nové metody rozšíření.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>Změny rozhraní METADAT SPECIFICKÉ Pro zprostředkovatele

[Sledování #214 problému](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Nové chování**

Metody rozšíření specifické pro zprostředkovatele budou srovnány se sloučí:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**Proč**

Tato změna zjednodušuje implementaci výše uvedených metod rozšíření.

**Omezení rizik**

Použijte nové metody rozšíření.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core již neodesílá pragma pro vynucení SQLite FK

[Sledování #12151 problému](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

**Staré chování**

Před EF Core 3.0 EF `PRAGMA foreign_keys = 1` Core by odeslat při otevření připojení k SQLite.

**Nové chování**

Počínaje EF Core 3.0 EF Core `PRAGMA foreign_keys = 1` již odešle při otevření připojení k SQLite.

**Proč**

Tato změna byla provedena, `SQLitePCLRaw.bundle_e_sqlite3` protože EF Core používá ve výchozím nastavení, což zase znamená, že vynucení FK je ve výchozím nastavení zapnuto a nemusí být explicitně povoleno při každém otevření připojení.

**Omezení rizik**

Cizí klíče jsou ve výchozím nastavení povoleny v souboru SQLitePCLRaw.bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.
V ostatních případech lze povolit `Foreign Keys=True` cizí klíče zadáním v připojovacím řetězci.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3

**Staré chování**

Před EF Core 3.0 `SQLitePCLRaw.bundle_green`použil EF Core .

**Nové chování**

Počínaje EF Core 3.0, `SQLitePCLRaw.bundle_e_sqlite3`EF Core používá .

**Proč**

Tato změna byla provedena tak, aby verze SQLite používané v systému iOS v souladu s jinými platformami.

**Omezení rizik**

Chcete-li použít nativní verzi SQLite `Microsoft.Data.Sqlite` v systému iOS, nakonfigurujte použití jiného `SQLitePCLRaw` balíčku.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Hodnoty guid jsou nyní uloženy jako TEXT na SQLite

[Sledování #15078 problému](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

**Staré chování**

Guid hodnoty byly dříve uloženy jako hodnoty BLOB na SQLite.

**Nové chování**

Hodnoty guid jsou nyní uloženy jako TEXT.

**Proč**

Binární formát identifikátorů Guids není standardizován. Ukládání hodnot jako TEXT umožňuje databázi více kompatibilní s jinými technologiemi.

**Omezení rizik**

Existující databáze můžete migrovat do nového formátu spuštěním sql jako následující.

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

V EF Core můžete také pokračovat v používání předchozí chování konfigurací převaděče hodnot na tyto vlastnosti.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft.Data.Sqlite zůstává schopen číst guid hodnoty z blob a TEXT sloupce; vzhledem k tomu, že výchozí formát parametrů a konstant se však změnil, budete pravděpodobně muset provést akci pro většinu scénářů zahrnujících identifikátory GUID.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Hodnoty Char jsou nyní uloženy jako TEXT na SQLite

[Sledování #15020 problému](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

**Staré chování**

Char hodnoty byly dříve sored jako integer hodnoty na SQLite. Například hodnota char *A* byla uložena jako celá hodnota 65.

**Nové chování**

Hodnoty Char jsou nyní uloženy jako TEXT.

**Proč**

Ukládání hodnot jako TEXT je přirozenější a databáze je více kompatibilní s jinými technologiemi.

**Omezení rizik**

Existující databáze můžete migrovat do nového formátu spuštěním sql jako následující.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

V EF Core můžete také pokračovat v používání předchozí chování konfigurací převaděče hodnot na tyto vlastnosti.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft.Data.Sqlite také zůstává schopen číst hodnoty znaků z obou INTEGER a TEXT sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>ID migrace jsou nyní generovány pomocí kalendáře invariantní jazykové verze

[Sledování #12978 problému](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

**Staré chování**

ID migrace byly neúmyslně generovány pomocí kalendáře aktuální jazykové verze.

**Nové chování**

ID migrace jsou nyní vždy generovány pomocí kalendáře invariantní jazykové verze (gregoriánský).

**Proč**

Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů sloučení. Použití invariantní kalendář vyhýbá řazení problémy, které mohou vyplývat z členů týmu, které mají různé systémové kalendáře.

**Omezení rizik**

Tato změna ovlivní každého, kdo používá kalendář mimo gregoriánský, kde je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář). Existující ID migrace bude nutné aktualizovat tak, aby nové migrace jsou seřazeny po existující migrace.

ID migrace lze nalézt v atributu Migrace v souborech návrháře migrace.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Tabulka historie migrace je také třeba aktualizovat.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging byl odebrán.

[Sledování #16400 problému](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

**Staré chování**

Před EF Core 3.0, `UseRowNumberForPaging` lze použít ke generování SQL pro stránkování, který je kompatibilní s SQL Server 2008.

**Nové chování**

Počínaje EF Core 3.0 ef bude generovat pouze SQL pro stránkování, který je kompatibilní pouze s novějšími verzemi SERVERU SQL. 

**Proč**

Tuto změnu provádíme, protože [SQL Server 2008 již není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce pro práci se změnami dotazu provedenými v EF Core 3.0 je významná práce.

**Omezení rizik**

Doporučujeme aktualizovat na novější verzi SERVERU SQL Nebo pomocí vyšší úrovně kompatibility, aby byl podporován generovaný SQL. Jak bylo řečeno, pokud to nemůžete udělat, pak prosím [komentujte problém se sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/16400) s podrobnostmi. Toto rozhodnutí můžeme přehodnotit na základě zpětné vazby.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>Informace/metadata rozšíření byly odebrány z rozšíření IDbContextOptionsExtension.

[#16119 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

**Staré chování**

`IDbContextOptionsExtension`obsažené metody pro poskytování metadat o rozšíření.

**Nové chování**

Tyto metody byly přesunuty `DbContextOptionsExtensionInfo` do nové abstraktní základní třídy, která je vrácena z nové `IDbContextOptionsExtension.Info` vlastnosti.

**Proč**

Během vydání od 2.0 do 3.0 jsme potřebovali přidat nebo změnit tyto metody několikrát.
Jejich rozdělení do nové abstraktní základní třídy usnadní provádění těchto změn bez přerušení existujících rozšíření.

**Omezení rizik**

Aktualizujte rozšíření tak, aby sledovala nový vzor.
Příklady jsou nalezeny v `IDbContextOptionsExtension` mnoha implementací pro různé druhy rozšíření ve zdrojovém kódu EF Core.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.

[Sledování #10985 problému](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

**Změnit**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`byl přejmenován `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na .

**Proč**

Zarovná pojmenování této události upozornění se všemi ostatními událostmi upozornění.

**Omezení rizik**

Použijte nový název. (Všimněte si, že číslo ID události se nezměnilo.)

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>Objasnit rozhraní API pro názvy omezení cizího klíče

[Sledování #10730 problému](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

**Staré chování**

Před EF Core 3.0, názvy omezení cizího klíče byly označovány jako jednoduše "název". Příklad:

```csharp
var constraintName = myForeignKey.Name;
```

**Nové chování**

Počínaje EF Core 3.0, názvy omezení cizího klíče jsou nyní označovány jako "název omezení". Příklad:

```csharp
var constraintName = myForeignKey.ConstraintName;
```

**Proč**

Tato změna přináší konzistenci pojmenování v této oblasti a také objasňuje, že se jedná o název omezení cizího klíče a nikoli název sloupce nebo vlastnosti, na který je cizí klíč definován.

**Omezení rizik**

Použijte nový název.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>IRelationalDatabaseCreator.HasTables/HasTablesAsync byly zveřejněny

[Sledování #15997 problému](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

**Staré chování**

Před EF Core 3.0 byly tyto metody chráněny.

**Nové chování**

Počínaje EF Core 3.0 jsou tyto metody veřejné.

**Proč**

Tyto metody jsou používány EF k určení, pokud je databáze vytvořena, ale prázdné. To může být také užitečné z mimo EF při určování, zda použít migrace.

**Omezení rizik**

Změňte přístupnost všech přepsání.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>Microsoft.EntityFrameworkCore.Design je nyní balíček DevelopmentDependency

[Sledování #11506 problému](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

**Staré chování**

Před EF Core 3.0, Microsoft.EntityFrameworkCore.Design byl pravidelný Balíček NuGet, jehož sestavení může být odkazováno projekty, které na něm závisely.

**Nové chování**

Počínaje EF Core 3.0, je balíček DevelopmentDependency. To znamená, že závislost nebude toku přechodně do jiných projektů a že již nelze ve výchozím nastavení odkazovat na jeho sestavení.

**Proč**

Tento balíček je určen pouze k použití v době návrhu. Nasazené aplikace by na něj neměly odkazovat. Vytvoření balíčku DevelopmentDependency posiluje toto doporučení.

**Omezení rizik**

Pokud potřebujete odkazovat na tento balíček přepsat EF Core chování návrhu, pak můžete aktualizovat PackageReference metadata položky ve vašem projektu.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

Pokud je balíček odkazován přechodně prostřednictvím Microsoft.EntityFrameworkCore.Tools, budete muset přidat explicitní PackageReference do balíčku změnit jeho metadata. Takový explicitní odkaz musí být přidán do každého projektu, kde jsou potřebné typy z balíčku.

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL.raw aktualizován na verzi 2.0.0

[Sledování #14824 problému](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

**Staré chování**

Microsoft.EntityFrameworkCore.Sqlite dříve závisel na verzi 1.1.12 SQLitePCL.raw.

**Nové chování**

Aktualizovali jsme náš balíček tak, aby závisel na verzi 2.0.0.

**Proč**

Verze 2.0.0 cílů SQLitePCL.raw .NET Standard 2.0. Dříve se zaměřila na standard .NET Standard 1.1, který vyžadoval rozsáhlé uzavření přenositých balíčků.

**Omezení rizik**

SQLitePCL.raw verze 2.0.0 obsahuje některé změny. Podrobnosti najdete v [poznámkách k verzi.](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md)

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>NetTopologySuite aktualizován na verzi 2.0.0

[Sledování #14825 problému](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

**Staré chování**

Prostorové balíčky dříve závisely na verzi 1.15.1 NetTopologySuite.

**Nové chování**

Aktualizovali jsme náš balíček tak, aby závisel na verzi 2.0.0.

**Proč**

Verze 2.0.0 NetTopologySuite si klade za cíl řešit několik problémů použitelnosti, s nimiž se setkávají uživatelé EF Core.

**Omezení rizik**

NetTopologySuite verze 2.0.0 obsahuje některé změny. Podrobnosti najdete v [poznámkách k verzi.](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001)

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a>Místo klienta System.Data.SqlClient se používá místo klienta System.Data.SqlClient

[Sledování #15636 problému](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

**Staré chování**

Microsoft.EntityFrameworkCore.SqlServer dříve závisel na systému.Data.SqlClient.

**Nové chování**

Aktualizovali jsme náš balíček tak, aby závisel na microsoft.Data.SqlClient.

**Proč**

Microsoft.Data.SqlClient je vlajkovou lodí ovladač přístupu k datům pro SQL Server do budoucna a System.Data.SqlClient již není středem vývoje.
Některé důležité funkce, například Vždy šifrované, jsou k dispozici pouze na Microsoft.Data.SqlClient.

**Omezení rizik**

Pokud váš kód přebírá přímou závislost na System.Data.SqlClient, musíte jej změnit tak, aby odkazoval microsoft.Data.SqlClient místo; jako dva balíčky udržovat velmi vysoký stupeň kompatibility rozhraní API, by to mělo být pouze jednoduchý balíček a změna oboru názvů.

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>Musí být nakonfigurováno více nejednoznačných vztahů, které samy odkazují na sebe. 

[#13573 problému sledování](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

**Staré chování**

Typ entity s více samoodkazujícími jednosměrnými navigačními vlastnostmi a odpovídajícími jednotkami FK byl nesprávně nakonfigurován jako jeden vztah. Příklad:

```csharp
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

Tento scénář je nyní zjištěna v budově modelu a je vyvolána výjimka označující, že model je nejednoznačný.

**Proč**

Výsledný model byl nejednoznačný a bude pravděpodobně obvykle špatné pro tento případ.

**Omezení rizik**

Použijte úplnou konfiguraci relace. Příklad:

```csharp
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a>DbFunction.Schema je null nebo prázdný řetězec nakonfiguruje, aby byl ve výchozím schématu modelu

[Sledování #12757 problému](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

**Staré chování**

Funkce DbFunction nakonfigurovaná se schématem jako prázdný řetězec byla považována za vestavěnou funkci bez schématu. Například následující kód `DatePart` namapuje `DATEPART` funkci CLR na vestavěnou funkci na serveru SqlServer.

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

**Nové chování**

Všechna mapování funkce DbFunction jsou považována za mapovaná na uživatelem definované funkce. Proto prázdná hodnota řetězce by umístit funkci uvnitř výchozí schéma pro model. Což by mohlo být schéma nakonfigurované explicitně prostřednictvím fluent API `modelBuilder.HasDefaultSchema()` nebo `dbo` jinak.

**Proč**

Dříve schéma prázdné byl způsob, jak zacházet s funkcí je vestavěný, ale tato logika je použitelná pouze pro SqlServer, kde vestavěné funkce nepatří do žádné schéma.

**Omezení rizik**

Nakonfigurujte překlad dbfunction ručně, abyste jej namapovali na vestavěnou funkci.

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
