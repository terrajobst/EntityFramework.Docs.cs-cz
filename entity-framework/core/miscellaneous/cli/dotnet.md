---
title: .NET core CLI – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995294"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="26d66-102">Nástroje příkazového řádku .NET EF Core</span><span class="sxs-lookup"><span data-stu-id="26d66-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="26d66-103">Nástroje příkazového řádku .NET Core Entity Framework jsou rozšíření pro různé platformy **dotnet** příkaz, který je součástí sady [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="26d66-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="26d66-104">Pokud používáte Visual Studio, doporučujeme [nástroje PMC] [ 1] místo toho od té doby poskytují více integrované prostředí.</span><span class="sxs-lookup"><span data-stu-id="26d66-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="26d66-105">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="26d66-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="26d66-106">Sada .NET Core SDK verze 2.1.300 a novější obsahuje **dotnet ef** příkazy, které jsou kompatibilní s EF Core 2.0 a novějších verzích.</span><span class="sxs-lookup"><span data-stu-id="26d66-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="26d66-107">Proto pokud používáte nejnovější verze sady .NET Core SDK a modul runtime EF Core, bez instalace je vyžadován a zbytku této části můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="26d66-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="26d66-108">Na druhé straně **dotnet ef** nástroj obsažené ve verzi .NET Core SDK 2.1.300 a novějších verzí není kompatibilní s verzí EF Core 1.0 a 1.1.</span><span class="sxs-lookup"><span data-stu-id="26d66-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="26d66-109">Předtím, než můžete pracovat s projektem, který používá tyto starší verze EF Core na počítači s .NET Core SDK 2.1.300 nebo novější nainstalován, je nutné také nainstalovat verzi 2.1.200 nebo starší sady SDK a nakonfigurovat aplikaci, aby používala tuto starší verzi změnou jeho  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) souboru.</span><span class="sxs-lookup"><span data-stu-id="26d66-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="26d66-110">Tento soubor je obvykle součástí adresář řešení (jedna nad projektu).</span><span class="sxs-lookup"><span data-stu-id="26d66-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="26d66-111">Pak pokračujte podle pokynů installlation níže.</span><span class="sxs-lookup"><span data-stu-id="26d66-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="26d66-112">V předchozích verzích sady .NET Core SDK můžete nainstalovat nástroje pro .NET příkazového řádku EF Core pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="26d66-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="26d66-113">Upravte soubor projektu a přidejte Microsoft.EntityFrameworkCore.Tools.DotNet jako položka DotNetCliToolReference (viz níže)</span><span class="sxs-lookup"><span data-stu-id="26d66-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="26d66-114">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="26d66-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="26d66-115">Výsledný projekt by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="26d66-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="26d66-116">Odkaz na balíček s `PrivateAssets="All"` znamená, že není vystavený projekty, které odkazují na tento projekt, což je užitečné zejména pro balíčky, které se obvykle používají pouze během vývoje.</span><span class="sxs-lookup"><span data-stu-id="26d66-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="26d66-117">Pokud jste to udělali všechno hned, byste měli mít k úspěšnému spuštění následujícího příkazu v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="26d66-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="26d66-118">Pomocí nástrojů</span><span class="sxs-lookup"><span data-stu-id="26d66-118">Using the tools</span></span>
---------------
<span data-ttu-id="26d66-119">Při každém vyvolání příkazu, existují dva projekty zahrnuté:</span><span class="sxs-lookup"><span data-stu-id="26d66-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="26d66-120">Cílový projekt je tam, kde se přidají všechny soubory (nebo v některých případech odebrána).</span><span class="sxs-lookup"><span data-stu-id="26d66-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="26d66-121">Výchozí hodnota je projekt v aktuálním adresáři na cílový projekt, ale můžete změnit pomocí <nobr> **– projekt** </nobr> možnost.</span><span class="sxs-lookup"><span data-stu-id="26d66-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="26d66-122">Projekt po spuštění je emulován nástroji při spuštění kódu projektu.</span><span class="sxs-lookup"><span data-stu-id="26d66-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="26d66-123">Je také ve výchozím nastavení do aktuálního adresáře projektu, ale lze změnit pomocí **– projekt po spuštění** možnost.</span><span class="sxs-lookup"><span data-stu-id="26d66-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="26d66-124">Aktualizuje se databáze webové aplikace, který má nainstalovaný v jiném projektu EF Core, by vypadalo takto: `dotnet ef database update --project {project-path}` (v adresáři webové aplikace)</span><span class="sxs-lookup"><span data-stu-id="26d66-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="26d66-125">Běžné možnosti:</span><span class="sxs-lookup"><span data-stu-id="26d66-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="26d66-126">--json</span><span class="sxs-lookup"><span data-stu-id="26d66-126">--json</span></span>                           | <span data-ttu-id="26d66-127">Zobrazit výstup ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="26d66-127">Show JSON output.</span></span>           |
| <span data-ttu-id="26d66-128">-c</span><span class="sxs-lookup"><span data-stu-id="26d66-128">-c</span></span> | <span data-ttu-id="26d66-129">--kontextu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="26d66-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="26d66-130">Chcete-li použít uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="26d66-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="26d66-131">-p</span><span class="sxs-lookup"><span data-stu-id="26d66-131">-p</span></span> | <span data-ttu-id="26d66-132">--projektu \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="26d66-132">--project \<PROJECT></span></span>             | <span data-ttu-id="26d66-133">Projekt pro použití.</span><span class="sxs-lookup"><span data-stu-id="26d66-133">The project to use.</span></span>         |
| <span data-ttu-id="26d66-134">-s</span><span class="sxs-lookup"><span data-stu-id="26d66-134">-s</span></span> | <span data-ttu-id="26d66-135">--projekt po spuštění \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="26d66-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="26d66-136">Použít spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="26d66-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="26d66-137">--framework \<rámec ></span><span class="sxs-lookup"><span data-stu-id="26d66-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="26d66-138">Cílová architektura.</span><span class="sxs-lookup"><span data-stu-id="26d66-138">The target framework.</span></span>       |
|    | <span data-ttu-id="26d66-139">--konfigurace \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="26d66-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="26d66-140">Konfigurace pro použití.</span><span class="sxs-lookup"><span data-stu-id="26d66-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="26d66-141">modulu runtime-- \<IDENTIFIKÁTOR ></span><span class="sxs-lookup"><span data-stu-id="26d66-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="26d66-142">Modul runtime pro použití.</span><span class="sxs-lookup"><span data-stu-id="26d66-142">The runtime to use.</span></span>         |
| <span data-ttu-id="26d66-143">-h</span><span class="sxs-lookup"><span data-stu-id="26d66-143">-h</span></span> | <span data-ttu-id="26d66-144">– Nápověda</span><span class="sxs-lookup"><span data-stu-id="26d66-144">--help</span></span>                           | <span data-ttu-id="26d66-145">Zobrazit informace nápovědy.</span><span class="sxs-lookup"><span data-stu-id="26d66-145">Show help information.</span></span>      |
| <span data-ttu-id="26d66-146">-v</span><span class="sxs-lookup"><span data-stu-id="26d66-146">-v</span></span> | <span data-ttu-id="26d66-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="26d66-147">--verbose</span></span>                        | <span data-ttu-id="26d66-148">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="26d66-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="26d66-149">--no-color</span><span class="sxs-lookup"><span data-stu-id="26d66-149">--no-color</span></span>                       | <span data-ttu-id="26d66-150">Není barevně zvýrazňovat výstup.</span><span class="sxs-lookup"><span data-stu-id="26d66-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="26d66-151">--předponu výstupu</span><span class="sxs-lookup"><span data-stu-id="26d66-151">--prefix-output</span></span>                  | <span data-ttu-id="26d66-152">Předpona výstup s úrovní.</span><span class="sxs-lookup"><span data-stu-id="26d66-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="26d66-153">Chcete-li určit prostředí ASP.NET Core, nastavte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí před spuštěním.</span><span class="sxs-lookup"><span data-stu-id="26d66-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="26d66-154">Příkazy</span><span class="sxs-lookup"><span data-stu-id="26d66-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="26d66-155">DotNet ef database přetažení</span><span class="sxs-lookup"><span data-stu-id="26d66-155">dotnet ef database drop</span></span>

