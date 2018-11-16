---
title: EF Core tools odkaz (konzola Správce balíčků) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 468698d1bbd17d4ad10b1b1601bfbc315a01c1ff
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688703"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Entity Framework Core tools reference - Konzola správce balíčků v sadě Visual Studio

Konzola správce balíčků (PMC) nástroje pro Entity Framework Core provádět úlohy vývojem během návrhu. Například vytvořit [migrace](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations)použít migrace a generovat kód pro model založený na existující databázi. Příkazy se spouští v rámci služby Visual Studio pomocí [Konzola správce balíčků](/nuget/tools/package-manager-console). Tyto nástroje pracují s projekty rozhraní .NET Framework a .NET Core.

Pokud nepoužíváte Visual Studio, doporučujeme [nástroje příkazového řádku EF Core](dotnet.md) místo. Nástroje rozhraní příkazového řádku jsou různé platformy a spusťte v příkazovém řádku.

## <a name="installing-the-tools"></a>Instalace nástrojů

Postupy pro instalaci a aktualizaci nástroje se liší mezi ASP.NET Core 2.1 + a starší verze nebo jiné typy projektů.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core 2.1 nebo novější verze

Nástroje jsou automaticky obsažené v projektu aplikace ASP.NET Core 2.1 +, protože `Microsoft.EntityFrameworkCore.Tools` je součástí balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](/aspnet/core/fundamentals/metapackage-app).

Proto nemusíte dělat nic. instalace nástrojů, ale budete muset:
* Obnovení balíčků před použitím nástroje v novém projektu.
* Instalovat balíček aktualizace na novější verzi nástrojů.

Pokud chcete mít jistotu, že se zobrazuje nejnovější verzi nástrojů, doporučujeme také provést následující kroky:

* Upravit vaše *.csproj* a přidejte řádek určující nejnovější verzi [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) balíčku. Například *.csproj* může obsahovat soubor `ItemGroup` , která vypadá takto:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Aktualizace nástrojů, když se zobrazí zpráva jako v následujícím příkladu:

> Verze nástrojů EF Core "2.1.1-rtm-30846" je starší než modul runtime "2.1.3-rtm-32065". Aktualizace nástrojů pro nejnovější funkce a opravy chyb.

Aktualizace nástrojů:
* Instalace nejnovější .NET Core SDK.
* Aktualizace na nejnovější verzi sady Visual Studio.
* Upravit *.csproj* souboru tak, že obsahují odkaz na balíček na nejnovější nástroje pro balíček, jak je uvedeno výše.

### <a name="other-versions-and-project-types"></a>Jiné verze a typy projektů

Nainstalujte nástroje konzoly Správce balíčků spuštěním následujícího příkazu v **Konzola správce balíčků**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Aktualizace nástrojů spuštěním následujícího příkazu v **Konzola správce balíčků**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Ověření instalace

Ověřte, zda jsou nástroje nainstalovány spuštěním tohoto příkazu:

``` powershell
Get-Help about_EntityFrameworkCore
```

Výstup vypadá takto (neříká je verze nástrojů, které používáte):

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a>Pomocí nástrojů

Před použitím nástroje:
* Pochopili rozdíl mezi cíl a spuštění projektu.
* Zjistěte, jak pomocí nástrojů pomocí knihovny tříd .NET Standard.
* Pro projekty ASP.NET Core nastavte prostředí.

### <a name="target-and-startup-project"></a>Cíl a spuštění projektu

Příkazy odkazovat *projektu* a *spouštěný projekt*.

* *Projektu* se také označuje jako *cílový projekt* protože se jedná, kde příkazy přidávání nebo odebírání souborů. Ve výchozím nastavení **výchozí projekt** vybrané v **Konzola správce balíčků** je na cílový projekt. Můžete zadat jiný projekt jako cílový projekt pomocí <nobr> `--project` </nobr> možnost.

* *Spouštěný projekt* je ta, kterou nástroje pro sestavení a spuštění. Nástroje třeba spustit kód aplikace v době návrhu a získat informace o projektu, jako je například připojovací řetězec databáze a konfigurace modelu. Ve výchozím nastavení **spouštěný projekt** v **Průzkumníku řešení** je projekt po spuštění. Můžete určit jiný projekt jako spouštěný projekt pomocí <nobr> `--startup-project` </nobr> možnost.

Projekt po spuštění a cílový projekt se často stejného projektu. Typický scénář, kde jsou samostatné projekty je těchto případech:

