---
title: Upgrade z předchozích verzí na EF Core 2 – EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 42e59b47f569ef6fcf72fc5bd5f94d3e9d807a24
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813573"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Upgrade aplikací z předchozích verzí na EF Core 2,0

Máme příležitost významně vylepšit naše existující rozhraní API a chování v 2,0. Existuje několik vylepšení, která mohou vyžadovat úpravu stávajícího kódu aplikace, i když se domníváme, že většina aplikací bude mít dopad na nízký výkon, ve většině případů, kdy je potřeba pouze opětovná kompilace a minimální změny Průvodce pro nahrazení zastaralých rozhraní API.

Aktualizace existující aplikace na EF Core 2,0 může vyžadovat:

1. Upgrade cílové implementace .NET aplikace na jednu, která podporuje .NET Standard 2,0. Další podrobnosti najdete v tématu [podporované implementace rozhraní .NET](../platforms/index.md) .

2. Identifikujte poskytovatele cílové databáze, který je kompatibilní s EF Core 2,0. Viz [EF Core 2,0 vyžaduje níže uvedený zprostředkovatel databáze 2,0](#ef-core-20-requires-a-20-database-provider) .

3. Upgradují se všechny balíčky EF Core (běhové prostředí a nástroje) na 2,0. Další podrobnosti najdete v tématu [instalace EF Core](../get-started/install/index.md) .

4. Proveďte potřebné změny kódu, abyste mohli kompenzovat zásadní změny popsané ve zbývající části tohoto dokumentu.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core nyní zahrnuje EF Core

Aplikace cílené na ASP.NET Core 2,0 mohou používat EF Core 2,0 bez dalších závislostí kromě poskytovatelů databáze třetích stran. Avšak aplikace cílené na předchozí verze ASP.NET Core nutné upgradovat na ASP.NET Core 2,0, aby bylo možné použít EF Core 2,0. Další podrobnosti o upgradu ASP.NET Corech aplikací na 2,0 najdete v [dokumentaci k ASP.NET Core na daném předmětu](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Nový způsob získání aplikačních služeb v ASP.NET Core

Doporučený vzor pro ASP.NET Core webové aplikace byl aktualizován pro 2,0 způsobem, který podařilo přerušit logiku návrhu EF Core použitou v 1. x. Dříve v době návrhu EF Core pokus o vyvolání `Startup.ConfigureServices` přímo za účelem přístupu k poskytovateli služeb aplikace. V ASP.NET Core 2,0 je konfigurace inicializována mimo `Startup` třídu. Aplikace používající EF Core obvykle ke svému připojovacímu řetězci přistupuje z konfigurace `Startup` , takže sám o sobě nestačí. Pokud upgradujete ASP.NET Core 1. x aplikace, může se při použití EF Core nástrojů zobrazit následující chyba.

> V ' ApplicationContext ' nebyl nalezen žádný konstruktor bez parametrů. Buď přidejte konstruktor bez parametrů do ' ApplicationContext ' nebo přidejte implementaci ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' do stejného sestavení jako ' ApplicationContext '

Do výchozí šablony ASP.NET Core 2.0 byl přidán nový přidaný čas návrhu. Statická `Program.BuildWebHost` metoda umožňuje EF Core přistupovat k poskytovateli služeb aplikace v době návrhu. Pokud upgradujete ASP.NET Core 1. x aplikace, budete muset `Program` třídu aktualizovat tak, aby vypadala jako následující.

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

Přijetí tohoto nového vzoru, když se aktualizuje aplikace na 2,0, se důrazně doporučuje a je potřeba, aby se funkce produktu, jako Entity Framework Core migrace, daly pracovat. Další běžnou alternativou je [implementace *IDesignTimeDbContextFactory\<TContext >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>Přejmenované IDbContextFactory

Aby bylo možné podporovat různé vzorce aplikací a poskytnout uživatelům větší kontrolu nad tím `DbContext` `IDbContextFactory<TContext>` , jak se používají v době návrhu, máme v minulosti za rozhraní. V době návrhu budou EF Core nástroje ve vašem projektu zjišťovat implementace tohoto rozhraní a použít ho k vytvoření `DbContext` objektů.

Toto rozhraní má velmi obecný název, který je v omylu pro uživatele, kteří si ho můžou `DbContext`zkusit znovu použít pro jiné scénáře vytváření. Odcházejí z ochrany, když se nástroje EF potom pokusily použít jejich implementaci v době návrhu a způsobily selhání příkazů `Update-Database` , `dotnet ef database update` jako je nebo chyba.

Abychom mohli sdělit silný sémantiku tohoto rozhraní v době návrhu, přejmenovali jsme ho na `IDesignTimeDbContextFactory<TContext>`.

Pro vydání 2,0 verze `IDbContextFactory<TContext>` stále existuje, ale je označena jako zastaralá.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions odebrané

Vzhledem k tomu, že se výše popsané změny ASP.NET Core 2,0, `DbContextFactoryOptions` zjistili jsme, že už na novém `IDesignTimeDbContextFactory<TContext>` rozhraní nepotřebujeme. Tady jsou alternativy, které byste měli použít místo toho.

| DbContextFactoryOptions | Jiné                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext. BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment. GetEnvironmentVariable ("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Pracovní adresář v době návrhu se změnil.

Změny ASP.NET Core 2,0 také vyžadují pracovní adresář používaný nástrojem `dotnet ef` k zarovnávání s pracovním adresářem používaným sadou Visual Studio při spuštění aplikace. Jedním z podobných vedlejších účinků je, že názvy souborů SQLite jsou nyní relativní vzhledem k adresáři projektu, a nikoli výstupní adresář, jako by se použily.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0 vyžaduje poskytovatele databáze 2,0.

Pro EF Core 2,0 jsme provedli mnoho zjednodušení a vylepšení, jak fungují poskytovatelé databáze. To znamená, že zprostředkovatelé 1.0. x a 1.1. x nebudou fungovat s EF Core 2,0.

Poskytovatelé SQL Server a SQLite jsou dodány týmem EF a verze 2,0 budou k dispozici jako součást verze 2,0. Open Source poskytovatelé třetích stran pro [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)a [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) se aktualizují na 2,0. U všech ostatních zprostředkovatelů se prosím obraťte na zapisovač poskytovatele.

## <a name="logging-and-diagnostics-events-have-changed"></a>Události protokolování a diagnostiky se změnily.

Poznámka: tyto změny by neměly mít vliv na většinu kódu aplikace.

ID událostí pro zprávy odeslané do [ILogger](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger) se změnila v 2,0. Identifikátory událostí jsou teď v rámci EF Core kódu jedinečné. Tyto zprávy se teď také řídí standardním vzorem pro strukturované protokolování, které používá, například MVC.

Změnily se také kategorie protokolovacího nástroje. K dispozici je teď známá sada kategorií, ke které se dostanete prostřednictvím [DbLoggerCategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs).

Události [DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) nyní používají stejné názvy ID události jako odpovídající `ILogger` zprávy. Datové části událostí jsou všechny nominální typy odvozené z [EventData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata).

ID událostí, typy datových částí a kategorie jsou zdokumentovány v [CoreEventId](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) a třídách [RelationalEventId](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) .

ID se také přesunula z Microsoft. EntityFrameworkCore. Infrastructure na nový obor názvů Microsoft. EntityFrameworkCore. Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core změny v rozhraní API relačních metadat

EF Core 2,0 teď pro každého jiného zprostředkovatele vytvoří jiný [IModel](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) . To je obvykle transparentní pro aplikaci. Usnadnili jsme tak zjednodušení rozhraní API na nižší úrovni tak, aby jakýkoliv přístup k _běžným koncepcím relačních metadat_ byl vždy proveden prostřednictvím volání `.Relational` namísto `.SqlServer`, `.Sqlite`atd. Například kód 1.1. x podobný tomuto:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

By teď mělo být zapsáno takto:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Namísto použití metod jako `ForSqlServerToTable`jsou nyní k dispozici metody rozšíření pro zápis podmíněného kódu na základě aktuálně používaného poskytovatele. Příklad:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Tato změna se vztahuje pouze na rozhraní API/metadata, která jsou definována pro _všechny_ relační zprostředkovatele. Rozhraní API a metadata zůstávají stejné, pokud jsou specifické jenom pro jednoho poskytovatele. Například clusterované indexy jsou specifické pro SQL Server, takže `ForSqlServerIsClustered` a `.SqlServer().IsClustered()` musí se dál používat.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Nepřevzít kontrolu nad poskytovatelem služby EF

EF Core pro svou interní `IServiceProvider` implementaci používá interní (kontejner vkládání s závislostmi). Aplikace by měly EF Core vytvářet a spravovat tohoto poskytovatele s výjimkou zvláštních případů. Důrazně zvažte odebrání všech volání `UseInternalServiceProvider`. Pokud aplikace vyžaduje volání `UseInternalServiceProvider`, zvažte, jestli je potřeba nahlásit [problém](https://github.com/aspnet/EntityFramework/Issues) , abychom mohli prozkoumat další způsoby, jak váš scénář zpracovat.

Volání `AddEntityFramework`, `AddEntityFrameworkSqlServer`, atd. není vyžadováno kódem aplikace, pokud `UseInternalServiceProvider` není volána také metoda. Odeberte všechna existující volání `AddEntityFramework` nebo `AddEntityFrameworkSqlServer`atd. `AddDbContext` by měla být použita stejným způsobem jako předtím.

## <a name="in-memory-databases-must-be-named"></a>Databáze v paměti musí mít název.

Globální Nepojmenovaná databáze v paměti byla odebrána a místo toho musí být všechny databáze v paměti pojmenovány. Příklad:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Tím se vytvoří nebo použije databáze s názvem "MyDatabase". Pokud `UseInMemoryDatabase` je volána znovu se stejným názvem, bude použita stejná databáze v paměti, což umožňuje sdílení více instancí kontextu.

## <a name="read-only-api-changes"></a>Změny rozhraní API jen pro čtení

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`a `IsStoreGeneratedAlways` byly zastaralé a nahrazeny [BeforeSaveBehavior](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) a [AfterSaveBehavior](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior). Toto chování platí pro libovolnou vlastnost (nikoli pouze vlastnosti generované úložištěm) a určuje, jak má být hodnota vlastnosti použita při vložení do řádku databáze (`BeforeSaveBehavior`) nebo při aktualizaci stávajícího řádku databáze (`AfterSaveBehavior`).

Vlastnosti označené jako [ValueGenerated. OnAddOrUpdate](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (například pro počítané sloupce) budou ve výchozím nastavení ignorovat všechny hodnoty aktuálně nastavené u vlastnosti. To znamená, že hodnota generovaná úložištěm bude vždycky získaná bez ohledu na to, jestli se pro sledovanou entitu nastavila nebo změnila nějaká hodnota. To lze změnit nastavením jiného `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Nové chování při odstraňování ClientSetNull

V předchozích verzích měla [DeleteBehavior. restrict](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.deletebehavior) chování pro entity sledované kontextem, který je více uzavřenou `SetNull` shodnou sémantikou. V EF Core 2,0 byl pro volitelné `ClientSetNull` relace zaveden nové chování jako výchozí. Toto chování má `SetNull` sémantiku pro sledované entity a `Restrict` chování pro databáze vytvořené pomocí EF Core. V našem prostředí se jedná o nejpravděpodobnější/užitečné chování sledovaných entit a databáze. `DeleteBehavior.Restrict`se teď u sledovaných entit dodrží při nastavení pro volitelné vztahy.

## <a name="provider-design-time-packages-removed"></a>Odebrané balíčky pro dobu návrhu zprostředkovatele

`Microsoft.EntityFrameworkCore.Relational.Design` Balíček se odebral. Obsah byl konsolidován do `Microsoft.EntityFrameworkCore.Relational` a. `Microsoft.EntityFrameworkCore.Design`

To se šíří do balíčků pro dobu návrhu zprostředkovatele. Balíčky (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`atd.) se odebraly a jejich obsah se konsoliduje do hlavních balíčků poskytovatele.

Chcete- `Scaffold-DbContext` li `dotnet ef dbcontext scaffold` povolit nebo v EF Core 2,0, stačí pouze odkazovat na balíček s jedním zprostředkovatelem:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
