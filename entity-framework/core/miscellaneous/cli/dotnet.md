---
title: .NET core CLI – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489606"
---
<a name="ef-core-net-command-line-tools"></a>Nástroje příkazového řádku .NET EF Core
===============================
Nástroje příkazového řádku .NET Core Entity Framework jsou rozšíření pro různé platformy **dotnet** příkaz, který je součástí sady [.NET Core SDK][2].

> [!TIP]
> Pokud používáte Visual Studio, doporučujeme [nástroje PMC] [ 1] místo toho od té doby poskytují více integrované prostředí.

<a name="installing-the-tools"></a>Instalace nástrojů
--------------------
> [!NOTE]
> Sada .NET Core SDK verze 2.1.300 a novější obsahuje **dotnet ef** příkazy, které jsou kompatibilní s EF Core 2.0 a novějších verzích. Proto pokud používáte nejnovější verze sady .NET Core SDK a modul runtime EF Core, bez instalace je vyžadován a zbytku této části můžete ignorovat.
>
> Na druhé straně **dotnet ef** nástroj obsažené ve verzi .NET Core SDK 2.1.300 a novějších verzí není kompatibilní s verzí EF Core 1.0 a 1.1. Předtím, než můžete pracovat s projektem, který používá tyto starší verze EF Core na počítači s .NET Core SDK 2.1.300 nebo novější nainstalován, je nutné také nainstalovat verzi 2.1.200 nebo starší sady SDK a nakonfigurovat aplikaci, aby používala tuto starší verzi změnou jeho  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) souboru. Tento soubor je obvykle součástí adresář řešení (jedna nad projektu). Pak pokračujte podle pokynů installlation níže.

V předchozích verzích sady .NET Core SDK můžete nainstalovat nástroje pro .NET příkazového řádku EF Core pomocí těchto kroků:

1. Upravte soubor projektu a přidejte Microsoft.EntityFrameworkCore.Tools.DotNet jako položka DotNetCliToolReference (viz níže)
2. Spusťte následující příkazy:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

Výsledný projekt by měl vypadat přibližně takto:

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> Odkaz na balíček s `PrivateAssets="All"` znamená, že není vystavený projekty, které odkazují na tento projekt, což je užitečné zejména pro balíčky, které se obvykle používají pouze během vývoje.

Pokud jste to udělali všechno hned, byste měli mít k úspěšnému spuštění následujícího příkazu v příkazovém řádku.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Pomocí nástrojů
---------------
Při každém vyvolání příkazu, existují dva projekty zahrnuté:

Cílový projekt je tam, kde se přidají všechny soubory (nebo v některých případech odebrána). Výchozí hodnota je projekt v aktuálním adresáři na cílový projekt, ale můžete změnit pomocí <nobr> **`--project`** </nobr> možnost.

Projekt po spuštění je emulován nástroji při spuštění kódu projektu. Je také ve výchozím nastavení do aktuálního adresáře projektu, ale lze změnit pomocí **`--startup-project`** možnost.

> [!NOTE]
> Aktualizuje se databáze webové aplikace, který má nainstalovaný v jiném projektu EF Core, by vypadalo takto: `dotnet ef database update --project {project-path}` (v adresáři webové aplikace)

Běžné možnosti:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | Zobrazit výstup ve formátu JSON.           |
| -c | `--context <DBCONTEXT>`           | Chcete-li použít uvolněn objekt DbContext.       |
| -p | `--project <PROJECT>`             | Projekt pro použití.         |
| -s | `--startup-project <PROJECT>`     | Použít spouštěný projekt. |
|    | `--framework <FRAMEWORK>`         | Cílová architektura.       |
|    | `--configuration <CONFIGURATION>` | Konfigurace pro použití.   |
|    | `--runtime <IDENTIFIER>`          | Modul runtime pro použití.         |
| -h | `--help`                           | Zobrazit informace nápovědy.      |
| -v | `--verbose`                        | Zobrazit podrobný výstup.        |
|    | `--no-color`                       | Není barevně zvýrazňovat výstup.      |
|    | `--prefix-output`                  | Předpona výstup s úrovní.   |


> [!TIP]
> Chcete-li určit prostředí ASP.NET Core, nastavte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí před spuštěním.

<a name="commands"></a>Příkazy
--------

### <a name="dotnet-ef-database-drop"></a>DotNet ef database přetažení

Zahodí databáze.

Možnosti:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | `--force`   | Není potvrzení.                                           |
|    | `--dry-run` | Zobrazit, které databáze bude vyřazena, ale ji. |

### <a name="dotnet-ef-database-update"></a>aktualizace databáze ef DotNet

Aktualizace databáze na zadané migrace.

Argumenty:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | Cíl migrace. Pokud je 0, se vrátí zpět všechny migrace. Výchozí hodnota je poslední migrace. |

### <a name="dotnet-ef-dbcontext-info"></a>informace o DotNet ef dbcontext

Získá informace o typu DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>DotNet ef dbcontext v seznamu

Zobrazí seznam dostupných typů DbContext.

### <a name="dotnet-ef-dbcontext-scaffold"></a>DotNet ef dbcontext vygenerované uživatelské rozhraní

Nástroj scaffold kontext databáze a entity typy pro databázi.

Argumenty:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | Připojovací řetězec k databázi.                                      |
| `<PROVIDER>`   | Zprostředkovatel k použití. (například Microsoft.EntityFrameworkCore.SqlServer) |

Možnosti:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                      | Atributy lze použijte ke konfiguraci modelu (Pokud je to možné). Pokud tento parametr vynechán, použije se pouze rozhraní fluent API. |
| -c              | `--context <NAME>`                       | Název uvolněn objekt DbContext.                                                                       |
|                 | `--context-dir <PATH>`                   | Adresář, který se má vložit DbContext. Cesty jsou relativní vzhledem k adresáři projektu.             |
| -f              | `--force`                                 | Přepište existující soubory.                                                                        |
| -o              | `--output-dir <PATH>`                    | Adresář, který se má vložit soubory. Cesty jsou relativní vzhledem k adresáři projektu.                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Schémata tabulek ke generování typů entit pro.                                              |
| -t              | `--table <TABLE_NAME>`...                | Tabulky pro typy entit pro generování.                                                         |
|                 | `--use-database-names`                    | Použijte názvy tabulek a sloupců přímo z databáze.                                           |

### <a name="dotnet-ef-migrations-add"></a>Přidejte migraci ef DotNet

Přidá novou migraci.

Argumenty:

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | Název migrace. |

Možnosti:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr> `--output-dir <PATH>` </nobr> | Adresáři (a v podřízeném oboru názvů) používat. Cesty jsou relativní vzhledem k adresáři projektu. Výchozí hodnota je "Migrace". |

### <a name="dotnet-ef-migrations-list"></a>Přehled migrace ef DotNet

Zobrazí seznam dostupných migrace.

### <a name="dotnet-ef-migrations-remove"></a>odebrat migrace ef DotNet

Odebere poslední migrace.

Možnosti:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | `--force` | Vrácení migrace, pokud byl použit k databázi. |

### <a name="dotnet-ef-migrations-script"></a>skript migrace ef DotNet

Vygeneruje skript SQL z migrace.

Argumenty:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | Počáteční migraci. Výchozí hodnota je 0 (výchozí databáze). |
| `<TO>`   | Dokončení migrace. Výchozí hodnota je poslední migrace.         |

Možnosti:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | `--output <FILE>` | Soubor pro zápis výsledek.                                   |
| -i | `--idempotent`     | Generovat skript, který jde použít na databáze v libovolné migrace. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
