---
title: EF Core tools odkaz (konzola Správce balíčků) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 9a57b58f8569ee1241e40c3809b03487d1d88e02
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834757"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="00065-102">Entity Framework Core tools reference - Konzola správce balíčků v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00065-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="00065-103">Konzola správce balíčků (PMC) nástroje pro Entity Framework Core provádět úlohy vývojem během návrhu.</span><span class="sxs-lookup"><span data-stu-id="00065-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="00065-104">Například vytvořit [migrace](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations)použít migrace a generovat kód pro model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="00065-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="00065-105">Příkazy se spouští v rámci služby Visual Studio pomocí [Konzola správce balíčků](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="00065-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="00065-106">Tyto nástroje pracují s projekty rozhraní .NET Framework a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="00065-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="00065-107">Pokud nepoužíváte Visual Studio, doporučujeme [nástroje příkazového řádku EF Core](dotnet.md) místo.</span><span class="sxs-lookup"><span data-stu-id="00065-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="00065-108">Nástroje rozhraní příkazového řádku jsou různé platformy a spusťte v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="00065-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="00065-109">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="00065-109">Installing the tools</span></span>

<span data-ttu-id="00065-110">Postupy pro instalaci a aktualizaci nástroje se liší mezi ASP.NET Core 2.1 + a starší verze nebo jiné typy projektů.</span><span class="sxs-lookup"><span data-stu-id="00065-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="00065-111">ASP.NET Core 2.1 nebo novější verze</span><span class="sxs-lookup"><span data-stu-id="00065-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="00065-112">Nástroje jsou automaticky obsažené v projektu aplikace ASP.NET Core 2.1 +, protože `Microsoft.EntityFrameworkCore.Tools` je součástí balíčku [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="00065-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="00065-113">Proto nemusíte dělat nic. instalace nástrojů, ale budete muset:</span><span class="sxs-lookup"><span data-stu-id="00065-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="00065-114">Obnovení balíčků před použitím nástroje v novém projektu.</span><span class="sxs-lookup"><span data-stu-id="00065-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="00065-115">Instalovat balíček aktualizace na novější verzi nástrojů.</span><span class="sxs-lookup"><span data-stu-id="00065-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="00065-116">Pokud chcete mít jistotu, že se zobrazuje nejnovější verzi nástrojů, doporučujeme také provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="00065-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="00065-117">Upravit vaše *.csproj* a přidejte řádek určující nejnovější verzi [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="00065-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="00065-118">Například *.csproj* může obsahovat soubor `ItemGroup` , která vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="00065-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="00065-119">Aktualizace nástrojů, když se zobrazí zpráva jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="00065-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="00065-120">Verze nástrojů EF Core "2.1.1-rtm-30846" je starší než modul runtime "2.1.3-rtm-32065".</span><span class="sxs-lookup"><span data-stu-id="00065-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="00065-121">Aktualizace nástrojů pro nejnovější funkce a opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="00065-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="00065-122">Aktualizace nástrojů:</span><span class="sxs-lookup"><span data-stu-id="00065-122">To update the tools:</span></span>
* <span data-ttu-id="00065-123">Instalace nejnovější .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="00065-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="00065-124">Aktualizace na nejnovější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00065-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="00065-125">Upravit *.csproj* souboru tak, že obsahují odkaz na balíček na nejnovější nástroje pro balíček, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="00065-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="00065-126">Jiné verze a typy projektů</span><span class="sxs-lookup"><span data-stu-id="00065-126">Other versions and project types</span></span>

<span data-ttu-id="00065-127">Nainstalujte nástroje konzoly Správce balíčků spuštěním následujícího příkazu v **Konzola správce balíčků**:</span><span class="sxs-lookup"><span data-stu-id="00065-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="00065-128">Aktualizace nástrojů spuštěním následujícího příkazu v **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="00065-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="00065-129">Ověření instalace</span><span class="sxs-lookup"><span data-stu-id="00065-129">Verify the installation</span></span>

<span data-ttu-id="00065-130">Ověřte, zda jsou nástroje nainstalovány spuštěním tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="00065-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="00065-131">Výstup vypadá takto (neříká je verze nástrojů, které používáte):</span><span class="sxs-lookup"><span data-stu-id="00065-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="00065-132">Pomocí nástrojů</span><span class="sxs-lookup"><span data-stu-id="00065-132">Using the tools</span></span>

<span data-ttu-id="00065-133">Před použitím nástroje:</span><span class="sxs-lookup"><span data-stu-id="00065-133">Before using the tools:</span></span>
* <span data-ttu-id="00065-134">Pochopili rozdíl mezi cíl a spuštění projektu.</span><span class="sxs-lookup"><span data-stu-id="00065-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="00065-135">Zjistěte, jak pomocí nástrojů pomocí knihovny tříd .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="00065-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="00065-136">Pro projekty ASP.NET Core nastavte prostředí.</span><span class="sxs-lookup"><span data-stu-id="00065-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="00065-137">Cíl a spuštění projektu</span><span class="sxs-lookup"><span data-stu-id="00065-137">Target and startup project</span></span>

<span data-ttu-id="00065-138">Příkazy odkazovat *projektu* a *spouštěný projekt*.</span><span class="sxs-lookup"><span data-stu-id="00065-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="00065-139">*Projektu* se také označuje jako *cílový projekt* protože se jedná, kde příkazy přidávání nebo odebírání souborů.</span><span class="sxs-lookup"><span data-stu-id="00065-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="00065-140">Ve výchozím nastavení **výchozí projekt** vybrané v **Konzola správce balíčků** je na cílový projekt.</span><span class="sxs-lookup"><span data-stu-id="00065-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="00065-141">Můžete zadat jiný projekt jako cílový projekt pomocí <nobr> `--project` </nobr> možnost.</span><span class="sxs-lookup"><span data-stu-id="00065-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="00065-142">*Spouštěný projekt* je ta, kterou nástroje pro sestavení a spuštění.</span><span class="sxs-lookup"><span data-stu-id="00065-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="00065-143">Nástroje třeba spustit kód aplikace v době návrhu a získat informace o projektu, jako je například připojovací řetězec databáze a konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="00065-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="00065-144">Ve výchozím nastavení **spouštěný projekt** v **Průzkumníku řešení** je projekt po spuštění.</span><span class="sxs-lookup"><span data-stu-id="00065-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="00065-145">Můžete určit jiný projekt jako spouštěný projekt pomocí <nobr> `--startup-project` </nobr> možnost.</span><span class="sxs-lookup"><span data-stu-id="00065-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="00065-146">Projekt po spuštění a cílový projekt se často stejného projektu.</span><span class="sxs-lookup"><span data-stu-id="00065-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="00065-147">Typický scénář, kde jsou samostatné projekty je těchto případech:</span><span class="sxs-lookup"><span data-stu-id="00065-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="00065-148">EF Core třídy kontextu a entity jsou v knihovně tříd .NET Core.</span><span class="sxs-lookup"><span data-stu-id="00065-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="00065-149">Aplikace konzoly .NET Core nebo webové aplikace odkazuje na knihovnu tříd.</span><span class="sxs-lookup"><span data-stu-id="00065-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="00065-150">Je také možné [migrace kódu do knihovny tříd nezávisle na EF Core kontextu](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="00065-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="00065-151">Ostatní cílové platformy</span><span class="sxs-lookup"><span data-stu-id="00065-151">Other target frameworks</span></span>

<span data-ttu-id="00065-152">Konzola správce balíčků nástroje pracovat s projekty .NET Core nebo .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00065-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="00065-153">Nemusí mít aplikace, které mají modelu EF Core v knihovně tříd .NET Standard, .NET Core nebo .NET Framework projektu.</span><span class="sxs-lookup"><span data-stu-id="00065-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="00065-154">Například to platí pro aplikace Xamarin a univerzální platformu Windows.</span><span class="sxs-lookup"><span data-stu-id="00065-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="00065-155">V takovém případě můžete vytvořit .NET Core nebo .NET Framework projektu aplikace konzoly jehož jediným účelem je tak, aby fungoval jako projekt po spuštění pro nástroje.</span><span class="sxs-lookup"><span data-stu-id="00065-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="00065-156">Projekt může být fiktivní projekt bez skutečné kódu &mdash; je pouze potřebných k poskytování cíl pro nástroje.</span><span class="sxs-lookup"><span data-stu-id="00065-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="00065-157">Proč je fiktivní projektu vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="00065-157">Why is a dummy project required?</span></span> <span data-ttu-id="00065-158">Jak už bylo zmíněno dříve, mají nástroje k provádění kódu aplikace v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="00065-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="00065-159">K tomu, které potřebují na využití modulu runtime .NET Core nebo .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00065-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="00065-160">Když v projektu, který cílí na .NET Core nebo .NET Framework je model EF Core, si půjčte nástroje EF Core runtime z projektu.</span><span class="sxs-lookup"><span data-stu-id="00065-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="00065-161">Nelze provést, pokud je model EF Core v knihovně tříd .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="00065-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="00065-162">.NET Standard není Skutečná implementace .NET; je specifikace sady rozhraní API, která musí podporovat implementace .NET.</span><span class="sxs-lookup"><span data-stu-id="00065-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="00065-163">Proto .NET Standard není dostatečná pro EF Core nástroje k provádění kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="00065-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="00065-164">Fiktivní projekt, který vytvoříte pro použití jako spouštěný projekt obsahuje konkrétní cílovou platformu, do kterého nástroje můžete načíst knihovně tříd rozhraní .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="00065-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="00065-165">Prostředí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00065-165">ASP.NET Core environment</span></span>

<span data-ttu-id="00065-166">Chcete-li určit prostředí pro projekty ASP.NET Core, nastavte **env:ASPNETCORE_ENVIRONMENT** před spuštěním příkazů.</span><span class="sxs-lookup"><span data-stu-id="00065-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="00065-167">Společné parametry</span><span class="sxs-lookup"><span data-stu-id="00065-167">Common parameters</span></span>

<span data-ttu-id="00065-168">V následující tabulce jsou uvedeny parametry, které jsou společné pro všechny příkazy EF Core:</span><span class="sxs-lookup"><span data-stu-id="00065-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="00065-169">Parametr</span><span class="sxs-lookup"><span data-stu-id="00065-169">Parameter</span></span>                 | <span data-ttu-id="00065-170">Popis</span><span class="sxs-lookup"><span data-stu-id="00065-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="00065-171">-Kontextu \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-171">-Context \<String></span></span>        | <span data-ttu-id="00065-172">`DbContext` Třídu použít.</span><span class="sxs-lookup"><span data-stu-id="00065-172">The `DbContext` class to use.</span></span> <span data-ttu-id="00065-173">Název třídy pouze nebo plně kvalifikovaný s obory názvů.</span><span class="sxs-lookup"><span data-stu-id="00065-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="00065-174">Pokud je tento parametr vynechán, vyhledá EF Core třídy kontextu.</span><span class="sxs-lookup"><span data-stu-id="00065-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="00065-175">Pokud existuje více tříd kontextu, tento parametr je povinný.</span><span class="sxs-lookup"><span data-stu-id="00065-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="00065-176">-Projektu \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-176">-Project \<String></span></span>        | <span data-ttu-id="00065-177">Cílový projekt.</span><span class="sxs-lookup"><span data-stu-id="00065-177">The target project.</span></span> <span data-ttu-id="00065-178">Pokud je tento parametr vynechán, **výchozí projekt** pro **Konzola správce balíčků** se používá jako cílový projekt.</span><span class="sxs-lookup"><span data-stu-id="00065-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="00065-179">– Výchozí projekt \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-179">-StartupProject \<String></span></span> | <span data-ttu-id="00065-180">Spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="00065-180">The startup project.</span></span> <span data-ttu-id="00065-181">Pokud je tento parametr vynechán, **spouštěný projekt** v **vlastnosti řešení** se používá jako cílový projekt.</span><span class="sxs-lookup"><span data-stu-id="00065-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="00065-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="00065-182">-Verbose</span></span>                  | <span data-ttu-id="00065-183">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="00065-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="00065-184">Chcete-li zobrazit nápovědu informace o příkazu, pomocí Powershellu `Get-Help` příkazu.</span><span class="sxs-lookup"><span data-stu-id="00065-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="00065-185">Parametry kontextu, projektu a výchozí projekt podporují rozšíření tab.</span><span class="sxs-lookup"><span data-stu-id="00065-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="00065-186">Přidat migrace</span><span class="sxs-lookup"><span data-stu-id="00065-186">Add-Migration</span></span>

<span data-ttu-id="00065-187">Přidá novou migraci.</span><span class="sxs-lookup"><span data-stu-id="00065-187">Adds a new migration.</span></span>

<span data-ttu-id="00065-188">Parametry:</span><span class="sxs-lookup"><span data-stu-id="00065-188">Parameters:</span></span>

| <span data-ttu-id="00065-189">Parametr</span><span class="sxs-lookup"><span data-stu-id="00065-189">Parameter</span></span>                         | <span data-ttu-id="00065-190">Popis</span><span class="sxs-lookup"><span data-stu-id="00065-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="00065-191"><nobr>-Název \<řetězec ><nobr></span><span class="sxs-lookup"><span data-stu-id="00065-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="00065-192">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-192">The name of the migration.</span></span> <span data-ttu-id="00065-193">Toto je pozičních parametrů a je povinný.</span><span class="sxs-lookup"><span data-stu-id="00065-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="00065-194"><nobr>-OutputDir \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="00065-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="00065-195">Adresáři (a v podřízeném oboru názvů) používat.</span><span class="sxs-lookup"><span data-stu-id="00065-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="00065-196">Cesty jsou relativní vzhledem k adresáři projektu cíl.</span><span class="sxs-lookup"><span data-stu-id="00065-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="00065-197">Výchozí hodnota je "Migrace".</span><span class="sxs-lookup"><span data-stu-id="00065-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="00065-198">Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="00065-198">Drop-Database</span></span>

<span data-ttu-id="00065-199">Zahodí databáze.</span><span class="sxs-lookup"><span data-stu-id="00065-199">Drops the database.</span></span>

<span data-ttu-id="00065-200">Parametry:</span><span class="sxs-lookup"><span data-stu-id="00065-200">Parameters:</span></span>

| <span data-ttu-id="00065-201">Parametr</span><span class="sxs-lookup"><span data-stu-id="00065-201">Parameter</span></span> | <span data-ttu-id="00065-202">Popis</span><span class="sxs-lookup"><span data-stu-id="00065-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="00065-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="00065-203">-WhatIf</span></span>   | <span data-ttu-id="00065-204">Zobrazit, které databáze bude vyřazena, ale ji.</span><span class="sxs-lookup"><span data-stu-id="00065-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="00065-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="00065-205">Get-DbContext</span></span>

<span data-ttu-id="00065-206">Seznamy, které jsou k dispozici `DbContext` typy.</span><span class="sxs-lookup"><span data-stu-id="00065-206">Lists available `DbContext` types.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="00065-207">Remove migrace</span><span class="sxs-lookup"><span data-stu-id="00065-207">Remove-Migration</span></span>

<span data-ttu-id="00065-208">Odebere poslední migrace (vrátí zpět změny kódu, které jste dokončili migraci).</span><span class="sxs-lookup"><span data-stu-id="00065-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="00065-209">Parametry:</span><span class="sxs-lookup"><span data-stu-id="00065-209">Parameters:</span></span>

| <span data-ttu-id="00065-210">Parametr</span><span class="sxs-lookup"><span data-stu-id="00065-210">Parameter</span></span> | <span data-ttu-id="00065-211">Popis</span><span class="sxs-lookup"><span data-stu-id="00065-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="00065-212">-Force</span><span class="sxs-lookup"><span data-stu-id="00065-212">-Force</span></span>    | <span data-ttu-id="00065-213">Vrácení migrace (vrátit zpět změny, které byly použity k databázi).</span><span class="sxs-lookup"><span data-stu-id="00065-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="00065-214">Vygenerované uživatelské rozhraní DbContext</span><span class="sxs-lookup"><span data-stu-id="00065-214">Scaffold-DbContext</span></span>

<span data-ttu-id="00065-215">Generuje kód `DbContext` a typy entit pro databázi.</span><span class="sxs-lookup"><span data-stu-id="00065-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="00065-216">Aby `Scaffold-DbContext` generovat typ entity, databázové tabulky musí mít primární klíč.</span><span class="sxs-lookup"><span data-stu-id="00065-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="00065-217">Parametry:</span><span class="sxs-lookup"><span data-stu-id="00065-217">Parameters:</span></span>

| <span data-ttu-id="00065-218">Parametr</span><span class="sxs-lookup"><span data-stu-id="00065-218">Parameter</span></span>                          | <span data-ttu-id="00065-219">Popis</span><span class="sxs-lookup"><span data-stu-id="00065-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="00065-220"><nobr>-Connection \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="00065-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="00065-221">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="00065-221">The connection string to the database.</span></span> <span data-ttu-id="00065-222">Pro projekty ASP.NET Core 2.x, může být hodnota *název =\<název připojovacího řetězce >*.</span><span class="sxs-lookup"><span data-stu-id="00065-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="00065-223">Název v tomto případě pochází ze zdroje konfigurace, které jsou nastavené pro projekt.</span><span class="sxs-lookup"><span data-stu-id="00065-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="00065-224">Toto je pozičních parametrů a je povinný.</span><span class="sxs-lookup"><span data-stu-id="00065-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="00065-225"><nobr>-Poskytovatel \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="00065-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="00065-226">Zprostředkovatel k použití.</span><span class="sxs-lookup"><span data-stu-id="00065-226">The provider to use.</span></span> <span data-ttu-id="00065-227">Obvykle jde o název balíčku NuGet, například: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="00065-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="00065-228">Toto je pozičních parametrů a je povinný.</span><span class="sxs-lookup"><span data-stu-id="00065-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="00065-229">-OutputDir \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-229">-OutputDir \<String></span></span>               | <span data-ttu-id="00065-230">Adresář, který se má vložit soubory.</span><span class="sxs-lookup"><span data-stu-id="00065-230">The directory to put files in.</span></span> <span data-ttu-id="00065-231">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="00065-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="00065-232">-ContextDir \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-232">-ContextDir \<String></span></span>              | <span data-ttu-id="00065-233">Adresář, který chcete vložit `DbContext` v souboru.</span><span class="sxs-lookup"><span data-stu-id="00065-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="00065-234">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="00065-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="00065-235">-Kontextu \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-235">-Context \<String></span></span>                 | <span data-ttu-id="00065-236">Název `DbContext` k vygenerování.</span><span class="sxs-lookup"><span data-stu-id="00065-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="00065-237">-Schémata \<String [] ></span><span class="sxs-lookup"><span data-stu-id="00065-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="00065-238">Schémata tabulek ke generování typů entit pro.</span><span class="sxs-lookup"><span data-stu-id="00065-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="00065-239">Pokud je tento parametr vynechán, jsou všechna schémata zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="00065-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="00065-240">-Tabulky \<String [] ></span><span class="sxs-lookup"><span data-stu-id="00065-240">-Tables \<String[]></span></span>                | <span data-ttu-id="00065-241">Tabulky pro typy entit pro generování.</span><span class="sxs-lookup"><span data-stu-id="00065-241">The tables to generate entity types for.</span></span> <span data-ttu-id="00065-242">Pokud je tento parametr vynechán, budou zahrnuty všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="00065-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="00065-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="00065-243">-DataAnnotations</span></span>                   | <span data-ttu-id="00065-244">Atributy lze použijte ke konfiguraci modelu (Pokud je to možné).</span><span class="sxs-lookup"><span data-stu-id="00065-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="00065-245">Pokud je tento parametr vynechán, použije se pouze rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="00065-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="00065-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="00065-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="00065-247">Použijte názvy tabulek a sloupců přesně tak, jak jsou uvedeny v databázi.</span><span class="sxs-lookup"><span data-stu-id="00065-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="00065-248">Pokud je tento parametr vynechán, aby lépe odpovídaly konvence stylu název jazyka C# změna názvů databáze.</span><span class="sxs-lookup"><span data-stu-id="00065-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="00065-249">-Force</span><span class="sxs-lookup"><span data-stu-id="00065-249">-Force</span></span>                             | <span data-ttu-id="00065-250">Přepište existující soubory.</span><span class="sxs-lookup"><span data-stu-id="00065-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="00065-251">Příklad:</span><span class="sxs-lookup"><span data-stu-id="00065-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="00065-252">Příklad, který se vygeneruje pouze uživatelské rozhraní vybraných tabulek a vytvoří kontext do samostatné složky se zadaným názvem:</span><span class="sxs-lookup"><span data-stu-id="00065-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="00065-253">Skript migrace</span><span class="sxs-lookup"><span data-stu-id="00065-253">Script-Migration</span></span>

<span data-ttu-id="00065-254">Vygeneruje skript SQL, která se použije všechny změny z jednoho vybraného migrace do jiného vybrané migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="00065-255">Parametry:</span><span class="sxs-lookup"><span data-stu-id="00065-255">Parameters:</span></span>

| <span data-ttu-id="00065-256">Parametr</span><span class="sxs-lookup"><span data-stu-id="00065-256">Parameter</span></span>                | <span data-ttu-id="00065-257">Popis</span><span class="sxs-lookup"><span data-stu-id="00065-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="00065-258">*-Od* \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-258">*-From* \<String></span></span>        | <span data-ttu-id="00065-259">Počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="00065-259">The starting migration.</span></span> <span data-ttu-id="00065-260">Migrace může identifikovat podle názvu nebo podle ID.</span><span class="sxs-lookup"><span data-stu-id="00065-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="00065-261">Zvláštní případ, který znamená, že je číslo 0 *před první migraci*.</span><span class="sxs-lookup"><span data-stu-id="00065-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="00065-262">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="00065-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="00065-263">*-Na* \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-263">*-To* \<String></span></span>          | <span data-ttu-id="00065-264">Dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-264">The ending migration.</span></span> <span data-ttu-id="00065-265">Výchozí hodnota je poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="00065-266"><nobr>-Idempotentní</nobr></span><span class="sxs-lookup"><span data-stu-id="00065-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="00065-267">Generovat skript, který jde použít na databáze v libovolné migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="00065-268">-Výstupní \<řetězec ></span><span class="sxs-lookup"><span data-stu-id="00065-268">-Output \<String></span></span>        | <span data-ttu-id="00065-269">Soubor pro zápis výsledek.</span><span class="sxs-lookup"><span data-stu-id="00065-269">The file to write the result to.</span></span> <span data-ttu-id="00065-270">Pokud je tento parametr vynechán, soubor je vytvořen s vygenerovaným názvem ve stejné složce jako soubory modulu runtime aplikace se vytvářejí, například: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="00065-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="00065-271">Na, z, a podpora rozšíření tab výstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="00065-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="00065-272">Následující příklad vytvoří skript pro migraci InitialCreate, pomocí názvu migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="00065-273">Následující příklad vytvoří skript pro všechny migrace po dokončení migrace InitialCreate pomocí ID migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="00065-274">Aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="00065-274">Update-Database</span></span>

<span data-ttu-id="00065-275">Aktualizace databáze na poslední migraci nebo zadaný migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="00065-276">Parametr</span><span class="sxs-lookup"><span data-stu-id="00065-276">Parameter</span></span>                           | <span data-ttu-id="00065-277">Popis</span><span class="sxs-lookup"><span data-stu-id="00065-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="00065-278"><nobr>*– Migrace* \<řetězec ></nobr></span><span class="sxs-lookup"><span data-stu-id="00065-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="00065-279">Cíl migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-279">The target migration.</span></span> <span data-ttu-id="00065-280">Migrace může identifikovat podle názvu nebo podle ID.</span><span class="sxs-lookup"><span data-stu-id="00065-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="00065-281">Zvláštní případ, který znamená, že je číslo 0 *před první migraci* a způsobí, že všechny migrace se vrátí zpátky.</span><span class="sxs-lookup"><span data-stu-id="00065-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="00065-282">Pokud není zadána žádná migrace, výchozí příkaz na poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="00065-283">Parametr migrace podporuje rozšíření tab.</span><span class="sxs-lookup"><span data-stu-id="00065-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="00065-284">Následující příklad vrátí všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="00065-285">Následující příklady aktualizujte databázi na zadané migrace.</span><span class="sxs-lookup"><span data-stu-id="00065-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="00065-286">Název migrace používá první a druhé používá ID migrace:</span><span class="sxs-lookup"><span data-stu-id="00065-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```