* EF Core třídy kontextu a entity jsou v knihovně tříd .NET Core.
* Aplikace konzoly .NET Core nebo webové aplikace odkazuje na knihovnu tříd.

Je také možné [migrace kódu do knihovny tříd nezávisle na EF Core kontextu](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Ostatní cílové platformy

Konzola správce balíčků nástroje pracovat s projekty .NET Core nebo .NET Framework. Nemusí mít aplikace, které mají modelu EF Core v knihovně tříd .NET Standard, .NET Core nebo .NET Framework projektu. Například to platí pro aplikace Xamarin a univerzální platformu Windows. V takovém případě můžete vytvořit .NET Core nebo .NET Framework projektu aplikace konzoly jehož jediným účelem je tak, aby fungoval jako projekt po spuštění pro nástroje. Projekt může být fiktivní projekt bez skutečné kódu &mdash; je pouze potřebných k poskytování cíl pro nástroje.

Proč je fiktivní projektu vyžaduje? Jak už bylo zmíněno dříve, mají nástroje k provádění kódu aplikace v době návrhu. K tomu, které potřebují na využití modulu runtime .NET Core nebo .NET Framework. Když v projektu, který cílí na .NET Core nebo .NET Framework je model EF Core, si půjčte nástroje EF Core runtime z projektu. Nelze provést, pokud je model EF Core v knihovně tříd .NET Standard. .NET Standard není Skutečná implementace .NET; je specifikace sady rozhraní API, která musí podporovat implementace .NET. Proto .NET Standard není dostatečná pro EF Core nástroje k provádění kódu aplikace. Fiktivní projekt, který vytvoříte pro použití jako spouštěný projekt obsahuje konkrétní cílovou platformu, do kterého nástroje můžete načíst knihovně tříd rozhraní .NET Standard.

### <a name="aspnet-core-environment"></a>Prostředí ASP.NET Core

Chcete-li určit prostředí pro projekty ASP.NET Core, nastavte **env:ASPNETCORE_ENVIRONMENT** před spuštěním příkazů.

## <a name="common-parameters"></a>Společné parametry

V následující tabulce jsou uvedeny parametry, které jsou společné pro všechny příkazy EF Core:

| Parametr                 | Popis                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Kontextu \<řetězec >        | `DbContext` Třídu použít. Název třídy pouze nebo plně kvalifikovaný s obory názvů.  Pokud je tento parametr vynechán, vyhledá EF Core třídy kontextu. Pokud existuje více tříd kontextu, tento parametr je povinný. |
| -Projektu \<řetězec >        | Cílový projekt. Pokud je tento parametr vynechán, **výchozí projekt** pro **Konzola správce balíčků** se používá jako cílový projekt.                                                                             |
| – Výchozí projekt \<řetězec > | Spouštěný projekt. Pokud je tento parametr vynechán, **spouštěný projekt** v **vlastnosti řešení** se používá jako cílový projekt.                                                                                 |
| -Verbose                  | Zobrazit podrobný výstup.                                                                                                                                                                                                 |

Chcete-li zobrazit nápovědu informace o příkazu, pomocí Powershellu `Get-Help` příkazu.

> [!TIP]
> Parametry kontextu, projektu a výchozí projekt podporují rozšíření tab.

## <a name="add-migration"></a>Přidat migrace

Přidá novou migraci.

Parametry:

| Parametr                         | Popis                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Název \<řetězec ><nobr>       | Název migrace. Toto je pozičních parametrů a je povinný.                                              |
| <nobr>-OutputDir \<řetězec ></nobr> | Adresáři (a v podřízeném oboru názvů) používat. Cesty jsou relativní vzhledem k adresáři projektu cíl. Výchozí hodnota je "Migrace". |

## <a name="drop-database"></a>Odstranění databáze

Zahodí databáze.

Parametry:

| Parametr | Popis                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Zobrazit, které databáze bude vyřazena, ale ji. |

## <a name="get-dbcontext"></a>Get-DbContext

Seznamy, které jsou k dispozici `DbContext` typy.

## <a name="remove-migration"></a>Remove migrace

Odebere poslední migrace (vrátí zpět změny kódu, které jste dokončili migraci).

Parametry:

| Parametr | Popis                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Vrácení migrace (vrátit zpět změny, které byly použity k databázi). |

## <a name="scaffold-dbcontext"></a>Vygenerované uživatelské rozhraní DbContext

Generuje kód `DbContext` a typy entit pro databázi. Aby `Scaffold-DbContext` generovat typ entity, databázové tabulky musí mít primární klíč.

Parametry:

| Parametr                          | Popis                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Connection \<řetězec ></nobr> | Připojovací řetězec k databázi. Pro projekty ASP.NET Core 2.x, může být hodnota *název =\<název připojovacího řetězce >*. Název v tomto případě pochází ze zdroje konfigurace, které jsou nastavené pro projekt. Toto je pozičních parametrů a je povinný. |
| <nobr>-Poskytovatel \<řetězec ></nobr>   | Zprostředkovatel k použití. Obvykle jde o název balíčku NuGet, například: `Microsoft.EntityFrameworkCore.SqlServer`. Toto je pozičních parametrů a je povinný.                                                                                           |
| -OutputDir \<řetězec >               | Adresář, který se má vložit soubory. Cesty jsou relativní vzhledem k adresáři projektu.                                                                                                                                                                                             |
| -ContextDir \<řetězec >              | Adresář, který chcete vložit `DbContext` v souboru. Cesty jsou relativní vzhledem k adresáři projektu.                                                                                                                                                                              |
| -Kontextu \<řetězec >                 | Název `DbContext` k vygenerování.                                                                                                                                                                                                                          |
| -Schémata \<String [] >               | Schémata tabulek ke generování typů entit pro. Pokud je tento parametr vynechán, jsou všechna schémata zahrnuty.                                                                                                                                                             |
| -Tabulky \<String [] >                | Tabulky pro typy entit pro generování. Pokud je tento parametr vynechán, budou zahrnuty všechny tabulky.                                                                                                                                                                         |
| -DataAnnotations                   | Atributy lze použijte ke konfiguraci modelu (Pokud je to možné). Pokud je tento parametr vynechán, použije se pouze rozhraní fluent API.                                                                                                                                                      |
| -UseDatabaseNames                  | Použijte názvy tabulek a sloupců přesně tak, jak jsou uvedeny v databázi. Pokud je tento parametr vynechán, aby lépe odpovídaly konvence stylu název jazyka C# změna názvů databáze.                                                                                       |
| -Force                             | Přepište existující soubory.                                                                                                                                                                                                                                               |

Příklad:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Příklad, který se vygeneruje pouze uživatelské rozhraní vybraných tabulek a vytvoří kontext do samostatné složky se zadaným názvem:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Skript migrace

Vygeneruje skript SQL, která se použije všechny změny z jednoho vybraného migrace do jiného vybrané migrace.

Parametry:

| Parametr                | Popis                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-Od* \<řetězec >        | Počáteční migraci. Migrace může identifikovat podle názvu nebo podle ID. Zvláštní případ, který znamená, že je číslo 0 *před první migraci*. Výchozí hodnota je 0.                                                              |
| *-Na* \<řetězec >          | Dokončení migrace. Výchozí hodnota je poslední migrace.                                                                                                                                                                      |
| <nobr>-Idempotentní</nobr> | Generovat skript, který jde použít na databáze v libovolné migrace.                                                                                                                                                         |
| -Výstupní \<řetězec >        | Soubor pro zápis výsledek. Pokud je tento parametr vynechán, soubor je vytvořen s vygenerovaným názvem ve stejné složce jako soubory modulu runtime aplikace se vytvářejí, například: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*. |

> [!TIP]
> Na, z, a podpora rozšíření tab výstupní parametry.

Následující příklad vytvoří skript pro migraci InitialCreate, pomocí názvu migrace.

```powershell
Script-Migration -To InitialCreate
```

Následující příklad vytvoří skript pro všechny migrace po dokončení migrace InitialCreate pomocí ID migrace.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Aktualizace databáze

Aktualizace databáze na poslední migraci nebo zadaný migrace.

| Parametr                           | Popis                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>*– Migrace* \<řetězec ></nobr> | Cíl migrace. Migrace může identifikovat podle názvu nebo podle ID. Zvláštní případ, který znamená, že je číslo 0 *před první migraci* a způsobí, že všechny migrace se vrátí zpátky. Pokud není zadána žádná migrace, výchozí příkaz na poslední migrace. |

> [!TIP]
> Parametr migrace podporuje rozšíření tab.

Následující příklad vrátí všechny migrace.

```powershell
Update-Database -Migration 0
```

Následující příklady aktualizujte databázi na zadané migrace. Název migrace používá první a druhé používá ID migrace:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Další zdroje

* [Migrace](xref:core/managing-schemas/migrations/index)
* [Zpětná analýza](xref:core/managing-schemas/scaffolding)
