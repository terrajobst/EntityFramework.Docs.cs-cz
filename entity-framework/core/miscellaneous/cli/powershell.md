---
title: Konzola správce balíčků (Visual Studio) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490847"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="67db4-102">EF Core Package Manageru konzoly nástroje</span><span class="sxs-lookup"><span data-stu-id="67db4-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="67db4-103">Nástroje EF Core Package Manageru konzoly (PMC) spuštění v aplikaci Visual Studio pomocí Nugetu [Konzola správce balíčků][2].</span><span class="sxs-lookup"><span data-stu-id="67db4-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="67db4-104">Tyto nástroje pracují s projekty rozhraní .NET Framework a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="67db4-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="67db4-105">Bez použití sady Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="67db4-105">Not using Visual Studio?</span></span> <span data-ttu-id="67db4-106">[Nástroje příkazového řádku EF Core] [ 1] jsou různé platformy a spusťte v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="67db4-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="67db4-107">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="67db4-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="67db4-108">Instaluje se balíček Microsoft.EntityFrameworkCore.Tools NuGet nainstalujte nástroje konzoly Správce balíčků EF Core.</span><span class="sxs-lookup"><span data-stu-id="67db4-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="67db4-109">Můžete jej nainstalovat spuštěním následujícího příkazu v [Konzola správce balíčků][2].</span><span class="sxs-lookup"><span data-stu-id="67db4-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="67db4-110">Pokud vše funguje správně, je třeba spustit tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="67db4-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="67db4-111">Pokud váš spouštěný projekt cílí na .NET Standard [zacílení podporované architektury] [ 3] před použitím nástroje.</span><span class="sxs-lookup"><span data-stu-id="67db4-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67db4-112">Pokud používáte **Universal Windows** nebo **Xamarin**, přesuňte váš kód EF do knihovny tříd .NET Standard a [zacílení podporované architektury] [ 3] před použitím nástroje.</span><span class="sxs-lookup"><span data-stu-id="67db4-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="67db4-113">Zadejte jako spouštěný projekt knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="67db4-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="67db4-114">Pomocí nástrojů</span><span class="sxs-lookup"><span data-stu-id="67db4-114">Using the tools</span></span>
---------------
<span data-ttu-id="67db4-115">Při každém vyvolání příkazu, existují dva projekty zahrnuté:</span><span class="sxs-lookup"><span data-stu-id="67db4-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="67db4-116">Cílový projekt je tam, kde se přidají všechny soubory (nebo v některých případech odebrána).</span><span class="sxs-lookup"><span data-stu-id="67db4-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="67db4-117">Výchozí nastavení na cílový projekt **výchozí projekt** vybraný v konzole Správce balíčků, ale můžete také zadat pomocí parametru - projektu.</span><span class="sxs-lookup"><span data-stu-id="67db4-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="67db4-118">Projekt po spuštění je emulován nástroji při spuštění kódu projektu.</span><span class="sxs-lookup"><span data-stu-id="67db4-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="67db4-119">Použije se výchozí jeden **nastavit jako spouštěný projekt** v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="67db4-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="67db4-120">Lze také určit pomocí parametru - výchozí projekt.</span><span class="sxs-lookup"><span data-stu-id="67db4-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="67db4-121">Společné parametry:</span><span class="sxs-lookup"><span data-stu-id="67db4-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="67db4-122">-Kontextu \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-122">-Context \<String></span></span>        | <span data-ttu-id="67db4-123">Chcete-li použít uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="67db4-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="67db4-124">-Projektu \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-124">-Project \<String></span></span>        | <span data-ttu-id="67db4-125">Projekt pro použití.</span><span class="sxs-lookup"><span data-stu-id="67db4-125">The project to use.</span></span>         |
| <span data-ttu-id="67db4-126">– Výchozí projekt \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-126">-StartupProject \<String></span></span> | <span data-ttu-id="67db4-127">Použít spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="67db4-127">The startup project to use.</span></span> |
| <span data-ttu-id="67db4-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="67db4-128">-Verbose</span></span>                  | <span data-ttu-id="67db4-129">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="67db4-129">Show verbose output.</span></span>        |

