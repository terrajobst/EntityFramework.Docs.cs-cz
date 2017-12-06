---
title: "Konzola správce balíčků (Visual Studio) – EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="5685c-102">Nástroje konzoly Správce balíčků EF jádra</span><span class="sxs-lookup"><span data-stu-id="5685c-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="5685c-103">Nástroje EF základní balíček správce konzoly (pomocí PMC) spustit v aplikaci Visual Studio pomocí NuGet [Konzola správce balíčků][2].</span><span class="sxs-lookup"><span data-stu-id="5685c-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="5685c-104">Tyto nástroje pro práci s projekty rozhraní .NET Framework a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5685c-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="5685c-105">Není pomocí sady Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="5685c-105">Not using Visual Studio?</span></span> <span data-ttu-id="5685c-106">[Nástroje příkazového řádku základní EF] [ 1] jsou napříč platformami a spusťte v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="5685c-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="5685c-107">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="5685c-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="5685c-108">Instalace balíčku Microsoft.EntityFrameworkCore.Tools NuGet nainstalujte nástroje konzoly Správce balíčků EF jádra.</span><span class="sxs-lookup"><span data-stu-id="5685c-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="5685c-109">Můžete ho nainstalovat spuštěním následujícího příkazu uvnitř [Konzola správce balíčků][2].</span><span class="sxs-lookup"><span data-stu-id="5685c-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="5685c-110">Pokud všechno fungovala správně, měli byste mít moci spustit tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="5685c-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="5685c-111">Pokud je cílem vašeho projektu spuštění .NET Standard [cross cíl podporovaných prostředí] [ 3] před použitím nástroje.</span><span class="sxs-lookup"><span data-stu-id="5685c-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5685c-112">Pokud používáte **Universal Windows** nebo **Xamarin**, přesuňte EF kódu do .NET standardní knihovny tříd a [cross cíl podporovaných prostředí] [ 3] před použitím nástroje.</span><span class="sxs-lookup"><span data-stu-id="5685c-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="5685c-113">Zadejte knihovny tříd jako spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="5685c-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="5685c-114">Pomocí nástrojů</span><span class="sxs-lookup"><span data-stu-id="5685c-114">Using the tools</span></span>
---------------
<span data-ttu-id="5685c-115">Při každém vyvolání příkazu, existují dva projekty související se situací:</span><span class="sxs-lookup"><span data-stu-id="5685c-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="5685c-116">Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat).</span><span class="sxs-lookup"><span data-stu-id="5685c-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="5685c-117">Použije se výchozí hodnota cílový projekt **výchozí projekt** vybrali v konzole Správce balíčků, ale můžete také zadat pomocí parametru - projektu.</span><span class="sxs-lookup"><span data-stu-id="5685c-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="5685c-118">Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="5685c-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="5685c-119">Výchozí hodnota jeden **nastavit jako spouštěný projekt** v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="5685c-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="5685c-120">Je také lze pomocí parametru - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="5685c-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="5685c-121">Společné parametry:</span><span class="sxs-lookup"><span data-stu-id="5685c-121">Common parameters:</span></span>

|                           |                             |
| ------------------------- | --------------------------- |
| <span data-ttu-id="5685c-122">-Kontext- \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-122">-Context \<String></span></span>        | <span data-ttu-id="5685c-123">DbContext používat.</span><span class="sxs-lookup"><span data-stu-id="5685c-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="5685c-124">-Projektu \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-124">-Project \<String></span></span>        | <span data-ttu-id="5685c-125">Projekt, který používá.</span><span class="sxs-lookup"><span data-stu-id="5685c-125">The project to use.</span></span>         |
| <span data-ttu-id="5685c-126">-StartupProject \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-126">-StartupProject \<String></span></span> | <span data-ttu-id="5685c-127">Spuštění projektu pro použití.</span><span class="sxs-lookup"><span data-stu-id="5685c-127">The startup project to use.</span></span> |
| <span data-ttu-id="5685c-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="5685c-128">-Verbose</span></span>                  | <span data-ttu-id="5685c-129">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="5685c-129">Show verbose output.</span></span>        |