<span data-ttu-id="26d66-156">Zahodí databáze.</span><span class="sxs-lookup"><span data-stu-id="26d66-156">Drops the database.</span></span>

<span data-ttu-id="26d66-157">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="26d66-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="26d66-158">-f</span><span class="sxs-lookup"><span data-stu-id="26d66-158">-f</span></span> | <span data-ttu-id="26d66-159">--force</span><span class="sxs-lookup"><span data-stu-id="26d66-159">--force</span></span>   | <span data-ttu-id="26d66-160">Není potvrzení.</span><span class="sxs-lookup"><span data-stu-id="26d66-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="26d66-161">--Test</span><span class="sxs-lookup"><span data-stu-id="26d66-161">--dry-run</span></span> | <span data-ttu-id="26d66-162">Zobrazit, které databáze bude vyřazena, ale ji.</span><span class="sxs-lookup"><span data-stu-id="26d66-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="26d66-163">aktualizace databáze ef DotNet</span><span class="sxs-lookup"><span data-stu-id="26d66-163">dotnet ef database update</span></span>

<span data-ttu-id="26d66-164">Aktualizace databáze na zadané migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="26d66-165">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="26d66-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="26d66-166">\<MIGRACE &GT;</span><span class="sxs-lookup"><span data-stu-id="26d66-166">\<MIGRATION></span></span> | <span data-ttu-id="26d66-167">Cíl migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-167">The target migration.</span></span> <span data-ttu-id="26d66-168">Pokud je 0, se vrátí zpět všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="26d66-169">Výchozí hodnota je poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="26d66-170">informace o DotNet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="26d66-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="26d66-171">Získá informace o typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="26d66-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="26d66-172">DotNet ef dbcontext v seznamu</span><span class="sxs-lookup"><span data-stu-id="26d66-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="26d66-173">Zobrazí seznam dostupných typů DbContext.</span><span class="sxs-lookup"><span data-stu-id="26d66-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="26d66-174">DotNet ef dbcontext vygenerované uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="26d66-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="26d66-175">Nástroj scaffold kontext databáze a entity typy pro databázi.</span><span class="sxs-lookup"><span data-stu-id="26d66-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="26d66-176">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="26d66-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="26d66-177">\<PŘIPOJENÍ &GT;</span><span class="sxs-lookup"><span data-stu-id="26d66-177">\<CONNECTION></span></span> | <span data-ttu-id="26d66-178">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="26d66-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="26d66-179">\<ZPROSTŘEDKOVATEL &GT;</span><span class="sxs-lookup"><span data-stu-id="26d66-179">\<PROVIDER></span></span>   | <span data-ttu-id="26d66-180">Zprostředkovatel k použití.</span><span class="sxs-lookup"><span data-stu-id="26d66-180">The provider to use.</span></span> <span data-ttu-id="26d66-181">(například Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="26d66-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="26d66-182">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="26d66-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26d66-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="26d66-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="26d66-184">--datovými poznámkami</span><span class="sxs-lookup"><span data-stu-id="26d66-184">--data-annotations</span></span>                      | <span data-ttu-id="26d66-185">Atributy lze použijte ke konfiguraci modelu (Pokud je to možné).</span><span class="sxs-lookup"><span data-stu-id="26d66-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="26d66-186">Pokud tento parametr vynechán, použije se pouze rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="26d66-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="26d66-187">-c</span><span class="sxs-lookup"><span data-stu-id="26d66-187">-c</span></span>              | <span data-ttu-id="26d66-188">--kontextu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="26d66-188">--context \<NAME></span></span>                       | <span data-ttu-id="26d66-189">Název uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="26d66-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="26d66-190">-dir kontextu \<cesta ></span><span class="sxs-lookup"><span data-stu-id="26d66-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="26d66-191">Adresář, který se má vložit DbContext.</span><span class="sxs-lookup"><span data-stu-id="26d66-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="26d66-192">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="26d66-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="26d66-193">-f</span><span class="sxs-lookup"><span data-stu-id="26d66-193">-f</span></span>              | <span data-ttu-id="26d66-194">--force</span><span class="sxs-lookup"><span data-stu-id="26d66-194">--force</span></span>                                 | <span data-ttu-id="26d66-195">Přepište existující soubory.</span><span class="sxs-lookup"><span data-stu-id="26d66-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="26d66-196">-o</span><span class="sxs-lookup"><span data-stu-id="26d66-196">-o</span></span>              | <span data-ttu-id="26d66-197">-dir výstup \<cesta ></span><span class="sxs-lookup"><span data-stu-id="26d66-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="26d66-198">Adresář, který se má vložit soubory.</span><span class="sxs-lookup"><span data-stu-id="26d66-198">The directory to put files in.</span></span> <span data-ttu-id="26d66-199">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="26d66-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="26d66-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="26d66-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="26d66-201">Schémata tabulek ke generování typů entit pro.</span><span class="sxs-lookup"><span data-stu-id="26d66-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="26d66-202">-t</span><span class="sxs-lookup"><span data-stu-id="26d66-202">-t</span></span>              | <span data-ttu-id="26d66-203">--tabulky \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="26d66-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="26d66-204">Tabulky pro typy entit pro generování.</span><span class="sxs-lookup"><span data-stu-id="26d66-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="26d66-205">--use-database-names</span><span class="sxs-lookup"><span data-stu-id="26d66-205">--use-database-names</span></span>                    | <span data-ttu-id="26d66-206">Použijte názvy tabulek a sloupců přímo z databáze.</span><span class="sxs-lookup"><span data-stu-id="26d66-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="26d66-207">Přidejte migraci ef DotNet</span><span class="sxs-lookup"><span data-stu-id="26d66-207">dotnet ef migrations add</span></span>

<span data-ttu-id="26d66-208">Přidá novou migraci.</span><span class="sxs-lookup"><span data-stu-id="26d66-208">Adds a new migration.</span></span>

<span data-ttu-id="26d66-209">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="26d66-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="26d66-210">\<NÁZEV &GT;</span><span class="sxs-lookup"><span data-stu-id="26d66-210">\<NAME></span></span> | <span data-ttu-id="26d66-211">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-211">The name of the migration.</span></span> |

<span data-ttu-id="26d66-212">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="26d66-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="26d66-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="26d66-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="26d66-214"><nobr>-dir výstup \<cesta ></nobr></span><span class="sxs-lookup"><span data-stu-id="26d66-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="26d66-215">Adresáři (a v podřízeném oboru názvů) používat.</span><span class="sxs-lookup"><span data-stu-id="26d66-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="26d66-216">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="26d66-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="26d66-217">Výchozí hodnota je "Migrace".</span><span class="sxs-lookup"><span data-stu-id="26d66-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="26d66-218">Přehled migrace ef DotNet</span><span class="sxs-lookup"><span data-stu-id="26d66-218">dotnet ef migrations list</span></span>

<span data-ttu-id="26d66-219">Zobrazí seznam dostupných migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="26d66-220">odebrat migrace ef DotNet</span><span class="sxs-lookup"><span data-stu-id="26d66-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="26d66-221">Odebere poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-221">Removes the last migration.</span></span>

<span data-ttu-id="26d66-222">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="26d66-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="26d66-223">-f</span><span class="sxs-lookup"><span data-stu-id="26d66-223">-f</span></span> | <span data-ttu-id="26d66-224">--force</span><span class="sxs-lookup"><span data-stu-id="26d66-224">--force</span></span> | <span data-ttu-id="26d66-225">Vrácení migrace, pokud byl použit k databázi.</span><span class="sxs-lookup"><span data-stu-id="26d66-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="26d66-226">skript migrace ef DotNet</span><span class="sxs-lookup"><span data-stu-id="26d66-226">dotnet ef migrations script</span></span>

<span data-ttu-id="26d66-227">Vygeneruje skript SQL z migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="26d66-228">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="26d66-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="26d66-229">\<Z &GT;</span><span class="sxs-lookup"><span data-stu-id="26d66-229">\<FROM></span></span> | <span data-ttu-id="26d66-230">Počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="26d66-230">The starting migration.</span></span> <span data-ttu-id="26d66-231">Výchozí hodnota je 0 (výchozí databáze).</span><span class="sxs-lookup"><span data-stu-id="26d66-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="26d66-232">\<TO></span><span class="sxs-lookup"><span data-stu-id="26d66-232">\<TO></span></span>   | <span data-ttu-id="26d66-233">Dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-233">The ending migration.</span></span> <span data-ttu-id="26d66-234">Výchozí hodnota je poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="26d66-235">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="26d66-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="26d66-236">-o</span><span class="sxs-lookup"><span data-stu-id="26d66-236">-o</span></span> | <span data-ttu-id="26d66-237">--výstup \<souboru ></span><span class="sxs-lookup"><span data-stu-id="26d66-237">--output \<FILE></span></span> | <span data-ttu-id="26d66-238">Soubor pro zápis výsledek.</span><span class="sxs-lookup"><span data-stu-id="26d66-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="26d66-239">-i</span><span class="sxs-lookup"><span data-stu-id="26d66-239">-i</span></span> | <span data-ttu-id="26d66-240">--idempotentní</span><span class="sxs-lookup"><span data-stu-id="26d66-240">--idempotent</span></span>     | <span data-ttu-id="26d66-241">Generovat skript, který jde použít na databáze v libovolné migrace.</span><span class="sxs-lookup"><span data-stu-id="26d66-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
