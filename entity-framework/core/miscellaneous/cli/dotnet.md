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
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="2f71e-102">Nástroje příkazového řádku .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="2f71e-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="2f71e-103">Nástroje příkazového řádku .NET Core Entity Framework se rozšíření pro různé platformy **dotnet** příkaz, který je součástí systému [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="2f71e-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="2f71e-104">Pokud používáte Visual Studio, doporučujeme [nástroje pomocí PMC] [ 1] místo, protože poskytují lepší integrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="2f71e-105">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="2f71e-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="2f71e-106">.NET Core SDK verze 2.1.300 a novější obsahuje **dotnet ef** příkazy, které jsou kompatibilní s EF základní 2.0 a novějšími verzemi.</span><span class="sxs-lookup"><span data-stu-id="2f71e-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="2f71e-107">Proto pokud používáte nejnovější verze rozhraní .NET Core SDK a EF Core runtime, není třeba žádná instalace a zbývající část tohoto oddílu můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="2f71e-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="2f71e-108">Na druhé straně **dotnet ef** nástroj obsažené ve verzi rozhraní .NET Core SDK 2.1.300 a novějších není kompatibilní s EF základní verze 1.0 a 1.1.</span><span class="sxs-lookup"><span data-stu-id="2f71e-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="2f71e-109">Než můžete pracovat s projekt, který používá tyto starší verze EF jádra na počítači, který má .NET Core SDK 2.1.300 nebo novější nainstalována, je nutné také nainstalovat verzi 2.1.200 nebo starší sady SDK a konfigurace aplikace pro použití této starší verze úpravou jeho  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) souboru.</span><span class="sxs-lookup"><span data-stu-id="2f71e-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="2f71e-110">Tento soubor je obvykle součástí adresář řešení (předchozímu projekt).</span><span class="sxs-lookup"><span data-stu-id="2f71e-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="2f71e-111">Pak můžete pokračovat s installlation pokynů níže.</span><span class="sxs-lookup"><span data-stu-id="2f71e-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="2f71e-112">Pro předchozí verze rozhraní .NET Core SDK můžete nainstalovat nástroje příkazového řádku EF základní v rozhraní .NET, pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="2f71e-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="2f71e-113">Upravte soubor projektu a přidejte Microsoft.EntityFrameworkCore.Tools.DotNet jako položka DotNetCliToolReference (viz níže)</span><span class="sxs-lookup"><span data-stu-id="2f71e-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="2f71e-114">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2f71e-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="2f71e-115">Výsledný projekt by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="2f71e-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="2f71e-116">Odkaz balíček s `PrivateAssets="All"` znamená není vystavený projekty, které odkazují na tento projekt, který je obzvláště užitečná pro balíčky, které se obvykle používají pouze během vývoje.</span><span class="sxs-lookup"><span data-stu-id="2f71e-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="2f71e-117">Pokud jste to udělali všechno právo, byste měli moct úspěšně spuštěním následujícího příkazu v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="2f71e-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="2f71e-118">Pomocí nástrojů</span><span class="sxs-lookup"><span data-stu-id="2f71e-118">Using the tools</span></span>
---------------
<span data-ttu-id="2f71e-119">Při každém vyvolání příkazu, existují dva projekty související se situací:</span><span class="sxs-lookup"><span data-stu-id="2f71e-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="2f71e-120">Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat).</span><span class="sxs-lookup"><span data-stu-id="2f71e-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="2f71e-121">Cílový projekt výchozí nastavení na projekt v aktuálním adresáři, ale můžete změnit pomocí <nobr> **– projekt** </nobr> možnost.</span><span class="sxs-lookup"><span data-stu-id="2f71e-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="2f71e-122">Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="2f71e-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="2f71e-123">Také výchozí hodnoty na projekt v aktuálním adresáři, ale můžete změnit pomocí **– projekt po spuštění** možnost.</span><span class="sxs-lookup"><span data-stu-id="2f71e-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="2f71e-124">Například aktualizace databáze webové aplikace, který má základní EF nainstalované v na jiný projekt bude vypadat takto: `dotnet ef database update --project {project-path}` (z adresáře vaší webové aplikace)</span><span class="sxs-lookup"><span data-stu-id="2f71e-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="2f71e-125">Běžné možnosti:</span><span class="sxs-lookup"><span data-stu-id="2f71e-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="2f71e-126">– json</span><span class="sxs-lookup"><span data-stu-id="2f71e-126">--json</span></span>                           | <span data-ttu-id="2f71e-127">Zobrazit výstup JSON.</span><span class="sxs-lookup"><span data-stu-id="2f71e-127">Show JSON output.</span></span>           |
| <span data-ttu-id="2f71e-128">-c</span><span class="sxs-lookup"><span data-stu-id="2f71e-128">-c</span></span> | <span data-ttu-id="2f71e-129">--kontextu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="2f71e-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="2f71e-130">DbContext používat.</span><span class="sxs-lookup"><span data-stu-id="2f71e-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="2f71e-131">-p</span><span class="sxs-lookup"><span data-stu-id="2f71e-131">-p</span></span> | <span data-ttu-id="2f71e-132">--projektu \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="2f71e-132">--project \<PROJECT></span></span>             | <span data-ttu-id="2f71e-133">Projekt, který používá.</span><span class="sxs-lookup"><span data-stu-id="2f71e-133">The project to use.</span></span>         |
| <span data-ttu-id="2f71e-134">-s</span><span class="sxs-lookup"><span data-stu-id="2f71e-134">-s</span></span> | <span data-ttu-id="2f71e-135">--spouštěný projekt \<projektu ></span><span class="sxs-lookup"><span data-stu-id="2f71e-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="2f71e-136">Spuštění projektu pro použití.</span><span class="sxs-lookup"><span data-stu-id="2f71e-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="2f71e-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="2f71e-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="2f71e-138">Cílové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2f71e-138">The target framework.</span></span>       |
|    | <span data-ttu-id="2f71e-139">– konfigurace \<konfigurace ></span><span class="sxs-lookup"><span data-stu-id="2f71e-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="2f71e-140">Konfigurace používat.</span><span class="sxs-lookup"><span data-stu-id="2f71e-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="2f71e-141">modul runtime – \<IDENTIFIKÁTOR ></span><span class="sxs-lookup"><span data-stu-id="2f71e-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="2f71e-142">Modul runtime používat.</span><span class="sxs-lookup"><span data-stu-id="2f71e-142">The runtime to use.</span></span>         |
| <span data-ttu-id="2f71e-143">-h</span><span class="sxs-lookup"><span data-stu-id="2f71e-143">-h</span></span> | <span data-ttu-id="2f71e-144">– Nápověda</span><span class="sxs-lookup"><span data-stu-id="2f71e-144">--help</span></span>                           | <span data-ttu-id="2f71e-145">Zobrazit informace nápovědy.</span><span class="sxs-lookup"><span data-stu-id="2f71e-145">Show help information.</span></span>      |
| <span data-ttu-id="2f71e-146">-v</span><span class="sxs-lookup"><span data-stu-id="2f71e-146">-v</span></span> | <span data-ttu-id="2f71e-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="2f71e-147">--verbose</span></span>                        | <span data-ttu-id="2f71e-148">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="2f71e-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="2f71e-149">– bez barvy</span><span class="sxs-lookup"><span data-stu-id="2f71e-149">--no-color</span></span>                       | <span data-ttu-id="2f71e-150">Nemáte Kolorovat výstup.</span><span class="sxs-lookup"><span data-stu-id="2f71e-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="2f71e-151">– Předpona output</span><span class="sxs-lookup"><span data-stu-id="2f71e-151">--prefix-output</span></span>                  | <span data-ttu-id="2f71e-152">Předpona výstup s úrovní.</span><span class="sxs-lookup"><span data-stu-id="2f71e-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="2f71e-153">Chcete-li zadat prostředí ASP.NET Core, nastavte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí dřív, než spustíte.</span><span class="sxs-lookup"><span data-stu-id="2f71e-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="2f71e-154">Příkazy</span><span class="sxs-lookup"><span data-stu-id="2f71e-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="2f71e-155">Vyřaďte databázi ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="2f71e-155">dotnet ef database drop</span></span>

