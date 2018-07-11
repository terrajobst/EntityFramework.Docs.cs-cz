---
title: Upgrade z předchozí verze na EF Core 2 - EF Core
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: dca9a3fb9e514b6eb22281a0f0140539681efb71
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949253"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Upgrade aplikací z předchozí verze na EF Core 2.0

## <a name="procedures-common-to-all-applications"></a>Běžné postupy pro všechny aplikace

Aktualizuje existující aplikaci na EF Core 2.0 může vyžadovat:

1. Inovace platformy .NET aplikace, který podporuje .NET Standard 2.0. Zobrazit [podporované platformy](../platforms/index.md) další podrobnosti.

2. Identifikujte zprostředkovatele pro cílovou databázi, která je kompatibilní s EF Core 2.0. Zobrazit [EF Core 2.0 vyžaduje poskytovatele 2.0 databáze](#ef-core-20-requires-a-20-database-provider) níže.

3. Upgrade všech balíčků EF Core (runtime a nástroje) 2.0. Odkazovat na [instalace EF Core](../get-started/install/index.md) další podrobnosti.

4. Proveďte změny nezbytného kódu jako kompenzaci za rozbíjející změny. Zobrazit [Breaking Changes](#breaking-changes) níže v části Další podrobnosti.

## <a name="aspnet-core-applications"></a>Aplikace ASP.NET Core

1. Viz zejména [nový vzor pro inicializaci aplikace poskytovatele služeb](#new-way-of-getting-application-services) je popsáno níže.

> [!TIP]  
> Přijetí tohoto nového modelu při aktualizaci aplikace na 2.0 se důrazně doporučuje a je nutná pro funkce produktu, jako je migrace Entity Framework Core pracovat. Běžnou alternativou je [implementovat *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Aplikace pro ASP.NET Core 2.0, můžete použít EF Core 2.0 bez další závislosti kromě poskytovatelé databází výrobců. Aplikace cílené na předchozích verzích technologie ASP.NET Core je však nutné upgradovat na ASP.NET Core 2.0, aby bylo možné používat EF Core 2.0. Další podrobnosti o upgradu aplikace ASP.NET Core 2.0 najdete v tématu [dokumentace k ASP.NET Core v předmětu](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Nejnovější změny

Přesměrovali jsme možnost výrazně vylepšit naše stávající rozhraní API a chování ve verzi 2.0. Existuje několik vylepšení, které může vyžadovat úprava existující kód aplikace, i když se budeme domnívat, že pro většinu aplikací dopad bude nízká, ve většině případů vyžaduje právě rekompilace a minimální změny s průvodcem k nahrazení zastaralé rozhraní API.

### <a name="new-way-of-getting-application-services"></a>Nový způsob získávání aplikační služby

Doporučený model pro webové aplikace ASP.NET Core se aktualizovala tak, aby se podařilo přerušit návrhu logiky, použita v 1.x EF Core 2.0. Dříve v době návrhu, EF Core se pokusit o vyvolání `Startup.ConfigureServices` přímo, aby bylo možné získat přístup k poskytovateli služby vaší aplikace. V technologii ASP.NET Core 2.0, je konfigurace inicializována mimo `Startup` třídy. Aplikace obvykle pomocí EF Core k jejich připojovací řetězec z konfigurace, takže `Startup` sám o sobě už nestačí. Pokud upgradujete aplikaci ASP.NET Core 1.x, může zobrazit následující chyba, při používání nástrojů EF Core.

> Na 'ApplicationContext' nebyl nalezen žádný konstruktor bez parametrů. Přidejte konstruktor bez parametrů "ApplicationContext" nebo implementace "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" ve stejném sestavení jako "ApplicationContext.

Byl přidán nový hook návrhu v výchozí šablony ASP.NET Core 2.0. Statické `Program.BuildWebHost` metoda umožňuje EF Core při přístupu k poskytovateli služby vaší aplikace v době návrhu. Pokud provádíte upgrade aplikace ASP.NET Core 1.x, budete muset aktualizovat `Program` třídy tak, aby připomínala následující.

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

Aby bylo možné podporovat zpracování různých aplikací a dává uživatelům větší kontrolu nad jak jejich `DbContext` se používá v době návrhu, uvádíme, v minulosti `IDbContextFactory<TContext>` rozhraní. V době návrhu, EF Core tools zjistí implementace tohoto rozhraní ve vašem projektu a použijte ji k vytvoření `DbContext` objekty.

Toto rozhraní má velmi obecný název, který v omyl zkusit znovu použít pro ostatní někteří uživatelé `DbContext`– vytvoření scénářů. Byla zachycena vypnout guard při nástroje EF poté se pokusil použít jejich implementace v době návrhu a způsobila příkazů, jako jsou `Update-Database` nebo `dotnet ef database update` selhání.

Chcete-li komunikovat silné sémantiku návrhu tohoto rozhraní, jsme přejmenovali na `IDesignTimeDbContextFactory<TContext>`.

Rozhraní příkazového řádku 2.0 vydání `IDbContextFactory<TContext>` stále existuje, ale je označen jako zastaralý.

### <a name="dbcontextfactoryoptions-removed"></a>Odebrat DbContextFactoryOptions

Z důvodu změn v ASP.NET Core 2.0 je popsáno výše, jsme zjistili, že `DbContextFactoryOptions` byl už je nepotřebujete na novém `IDesignTimeDbContextFactory<TContext>` rozhraní. Tady jsou alternativy, které byste měli použít místo toho.

| DbContextFactoryOptions | Alternativní                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

### <a name="design-time-working-directory-changed"></a>Změnit návrhu pracovního adresáře

ASP.NET Core 2.0 změny také potřebné pracovní adresář, který používá `dotnet ef` souladu s pracovní adresář, který používá sada Visual Studio při spuštění aplikace. Jeden pozorovatelný vedlejší účinek tohoto objektu je tento SQLite názvy souborů jsou nyní relativní k adresáři projektu a ne k adresáři výstupu jako dříve.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 vyžaduje poskytovatele 2.0 databáze

EF Core 2.0 provedli jsme řadu zjednodušení a vylepšení v poskytovatelé databází způsob, jak pracovat. To znamená, že zprostředkovatele 1.0.x a 1.1.x nebude fungovat s EF Core 2.0.

Zprostředkovatele SQL Server a SQLite se dodávají týmem EF, verze 2.0 bude k dispozici jako součást rozhraní příkazového řádku 2.0 release. Zprostředkovatelé open source třetích stran pro [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), a [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) aktualizované 2.0. U jiných poskytovatelů obraťte se prosím na poskytovatele zapisovače.

### <a name="logging-and-diagnostics-events-have-changed"></a>Změnil se události protokolování a Diagnostika

Poznámka: tyto změny by neměla mít vliv většinu kódu aplikace.

ID událostí pro zprávy odeslané do [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) jste změnili ve verzi 2.0. ID událostí jsou nyní jedinečné v EF Core kódu. Tyto zprávy nyní také použít standardní vzor strukturované protokolování používá, například MVC.

Kategorie protokolovací nástroj se také změnily. Má nyní prostřednictvím dobře známé sady kategorií [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) události teď používají stejné ID názvy událostí jako odpovídající `ILogger` zprávy. Datové části události jsou všechny nominální typy odvozené od [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

ID událostí, datové typy a kategorie jsou dokumentovány v článku [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) a [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) třídy.

ID také přesunuli do nového oboru názvů Microsoft.EntityFrameworkCore.Diagnostics z Microsoft.EntityFrameworkCore.Infraestructure.

### <a name="ef-core-relational-metadata-api-changes"></a>Změny relační metadat rozhraní API EF Core

EF Core 2.0 nyní vytvořit jiný [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) pro každý jiný poskytovatel používá. Toto je obvykle zřejmé do aplikace. To má zajišťované zjednodušení nižší úrovně metadat rozhraní API tak, aby žádný přístup k _běžné koncepty relační metadat_ se vždycky provádí pomocí volání `.Relational` místo `.SqlServer`, `.Sqlite`atd. Například 1.1.x kód následujícím způsobem:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Musí teď zapisují takto:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Namísto použití metody, jako je `ForSqlServerToTable`, rozšiřující metody jsou nyní k dispozici pro zápis podmíněný kód podle aktuálního zprostředkovatele používá. Příklad:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Všimněte si, že tato změna platí pouze pro rozhraní API nebo metadata, která je definována pro _všechny_ relační poskytovatelů. Rozhraní API a metadata zůstává při je specifická pro pouze jednoho poskytovatele. Clusterované indexy jsou například specifické pro SQL Server, takže `ForSqlServerIsClustered` a `.SqlServer().IsClustered()` musí být použita.

### <a name="dont-take-control-of-the-ef-service-provider"></a>Není převzít kontrolu nad EF poskytovatele služeb

EF Core využívá interní `IServiceProvider` (kontejner vkládání závislostí) pro vnitřní implementace. Aplikace by měla umožňovat EF Core k vytváření a správě tohoto zprostředkovatele s výjimkou ve zvláštních případech. Důkladně zvážit možnost odebrání všechna volání do `UseInternalServiceProvider`. Pokud aplikace potřebuje k volání `UseInternalServiceProvider`, pak zvažte [založením problému](https://github.com/aspnet/EntityFramework/Issues) tak nám prozkoumat další možnosti, jak zpracovat váš scénář.

Volání `AddEntityFramework`, `AddEntityFrameworkSqlServer`, atd. není vyžadován kód aplikace, není-li `UseInternalServiceProvider` se označuje taky jako. Odeberte všechny existující volání `AddEntityFramework` nebo `AddEntityFrameworkSqlServer`atd `AddDbContext` měli stále použít stejným způsobem jako předtím.

### <a name="in-memory-databases-must-be-named"></a>Musí mít název databáze v paměti

Globální nepojmenované databáze v paměti jsme odebrali a místo toho musí mít název všechny databáze v paměti. Příklad:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

To vytváří a používá databázi s názvem "Databáze". Pokud `UseInMemoryDatabase` volána znovu se stejným názvem, pak stejnou databázi v paměti se použije, což umožňuje sdílet více instancí kontextu.

### <a name="read-only-api-changes"></a>Jen pro čtení změn rozhraní API

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, a `IsStoreGeneratedAlways` bylo vyřazeno a nahradí [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) a [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Tyto chování platí pro všechny vlastnosti (nejen generovaných úložištěm vlastnosti) a určit, jak hodnota vlastnosti má být použita při vkládání do řádku databáze (`BeforeSaveBehavior`) nebo když existující databáze se aktualizuje řádek (`AfterSaveBehavior`).

Vlastnosti jsou označeny jako [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (třeba u počítaných sloupcích) bude ve výchozím nastavení ignorovat libovolná hodnota aktuálně nastavená na vlastnost. To znamená, že hodnota generovaná úložištěm se budou získávat vždy bez ohledu na to, jestli nastavit nebo změnit u sledovaných entity libovolnou hodnotu. To se dá změnit tak, že nastavíte jinou `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Nové chování při odstraňování ClientSetNull

V předchozích verzích [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) měl chování entit sledován pomocí funkce kontextu, že více uzavřený odpovídající `SetNull` sémantiku. V EF Core 2.0 nový `ClientSetNull` chování je zavedený jako výchozí pro volitelné vztahy. Toto chování je `SetNull` sémantiku sledované entit a `Restrict` chování pro databáze vytvořené využitím EF Core. Podle našich zkušeností jedná se očekával/nejužitečnější chování sledované entit a databáze. `DeleteBehavior.Restrict` Zachovaný nyní sledované entit, pokud jsou nastavené pro volitelné vztahy.

### <a name="provider-design-time-packages-removed"></a>Balíčky návrhu zprostředkovatele odebrat

`Microsoft.EntityFrameworkCore.Relational.Design` Odebral balíček. Jeho obsah konsolidovalo do `Microsoft.EntityFrameworkCore.Relational` a `Microsoft.EntityFrameworkCore.Design`.

To se rozšíří do balíčků návrhu zprostředkovatele. Tyto balíčky (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`atd) byly odebrány a jejich obsah konsolidované do hlavní poskytovatel balíčky.

Chcete-li povolit `Scaffold-DbContext` nebo `dotnet ef dbcontext scaffold` v EF Core 2.0, je potřeba jenom odkazujte na balíček jednoho zprostředkovatele:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
