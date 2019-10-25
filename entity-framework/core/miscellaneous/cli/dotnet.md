---
title: Referenční dokumentace k nástrojům EF Core (rozhraní .NET CLI) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 07/11/2019
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 29434c26a503fabb16b43ee8f0c36136a0b5b745
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811974"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Referenční informace k nástrojům pro Entity Framework Core – .NET CLI

Nástroje rozhraní příkazového řádku (CLI) pro Entity Framework Core provádět úlohy vývoje v době návrhu. Například vytvářejí [migrace](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), aplikují migrace a generují kód pro model založený na stávající databázi. Příkazy jsou rozšířením příkazu [dotnet](/dotnet/core/tools) pro různé platformy, který je součástí [.NET Core SDK](https://www.microsoft.com/net/core). Tyto nástroje fungují v projektech .NET Core.

Pokud používáte sadu Visual Studio, doporučujeme místo toho použít [Nástroje konzoly Správce balíčků](powershell.md) :

* Automaticky pracují s aktuálním projektem vybraným v **konzole správce balíčků** bez nutnosti ručně přepínat adresáře.
* Po dokončení příkazu automaticky otevřou soubory vygenerované příkazem.

## <a name="installing-the-tools"></a>Instalace nástrojů

Postup instalace závisí na typu a verzi projektu:

* EF Core 3. x
* ASP.NET Core verze 2,1 a novější
* EF Core 2. x
* EF Core 1. x

### <a name="ef-core-3x"></a>EF Core 3. x

* `dotnet ef` musí být nainstalovaný jako globální nebo místní nástroj. Většina vývojářů se `dotnet ef` jako globální nástroj nainstaluje pomocí následujícího příkazu:

  ``` console
  dotnet tool install --global dotnet-ef
  ```

  `dotnet ef` můžete použít také jako místní nástroj. Chcete-li jej použít jako místní nástroj, obnovte závislosti projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).

* Nainstalujte [.NET Core SDK 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)). Sada SDK musí být nainstalována i v případě, že máte nejnovější verzi sady Visual Studio.

