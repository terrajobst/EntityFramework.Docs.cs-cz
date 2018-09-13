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
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="8d44b-102">Nástroje příkazového řádku .NET EF Core</span><span class="sxs-lookup"><span data-stu-id="8d44b-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="8d44b-103">Nástroje příkazového řádku .NET Core Entity Framework jsou rozšíření pro různé platformy **dotnet** příkaz, který je součástí sady [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="8d44b-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="8d44b-104">Pokud používáte Visual Studio, doporučujeme [nástroje PMC] [ 1] místo toho od té doby poskytují více integrované prostředí.</span><span class="sxs-lookup"><span data-stu-id="8d44b-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="8d44b-105">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="8d44b-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="8d44b-106">Sada .NET Core SDK verze 2.1.300 a novější obsahuje **dotnet ef** příkazy, které jsou kompatibilní s EF Core 2.0 a novějších verzích.</span><span class="sxs-lookup"><span data-stu-id="8d44b-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="8d44b-107">Proto pokud používáte nejnovější verze sady .NET Core SDK a modul runtime EF Core, bez instalace je vyžadován a zbytku této části můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="8d44b-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="8d44b-108">Na druhé straně **dotnet ef** nástroj obsažené ve verzi .NET Core SDK 2.1.300 a novějších verzí není kompatibilní s verzí EF Core 1.0 a 1.1.</span><span class="sxs-lookup"><span data-stu-id="8d44b-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="8d44b-109">Předtím, než můžete pracovat s projektem, který používá tyto starší verze EF Core na počítači s .NET Core SDK 2.1.300 nebo novější nainstalován, je nutné také nainstalovat verzi 2.1.200 nebo starší sady SDK a nakonfigurovat aplikaci, aby používala tuto starší verzi změnou jeho  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) souboru.</span><span class="sxs-lookup"><span data-stu-id="8d44b-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="8d44b-110">Tento soubor je obvykle součástí adresář řešení (jedna nad projektu).</span><span class="sxs-lookup"><span data-stu-id="8d44b-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="8d44b-111">Pak pokračujte podle pokynů installlation níže.</span><span class="sxs-lookup"><span data-stu-id="8d44b-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="8d44b-112">V předchozích verzích sady .NET Core SDK můžete nainstalovat nástroje pro .NET příkazového řádku EF Core pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="8d44b-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="8d44b-113">Upravte soubor projektu a přidejte Microsoft.EntityFrameworkCore.Tools.DotNet jako položka DotNetCliToolReference (viz níže)</span><span class="sxs-lookup"><span data-stu-id="8d44b-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="8d44b-114">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8d44b-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="8d44b-115">Výsledný projekt by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="8d44b-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="8d44b-116">Odkaz na balíček s `PrivateAssets="All"` znamená, že není vystavený projekty, které odkazují na tento projekt, což je užitečné zejména pro balíčky, které se obvykle používají pouze během vývoje.</span><span class="sxs-lookup"><span data-stu-id="8d44b-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="8d44b-117">Pokud jste to udělali všechno hned, byste měli mít k úspěšnému spuštění následujícího příkazu v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="8d44b-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="8d44b-118">Pomocí nástrojů</span><span class="sxs-lookup"><span data-stu-id="8d44b-118">Using the tools</span></span>
---------------
<span data-ttu-id="8d44b-119">Při každém vyvolání příkazu, existují dva projekty zahrnuté:</span><span class="sxs-lookup"><span data-stu-id="8d44b-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="8d44b-120">Cílový projekt je tam, kde se přidají všechny soubory (nebo v některých případech odebrána).</span><span class="sxs-lookup"><span data-stu-id="8d44b-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="8d44b-121">Výchozí hodnota je projekt v aktuálním adresáři na cílový projekt, ale můžete změnit pomocí <nobr> **`--project`** </nobr> možnost.</span><span class="sxs-lookup"><span data-stu-id="8d44b-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="8d44b-122">Projekt po spuštění je emulován nástroji při spuštění kódu projektu.</span><span class="sxs-lookup"><span data-stu-id="8d44b-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="8d44b-123">Je také ve výchozím nastavení do aktuálního adresáře projektu, ale lze změnit pomocí **`--startup-project`** možnost.</span><span class="sxs-lookup"><span data-stu-id="8d44b-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="8d44b-124">Aktualizuje se databáze webové aplikace, který má nainstalovaný v jiném projektu EF Core, by vypadalo takto: `dotnet ef database update --project {project-path}` (v adresáři webové aplikace)</span><span class="sxs-lookup"><span data-stu-id="8d44b-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="8d44b-125">Běžné možnosti:</span><span class="sxs-lookup"><span data-stu-id="8d44b-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="8d44b-126">Zobrazit výstup ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="8d44b-126">Show JSON output.</span></span>           |
| <span data-ttu-id="8d44b-127">-c</span><span class="sxs-lookup"><span data-stu-id="8d44b-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="8d44b-128">Chcete-li použít uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="8d44b-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="8d44b-129">-p</span><span class="sxs-lookup"><span data-stu-id="8d44b-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="8d44b-130">Projekt pro použití.</span><span class="sxs-lookup"><span data-stu-id="8d44b-130">The project to use.</span></span>         |
| <span data-ttu-id="8d44b-131">-s</span><span class="sxs-lookup"><span data-stu-id="8d44b-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="8d44b-132">Použít spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="8d44b-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="8d44b-133">Cílová architektura.</span><span class="sxs-lookup"><span data-stu-id="8d44b-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="8d44b-134">Konfigurace pro použití.</span><span class="sxs-lookup"><span data-stu-id="8d44b-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="8d44b-135">Modul runtime pro použití.</span><span class="sxs-lookup"><span data-stu-id="8d44b-135">The runtime to use.</span></span>         |
| <span data-ttu-id="8d44b-136">-h</span><span class="sxs-lookup"><span data-stu-id="8d44b-136">-h</span></span> | `--help`                           | <span data-ttu-id="8d44b-137">Zobrazit informace nápovědy.</span><span class="sxs-lookup"><span data-stu-id="8d44b-137">Show help information.</span></span>      |
| <span data-ttu-id="8d44b-138">-v</span><span class="sxs-lookup"><span data-stu-id="8d44b-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="8d44b-139">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="8d44b-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="8d44b-140">Není barevně zvýrazňovat výstup.</span><span class="sxs-lookup"><span data-stu-id="8d44b-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="8d44b-141">Předpona výstup s úrovní.</span><span class="sxs-lookup"><span data-stu-id="8d44b-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="8d44b-142">Chcete-li určit prostředí ASP.NET Core, nastavte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí před spuštěním.</span><span class="sxs-lookup"><span data-stu-id="8d44b-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="8d44b-143">Příkazy</span><span class="sxs-lookup"><span data-stu-id="8d44b-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="8d44b-144">DotNet ef database přetažení</span><span class="sxs-lookup"><span data-stu-id="8d44b-144">dotnet ef database drop</span></span>