<span data-ttu-id="67db4-130">Chcete-li zobrazit nápovědu informace o příkazu, pomocí Powershellu `Get-Help` příkazu.</span><span class="sxs-lookup"><span data-stu-id="67db4-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="67db4-131">Parametry kontextu, projektu a výchozí projekt podporují rozšíření tab.</span><span class="sxs-lookup"><span data-stu-id="67db4-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="67db4-132">Nastavte **env:ASPNETCORE_ENVIRONMENT** před spuštěním zadat prostředí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="67db4-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="67db4-133">Příkazy</span><span class="sxs-lookup"><span data-stu-id="67db4-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="67db4-134">Přidat migrace</span><span class="sxs-lookup"><span data-stu-id="67db4-134">Add-Migration</span></span>

<span data-ttu-id="67db4-135">Přidá novou migraci.</span><span class="sxs-lookup"><span data-stu-id="67db4-135">Adds a new migration.</span></span>

<span data-ttu-id="67db4-136">Parametry:</span><span class="sxs-lookup"><span data-stu-id="67db4-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="67db4-137">***-Název*** \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-137">***-Name*** \<String></span></span>             | <span data-ttu-id="67db4-138">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="67db4-139"><nobr>-OutputDir \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="67db4-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="67db4-140">Adresáři (a v podřízeném oboru názvů) používat.</span><span class="sxs-lookup"><span data-stu-id="67db4-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="67db4-141">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="67db4-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="67db4-142">Výchozí hodnota je "Migrace".</span><span class="sxs-lookup"><span data-stu-id="67db4-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="67db4-143">Parametry v **tučné** jsou povinné a těch, které jsou v *Kurzíva* jsou poziční.</span><span class="sxs-lookup"><span data-stu-id="67db4-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="67db4-144">Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="67db4-144">Drop-Database</span></span>

<span data-ttu-id="67db4-145">Zahodí databáze.</span><span class="sxs-lookup"><span data-stu-id="67db4-145">Drops the database.</span></span>

<span data-ttu-id="67db4-146">Parametry:</span><span class="sxs-lookup"><span data-stu-id="67db4-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="67db4-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="67db4-147">-WhatIf</span></span> | <span data-ttu-id="67db4-148">Zobrazit, které databáze bude vyřazena, ale ji.</span><span class="sxs-lookup"><span data-stu-id="67db4-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="67db4-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="67db4-149">Get-DbContext</span></span>

<span data-ttu-id="67db4-150">Získá informace o typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="67db4-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="67db4-151">Remove migrace</span><span class="sxs-lookup"><span data-stu-id="67db4-151">Remove-Migration</span></span>

<span data-ttu-id="67db4-152">Odebere poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-152">Removes the last migration.</span></span>

<span data-ttu-id="67db4-153">Parametry:</span><span class="sxs-lookup"><span data-stu-id="67db4-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="67db4-154">-Force</span><span class="sxs-lookup"><span data-stu-id="67db4-154">-Force</span></span> | <span data-ttu-id="67db4-155">Vrácení migrace, pokud byl použit k databázi.</span><span class="sxs-lookup"><span data-stu-id="67db4-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="67db4-156">Vygenerované uživatelské rozhraní DbContext</span><span class="sxs-lookup"><span data-stu-id="67db4-156">Scaffold-DbContext</span></span>

