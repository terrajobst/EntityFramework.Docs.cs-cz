---
title: "Upgrade z předchozí verze na 2 jádra EF – EF jádra"
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 30f4de794d42b1385145286e77c2e7c67987fea6
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Upgrade aplikace z předchozích verzí na EF základní 2.0

## <a name="procedures-common-to-all-applications"></a>Běžné postupy pro všechny aplikace

Aktualizace stávající aplikace do EF základní 2.0 může vyžadovat:

1. Upgrade aplikace podporující rozhraní .NET 2.0 standardní cílové platformy .NET. V tématu [podporované platformy](../platforms/index.md) další podrobnosti.

2. Identifikaci zprostředkovatele pro cílovou databázi, která je kompatibilní s EF základní 2.0. V tématu [EF základní 2.0 vyžaduje poskytovatele 2.0 databáze](#ef-core-20-requires-a-20-database-provider) níže.

3. Upgrade všech balíčků EF jádra (runtime a nástrojů) na 2.0. Odkazovat na [instalace jádra EF](../get-started/install/index.md) další podrobnosti.

4. Proveďte požadované změny nezbytného kódu kompenzovat nejnovější změny. Najdete v článku [nejnovějších změn](#breaking-changes) části níže další podrobnosti.

## <a name="aspnet-core-applications"></a>Aplikace ASP.NET Core

1. Viz zejména [nové vzor pro inicializaci aplikace poskytovatele služeb](#new-way-of-getting-application-services) popsané dole.

> [!TIP]  
> Přijetí této nové vzor při aktualizaci aplikace na 2.0 se důrazně doporučuje a je potřeba, aby na funkce produktu, jako je migrace základní Entity Framework pracovat. Další běžné alternativou je [implementovat *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Cílení na technologii ASP.NET 2.0 základní aplikace můžete použít EF základní 2.0 bez další závislosti, kromě zprostředkovatelů databáze třetích stran. Aplikace cílené na předchozích verzích ASP.NET Core je však nutné upgrade na technologii ASP.NET 2.0 Core-li používat EF základní 2.0. Další podrobnosti týkající se upgradu aplikací ASP.NET Core 2.0 najdete v tématu [ASP.NET Core dokumentaci na předmět](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Nejnovější změny

Přesměrovali jsme možnost výrazně vylepšit náš stávajících rozhraní API a chování v 2.0. Existuje několik vylepšení, které může vyžadovat úprava existující kód aplikace, i když se domníváme, že pro většinu aplikací dopad bude nízká, ve většině případů vyžaduje právě rekompilace a minimální s průvodcem změny nahradit zastaralá rozhraní API.

### <a name="new-way-of-getting-application-services"></a>Nový způsob získání aplikačních služeb

Doporučené vzor pro webové aplikace ASP.NET Core byla aktualizována pro 2.0 tak, aby překročila EF základní použita v 1.x logiku návrhu. Dříve se v době návrhu, má být vyvolán pokusit EF základní `Startup.ConfigureServices` přímo, aby bylo možné získat přístup k poskytovateli služby aplikace. V aplikaci ASP.NET 2.0 jádra, konfigurace je inicializována mimo `Startup` třídy. Aplikace, které používají základní EF obvykle přístup k jejich připojovací řetězec z konfigurace, takže `Startup` samostatně již není dostatečná. Pokud provádíte upgrade aplikace ASP.NET Core 1.x, může dojít k následující chybě, při použití nástroje EF jádra.

> Nenašel se žádný konstruktor bez parametrů na 'ApplicationContext'. Buď přidejte konstruktor bez parametrů do 'ApplicationContext' nebo přidání implementace ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;"ve stejném sestavení jako 'ApplicationContext.

V technologii ASP.NET Core 2.0 výchozí šablony byla přidána nové háku návrhu. Statické `Program.BuildWebHost` metoda umožňuje EF jádra pro přístup k poskytovateli služby aplikace v době návrhu. Pokud provádíte upgrade aplikace ASP.NET Core 1.x, budete muset aktualizovat můžete `Program` třídy vypadat takto.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

### <a name="idbcontextfactory-renamed"></a>IDbContextFactory přejmenovat

Aby bylo možné podporovat zpracování různé aplikace a poskytují uživatelům větší kontrolu nad postupy jejich `DbContext` se používá v době návrhu uvádíme, v minulosti `IDbContextFactory<TContext>` rozhraní. V době návrhu EF základní nástroje zjistí implementace tohoto rozhraní ve vašem projektu a použít k vytvoření `DbContext` objekty.

Toto rozhraní měl velmi obecný název, který v omyl některé uživatele a zkuste to znovu použití pro jiné `DbContext`– vytváření scénáře. By byly zachycena vypnout ochranu při nástroje EF poté se pokusil použít implementace v době návrhu a způsobila příkazy, jako je `Update-Database` nebo `dotnet ef database update` selhání.

Chcete-li komunikovat silné sémantiku návrhu tohoto rozhraní, jsme přejmenovaná jej do `IDesignTimeDbContextFactory<TContext>`.

Pro rozhraní 2.0 verze `IDbContextFactory<TContext>` stále existuje, ale je označen jako zastaralý.

### <a name="dbcontextfactoryoptions-removed"></a>Odebrat DbContextFactoryOptions

Z důvodu změn v technologii ASP.NET 2.0 základní popsané výše, jsme našli `DbContextFactoryOptions` byl již není potřeba na novém `IDesignTimeDbContextFactory<TContext>` rozhraní. Tady jsou alternativy, který by měl být místo toho používat.

| DbContextFactoryOptions | Alternativní                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

### <a name="design-time-working-directory-changed"></a>Změnit návrh pracovní adresář

Změny technologii ASP.NET 2.0 základní také potřebné pracovní adresář používané `dotnet ef` vyrovnání pracovní adresář využívá sada Visual Studio při spuštění aplikace. Jeden odběr pozorovatelného vedlejším účinkem této je tento SQLite názvy souborů jsou nyní relativně k adresáři projektu a není výstupnímu adresáři jako dříve.

### <a name="ef-core-20-requires-a-20-database-provider"></a>Základní EF 2.0 vyžaduje poskytovatele 2.0 databáze

Pro základní 2.0 EF jsme provedli mnoho zjednodušení a vylepšení zprostředkovatele databáze způsob práce. To znamená, že zprostředkovatele 1.0.x a 1.1.x nebude fungovat s EF základní 2.0.

Zprostředkovatele SQL Server a SQLite jsou sice EF tým a verze 2.0 bude k dispozici v rámci rozhraní 2.0 verzi. Zprostředkovatelé open-source třetích stran pro [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), a [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) aktualizované 2.0. Všichni ostatní poskytovatelé obraťte se na poskytovatele zapisovače.

### <a name="logging-and-diagnostics-events-have-changed"></a>Protokolování a diagnostiky události změnily.

Poznámka: tyto změny by neměla mít vliv na většinu kódu aplikace.

ID událostí pro zprávy odeslané do [objektu ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) změnily v 2.0. ID událostí jsou nyní jedinečné v rámci EF jádro kódu. Tyto zprávy teď také použít standardní vzor strukturovaných protokolování používá, například MVC.

Změnily také kategorie protokolovacího nástroje. Má nyní dobře známé sadu kategorií přistupovat prostřednictvím [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) události teď použít stejné názvy ID událostí jako odpovídající `ILogger` zprávy. Datové části události jsou všechny nominální typy odvozené od [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

ID událostí, kategorií a datové typy jsou dokumentovány v článku [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) a [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) třídy.

ID mít z Microsoft.EntityFrameworkCore.Infraestructure také přesouvat na nový obor názvů Microsoft.EntityFrameworkCore.Diagnostics.

### <a name="ef-core-relational-metadata-api-changes"></a>Změny EF základní relační metadat rozhraní API

Základní EF 2.0 nyní sestavení jiné [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) pro každý používaný jiný zprostředkovatel. To je obvykle transparentní k aplikaci. To má usnadňují zjednodušení nižší úrovně metadat rozhraní API, tak, aby žádný přístup k _běžné koncepty relační metadata_ je vždy provedeny prostřednictvím volání `.Relational` místo `.SqlServer`, `.Sqlite`atd. Například 1.1.x kód takto:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Nyní budou zasílány takto:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Místo použití metody, třeba `ForSqlServerToTable`, rozšiřující metody jsou nyní k dispozici napsat podmíněného kód podle aktuálního zprostředkovatele používán. Příklad:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Všimněte si, že tato změna se vztahuje pouze na rozhraní API nebo metadata, která je definována pro _všechny_ relační zprostředkovatele. Rozhraní API a metadata zůstává stejná, když je specifické pro pouze jednoho zprostředkovatele. Například Clusterované indexy jsou specifické pro SQL Server, takže `ForSqlServerIsClustered` a `.SqlServer().IsClustered()` musí být použita.

### <a name="dont-take-control-of-the-ef-service-provider"></a>Nemáte převzít kontrolu nad poskytovatele služeb EF

Základní EF používá interní `IServiceProvider` (tj. kontejner vkládání závislostí) pro vnitřní implementace. Aplikace by měla umožnit EF jádra k vytváření a správě tohoto zprostředkovatele s výjimkou ve zvláštních případech. Důrazně zvažte odebrání volání `UseInternalServiceProvider`. Pokud aplikace potřebuje k volání `UseInternalServiceProvider`, pak zvažte prosím [vyplnění problém](https://github.com/aspnet/EntityFramework/Issues) tak nám prozkoumat další způsoby, jak zpracovat váš scénář.

Volání metody `AddEntityFramework`, `AddEntityFrameworkSqlServer`, atd. není vyžadován kód aplikace, pokud `UseInternalServiceProvider` se označuje taky jako. Odeberte všechny existující volání `AddEntityFramework` nebo `AddEntityFrameworkSqlServer`atd `AddDbContext` by měl používat stejným způsobem jako předtím.

### <a name="in-memory-databases-must-be-named"></a>Databáze v paměti musí mít název.

Odebrali jsme globální nepojmenované databáze v paměti a místo toho musí mít název všechny databáze v paměti. Příklad:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

To vytvoří/používá databáze s názvem "Databáze". Pokud `UseInMemoryDatabase` nebude volán znovu se stejným názvem, pak stejnou databázi v paměti se použije, díky kterému jej ke sdílení více instancí kontextu.

### <a name="read-only-api-changes"></a>Rozhraní API změny jen pro čtení

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, a `IsStoreGeneratedAlways` byly zastaralé a nahradí [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) a [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Tyto chování použít na žádnou vlastnost (ne jenom generovaných úložištěm vlastnosti) a určit, jak hodnota vlastnosti má být použita při vkládání do řádku databáze (`BeforeSaveBehavior`) nebo při aktualizaci existující databáze řádek (`AfterSaveBehavior`).

Vlastnosti označen jako [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (např. pro počítané sloupce) bude ve výchozím nastavení ignorovat všechny aktuálně nastavená pro vlastnost hodnota. To znamená, že hodnota generovaná úložištěm se budou získávat vždy bez ohledu na to, jestli nastavit nebo změnit u sledovaných entity žádnou hodnotu. To se dá změnit nastavením jiné `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Nové chování ClientSetNull odstranění

V předchozích verzích [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) měl chování entit sledovanými kontextu jeden uzavřený odpovídající `SetNull` sémantiku. V EF základní 2.0 nový `ClientSetNull` chování je zavedený jako výchozí pro volitelné relace. Toto chování je `SetNull` sémantiku pro sledovaných entity a `Restrict` chování u databází vytvořených přes EF jádra. V našich zkušeností jedná se očekává/nejužitečnější chování pro sledovaných entity a databáze. `DeleteBehavior.Restrict` je nyní dodržení pro sledovaných entity, pokud nastavíte pro volitelné relace.

### <a name="provider-design-time-packages-removed"></a>Balíčky návrhu zprostředkovatele odebrat

`Microsoft.EntityFrameworkCore.Relational.Design` Balíček byl odebrán. Jeho obsah se konsolidují do `Microsoft.EntityFrameworkCore.Relational` a `Microsoft.EntityFrameworkCore.Design`.

Tím se rozšíří do balíčků návrhu zprostředkovatele. Tyto balíčky (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`atd) byly odebrány a jejich obsah konsolidovány do balíčků hlavní zprostředkovatele.

Chcete-li povolit `Scaffold-DbContext` nebo `dotnet ef dbcontext scaffold` v EF základní 2.0, je potřeba jenom odkazují na balíček jednoho zprostředkovatele:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
