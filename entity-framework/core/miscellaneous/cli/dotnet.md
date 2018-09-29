---
title: EF Core referenční dokumentace nástrojů .NET CLI () – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/20/2018
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: a280aad0344a89c41c30be27a249df3c28c44c70
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447167"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Reference – rozhraní příkazového řádku .NET nástroje Entity Framework Core

Nástroje rozhraní příkazového řádku (CLI) pro Entity Framework Core provádět úlohy vývojem během návrhu. Například vytvořit [migrace](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations)použít migrace a generovat kód pro model založený na existující databázi. Příkazy jsou rozšíření pro různé platformy [dotnet](/dotnet/core/tools) příkaz, který je součástí sady [.NET Core SDK](https://www.microsoft.com/net/core). Tyto nástroje pracují s projekty .NET Core.

Pokud používáte Visual Studio, doporučujeme [Konzola správce balíčků nástroje](powershell.md) místo:
* Automaticky fungují s aktuální projekt vybraný v **Konzola správce balíčků** bez nutnosti ručně přepnout adresáře.
* Automaticky otevřou soubory generované záznamem pro příkaz po dokončení příkazu.

## <a name="installing-the-tools"></a>Instalace nástrojů

Postup instalace závisí na typu projektu a verzi:

* ASP.NET Core 2.1 nebo novější verze
* EF Core 2.x
* EF Core 1.x

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Nainstalujte aktuální [.NET Core SDK](https://www.microsoft.com/net/download/core). Sada SDK musí být nainstalovaný, i v případě, že máte nejnovější verzi sady Visual Studio 2017.

  To je vše, co je potřeba pro ASP.NET Core 2.1 +, proto `Microsoft.EntityFrameworkCore.Design` je součástí balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (ne jádro technologie ASP.NET)

`dotnet ef` Příkazy jsou součástí sady .NET Core SDK, ale chcete-li povolit příkazy budete muset nainstalovat `Microsoft.EntityFrameworkCore.Design` balíčku.

* Nainstalujte aktuální [.NET Core SDK](https://www.microsoft.com/net/download/core). Sada SDK musí být nainstalovaný, i v případě, že máte nejnovější verzi sady Visual Studio 2017.

* Nainstalujte nejnovější stabilní verze `Microsoft.EntityFrameworkCore.Design` balíčku. 

  ``` Console   
  dotnet add package Microsoft.EntityFrameworkCore.Design   
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* Instalace .NET Core SDK verze 2.1.200. Novější verze nejsou kompatibilní s nástrojů rozhraní příkazového řádku pro EF Core 1.0 a 1.1.

* Konfigurace aplikace pro použití 2.1.200 verze sady SDK tak, že upravíte jeho [global.json](/dotnet/core/tools/global-json) souboru. Tento soubor je obvykle součástí adresář řešení (jedna nad projektu). 

* Upravte soubor projektu a přidejte `Microsoft.EntityFrameworkCore.Tools.DotNet` jako `DotNetCliToolReference` položky. Nejnovější verzi 1.x, zadejte například: 1.1.6. Podívejte se na příklad souboru projektu na konci této části.

* Nainstalujte nejnovější verzi 1.x `Microsoft.EntityFrameworkCore.Design` balíčků, například:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Soubor projektu s obě přidat odkazy na balíčky, vypadá přibližně takto:

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  Odkaz na balíček s `PrivateAssets="All"` není vystavený projekty, které odkazují na tento projekt. Toto omezení je zvláště užitečná pro balíčky, které se obvykle používají pouze během vývoje.

### <a name="verify-installation"></a>Ověření instalace

Spuštěním následujících příkazů ověřte, zda jsou správně nainstalovány nástroje rozhraní příkazového řádku EF Core:

  ``` Console
  dotnet restore
  dotnet ef
  ```

Výstup z příkazu identifikuje verzi nástrojů používá:

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>Pomocí nástrojů

Před použitím nástroje, budete muset vytvořit projekt při spuštění nebo nastavte prostředí.

### <a name="target-project-and-startup-project"></a>Cílový projekt a projekt po spuštění

Příkazy odkazovat *projektu* a *spouštěný projekt*.

* *Projektu* se také označuje jako *cílový projekt* protože se jedná, kde příkazy přidávání nebo odebírání souborů. Ve výchozím nastavení je projekt v aktuálním adresáři na cílový projekt. Můžete zadat jiný projekt jako cílový projekt pomocí <nobr> `--project` </nobr> možnost.

* *Spouštěný projekt* je ta, kterou nástroje pro sestavení a spuštění. Nástroje třeba spustit kód aplikace v době návrhu a získat informace o projektu, jako je například připojovací řetězec databáze a konfigurace modelu. Ve výchozím nastavení je projekt v aktuálním adresáři spouštěný projekt. Můžete určit jiný projekt jako spouštěný projekt pomocí <nobr> `--startup-project` </nobr> možnost.

Projekt po spuštění a cílový projekt se často stejného projektu. Typický scénář, kde jsou samostatné projekty je těchto případech:

* EF Core třídy kontextu a entity jsou v knihovně tříd .NET Core.
* Aplikace konzoly .NET Core nebo webové aplikace odkazuje na knihovnu tříd.

Je také možné [migrace kódu do knihovny tříd nezávisle na EF Core kontextu](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Ostatní cílové platformy

Nástroje rozhraní příkazového řádku pracovat s projekty .NET Core a projekty rozhraní .NET Framework. Nemusí mít aplikace, které mají modelu EF Core v knihovně tříd .NET Standard, .NET Core nebo .NET Framework projektu. Například to platí pro aplikace Xamarin a univerzální platformu Windows. V takovém případě můžete vytvořit projekt konzolové aplikace .NET Core, jehož jediným účelem je tak, aby fungoval jako projekt po spuštění pro nástroje. Projekt může být fiktivní projekt bez skutečné kódu &mdash; je pouze potřebných k poskytování cíl pro nástroje.

Proč je fiktivní projektu vyžaduje? Jak už bylo zmíněno dříve, mají nástroje k provádění kódu aplikace v době návrhu. K tomu, které potřebují na využití modulu runtime .NET Core. Když v projektu, který cílí na .NET Core nebo .NET Framework je model EF Core, si půjčte nástroje EF Core runtime z projektu. Nelze provést, pokud je model EF Core v knihovně tříd .NET Standard. .NET Standard není Skutečná implementace .NET; je specifikace sady rozhraní API, která musí podporovat implementace .NET. Proto .NET Standard není dostatečná pro EF Core nástroje k provádění kódu aplikace. Fiktivní projekt, který vytvoříte pro použití jako spouštěný projekt obsahuje konkrétní cílovou platformu, do kterého nástroje můžete načíst knihovně tříd rozhraní .NET Standard. 

### <a name="aspnet-core-environment"></a>Prostředí ASP.NET Core

Chcete-li určit prostředí pro projekty ASP.NET Core, nastavte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí před spuštěním příkazů.

## <a name="common-options"></a>Společné možnosti

|                   | Možnost                             | Popis                                                                                                                                                                                                                                                   |
|-------------------|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                           | Zobrazit výstup ve formátu JSON.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`            | `DbContext` Třídu použít. Název třídy pouze nebo plně kvalifikovaný s obory názvů.  Pokud je tento parametr vynechán, EF Core najdete třídy kontextu. Pokud existuje více tříd kontextu, tato možnost je vyžadována.                                            |
| `-p`              | `--project <PROJECT>`              | Relativní cesta ke složce projektu na cílový projekt.  Výchozí hodnota je do aktuální složky.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`      | Relativní cesta ke složce projektu po spuštění projektu. Výchozí hodnota je do aktuální složky.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`          | [Moniker cílového rozhraní](/dotnet/standard/frameworks#supported-target-framework-versions) pro [Cílová architektura](/dotnet/standard/frameworks).  Použijte, pokud soubor projektu určuje více cílových platforem, a chcete vybrat jeden z nich. |
|                   | `--configuration <CONFIGURATION>`  | Konfigurace sestavení, například: `Debug` nebo `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`           | Identifikátor cílový modul runtime pro obnovování balíčků pro. Seznam identifikátorů modulů Runtime (RID), najdete v článku [katalog identifikátorů RID](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                           | Zobrazit informace nápovědy.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                        | Zobrazit podrobný výstup.                                                                                                                                                                                                                                          |
|                   | `--no-color`                       | Není barevně zvýrazňovat výstup.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                  | Předpona výstup s úrovní.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>DotNet ef database přetažení

Zahodí databáze.

Možnosti:

|                   | Možnost                   | Popis                                                |
|-------------------|--------------------------|------------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Není potvrzení.                                             |
|                   | <nobr>`--dry-run`</nobr> | Zobrazit, které databáze bude vyřazena, ale ji.   |

## <a name="dotnet-ef-database-update"></a>aktualizace databáze ef DotNet

Aktualizace databáze na poslední migraci nebo zadaný migrace.

Argumenty:

| Argument       | Popis                                                                                                                                                                                                                                                     |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>`  | Cíl migrace. Migrace může identifikovat podle názvu nebo podle ID. Zvláštní případ, který znamená, že je číslo 0 *před první migraci* a způsobí, že všechny migrace se vrátí zpátky. Pokud není zadána žádná migrace, výchozí příkaz na poslední migrace. |

Následující příklady aktualizujte databázi na zadané migrace. Název migrace používá první a druhé používá ID migrace:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>informace o DotNet ef dbcontext

Získá informace `DbContext` typu.

## <a name="dotnet-ef-dbcontext-list"></a>DotNet ef dbcontext v seznamu

Seznamy, které jsou k dispozici `DbContext` typy.

## <a name="dotnet-ef-dbcontext-scaffold"></a>DotNet ef dbcontext vygenerované uživatelské rozhraní

Generuje kód `DbContext` a typy entit pro databázi.

Argumenty:

| Argument        | Popis                                                                                                                                                                                                             |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>`  | Připojovací řetězec k databázi. Pro projekty ASP.NET Core 2.x, může být hodnota *název =\<název připojovacího řetězce >*. Název v tomto případě pochází ze zdroje konfigurace, které jsou nastavené pro projekt. |
| `<PROVIDER>`    | Zprostředkovatel k použití. Obvykle jde o název balíčku NuGet, například: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Možnosti:

|                   | Možnost                                    | Popis                                                                                                                                                                    |
|-------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr>   | `--data-annotations`                      | Atributy lze použijte ke konfiguraci modelu (Pokud je to možné). Pokud je tento parametr vynechán, použije se pouze rozhraní fluent API.                                                                |
| `-c`              | `--context <NAME>`                        | Název `DbContext` k vygenerování.                                                                                                                                 |
|                   | `--context-dir <PATH>`                    | Adresář, který chcete vložit `DbContext` soubor třídy v. Cesty jsou relativní vzhledem k adresáři projektu. Obory názvů jsou odvozeny z názvy složek.                                 |
| `-f`              | `--force`                                 | Přepište existující soubory.                                                                                                                                                      |
| `-o`              | `--output-dir <PATH>`                     | Adresář, který se má vložit soubory tříd entit. Cesty jsou relativní vzhledem k adresáři projektu.                                                                                       |
|                   | <nobr>`--schema <SCHEMA_NAME>...`</nobr>  | Schémata tabulek ke generování typů entit pro. Chcete-li zadat více schémat, opakujte `--schema` pro každé z nich. Pokud je tento parametr vynechán, budou zahrnuty všem schématům.          |
| `-t`              | `--table <TABLE_NAME>`...                 | Tabulky pro typy entit pro generování. Chcete-li zadat více tabulek, opakujte `-t` nebo `--table` pro každé z nich. Pokud je tento parametr vynechán, budou zahrnuty všechny tabulky.                |
|                   | `--use-database-names`                    | Použijte názvy tabulek a sloupců přesně tak, jak jsou uvedeny v databázi. Pokud je tento parametr vynechán, aby lépe odpovídaly konvence stylu název jazyka C# změna názvů databáze. |

Následující příklad vygeneruje uživatelské rozhraní všechny schémat a tabulek a umístí nové soubory *modely* složky.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

Následující příklad vygeneruje uživatelské rozhraní pouze vybrané tabulky a vytvoří kontext do samostatné složky se zadaným názvem:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>Přidejte migraci ef DotNet

Přidá novou migraci.

Argumenty:

| Argument  | Popis                  |
|-----------|------------------------------|
| `<NAME>`  | Název migrace.   |

Možnosti:

|                   | Možnost                              | Popis                                                                                                        |
|-------------------|-------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr>  | Adresáři (a v podřízeném oboru názvů) používat. Cesty jsou relativní vzhledem k adresáři projektu. Výchozí hodnota je "Migrace".   |

## <a name="dotnet-ef-migrations-list"></a>Přehled migrace ef DotNet

Zobrazí seznam dostupných migrace.

## <a name="dotnet-ef-migrations-remove"></a>odebrat migrace ef DotNet

Odebere poslední migrace (vrátí zpět změny kódu, které jste dokončili migraci). 

Možnosti:

|                   | Možnost    | Popis                                                                        |
|-------------------|-----------|------------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Vrácení migrace (vrátit zpět změny, které byly použity k databázi).    |

## <a name="dotnet-ef-migrations-script"></a>skript migrace ef DotNet

Vygeneruje skript SQL z migrace.

Argumenty:

| Argument  | Popis                                                                                                                                                   |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>`  | Počáteční migraci. Migrace může identifikovat podle názvu nebo podle ID. Zvláštní případ, který znamená, že je číslo 0 *před první migraci*. Výchozí hodnota je 0. |
| `<TO>`    | Dokončení migrace. Výchozí hodnota je poslední migrace.                                                                                                         |

Možnosti:

|                   | Možnost             | Popis                                                          |
|-------------------|--------------------|----------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>`  | Soubor pro zápis skript, který chcete.                                     |
| `-i`              | `--idempotent`     | Generovat skript, který jde použít na databáze v libovolné migrace.   |

Následující příklad vytvoří skript pro migraci InitialCreate:

```console
dotnet ef migrations script 0 InitialCreate
```

Následující příklad vytvoří skript pro všechny migrace po migraci InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```