<span data-ttu-id="5685c-130">Pokud chcete zobrazit nápovědu informace o příkazu, pomocí prostředí PowerShell na `Get-Help` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5685c-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="5685c-131">Parametry kontextu, projekt a StartupProject podporovat karta rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5685c-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="5685c-132">Nastavit **env:ASPNETCORE_ENVIRONMENT** před spuštěním k určení prostředí ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5685c-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="5685c-133">Příkazy</span><span class="sxs-lookup"><span data-stu-id="5685c-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="5685c-134">Přidat migrace</span><span class="sxs-lookup"><span data-stu-id="5685c-134">Add-Migration</span></span>

<span data-ttu-id="5685c-135">Přidá nové migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-135">Adds a new migration.</span></span>

<span data-ttu-id="5685c-136">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5685c-136">Parameters:</span></span>

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| <span data-ttu-id="5685c-137">***-Název*** \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-137">***-Name*** \<String></span></span>              | <span data-ttu-id="5685c-138">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-138">The name of the migration.</span></span>                                                      |
| <span data-ttu-id="5685c-139"><nobr>-OutputDir \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="5685c-139"><nobr>-OutputDir \<String></nobr></span></span>  | <span data-ttu-id="5685c-140">Adresář (a podklíčů – obor názvů) používat.</span><span class="sxs-lookup"><span data-stu-id="5685c-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="5685c-141">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="5685c-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="5685c-142">Výchozí hodnota je "Migrace".</span><span class="sxs-lookup"><span data-stu-id="5685c-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="5685c-143">Parametry v **tučné** jsou povinné a ty, které v *italics* jsou poziční.</span><span class="sxs-lookup"><span data-stu-id="5685c-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="5685c-144">Vyřaďte databázi</span><span class="sxs-lookup"><span data-stu-id="5685c-144">Drop-Database</span></span>

<span data-ttu-id="5685c-145">Zahodí databáze.</span><span class="sxs-lookup"><span data-stu-id="5685c-145">Drops the database.</span></span>

<span data-ttu-id="5685c-146">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5685c-146">Parameters:</span></span>

|          |                                                          |
| -------- | -------------------------------------------------------- |
| <span data-ttu-id="5685c-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="5685c-147">-WhatIf</span></span>  | <span data-ttu-id="5685c-148">Zobrazit databázi, kterou by být vyřazen, ale nemáte vyřadit.</span><span class="sxs-lookup"><span data-stu-id="5685c-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="5685c-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="5685c-149">Get-DbContext</span></span>

<span data-ttu-id="5685c-150">Získá informace o typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="5685c-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="5685c-151">Odebrat migrace</span><span class="sxs-lookup"><span data-stu-id="5685c-151">Remove-Migration</span></span>

<span data-ttu-id="5685c-152">Odebere poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-152">Removes the last migration.</span></span>

<span data-ttu-id="5685c-153">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5685c-153">Parameters:</span></span>

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| <span data-ttu-id="5685c-154">-Force</span><span class="sxs-lookup"><span data-stu-id="5685c-154">-Force</span></span> | <span data-ttu-id="5685c-155">Nekontrolovat Pokud chcete zobrazit, pokud se migrace použil k databázi.</span><span class="sxs-lookup"><span data-stu-id="5685c-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="5685c-156">DbContext vygenerované uživatelské rozhraní</span><span class="sxs-lookup"><span data-stu-id="5685c-156">Scaffold-DbContext</span></span>

