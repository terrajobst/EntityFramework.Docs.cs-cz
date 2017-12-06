---
title: ".NET core rozhraní příkazového řádku - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="5196f-102">Nástroje příkazového řádku .NET Core EF</span><span class="sxs-lookup"><span data-stu-id="5196f-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="5196f-103">Nástroje příkazového řádku .NET Core Entity Framework se rozšíření pro různé platformy **dotnet** příkaz, který je součástí systému [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="5196f-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="5196f-104">Pokud používáte Visual Studio, doporučujeme [nástroje pomocí PMC] [ 1] místo, protože poskytují lepší integrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="5196f-105">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="5196f-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="5196f-106">Nainstalujte nástroje příkazového řádku EF základní v rozhraní .NET, pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="5196f-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="5196f-107">Upravte soubor projektu a přidejte Microsoft.EntityFrameworkCore.Tools.DotNet jako položka DotNetCliToolReference (viz níže)</span><span class="sxs-lookup"><span data-stu-id="5196f-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="5196f-108">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5196f-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="5196f-109">Výsledný projekt by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="5196f-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="5196f-110">Odkaz balíček s `PrivateAssets="All"` znamená není vystavený projekty, které odkazují na tento projekt, který je obzvláště užitečná pro balíčky, které se obvykle používají pouze během vývoje.</span><span class="sxs-lookup"><span data-stu-id="5196f-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="5196f-111">Pokud jste to udělali všechno právo, byste měli moct úspěšně spuštěním následujícího příkazu v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="5196f-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="5196f-112">Pomocí nástrojů</span><span class="sxs-lookup"><span data-stu-id="5196f-112">Using the tools</span></span>
---------------
<span data-ttu-id="5196f-113">Při každém vyvolání příkazu, existují dva projekty související se situací:</span><span class="sxs-lookup"><span data-stu-id="5196f-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="5196f-114">Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat).</span><span class="sxs-lookup"><span data-stu-id="5196f-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="5196f-115">Cílový projekt výchozí nastavení na projekt v aktuálním adresáři, ale můžete změnit pomocí <nobr> **– projekt** </nobr> možnost.</span><span class="sxs-lookup"><span data-stu-id="5196f-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="5196f-116">Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="5196f-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="5196f-117">Také výchozí hodnoty na projekt v aktuálním adresáři, ale můžete změnit pomocí **– projekt po spuštění** možnost.</span><span class="sxs-lookup"><span data-stu-id="5196f-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="5196f-118">Běžné možnosti:</span><span class="sxs-lookup"><span data-stu-id="5196f-118">Common options:</span></span>

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | <span data-ttu-id="5196f-119">– json</span><span class="sxs-lookup"><span data-stu-id="5196f-119">--json</span></span>                           | <span data-ttu-id="5196f-120">Zobrazit výstup JSON.</span><span class="sxs-lookup"><span data-stu-id="5196f-120">Show JSON output.</span></span>           |
| <span data-ttu-id="5196f-121">-c</span><span class="sxs-lookup"><span data-stu-id="5196f-121">-c</span></span> | <span data-ttu-id="5196f-122">--kontextu \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="5196f-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="5196f-123">DbContext používat.</span><span class="sxs-lookup"><span data-stu-id="5196f-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="5196f-124">-p</span><span class="sxs-lookup"><span data-stu-id="5196f-124">-p</span></span> | <span data-ttu-id="5196f-125">--projektu \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="5196f-125">--project \<PROJECT></span></span>             | <span data-ttu-id="5196f-126">Projekt, který používá.</span><span class="sxs-lookup"><span data-stu-id="5196f-126">The project to use.</span></span>         |
| <span data-ttu-id="5196f-127">-s</span><span class="sxs-lookup"><span data-stu-id="5196f-127">-s</span></span> | <span data-ttu-id="5196f-128">--spouštěný projekt \<projektu ></span><span class="sxs-lookup"><span data-stu-id="5196f-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="5196f-129">Spuštění projektu pro použití.</span><span class="sxs-lookup"><span data-stu-id="5196f-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="5196f-130">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="5196f-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="5196f-131">Cílové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5196f-131">The target framework.</span></span>       |
|    | <span data-ttu-id="5196f-132">– konfigurace \<konfigurace ></span><span class="sxs-lookup"><span data-stu-id="5196f-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="5196f-133">Konfigurace používat.</span><span class="sxs-lookup"><span data-stu-id="5196f-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="5196f-134">modul runtime – \<IDENTIFIKÁTOR ></span><span class="sxs-lookup"><span data-stu-id="5196f-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="5196f-135">Modul runtime používat.</span><span class="sxs-lookup"><span data-stu-id="5196f-135">The runtime to use.</span></span>         |
| <span data-ttu-id="5196f-136">-h</span><span class="sxs-lookup"><span data-stu-id="5196f-136">-h</span></span> | <span data-ttu-id="5196f-137">– Nápověda</span><span class="sxs-lookup"><span data-stu-id="5196f-137">--help</span></span>                           | <span data-ttu-id="5196f-138">Zobrazit informace nápovědy.</span><span class="sxs-lookup"><span data-stu-id="5196f-138">Show help information.</span></span>      |
| <span data-ttu-id="5196f-139">-v</span><span class="sxs-lookup"><span data-stu-id="5196f-139">-v</span></span> | <span data-ttu-id="5196f-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="5196f-140">--verbose</span></span>                        | <span data-ttu-id="5196f-141">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="5196f-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="5196f-142">– bez barvy</span><span class="sxs-lookup"><span data-stu-id="5196f-142">--no-color</span></span>                       | <span data-ttu-id="5196f-143">Nemáte Kolorovat výstup.</span><span class="sxs-lookup"><span data-stu-id="5196f-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="5196f-144">– Předpona output</span><span class="sxs-lookup"><span data-stu-id="5196f-144">--prefix-output</span></span>                  | <span data-ttu-id="5196f-145">Předpona výstup s úrovní.</span><span class="sxs-lookup"><span data-stu-id="5196f-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="5196f-146">Chcete-li zadat prostředí ASP.NET Core, nastavte **ASPNETCORE_ENVIRONMENT** proměnnou prostředí dřív, než spustíte.</span><span class="sxs-lookup"><span data-stu-id="5196f-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="5196f-147">Příkazy</span><span class="sxs-lookup"><span data-stu-id="5196f-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="5196f-148">Vyřaďte databázi ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="5196f-148">dotnet ef database drop</span></span>

<span data-ttu-id="5196f-149">Zahodí databáze.</span><span class="sxs-lookup"><span data-stu-id="5196f-149">Drops the database.</span></span>

<span data-ttu-id="5196f-150">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="5196f-150">Options:</span></span>

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| <span data-ttu-id="5196f-151">-f</span><span class="sxs-lookup"><span data-stu-id="5196f-151">-f</span></span> | <span data-ttu-id="5196f-152">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="5196f-152">--force</span></span>   | <span data-ttu-id="5196f-153">Nemáte potvrďte.</span><span class="sxs-lookup"><span data-stu-id="5196f-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="5196f-154">– Test</span><span class="sxs-lookup"><span data-stu-id="5196f-154">--dry-run</span></span> | <span data-ttu-id="5196f-155">Zobrazit databázi, kterou by být vyřazen, ale nemáte vyřadit.</span><span class="sxs-lookup"><span data-stu-id="5196f-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="5196f-156">aktualizace databáze ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="5196f-156">dotnet ef database update</span></span>

<span data-ttu-id="5196f-157">Aktualizuje databázi na zadané migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="5196f-158">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="5196f-158">Arguments:</span></span>

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| <span data-ttu-id="5196f-159">\<MIGRACE ></span><span class="sxs-lookup"><span data-stu-id="5196f-159">\<MIGRATION></span></span> | <span data-ttu-id="5196f-160">Cílová migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-160">The target migration.</span></span> <span data-ttu-id="5196f-161">Pokud je 0, budou vráceny všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="5196f-162">Výchozí hodnoty na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="5196f-163">informace o dbcontext ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="5196f-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="5196f-164">Získá informace o typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="5196f-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="5196f-165">seznam dbcontext ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="5196f-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="5196f-166">Jsou uvedeny dostupné typy DbContext.</span><span class="sxs-lookup"><span data-stu-id="5196f-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="5196f-167">DotNet ef dbcontext vygenerované uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="5196f-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="5196f-168">Scaffold a typy DbContext a entity pro databázi.</span><span class="sxs-lookup"><span data-stu-id="5196f-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="5196f-169">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="5196f-169">Arguments:</span></span>

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| <span data-ttu-id="5196f-170">\<PŘIPOJENÍ ></span><span class="sxs-lookup"><span data-stu-id="5196f-170">\<CONNECTION></span></span> | <span data-ttu-id="5196f-171">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="5196f-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="5196f-172">\<ZPROSTŘEDKOVATEL ></span><span class="sxs-lookup"><span data-stu-id="5196f-172">\<PROVIDER></span></span>   | <span data-ttu-id="5196f-173">Zprostředkovatel, který se má používat.</span><span class="sxs-lookup"><span data-stu-id="5196f-173">The provider to use.</span></span> <span data-ttu-id="5196f-174">(Např.)</span><span class="sxs-lookup"><span data-stu-id="5196f-174">(E.g.</span></span> <span data-ttu-id="5196f-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="5196f-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="5196f-176">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="5196f-176">Options:</span></span>

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <span data-ttu-id="5196f-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="5196f-177"><nobr>-d</nobr></span></span> |       <span data-ttu-id="5196f-178">--datovými poznámkami</span><span class="sxs-lookup"><span data-stu-id="5196f-178">--data-annotations</span></span>                | <span data-ttu-id="5196f-179">Pomocí atributů (kde je to možné), nakonfigurujte model.</span><span class="sxs-lookup"><span data-stu-id="5196f-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="5196f-180">Pokud tento parametr vynechán, použije se rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="5196f-180">If omitted, only the fluent API is used.</span></span> |
|       <span data-ttu-id="5196f-181">-c</span><span class="sxs-lookup"><span data-stu-id="5196f-181">-c</span></span>        |       <span data-ttu-id="5196f-182">--kontextu \<NAME ></span><span class="sxs-lookup"><span data-stu-id="5196f-182">--context \<NAME></span></span>                 | <span data-ttu-id="5196f-183">Název DbContext.</span><span class="sxs-lookup"><span data-stu-id="5196f-183">The name of the DbContext.</span></span>                               |
|       <span data-ttu-id="5196f-184">-f</span><span class="sxs-lookup"><span data-stu-id="5196f-184">-f</span></span>        |       <span data-ttu-id="5196f-185">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="5196f-185">--force</span></span>                           | <span data-ttu-id="5196f-186">Přepište existující soubory.</span><span class="sxs-lookup"><span data-stu-id="5196f-186">Overwrite existing files.</span></span>                                |
|       <span data-ttu-id="5196f-187">-o</span><span class="sxs-lookup"><span data-stu-id="5196f-187">-o</span></span>        |       <span data-ttu-id="5196f-188">--výstup dir \<cesta ></span><span class="sxs-lookup"><span data-stu-id="5196f-188">--output-dir \<PATH></span></span>              | <span data-ttu-id="5196f-189">Adresář, který chcete umístit soubory do.</span><span class="sxs-lookup"><span data-stu-id="5196f-189">The directory to put files in.</span></span> <span data-ttu-id="5196f-190">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="5196f-190">Paths are relative to the project directory.</span></span> |
|                 | <span data-ttu-id="5196f-191"><nobr>--schématu \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="5196f-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="5196f-192">Schémata tabulkami k vygenerování typy entit pro.</span><span class="sxs-lookup"><span data-stu-id="5196f-192">The schemas of tables to generate entity types for.</span></span>      |
|       <span data-ttu-id="5196f-193">-t</span><span class="sxs-lookup"><span data-stu-id="5196f-193">-t</span></span>        |       <span data-ttu-id="5196f-194">--tabulky \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="5196f-194">--table \<TABLE_NAME>...</span></span>          | <span data-ttu-id="5196f-195">S tabulkami k vygenerování typy entit pro.</span><span class="sxs-lookup"><span data-stu-id="5196f-195">The tables to generate entity types for.</span></span>                 |
|                 |       <span data-ttu-id="5196f-196">– použití názvy databází</span><span class="sxs-lookup"><span data-stu-id="5196f-196">--use-database-names</span></span>              | <span data-ttu-id="5196f-197">Používejte názvy tabulek a sloupců přímo z databáze.</span><span class="sxs-lookup"><span data-stu-id="5196f-197">Use table and column names directly from the database.</span></span>   |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="5196f-198">Přidat migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="5196f-198">dotnet ef migrations add</span></span>

<span data-ttu-id="5196f-199">Přidá nové migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-199">Adds a new migration.</span></span>

<span data-ttu-id="5196f-200">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="5196f-200">Arguments:</span></span>

|         |                            |
| ------- | -------------------------- |
| <span data-ttu-id="5196f-201">\<NAME ></span><span class="sxs-lookup"><span data-stu-id="5196f-201">\<NAME></span></span> | <span data-ttu-id="5196f-202">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-202">The name of the migration.</span></span> |

<span data-ttu-id="5196f-203">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="5196f-203">Options:</span></span>

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <span data-ttu-id="5196f-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="5196f-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="5196f-205"><nobr>--výstup dir \<cesta ></nobr></span><span class="sxs-lookup"><span data-stu-id="5196f-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="5196f-206">Adresář (a podklíčů – obor názvů) používat.</span><span class="sxs-lookup"><span data-stu-id="5196f-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="5196f-207">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="5196f-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="5196f-208">Výchozí hodnota je "Migrace".</span><span class="sxs-lookup"><span data-stu-id="5196f-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="5196f-209">seznam migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="5196f-209">dotnet ef migrations list</span></span>

<span data-ttu-id="5196f-210">Zobrazí seznam dostupných migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="5196f-211">odebrat migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="5196f-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="5196f-212">Odebere poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-212">Removes the last migration.</span></span>

<span data-ttu-id="5196f-213">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="5196f-213">Options:</span></span>

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| <span data-ttu-id="5196f-214">-f</span><span class="sxs-lookup"><span data-stu-id="5196f-214">-f</span></span> | <span data-ttu-id="5196f-215">--Vynutit</span><span class="sxs-lookup"><span data-stu-id="5196f-215">--force</span></span> | <span data-ttu-id="5196f-216">Nekontrolovat Pokud chcete zobrazit, pokud se migrace použil k databázi.</span><span class="sxs-lookup"><span data-stu-id="5196f-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="5196f-217">skript migrace ef DotNet.</span><span class="sxs-lookup"><span data-stu-id="5196f-217">dotnet ef migrations script</span></span>

<span data-ttu-id="5196f-218">Generuje skriptu SQL z migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="5196f-219">Argumenty:</span><span class="sxs-lookup"><span data-stu-id="5196f-219">Arguments:</span></span>

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| <span data-ttu-id="5196f-220">\<Z ></span><span class="sxs-lookup"><span data-stu-id="5196f-220">\<FROM></span></span> | <span data-ttu-id="5196f-221">Počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-221">The starting migration.</span></span> <span data-ttu-id="5196f-222">Výchozí hodnota je 0 (počáteční databáze).</span><span class="sxs-lookup"><span data-stu-id="5196f-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="5196f-223">\<K ></span><span class="sxs-lookup"><span data-stu-id="5196f-223">\<TO></span></span>   | <span data-ttu-id="5196f-224">Koncová migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-224">The ending migration.</span></span> <span data-ttu-id="5196f-225">Výchozí hodnoty na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="5196f-226">Možnosti:</span><span class="sxs-lookup"><span data-stu-id="5196f-226">Options:</span></span>

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="5196f-227">-o</span><span class="sxs-lookup"><span data-stu-id="5196f-227">-o</span></span> | <span data-ttu-id="5196f-228">--výstup \<souboru ></span><span class="sxs-lookup"><span data-stu-id="5196f-228">--output \<FILE></span></span> | <span data-ttu-id="5196f-229">Soubor k zápisu výsledek, který má.</span><span class="sxs-lookup"><span data-stu-id="5196f-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="5196f-230">-i</span><span class="sxs-lookup"><span data-stu-id="5196f-230">-i</span></span> | <span data-ttu-id="5196f-231">--idempotent</span><span class="sxs-lookup"><span data-stu-id="5196f-231">--idempotent</span></span>     | <span data-ttu-id="5196f-232">Generovat skript, který lze použít v databázi v žádné migrace.</span><span class="sxs-lookup"><span data-stu-id="5196f-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