<span data-ttu-id="67db4-157">Nástroj scaffold kontext databáze a entity typy pro databázi.</span><span class="sxs-lookup"><span data-stu-id="67db4-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="67db4-158">Parametry:</span><span class="sxs-lookup"><span data-stu-id="67db4-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="67db4-159"><nobr>***-Connection*** \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="67db4-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="67db4-160">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="67db4-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="67db4-161">***-Poskytovatel*** \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="67db4-162">Zprostředkovatel k použití.</span><span class="sxs-lookup"><span data-stu-id="67db4-162">The provider to use.</span></span> <span data-ttu-id="67db4-163">(například Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="67db4-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="67db4-164">-OutputDir \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="67db4-165">Adresář, který se má vložit soubory.</span><span class="sxs-lookup"><span data-stu-id="67db4-165">The directory to put files in.</span></span> <span data-ttu-id="67db4-166">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="67db4-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="67db4-167">-ContextDir \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="67db4-168">Adresář, který se má vložit DbContext.</span><span class="sxs-lookup"><span data-stu-id="67db4-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="67db4-169">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="67db4-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="67db4-170">-Kontextu \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-170">-Context \<String></span></span>                       | <span data-ttu-id="67db4-171">Název DbContext ke generování.</span><span class="sxs-lookup"><span data-stu-id="67db4-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="67db4-172">-Schémata \<String [] ></span><span class="sxs-lookup"><span data-stu-id="67db4-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="67db4-173">Schémata tabulek ke generování typů entit pro.</span><span class="sxs-lookup"><span data-stu-id="67db4-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="67db4-174">-Tabulky \<String [] ></span><span class="sxs-lookup"><span data-stu-id="67db4-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="67db4-175">Tabulky pro typy entit pro generování.</span><span class="sxs-lookup"><span data-stu-id="67db4-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="67db4-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="67db4-176">-DataAnnotations</span></span>                         | <span data-ttu-id="67db4-177">Atributy lze použijte ke konfiguraci modelu (Pokud je to možné).</span><span class="sxs-lookup"><span data-stu-id="67db4-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="67db4-178">Pokud tento parametr vynechán, použije se pouze rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="67db4-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="67db4-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="67db4-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="67db4-180">Použijte názvy tabulek a sloupců přímo z databáze.</span><span class="sxs-lookup"><span data-stu-id="67db4-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="67db4-181">-Force</span><span class="sxs-lookup"><span data-stu-id="67db4-181">-Force</span></span>                                   | <span data-ttu-id="67db4-182">Přepište existující soubory.</span><span class="sxs-lookup"><span data-stu-id="67db4-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="67db4-183">Skript migrace</span><span class="sxs-lookup"><span data-stu-id="67db4-183">Script-Migration</span></span>

<span data-ttu-id="67db4-184">Vygeneruje skript SQL z migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="67db4-185">Parametry:</span><span class="sxs-lookup"><span data-stu-id="67db4-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="67db4-186">*-Od* \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-186">*-From* \<String></span></span> | <span data-ttu-id="67db4-187">Počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="67db4-187">The starting migration.</span></span> <span data-ttu-id="67db4-188">Výchozí hodnota je 0 (výchozí databáze).</span><span class="sxs-lookup"><span data-stu-id="67db4-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="67db4-189">*-Na* \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-189">*-To* \<String></span></span>   | <span data-ttu-id="67db4-190">Dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-190">The ending migration.</span></span> <span data-ttu-id="67db4-191">Výchozí hodnota je poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="67db4-192">-Idempotentní</span><span class="sxs-lookup"><span data-stu-id="67db4-192">-Idempotent</span></span>       | <span data-ttu-id="67db4-193">Generovat skript, který jde použít na databáze v libovolné migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="67db4-194">-Výstupní \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="67db4-194">-Output \<String></span></span> | <span data-ttu-id="67db4-195">Soubor pro zápis výsledek.</span><span class="sxs-lookup"><span data-stu-id="67db4-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="67db4-196">Na, z, a podpora rozšíření tab výstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="67db4-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="67db4-197">Aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="67db4-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="67db4-198"><nobr>*– Migrace* \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="67db4-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="67db4-199">Cíl migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-199">The target migration.</span></span> <span data-ttu-id="67db4-200">Pokud je "0", se vrátí zpět všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="67db4-201">Výchozí hodnota je poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="67db4-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="67db4-202">Parametr migrace podporuje rozšíření tab.</span><span class="sxs-lookup"><span data-stu-id="67db4-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