<span data-ttu-id="2f71e-156">Zahodí databáze.</span><span class="sxs-lookup"><span data-stu-id="2f71e-156">Drops the database.</span></span>

<span data-ttu-id="2f71e-157">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="2f71e-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="2f71e-158">-f</span><span class="sxs-lookup"><span data-stu-id="2f71e-158">-f</span></span> | <span data-ttu-id="2f71e-159">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="2f71e-159">--force</span></span>   | <span data-ttu-id="2f71e-160">Nemáte potvrďte.</span><span class="sxs-lookup"><span data-stu-id="2f71e-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="2f71e-161">– Test</span><span class="sxs-lookup"><span data-stu-id="2f71e-161">--dry-run</span></span> | <span data-ttu-id="2f71e-162">Zobrazit databázi, kterou by být vyřazen, ale nemáte vyřadit.</span><span class="sxs-lookup"><span data-stu-id="2f71e-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="2f71e-163">aktualizace databáze ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="2f71e-163">dotnet ef database update</span></span>

<span data-ttu-id="2f71e-164">Aktualizuje databázi na zadané migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="2f71e-165">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="2f71e-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="2f71e-166">\<MIGRACE &GT;</span><span class="sxs-lookup"><span data-stu-id="2f71e-166">\<MIGRATION></span></span> | <span data-ttu-id="2f71e-167">Cílová migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-167">The target migration.</span></span> <span data-ttu-id="2f71e-168">Pokud je 0, budou vráceny všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="2f71e-169">Výchozí hodnoty na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="2f71e-170">informace o dbcontext ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="2f71e-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="2f71e-171">Získá informace o typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="2f71e-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="2f71e-172">seznam dbcontext ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="2f71e-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="2f71e-173">Jsou uvedeny dostupné typy DbContext.</span><span class="sxs-lookup"><span data-stu-id="2f71e-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="2f71e-174">DotNet ef dbcontext vygenerované uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="2f71e-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="2f71e-175">Scaffold a typy DbContext a entity pro databázi.</span><span class="sxs-lookup"><span data-stu-id="2f71e-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="2f71e-176">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="2f71e-176">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="2f71e-177">\<PŘIPOJENÍ &GT;</span><span class="sxs-lookup"><span data-stu-id="2f71e-177">\<CONNECTION></span></span> | <span data-ttu-id="2f71e-178">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="2f71e-178">The connection string to the database.</span></span>                              |
| <span data-ttu-id="2f71e-179">\<ZPROSTŘEDKOVATEL &GT;</span><span class="sxs-lookup"><span data-stu-id="2f71e-179">\<PROVIDER></span></span>   | <span data-ttu-id="2f71e-180">Zprostředkovatel, který se má používat.</span><span class="sxs-lookup"><span data-stu-id="2f71e-180">The provider to use.</span></span> <span data-ttu-id="2f71e-181">(Např.)</span><span class="sxs-lookup"><span data-stu-id="2f71e-181">(E.g.</span></span> <span data-ttu-id="2f71e-182">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="2f71e-182">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="2f71e-183">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="2f71e-183">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2f71e-184"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="2f71e-184"><nobr>-d</nobr></span></span> | <span data-ttu-id="2f71e-185">--datovými poznámkami</span><span class="sxs-lookup"><span data-stu-id="2f71e-185">--data-annotations</span></span>                      | <span data-ttu-id="2f71e-186">Pomocí atributů (kde je to možné), nakonfigurujte model.</span><span class="sxs-lookup"><span data-stu-id="2f71e-186">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="2f71e-187">Pokud tento parametr vynechán, použije se rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="2f71e-187">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="2f71e-188">-c</span><span class="sxs-lookup"><span data-stu-id="2f71e-188">-c</span></span>              | <span data-ttu-id="2f71e-189">--kontextu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="2f71e-189">--context \<NAME></span></span>                       | <span data-ttu-id="2f71e-190">Název DbContext.</span><span class="sxs-lookup"><span data-stu-id="2f71e-190">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="2f71e-191">--kontextu dir \<cesta ></span><span class="sxs-lookup"><span data-stu-id="2f71e-191">--context-dir \<PATH></span></span>                   | <span data-ttu-id="2f71e-192">Adresář, který chcete umístit do souboru DbContext.</span><span class="sxs-lookup"><span data-stu-id="2f71e-192">The directory to put DbContext file in.</span></span> <span data-ttu-id="2f71e-193">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="2f71e-193">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="2f71e-194">-f</span><span class="sxs-lookup"><span data-stu-id="2f71e-194">-f</span></span>              | <span data-ttu-id="2f71e-195">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="2f71e-195">--force</span></span>                                 | <span data-ttu-id="2f71e-196">Přepište existující soubory.</span><span class="sxs-lookup"><span data-stu-id="2f71e-196">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="2f71e-197">-o</span><span class="sxs-lookup"><span data-stu-id="2f71e-197">-o</span></span>              | <span data-ttu-id="2f71e-198">--výstup dir \<cesta ></span><span class="sxs-lookup"><span data-stu-id="2f71e-198">--output-dir \<PATH></span></span>                    | <span data-ttu-id="2f71e-199">Adresář, který chcete umístit soubory do.</span><span class="sxs-lookup"><span data-stu-id="2f71e-199">The directory to put files in.</span></span> <span data-ttu-id="2f71e-200">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="2f71e-200">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="2f71e-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="2f71e-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="2f71e-202">Schémata tabulkami k vygenerování typy entit pro.</span><span class="sxs-lookup"><span data-stu-id="2f71e-202">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="2f71e-203">-t</span><span class="sxs-lookup"><span data-stu-id="2f71e-203">-t</span></span>              | <span data-ttu-id="2f71e-204">--tabulky \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="2f71e-204">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="2f71e-205">S tabulkami k vygenerování typy entit pro.</span><span class="sxs-lookup"><span data-stu-id="2f71e-205">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="2f71e-206">--use-database-names</span><span class="sxs-lookup"><span data-stu-id="2f71e-206">--use-database-names</span></span>                    | <span data-ttu-id="2f71e-207">Používejte názvy tabulek a sloupců přímo z databáze.</span><span class="sxs-lookup"><span data-stu-id="2f71e-207">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="2f71e-208">Přidat migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="2f71e-208">dotnet ef migrations add</span></span>

<span data-ttu-id="2f71e-209">Přidá nové migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-209">Adds a new migration.</span></span>

<span data-ttu-id="2f71e-210">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="2f71e-210">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="2f71e-211">\<NAME &GT;</span><span class="sxs-lookup"><span data-stu-id="2f71e-211">\<NAME></span></span> | <span data-ttu-id="2f71e-212">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-212">The name of the migration.</span></span> |

<span data-ttu-id="2f71e-213">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="2f71e-213">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2f71e-214"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="2f71e-214"><nobr>-o</nobr></span></span> | <span data-ttu-id="2f71e-215"><nobr>--výstup dir \<cesta ></nobr></span><span class="sxs-lookup"><span data-stu-id="2f71e-215"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="2f71e-216">Adresář (a podklíčů – obor názvů) používat.</span><span class="sxs-lookup"><span data-stu-id="2f71e-216">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="2f71e-217">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="2f71e-217">Paths are relative to the project directory.</span></span> <span data-ttu-id="2f71e-218">Výchozí hodnota je "Migrace".</span><span class="sxs-lookup"><span data-stu-id="2f71e-218">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="2f71e-219">seznam migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="2f71e-219">dotnet ef migrations list</span></span>

<span data-ttu-id="2f71e-220">Zobrazí seznam dostupných migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-220">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="2f71e-221">odebrat migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="2f71e-221">dotnet ef migrations remove</span></span>

<span data-ttu-id="2f71e-222">Odebere poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-222">Removes the last migration.</span></span>

<span data-ttu-id="2f71e-223">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="2f71e-223">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="2f71e-224">-f</span><span class="sxs-lookup"><span data-stu-id="2f71e-224">-f</span></span> | <span data-ttu-id="2f71e-225">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="2f71e-225">--force</span></span> | <span data-ttu-id="2f71e-226">Migrace vrátit, pokud nebyla použita k databázi.</span><span class="sxs-lookup"><span data-stu-id="2f71e-226">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="2f71e-227">skript migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="2f71e-227">dotnet ef migrations script</span></span>

<span data-ttu-id="2f71e-228">Generuje skriptu SQL z migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-228">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="2f71e-229">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="2f71e-229">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="2f71e-230">\<Z &GT;</span><span class="sxs-lookup"><span data-stu-id="2f71e-230">\<FROM></span></span> | <span data-ttu-id="2f71e-231">Počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-231">The starting migration.</span></span> <span data-ttu-id="2f71e-232">Výchozí hodnota je 0 (počáteční databáze).</span><span class="sxs-lookup"><span data-stu-id="2f71e-232">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="2f71e-233">\<TO></span><span class="sxs-lookup"><span data-stu-id="2f71e-233">\<TO></span></span>   | <span data-ttu-id="2f71e-234">Koncová migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-234">The ending migration.</span></span> <span data-ttu-id="2f71e-235">Výchozí hodnoty na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-235">Defaults to the last migration.</span></span>         |

<span data-ttu-id="2f71e-236">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="2f71e-236">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="2f71e-237">-o</span><span class="sxs-lookup"><span data-stu-id="2f71e-237">-o</span></span> | <span data-ttu-id="2f71e-238">--výstup \<souboru ></span><span class="sxs-lookup"><span data-stu-id="2f71e-238">--output \<FILE></span></span> | <span data-ttu-id="2f71e-239">Soubor k zápisu výsledek, který má.</span><span class="sxs-lookup"><span data-stu-id="2f71e-239">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="2f71e-240">-i</span><span class="sxs-lookup"><span data-stu-id="2f71e-240">-i</span></span> | <span data-ttu-id="2f71e-241">--idempotent</span><span class="sxs-lookup"><span data-stu-id="2f71e-241">--idempotent</span></span>     | <span data-ttu-id="2f71e-242">Generovat skript, který lze použít v databázi v žádné migrace.</span><span class="sxs-lookup"><span data-stu-id="2f71e-242">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