<span data-ttu-id="5685c-157">Scaffold a typy DbContext a entity pro databázi.</span><span class="sxs-lookup"><span data-stu-id="5685c-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="5685c-158">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5685c-158">Parameters:</span></span>

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <span data-ttu-id="5685c-159"><nobr>***-Připojení*** \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="5685c-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="5685c-160">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="5685c-160">The connection string to the database.</span></span>                                    |
| <span data-ttu-id="5685c-161">***-Poskytovatel*** \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="5685c-162">Zprostředkovatel, který se má používat.</span><span class="sxs-lookup"><span data-stu-id="5685c-162">The provider to use.</span></span> <span data-ttu-id="5685c-163">(Např.)</span><span class="sxs-lookup"><span data-stu-id="5685c-163">(E.g.</span></span> <span data-ttu-id="5685c-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="5685c-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>       |
| <span data-ttu-id="5685c-165">-OutputDir \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="5685c-166">Adresář, který chcete umístit soubory do.</span><span class="sxs-lookup"><span data-stu-id="5685c-166">The directory to put files in.</span></span> <span data-ttu-id="5685c-167">Cesty se vztahují k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="5685c-167">Paths are relative to the project directory.</span></span> |
| <span data-ttu-id="5685c-168">-Kontext- \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-168">-Context \<String></span></span>                       | <span data-ttu-id="5685c-169">Název DbContext ke generování.</span><span class="sxs-lookup"><span data-stu-id="5685c-169">The name of the DbContext to generate.</span></span>                                    |
| <span data-ttu-id="5685c-170">-Schémata \<řetězec [] ></span><span class="sxs-lookup"><span data-stu-id="5685c-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="5685c-171">Schémata tabulkami k vygenerování typy entit pro.</span><span class="sxs-lookup"><span data-stu-id="5685c-171">The schemas of tables to generate entity types for.</span></span>                       |
| <span data-ttu-id="5685c-172">-Tabulky \<řetězec [] ></span><span class="sxs-lookup"><span data-stu-id="5685c-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="5685c-173">S tabulkami k vygenerování typy entit pro.</span><span class="sxs-lookup"><span data-stu-id="5685c-173">The tables to generate entity types for.</span></span>                                  |
| <span data-ttu-id="5685c-174">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="5685c-174">-DataAnnotations</span></span>                         | <span data-ttu-id="5685c-175">Pomocí atributů (kde je to možné), nakonfigurujte model.</span><span class="sxs-lookup"><span data-stu-id="5685c-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="5685c-176">Pokud tento parametr vynechán, použije se rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="5685c-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="5685c-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="5685c-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="5685c-178">Používejte názvy tabulek a sloupců přímo z databáze.</span><span class="sxs-lookup"><span data-stu-id="5685c-178">Use table and column names directly from the database.</span></span>                    |
| <span data-ttu-id="5685c-179">-Force</span><span class="sxs-lookup"><span data-stu-id="5685c-179">-Force</span></span>                                   | <span data-ttu-id="5685c-180">Přepište existující soubory.</span><span class="sxs-lookup"><span data-stu-id="5685c-180">Overwrite existing files.</span></span>                                                 |

### <a name="script-migration"></a><span data-ttu-id="5685c-181">Skript migrace</span><span class="sxs-lookup"><span data-stu-id="5685c-181">Script-Migration</span></span>

<span data-ttu-id="5685c-182">Generuje skriptu SQL z migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="5685c-183">Parametry:</span><span class="sxs-lookup"><span data-stu-id="5685c-183">Parameters:</span></span>

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="5685c-184">*-Z* \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-184">*-From* \<String></span></span> | <span data-ttu-id="5685c-185">Počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-185">The starting migration.</span></span> <span data-ttu-id="5685c-186">Výchozí hodnota je 0 (počáteční databáze).</span><span class="sxs-lookup"><span data-stu-id="5685c-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="5685c-187">*-Na* \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-187">*-To* \<String></span></span>   | <span data-ttu-id="5685c-188">Koncová migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-188">The ending migration.</span></span> <span data-ttu-id="5685c-189">Výchozí hodnoty na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="5685c-190">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="5685c-190">-Idempotent</span></span>       | <span data-ttu-id="5685c-191">Generovat skript, který lze použít v databázi v žádné migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="5685c-192">-Výstup \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="5685c-192">-Output \<String></span></span> | <span data-ttu-id="5685c-193">Soubor k zápisu výsledek, který má.</span><span class="sxs-lookup"><span data-stu-id="5685c-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="5685c-194">Komu, z, a podporují karta rozšíření výstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="5685c-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="5685c-195">Update-Database</span><span class="sxs-lookup"><span data-stu-id="5685c-195">Update-Database</span></span>

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="5685c-196"><nobr>*-Migrace* \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="5685c-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="5685c-197">Cílová migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-197">The target migration.</span></span> <span data-ttu-id="5685c-198">Pokud je to "0", budou vráceny všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="5685c-199">Výchozí hodnoty na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="5685c-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="5685c-200">Parametr migrace podporuje karta rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5685c-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