<span data-ttu-id="8d44b-145">Zahodí databáze.</span><span class="sxs-lookup"><span data-stu-id="8d44b-145">Drops the database.</span></span>

<span data-ttu-id="8d44b-146">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="8d44b-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="8d44b-147">-f</span><span class="sxs-lookup"><span data-stu-id="8d44b-147">-f</span></span> | `--force`   | <span data-ttu-id="8d44b-148">Není potvrzení.</span><span class="sxs-lookup"><span data-stu-id="8d44b-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="8d44b-149">Zobrazit, které databáze bude vyřazena, ale ji.</span><span class="sxs-lookup"><span data-stu-id="8d44b-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="8d44b-150">aktualizace databáze ef DotNet</span><span class="sxs-lookup"><span data-stu-id="8d44b-150">dotnet ef database update</span></span>

<span data-ttu-id="8d44b-151">Aktualizace databáze na zadané migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="8d44b-152">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="8d44b-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="8d44b-153">Cíl migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-153">The target migration.</span></span> <span data-ttu-id="8d44b-154">Pokud je 0, se vrátí zpět všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="8d44b-155">Výchozí hodnota je poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="8d44b-156">informace o DotNet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="8d44b-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="8d44b-157">Získá informace o typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="8d44b-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="8d44b-158">DotNet ef dbcontext v seznamu</span><span class="sxs-lookup"><span data-stu-id="8d44b-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="8d44b-159">Zobrazí seznam dostupných typů DbContext.</span><span class="sxs-lookup"><span data-stu-id="8d44b-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="8d44b-160">DotNet ef dbcontext vygenerované uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="8d44b-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="8d44b-161">Nástroj scaffold kontext databáze a entity typy pro databázi.</span><span class="sxs-lookup"><span data-stu-id="8d44b-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="8d44b-162">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="8d44b-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="8d44b-163">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="8d44b-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="8d44b-164">Zprostředkovatel k použití.</span><span class="sxs-lookup"><span data-stu-id="8d44b-164">The provider to use.</span></span> <span data-ttu-id="8d44b-165">(například Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="8d44b-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="8d44b-166">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="8d44b-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8d44b-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="8d44b-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="8d44b-168">Atributy lze použijte ke konfiguraci modelu (Pokud je to možné).</span><span class="sxs-lookup"><span data-stu-id="8d44b-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="8d44b-169">Pokud tento parametr vynechán, použije se pouze rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="8d44b-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="8d44b-170">-c</span><span class="sxs-lookup"><span data-stu-id="8d44b-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="8d44b-171">Název uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="8d44b-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="8d44b-172">Adresář, který se má vložit DbContext.</span><span class="sxs-lookup"><span data-stu-id="8d44b-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="8d44b-173">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="8d44b-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="8d44b-174">-f</span><span class="sxs-lookup"><span data-stu-id="8d44b-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="8d44b-175">Přepište existující soubory.</span><span class="sxs-lookup"><span data-stu-id="8d44b-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="8d44b-176">-o</span><span class="sxs-lookup"><span data-stu-id="8d44b-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="8d44b-177">Adresář, který se má vložit soubory.</span><span class="sxs-lookup"><span data-stu-id="8d44b-177">The directory to put files in.</span></span> <span data-ttu-id="8d44b-178">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="8d44b-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="8d44b-179">Schémata tabulek ke generování typů entit pro.</span><span class="sxs-lookup"><span data-stu-id="8d44b-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="8d44b-180">-t</span><span class="sxs-lookup"><span data-stu-id="8d44b-180">-t</span></span>              | <span data-ttu-id="8d44b-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="8d44b-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="8d44b-182">Tabulky pro typy entit pro generování.</span><span class="sxs-lookup"><span data-stu-id="8d44b-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="8d44b-183">Použijte názvy tabulek a sloupců přímo z databáze.</span><span class="sxs-lookup"><span data-stu-id="8d44b-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="8d44b-184">Přidejte migraci ef DotNet</span><span class="sxs-lookup"><span data-stu-id="8d44b-184">dotnet ef migrations add</span></span>

<span data-ttu-id="8d44b-185">Přidá novou migraci.</span><span class="sxs-lookup"><span data-stu-id="8d44b-185">Adds a new migration.</span></span>

<span data-ttu-id="8d44b-186">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="8d44b-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="8d44b-187">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-187">The name of the migration.</span></span> |

<span data-ttu-id="8d44b-188">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="8d44b-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8d44b-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="8d44b-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="8d44b-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="8d44b-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="8d44b-191">Adresáři (a v podřízeném oboru názvů) používat.</span><span class="sxs-lookup"><span data-stu-id="8d44b-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="8d44b-192">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="8d44b-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="8d44b-193">Výchozí hodnota je "Migrace".</span><span class="sxs-lookup"><span data-stu-id="8d44b-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="8d44b-194">Přehled migrace ef DotNet</span><span class="sxs-lookup"><span data-stu-id="8d44b-194">dotnet ef migrations list</span></span>

<span data-ttu-id="8d44b-195">Zobrazí seznam dostupných migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="8d44b-196">odebrat migrace ef DotNet</span><span class="sxs-lookup"><span data-stu-id="8d44b-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="8d44b-197">Odebere poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-197">Removes the last migration.</span></span>

<span data-ttu-id="8d44b-198">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="8d44b-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="8d44b-199">-f</span><span class="sxs-lookup"><span data-stu-id="8d44b-199">-f</span></span> | `--force` | <span data-ttu-id="8d44b-200">Vrácení migrace, pokud byl použit k databázi.</span><span class="sxs-lookup"><span data-stu-id="8d44b-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="8d44b-201">skript migrace ef DotNet</span><span class="sxs-lookup"><span data-stu-id="8d44b-201">dotnet ef migrations script</span></span>

<span data-ttu-id="8d44b-202">Vygeneruje skript SQL z migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="8d44b-203">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="8d44b-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="8d44b-204">Počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="8d44b-204">The starting migration.</span></span> <span data-ttu-id="8d44b-205">Výchozí hodnota je 0 (výchozí databáze).</span><span class="sxs-lookup"><span data-stu-id="8d44b-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="8d44b-206">Dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-206">The ending migration.</span></span> <span data-ttu-id="8d44b-207">Výchozí hodnota je poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="8d44b-208">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="8d44b-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="8d44b-209">-o</span><span class="sxs-lookup"><span data-stu-id="8d44b-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="8d44b-210">Soubor pro zápis výsledek.</span><span class="sxs-lookup"><span data-stu-id="8d44b-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="8d44b-211">-i</span><span class="sxs-lookup"><span data-stu-id="8d44b-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="8d44b-212">Generovat skript, který jde použít na databáze v libovolné migrace.</span><span class="sxs-lookup"><span data-stu-id="8d44b-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
