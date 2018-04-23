---
title: .NET core rozhraní příkazového řádku - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="f5c2e-102">Nástroje příkazového řádku .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="f5c2e-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="f5c2e-103">Nástroje příkazového řádku .NET Core Entity Framework se rozšíření pro různé platformy **dotnet** příkaz, který je součástí systému [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="f5c2e-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="f5c2e-104">Pokud používáte Visual Studio, doporučujeme [nástroje pomocí PMC] [ 1] místo, protože poskytují lepší integrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="f5c2e-105">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="f5c2e-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="f5c2e-106">Nainstalujte nástroje příkazového řádku EF základní v rozhraní .NET, pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="f5c2e-107">Upravte soubor projektu a přidejte Microsoft.EntityFrameworkCore.Tools.DotNet jako položka DotNetCliToolReference (viz níže)</span><span class="sxs-lookup"><span data-stu-id="f5c2e-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="f5c2e-108">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="f5c2e-109">Výsledný projekt by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="f5c2e-110">Odkaz balíček s `PrivateAssets="All"` znamená není vystavený projekty, které odkazují na tento projekt, který je obzvláště užitečná pro balíčky, které se obvykle používají pouze během vývoje.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="f5c2e-111">Pokud jste to udělali všechno právo, byste měli moct úspěšně spuštěním následujícího příkazu v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="f5c2e-112">Pomocí nástrojů</span><span class="sxs-lookup"><span data-stu-id="f5c2e-112">Using the tools</span></span>
---------------
<span data-ttu-id="f5c2e-113">Při každém vyvolání příkazu, existují dva projekty související se situací:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="f5c2e-114">Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat).</span><span class="sxs-lookup"><span data-stu-id="f5c2e-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="f5c2e-115">Cílový projekt výchozí nastavení na projekt v aktuálním adresáři, ale můžete změnit pomocí <nobr> **– projekt** </nobr> možnost.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="f5c2e-116">Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="f5c2e-117">Také výchozí hodnoty na projekt v aktuálním adresáři, ale můžete změnit pomocí **– projekt po spuštění** možnost.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="f5c2e-118">Například aktualizace databáze webové aplikace, který má základní EF nainstalované v na jiný projekt bude vypadat takto: `dotnet ef database update --project {project-path}` (z adresáře vaší webové aplikace)</span><span class="sxs-lookup"><span data-stu-id="f5c2e-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="f5c2e-119">Běžné možnosti:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="f5c2e-120">– json</span><span class="sxs-lookup"><span data-stu-id="f5c2e-120">--json</span></span>                           | <span data-ttu-id="f5c2e-121">Zobrazit výstup JSON.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-121">Show JSON output.</span></span>           |
| <span data-ttu-id="f5c2e-122">-c</span><span class="sxs-lookup"><span data-stu-id="f5c2e-122">-c</span></span> | <span data-ttu-id="f5c2e-123">--kontextu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="f5c2e-124">DbContext používat.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="f5c2e-125">-p</span><span class="sxs-lookup"><span data-stu-id="f5c2e-125">-p</span></span> | <span data-ttu-id="f5c2e-126">--projektu \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-126">--project \<PROJECT></span></span>             | <span data-ttu-id="f5c2e-127">Projekt, který používá.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-127">The project to use.</span></span>         |
| <span data-ttu-id="f5c2e-128">-s</span><span class="sxs-lookup"><span data-stu-id="f5c2e-128">-s</span></span> | <span data-ttu-id="f5c2e-129">--spouštěný projekt \<projektu ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="f5c2e-130">Spuštění projektu pro použití.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="f5c2e-131">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="f5c2e-132">Cílové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-132">The target framework.</span></span>       |
|    | <span data-ttu-id="f5c2e-133">– konfigurace \<konfigurace ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="f5c2e-134">Konfigurace používat.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="f5c2e-135">modul runtime – \<IDENTIFIKÁTOR ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="f5c2e-136">Modul runtime používat.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-136">The runtime to use.</span></span>         |
| <span data-ttu-id="f5c2e-137">-h</span><span class="sxs-lookup"><span data-stu-id="f5c2e-137">-h</span></span> | <span data-ttu-id="f5c2e-138">– Nápověda</span><span class="sxs-lookup"><span data-stu-id="f5c2e-138">--help</span></span>                           | <span data-ttu-id="f5c2e-139">Zobrazit informace nápovědy.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-139">Show help information.</span></span>      |
| <span data-ttu-id="f5c2e-140">-v</span><span class="sxs-lookup"><span data-stu-id="f5c2e-140">-v</span></span> | <span data-ttu-id="f5c2e-141">-verbose</span><span class="sxs-lookup"><span data-stu-id="f5c2e-141">--verbose</span></span>                        | <span data-ttu-id="f5c2e-142">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="f5c2e-143">– bez barvy</span><span class="sxs-lookup"><span data-stu-id="f5c2e-143">--no-color</span></span>                       | <span data-ttu-id="f5c2e-144">Nemáte Kolorovat výstup.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="f5c2e-145">– Předpona output</span><span class="sxs-lookup"><span data-stu-id="f5c2e-145">--prefix-output</span></span>                  | <span data-ttu-id="f5c2e-146">Předpona výstup s úrovní.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="f5c2e-147">Chcete-li zadat prostředí ASP.NET Core, nastavte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí dřív, než spustíte.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="f5c2e-148">Příkazy</span><span class="sxs-lookup"><span data-stu-id="f5c2e-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="f5c2e-149">Vyřaďte databázi ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-149">dotnet ef database drop</span></span>

