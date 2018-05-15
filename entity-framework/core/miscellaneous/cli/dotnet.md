---
title: .NET core rozhraní příkazového řádku - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a>Nástroje příkazového řádku .NET Core EF
===============================
Nástroje příkazového řádku .NET Core Entity Framework se rozšíření pro různé platformy **dotnet** příkaz, který je součástí systému [.NET Core SDK][2].

> [!TIP]
> Pokud používáte Visual Studio, doporučujeme [nástroje pomocí PMC] [ 1] místo, protože poskytují lepší integrace.

<a name="installing-the-tools"></a>Instalace nástrojů
--------------------
> [!NOTE]
> .NET Core SDK verze 2.1.300 a novější obsahuje **dotnet ef** příkazy, které jsou kompatibilní s EF základní 2.0 a novějšími verzemi. Proto pokud používáte nejnovější verze rozhraní .NET Core SDK a EF Core runtime, není třeba žádná instalace a zbývající část tohoto oddílu můžete ignorovat.
>
> Na druhé straně **dotnet ef** nástroj obsažené ve verzi rozhraní .NET Core SDK 2.1.300 a novějších není kompatibilní s EF základní verze 1.0 a 1.1. Než můžete pracovat s projekt, který používá tyto starší verze EF jádra na počítači, který má .NET Core SDK 2.1.300 nebo novější nainstalována, je nutné také nainstalovat verzi 2.1.200 nebo starší sady SDK a konfigurace aplikace pro použití této starší verze úpravou jeho  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) souboru. Tento soubor je obvykle součástí adresář řešení (předchozímu projekt). Pak můžete pokračovat s installlation pokynů níže.

Pro předchozí verze rozhraní .NET Core SDK můžete nainstalovat nástroje příkazového řádku EF základní v rozhraní .NET, pomocí těchto kroků:

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
> Odkaz balíček s `PrivateAssets="All"` znamená není vystavený projekty, které odkazují na tento projekt, který je obzvláště užitečná pro balíčky, které se obvykle používají pouze během vývoje.

Pokud jste to udělali všechno právo, byste měli moct úspěšně spuštěním následujícího příkazu v příkazovém řádku.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Pomocí nástrojů
---------------
Při každém vyvolání příkazu, existují dva projekty související se situací:

Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat). Cílový projekt výchozí nastavení na projekt v aktuálním adresáři, ale můžete změnit pomocí <nobr> **– projekt** </nobr> možnost.

Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu. Také výchozí hodnoty na projekt v aktuálním adresáři, ale můžete změnit pomocí **– projekt po spuštění** možnost.

> [!NOTE]
> Například aktualizace databáze webové aplikace, který má základní EF nainstalované v na jiný projekt bude vypadat takto: `dotnet ef database update --project {project-path}` (z adresáře vaší webové aplikace)

Běžné možnosti:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | – json                           | Zobrazit výstup JSON.           |
| -c | --kontextu \<DBCONTEXT >           | DbContext používat.       |
| -p | --projektu \<Projekt >             | Projekt, který používá.         |
| -s | --spouštěný projekt \<projektu >     | Spuštění projektu pro použití. |
|    | --framework \<FRAMEWORK >         | Cílové rozhraní.       |
|    | – konfigurace \<konfigurace > | Konfigurace používat.   |
|    | modul runtime – \<IDENTIFIKÁTOR >          | Modul runtime používat.         |
| -h | – Nápověda                           | Zobrazit informace nápovědy.      |
| -v | -verbose                        | Zobrazit podrobný výstup.        |
|    | – bez barvy                       | Nemáte Kolorovat výstup.      |
|    | – Předpona output                  | Předpona výstup s úrovní.   |


> [!TIP]
> Chcete-li zadat prostředí ASP.NET Core, nastavte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí dřív, než spustíte.

<a name="commands"></a>Příkazy
--------

### <a name="dotnet-ef-database-drop"></a>Vyřaďte databázi ef DotNet.

Zahodí databáze.

Možnosti:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | --Vynutit   | Nemáte potvrďte.                                           |
|    | – Test | Zobrazit databázi, kterou by být vyřazen, ale nemáte vyřadit. |

### <a name="dotnet-ef-database-update"></a>aktualizace databáze ef DotNet.

Aktualizuje databázi na zadané migrace.

Argumenty:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<MIGRACE &GT; | Cílová migrace. Pokud je 0, budou vráceny všechny migrace. Výchozí hodnoty na poslední migrace. |

### <a name="dotnet-ef-dbcontext-info"></a>informace o dbcontext ef DotNet.

Získá informace o typu DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>seznam dbcontext ef DotNet.

Jsou uvedeny dostupné typy DbContext.

### <a name="dotnet-ef-dbcontext-scaffold"></a>DotNet ef dbcontext vygenerované uživatelské rozhraní

Scaffold a typy DbContext a entity pro databázi.

Argumenty:

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| \<PŘIPOJENÍ &GT; | Připojovací řetězec k databázi.                              |
| \<ZPROSTŘEDKOVATEL &GT;   | Zprostředkovatel, který se má používat. (Např.) Microsoft.EntityFrameworkCore.SqlServer) |

Možnosti:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --datovými poznámkami                      | Pomocí atributů (kde je to možné), nakonfigurujte model. Pokud tento parametr vynechán, použije se rozhraní fluent API. |
| -c              | --kontextu \<NAME >                       | Název DbContext.                                                                       |
|                 | --kontextu dir \<cesta >                   | Adresář, který chcete umístit do souboru DbContext. Cesty se vztahují k adresáři projektu.             |
| -f              | --Vynutit                                 | Přepište existující soubory.                                                                        |
| -o              | --výstup dir \<cesta >                    | Adresář, který chcete umístit soubory do. Cesty se vztahují k adresáři projektu.                      |
|                 | <nobr>--schema \<SCHEMA_NAME>...</nobr> | Schémata tabulkami k vygenerování typy entit pro.                                              |
| -t              | --tabulky \<TABLE_NAME >...                | S tabulkami k vygenerování typy entit pro.                                                         |
|                 | --use-database-names                    | Používejte názvy tabulek a sloupců přímo z databáze.                                           |

### <a name="dotnet-ef-migrations-add"></a>Přidat migrace ef DotNet.

Přidá nové migrace.

Argumenty:

|         |                            |
|:--------|:---------------------------|
| \<NAME &GT; | Název migrace. |

Možnosti:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--výstup dir \<cesta ></nobr> | Adresář (a podklíčů – obor názvů) používat. Cesty se vztahují k adresáři projektu. Výchozí hodnota je "Migrace". |

### <a name="dotnet-ef-migrations-list"></a>seznam migrace ef DotNet.

Zobrazí seznam dostupných migrace.

### <a name="dotnet-ef-migrations-remove"></a>odebrat migrace ef DotNet.

Odebere poslední migrace.

Možnosti:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | --Vynutit | Migrace vrátit, pokud nebyla použita k databázi. |

### <a name="dotnet-ef-migrations-script"></a>skript migrace ef DotNet.

Generuje skriptu SQL z migrace.

Argumenty:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<Z &GT; | Počáteční migrace. Výchozí hodnota je 0 (počáteční databáze). |
| \<TO>   | Koncová migrace. Výchozí hodnoty na poslední migrace.         |

Možnosti:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | --výstup \<souboru > | Soubor k zápisu výsledek, který má.                                   |
| -i | --idempotent     | Generovat skript, který lze použít v databázi v žádné migrace. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