* Nainstalujte nejnovější balíček `Microsoft.EntityFrameworkCore.Design`.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Nainstalujte aktuální [.NET Core SDK](https://www.microsoft.com/net/download/core). Sada SDK musí být nainstalována i v případě, že máte nejnovější verzi sady Visual Studio 2017.

  To je všechno, co je potřeba pro ASP.NET Core 2.1 +, protože balíček `Microsoft.EntityFrameworkCore.Design` obsahuje [Microsoft. AspNetCore. app Metapackage](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2. x (není ASP.NET Core)

Příkazy `dotnet ef` jsou součástí .NET Core SDK, ale k povolení příkazů, které musíte nainstalovat `Microsoft.EntityFrameworkCore.Design` balíček.

* Nainstalujte aktuální [.NET Core SDK](https://www.microsoft.com/net/download/core). Sada SDK musí být nainstalována i v případě, že máte nejnovější verzi sady Visual Studio.

* Nainstalujte nejnovější stabilní balíček `Microsoft.EntityFrameworkCore.Design`.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1. x

* Nainstalujte .NET Core SDK verze 2.1.200. Novější verze nejsou kompatibilní s nástroji rozhraní příkazového řádku pro EF Core 1,0 a 1,1.

* Nakonfigurujte aplikaci tak, aby používala verzi sady 2.1.200 SDK úpravou jejího [globálního souboru. JSON](/dotnet/core/tools/global-json) . Tento soubor je obvykle zahrnutý v adresáři řešení (jeden nad projektem).

* Upravte soubor projektu a přidejte `Microsoft.EntityFrameworkCore.Tools.DotNet` jako položku `DotNetCliToolReference`. Zadejte nejnovější verzi 1. x, například: 1.1.6. Podívejte se na příklad soubor projektu na konci této části.

* Nainstalujte nejnovější verzi balíčku `Microsoft.EntityFrameworkCore.Design` (1. x), například:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Jak byly přidány oba odkazy na balíčky, soubor projektu vypadá nějak takto:

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

  Odkaz na balíček s `PrivateAssets="All"` není zveřejněn pro projekty, které odkazují na tento projekt. Toto omezení je zvláště užitečné pro balíčky, které se obvykle používají jenom během vývoje.

### <a name="verify-installation"></a>Ověřit instalaci

Spusťte následující příkazy, abyste ověřili, že EF Core nástroje rozhraní příkazového řádku jsou správně nainstalované:

  ``` Console
  dotnet restore
  dotnet ef
  ```

Výstup příkazu určuje verzi používaných nástrojů:

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

## <a name="using-the-tools"></a>Používání nástrojů

Před použitím těchto nástrojů možná budete muset vytvořit projekt po spuštění nebo nastavit prostředí.

### <a name="target-project-and-startup-project"></a>Cílový projekt a projekt po spuštění

Příkazy odkazují na *projekt* a na *spouštěný projekt*.

* *Projekt* je také označován jako *cílový projekt* , protože je tam, kde příkazy přidávají nebo odebírají soubory. Ve výchozím nastavení je projekt v aktuálním adresáři cílovým projektem. Můžete určit jiný projekt jako cílový projekt pomocí možnosti <nobr>`--project`</nobr> .

* Spouštěný *projekt* je ten, který nástroje sestavují a spouštějí. Nástroje musí spustit kód aplikace v době návrhu, aby získali informace o projektu, jako je například připojovací řetězec databáze a konfigurace modelu. Ve výchozím nastavení je projekt v aktuálním adresáři spouštěným projektem. Můžete určit jiný projekt jako spouštěný projekt pomocí možnosti <nobr>`--startup-project`</nobr> .

Spouštěcí projekt a cílový projekt jsou často stejný projekt. Typický scénář, kde jsou samostatné projekty, je:

* EF Core kontext a třídy entit jsou v knihovně tříd .NET Core.
* Aplikace konzoly .NET Core nebo webová aplikace odkazuje na knihovnu tříd.

Je také možné [umístit kód migrace do knihovny tříd odděleně od EF Coreho kontextu](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Další cílová rozhraní

Nástroje rozhraní příkazového řádku fungují s projekty .NET Core a projekty .NET Framework. Aplikace s modelem EF Core v knihovně tříd .NET Standard nemusí mít projekt .NET Core nebo .NET Framework. Například to platí pro Xamarin a Univerzální platforma Windows aplikace. V takových případech můžete vytvořit projekt konzolové aplikace .NET Core, jehož jediným účelem je jednat jako projekt po spuštění pro nástroje. Projekt může být fiktivní projekt bez reálného kódu, &mdash; je třeba zadat cíl pro nástroje.

Proč je vyžadován fiktivní projekt? Jak bylo zmíněno dříve, nástroje musí spustit kód aplikace v době návrhu. K tomu je potřeba použít modul runtime .NET Core. Když je model EF Core v projektu cíleném na rozhraní .NET Core nebo .NET Framework, EF Core nástroje vypůjčí modul runtime z projektu. Nemůžou to dělat, pokud je model EF Core v knihovně tříd .NET Standard. .NET Standard není skutečná implementace rozhraní .NET; je to specifikace sady rozhraní API, které musí implementace rozhraní .NET podporovat. Proto .NET Standard není dostačující pro EF Core nástroje pro spouštění kódu aplikace. Fiktivní projekt, který vytvoříte pro použití jako spouštěný projekt, poskytuje konkrétní cílovou platformu, do které mohou nástroje načíst .NET Standard knihovny tříd.

### <a name="aspnet-core-environment"></a>ASP.NET Core prostředí

Chcete-li určit prostředí pro ASP.NET Core projekty, nastavte před spuštěním příkazů proměnnou prostředí **ASPNETCORE_ENVIRONMENT** .

## <a name="common-options"></a>Společné možnosti

|                   | Možnost                            | Popis                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | Zobrazit výstup JSON.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | Třída `DbContext`, která se má použít Pouze název třídy nebo plně kvalifikovaný obory názvů.  Pokud je tato možnost vynechána, EF Core najde kontextovou třídu. Pokud existuje více kontextových tříd, je tato možnost povinná.                                            |
| `-p`              | `--project <PROJECT>`             | Relativní cesta ke složce projektu cílového projektu  Výchozí hodnota je aktuální složka.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | Relativní cesta ke složce projektu spouštěného projektu Výchozí hodnota je aktuální složka.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | [Moniker cílového rozhraní .NET Framework](/dotnet/standard/frameworks#supported-target-framework-versions) pro [cílovou architekturu](/dotnet/standard/frameworks).  Použijte, pokud soubor projektu určuje více cílových rozhraní a chcete vybrat jeden z nich. |
|                   | `--configuration <CONFIGURATION>` | Konfigurace sestavení, například: `Debug` nebo `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | Identifikátor cílového modulu runtime, pro který mají být obnoveny balíčky. Seznam identifikátorů modulu runtime (identifikátorů RID) najdete v [katalogu RID](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                          | Zobrazí informace o nápovědě.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | Zobrazit podrobný výstup.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | Zabarvovat výstup.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | Výstup předpony s úrovní.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>dotnet – vyřazení databáze EF

Zruší databázi.

Nastavení

|                   | Možnost                   | Popis                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Nepotvrzujte.                                           |
|                   | <nobr>`--dry-run`</nobr> | Zobrazit, která databáze bude vyřazena, ale neodstraňovat ji. |

## <a name="dotnet-ef-database-update"></a>dotnet – aktualizace databáze EF EF

Aktualizuje databázi na poslední migraci nebo na určenou migraci.

Náhodné

| Argument      | Popis                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | Cílová migrace Migrace může být identifikována podle názvu nebo podle ID. Číslo 0 je zvláštní případ, který znamená *před první migrací* a způsobí, že se všechny migrace vrátí zpět. Pokud není zadaná žádná migrace, příkaz se nastaví jako výchozí pro poslední migraci. |

V následujících příkladech je databáze aktualizována na určenou migraci. První používá název migrace a druhý používá ID migrace:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>dotnet EF DbContext – informace

Načte informace o typu `DbContext`.

## <a name="dotnet-ef-dbcontext-list"></a>dotnet EF – seznam DbContext

Zobrazí seznam dostupných typů `DbContext`.

## <a name="dotnet-ef-dbcontext-scaffold"></a>dotnet EF DbContext – generování uživatelského rozhraní

Generuje kód pro `DbContext` a typy entit pro databázi. Aby tento příkaz vygeneroval typ entity, musí mít databázová tabulka primární klíč.

Náhodné

| Argument       | Popis                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | Připojovací řetězec k databázi. U ASP.NET Core 2. x se hodnota může *jmenovat název =\<název připojovacího řetězce >* . V takovém případě název pochází ze zdrojů konfigurace, které jsou nastaveny pro projekt. |
| `<PROVIDER>`   | Poskytovatel, který se má použít. Obvykle se jedná o název balíčku NuGet, například: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Nastavení

|                 | Možnost                                   | Popis                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | Použijte atributy ke konfiguraci modelu (Pokud je to možné). Pokud je tato možnost vynechána, je použita pouze funkce Fluent API.                                                                |
| `-c`            | `--context <NAME>`                       | Název třídy `DbContext`, která se má generovat                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | Adresář, do kterého se má umístit soubor třídy `DbContext`. Cesty jsou relativní vzhledem k adresáři projektu. Obory názvů jsou odvozeny z názvů složek.                                 |
| `-f`            | `--force`                                | Přepsat existující soubory.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | Adresář, do kterého se mají vložit soubory třídy entity Cesty jsou relativní vzhledem k adresáři projektu.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Schémata tabulek, pro které se mají generovat typy entit Chcete-li zadat více schémat, opakujte `--schema` pro každé z nich. Pokud je tato možnost vynechána, jsou uvedena všechna schémata.          |
| `-t`            | `--table <TABLE_NAME>`...                | Tabulky, pro které se mají generovat typy entit Chcete-li zadat více tabulek, opakujte `-t` nebo `--table` pro každé z nich. Pokud je tato možnost vynechána, jsou zahrnuty všechny tabulky.                |
|                 | `--use-database-names`                   | Názvy tabulek a sloupců používejte přesně tak, jak se zobrazí v databázi. Pokud je tato možnost vynechána, názvy databází jsou změněny, aby lépe odpovídaly C# konvencím stylu názvu. |

Následující příklad vygeneruje všechna schémata a tabulky a vloží nové soubory do složky *modely* .

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

Následující příklad vytvoří pouze tabulky, které jsou vybrány pouze v tabulkách a vytvoří kontext v samostatné složce se zadaným názvem:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>Přidání migrace do dotnet EF

Přidá novou migraci.

Náhodné

| Argument | Popis                |
|:---------|:---------------------------|
| `<NAME>` | Název migrace. |

Nastavení

|                   | Možnost                             | Popis                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | Adresář (a dílčí obor názvů), který se má použít. Cesty jsou relativní vzhledem k adresáři projektu. Ve výchozím nastavení se jedná o "migrace". |

## <a name="dotnet-ef-migrations-list"></a>dotnet EF – seznam migrace

Zobrazí seznam dostupných migrací.

## <a name="dotnet-ef-migrations-remove"></a>odebrání migrace příkazu dotnet EF

Odebere poslední migraci (vrátí zpět změny kódu, které byly provedeny pro migraci).

Nastavení

|                   | Možnost    | Popis                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Obnovte migraci (vraťte zpět změny, které byly pro databázi aplikovány). |

## <a name="dotnet-ef-migrations-script"></a>skript pro migraci dotnet EF

Vygeneruje skript SQL z migrací.

Náhodné

| Argument | Popis                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | Spouští se migrace. Migrace může být identifikována podle názvu nebo podle ID. Číslo 0 je zvláštní případ, který znamená *před první migrací*. Výchozí hodnota je 0. |
| `<TO>`   | Koncová migrace. Výchozí hodnota je poslední migrace.                                                                                                         |

Nastavení

|                   | Možnost            | Popis                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | Soubor, do kterého se má skript zapsat.                                   |
| `-i`              | `--idempotent`    | Vygenerujte skript, který se dá použít v databázi při libovolné migraci. |

Následující příklad vytvoří skript pro migraci InitialCreate:

```console
dotnet ef migrations script 0 InitialCreate
```

Následující příklad vytvoří skript pro všechny migrace po migraci InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Další zdroje

* [Migrace](xref:core/managing-schemas/migrations/index)
* [Zpětná analýza](xref:core/managing-schemas/scaffolding)