<span data-ttu-id="f5c2e-150">Zahodí databáze.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-150">Drops the database.</span></span>

<span data-ttu-id="f5c2e-151">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="f5c2e-152">-f</span><span class="sxs-lookup"><span data-stu-id="f5c2e-152">-f</span></span> | <span data-ttu-id="f5c2e-153">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="f5c2e-153">--force</span></span>   | <span data-ttu-id="f5c2e-154">Nemáte potvrďte.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="f5c2e-155">– Test</span><span class="sxs-lookup"><span data-stu-id="f5c2e-155">--dry-run</span></span> | <span data-ttu-id="f5c2e-156">Zobrazit databázi, kterou by být vyřazen, ale nemáte vyřadit.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="f5c2e-157">aktualizace databáze ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-157">dotnet ef database update</span></span>

<span data-ttu-id="f5c2e-158">Aktualizuje databázi na zadané migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="f5c2e-159">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="f5c2e-160">\<MIGRACE &GT;</span><span class="sxs-lookup"><span data-stu-id="f5c2e-160">\<MIGRATION></span></span> | <span data-ttu-id="f5c2e-161">Cílová migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-161">The target migration.</span></span> <span data-ttu-id="f5c2e-162">Pokud je 0, budou vráceny všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="f5c2e-163">Výchozí hodnoty na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="f5c2e-164">informace o dbcontext ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="f5c2e-165">Získá informace o typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="f5c2e-166">seznam dbcontext ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="f5c2e-167">Jsou uvedeny dostupné typy DbContext.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="f5c2e-168">DotNet ef dbcontext vygenerované uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="f5c2e-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="f5c2e-169">Scaffold a typy DbContext a entity pro databázi.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="f5c2e-170">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="f5c2e-171">\<PŘIPOJENÍ &GT;</span><span class="sxs-lookup"><span data-stu-id="f5c2e-171">\<CONNECTION></span></span> | <span data-ttu-id="f5c2e-172">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="f5c2e-173">\<ZPROSTŘEDKOVATEL &GT;</span><span class="sxs-lookup"><span data-stu-id="f5c2e-173">\<PROVIDER></span></span>   | <span data-ttu-id="f5c2e-174">Zprostředkovatel, který se má používat.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-174">The provider to use.</span></span> <span data-ttu-id="f5c2e-175">(Např.)</span><span class="sxs-lookup"><span data-stu-id="f5c2e-175">(E.g.</span></span> <span data-ttu-id="f5c2e-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="f5c2e-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="f5c2e-177">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f5c2e-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="f5c2e-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="f5c2e-179">--datovými poznámkami</span><span class="sxs-lookup"><span data-stu-id="f5c2e-179">--data-annotations</span></span>                      | <span data-ttu-id="f5c2e-180">Pomocí atributů (kde je to možné), nakonfigurujte model.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="f5c2e-181">Pokud tento parametr vynechán, použije se rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="f5c2e-182">-c</span><span class="sxs-lookup"><span data-stu-id="f5c2e-182">-c</span></span>              | <span data-ttu-id="f5c2e-183">--kontextu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-183">--context \<NAME></span></span>                       | <span data-ttu-id="f5c2e-184">Název DbContext.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="f5c2e-185">--kontextu dir \<cesta ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="f5c2e-186">Adresář, který chcete umístit do souboru DbContext.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="f5c2e-187">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="f5c2e-188">-f</span><span class="sxs-lookup"><span data-stu-id="f5c2e-188">-f</span></span>              | <span data-ttu-id="f5c2e-189">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="f5c2e-189">--force</span></span>                                 | <span data-ttu-id="f5c2e-190">Přepište existující soubory.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="f5c2e-191">-o</span><span class="sxs-lookup"><span data-stu-id="f5c2e-191">-o</span></span>              | <span data-ttu-id="f5c2e-192">--výstup dir \<cesta ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="f5c2e-193">Adresář, který chcete umístit soubory do.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-193">The directory to put files in.</span></span> <span data-ttu-id="f5c2e-194">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="f5c2e-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="f5c2e-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="f5c2e-196">Schémata tabulkami k vygenerování typy entit pro.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="f5c2e-197">-t</span><span class="sxs-lookup"><span data-stu-id="f5c2e-197">-t</span></span>              | <span data-ttu-id="f5c2e-198">--tabulky \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="f5c2e-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="f5c2e-199">S tabulkami k vygenerování typy entit pro.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="f5c2e-200">--use-database-names</span><span class="sxs-lookup"><span data-stu-id="f5c2e-200">--use-database-names</span></span>                    | <span data-ttu-id="f5c2e-201">Používejte názvy tabulek a sloupců přímo z databáze.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="f5c2e-202">Přidat migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-202">dotnet ef migrations add</span></span>

<span data-ttu-id="f5c2e-203">Přidá nové migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-203">Adds a new migration.</span></span>

<span data-ttu-id="f5c2e-204">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="f5c2e-205">\<NAME &GT;</span><span class="sxs-lookup"><span data-stu-id="f5c2e-205">\<NAME></span></span> | <span data-ttu-id="f5c2e-206">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-206">The name of the migration.</span></span> |

<span data-ttu-id="f5c2e-207">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f5c2e-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="f5c2e-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="f5c2e-209"><nobr>--výstup dir \<cesta ></nobr></span><span class="sxs-lookup"><span data-stu-id="f5c2e-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="f5c2e-210">Adresář (a podklíčů – obor názvů) používat.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="f5c2e-211">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="f5c2e-212">Výchozí hodnota je "Migrace".</span><span class="sxs-lookup"><span data-stu-id="f5c2e-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="f5c2e-213">seznam migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-213">dotnet ef migrations list</span></span>

<span data-ttu-id="f5c2e-214">Zobrazí seznam dostupných migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="f5c2e-215">odebrat migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="f5c2e-216">Odebere poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-216">Removes the last migration.</span></span>

<span data-ttu-id="f5c2e-217">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="f5c2e-218">-f</span><span class="sxs-lookup"><span data-stu-id="f5c2e-218">-f</span></span> | <span data-ttu-id="f5c2e-219">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="f5c2e-219">--force</span></span> | <span data-ttu-id="f5c2e-220">Migrace vrátit, pokud nebyla použita k databázi.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="f5c2e-221">skript migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-221">dotnet ef migrations script</span></span>

<span data-ttu-id="f5c2e-222">Generuje skriptu SQL z migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="f5c2e-223">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="f5c2e-224">\<Z &GT;</span><span class="sxs-lookup"><span data-stu-id="f5c2e-224">\<FROM></span></span> | <span data-ttu-id="f5c2e-225">Počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-225">The starting migration.</span></span> <span data-ttu-id="f5c2e-226">Výchozí hodnota je 0 (počáteční databáze).</span><span class="sxs-lookup"><span data-stu-id="f5c2e-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="f5c2e-227">\<TO></span><span class="sxs-lookup"><span data-stu-id="f5c2e-227">\<TO></span></span>   | <span data-ttu-id="f5c2e-228">Koncová migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-228">The ending migration.</span></span> <span data-ttu-id="f5c2e-229">Výchozí hodnoty na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="f5c2e-230">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="f5c2e-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="f5c2e-231">-o</span><span class="sxs-lookup"><span data-stu-id="f5c2e-231">-o</span></span> | <span data-ttu-id="f5c2e-232">--výstup \<souboru ></span><span class="sxs-lookup"><span data-stu-id="f5c2e-232">--output \<FILE></span></span> | <span data-ttu-id="f5c2e-233">Soubor k zápisu výsledek, který má.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="f5c2e-234">-i</span><span class="sxs-lookup"><span data-stu-id="f5c2e-234">-i</span></span> | <span data-ttu-id="f5c2e-235">--idempotent</span><span class="sxs-lookup"><span data-stu-id="f5c2e-235">--idempotent</span></span>     | <span data-ttu-id="f5c2e-236">Generovat skript, který lze použít v databázi v žádné migrace.</span><span class="sxs-lookup"><span data-stu-id="f5c2e-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
