---
title: Referenční dokumentace k nástrojům pro EF Core (konzola správce balíčků) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 45370a82131da9db8b724fe395d41b1e3641fcf8
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181336"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="8aa99-102">Referenční informace o nástrojích Entity Framework Core Tools – konzola správce balíčků v aplikaci Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8aa99-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="8aa99-103">Nástroje konzoly Správce balíčků (PMC) pro Entity Framework Core prováděly úlohy vývoje v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="8aa99-104">Například vytvářejí [migrace](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), aplikují migrace a generují kód pro model založený na stávající databázi.</span><span class="sxs-lookup"><span data-stu-id="8aa99-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="8aa99-105">Příkazy se spouští uvnitř sady Visual Studio pomocí [konzoly Správce balíčků](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="8aa99-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="8aa99-106">Tyto nástroje fungují v projektech .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8aa99-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="8aa99-107">Pokud nepoužíváte aplikaci Visual Studio, doporučujeme místo toho použít [EF Core nástroje příkazového řádku](dotnet.md) .</span><span class="sxs-lookup"><span data-stu-id="8aa99-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="8aa99-108">Nástroje rozhraní příkazového řádku jsou různé platformy a spouštějí se v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="8aa99-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="8aa99-109">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="8aa99-109">Installing the tools</span></span>

<span data-ttu-id="8aa99-110">Postupy pro instalaci a aktualizaci nástrojů se liší od ASP.NET Core 2.1 + a starších verzí nebo jiných typů projektů.</span><span class="sxs-lookup"><span data-stu-id="8aa99-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="8aa99-111">ASP.NET Core verze 2,1 a novější</span><span class="sxs-lookup"><span data-stu-id="8aa99-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="8aa99-112">Nástroje jsou automaticky zahrnuty v projektu ASP.NET Core 2.1 +, protože balíček `Microsoft.EntityFrameworkCore.Tools` je součástí [Microsoft. AspNetCore. app Metapackage](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8aa99-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="8aa99-113">Proto nemusíte nic dělat k instalaci nástrojů, ale budete muset:</span><span class="sxs-lookup"><span data-stu-id="8aa99-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="8aa99-114">Obnovte balíčky před použitím nástrojů v novém projektu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="8aa99-115">Nainstalujte balíček pro aktualizaci nástrojů na novější verzi.</span><span class="sxs-lookup"><span data-stu-id="8aa99-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="8aa99-116">Abyste se ujistili, že budete získávat nejnovější verze nástrojů, doporučujeme, abyste provedli také následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8aa99-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="8aa99-117">Upravte soubor *. csproj* a přidejte řádek, který určuje nejnovější verzi balíčku [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="8aa99-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="8aa99-118">Například soubor *. csproj* může zahrnovat `ItemGroup`, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8aa99-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="8aa99-119">Aktualizujte nástroje, když se zobrazí zpráva podobná následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="8aa99-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="8aa99-120">Verze EF Core Tools "2.1.1-RTM-30846" je starší než modul runtime "2.1.3-RTM-32065".</span><span class="sxs-lookup"><span data-stu-id="8aa99-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="8aa99-121">Aktualizujte nástroje na nejnovější funkce a opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="8aa99-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="8aa99-122">Aktualizace nástrojů:</span><span class="sxs-lookup"><span data-stu-id="8aa99-122">To update the tools:</span></span>
* <span data-ttu-id="8aa99-123">Nainstalujte nejnovější .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="8aa99-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="8aa99-124">Aktualizujte si Visual Studio na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="8aa99-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="8aa99-125">Upravte soubor *. csproj* tak, aby zahrnoval odkaz na balíček na nejnovější balíček nástrojů, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="8aa99-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="8aa99-126">Další verze a typy projektů</span><span class="sxs-lookup"><span data-stu-id="8aa99-126">Other versions and project types</span></span>

<span data-ttu-id="8aa99-127">Nástroje konzoly Správce balíčků nainstalujete spuštěním následujícího příkazu v **konzole správce balíčků**:</span><span class="sxs-lookup"><span data-stu-id="8aa99-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="8aa99-128">Aktualizujte nástroje spuštěním následujícího příkazu v **konzole správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="8aa99-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="8aa99-129">Ověření instalace</span><span class="sxs-lookup"><span data-stu-id="8aa99-129">Verify the installation</span></span>

<span data-ttu-id="8aa99-130">Spuštěním tohoto příkazu ověřte, zda jsou nástroje nainstalovány:</span><span class="sxs-lookup"><span data-stu-id="8aa99-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="8aa99-131">Výstup vypadá takto (neříká vám to, jakou verzi nástrojů používáte):</span><span class="sxs-lookup"><span data-stu-id="8aa99-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="8aa99-132">Používání nástrojů</span><span class="sxs-lookup"><span data-stu-id="8aa99-132">Using the tools</span></span>

<span data-ttu-id="8aa99-133">Před použitím těchto nástrojů:</span><span class="sxs-lookup"><span data-stu-id="8aa99-133">Before using the tools:</span></span>
* <span data-ttu-id="8aa99-134">Pochopte rozdíl mezi cílovým a spouštěným projektem.</span><span class="sxs-lookup"><span data-stu-id="8aa99-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="8aa99-135">Naučte se používat nástroje s .NET Standard knihoven tříd.</span><span class="sxs-lookup"><span data-stu-id="8aa99-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="8aa99-136">Pro ASP.NET Core projekty nastavte prostředí.</span><span class="sxs-lookup"><span data-stu-id="8aa99-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="8aa99-137">Cílový a spouštěný projekt</span><span class="sxs-lookup"><span data-stu-id="8aa99-137">Target and startup project</span></span>

<span data-ttu-id="8aa99-138">Příkazy odkazují na *projekt* a na *spouštěný projekt*.</span><span class="sxs-lookup"><span data-stu-id="8aa99-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="8aa99-139">*Projekt* je také označován jako *cílový projekt* , protože je tam, kde příkazy přidávají nebo odebírají soubory.</span><span class="sxs-lookup"><span data-stu-id="8aa99-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="8aa99-140">Ve výchozím nastavení je **výchozí projekt** vybraný v **konzole správce balíčků** cílový projekt.</span><span class="sxs-lookup"><span data-stu-id="8aa99-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="8aa99-141">Můžete určit jiný projekt jako cílový projekt pomocí možnosti <nobr>`--project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="8aa99-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="8aa99-142">Spouštěný *projekt* je ten, který nástroje sestavují a spouštějí.</span><span class="sxs-lookup"><span data-stu-id="8aa99-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="8aa99-143">Nástroje musí spustit kód aplikace v době návrhu, aby získali informace o projektu, jako je například připojovací řetězec databáze a konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="8aa99-144">Ve výchozím nastavení je **projekt po spuštění** v **Průzkumník řešení** spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="8aa99-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="8aa99-145">Můžete určit jiný projekt jako spouštěný projekt pomocí možnosti <nobr>`--startup-project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="8aa99-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="8aa99-146">Spouštěcí projekt a cílový projekt jsou často stejný projekt.</span><span class="sxs-lookup"><span data-stu-id="8aa99-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="8aa99-147">Typický scénář, kde jsou samostatné projekty, je:</span><span class="sxs-lookup"><span data-stu-id="8aa99-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="8aa99-148">EF Core kontext a třídy entit jsou v knihovně tříd .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8aa99-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="8aa99-149">Aplikace konzoly .NET Core nebo webová aplikace odkazuje na knihovnu tříd.</span><span class="sxs-lookup"><span data-stu-id="8aa99-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="8aa99-150">Je také možné [umístit kód migrace do knihovny tříd odděleně od EF Coreho kontextu](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="8aa99-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="8aa99-151">Další cílová rozhraní</span><span class="sxs-lookup"><span data-stu-id="8aa99-151">Other target frameworks</span></span>

<span data-ttu-id="8aa99-152">Nástroje konzoly Správce balíčků fungují v projektech .NET Core nebo .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aa99-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="8aa99-153">Aplikace s modelem EF Core v knihovně tříd .NET Standard nemusí mít projekt .NET Core nebo .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aa99-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="8aa99-154">Například to platí pro Xamarin a Univerzální platforma Windows aplikace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="8aa99-155">V takových případech můžete vytvořit projekt konzolové aplikace .NET Core nebo .NET Framework, jehož jediným účelem je jednat jako projekt po spuštění pro nástroje.</span><span class="sxs-lookup"><span data-stu-id="8aa99-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="8aa99-156">Projekt může být fiktivní projekt bez skutečného kódu &mdash; je potřeba jenom poskytnout cíl pro nástroje.</span><span class="sxs-lookup"><span data-stu-id="8aa99-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="8aa99-157">Proč je vyžadován fiktivní projekt?</span><span class="sxs-lookup"><span data-stu-id="8aa99-157">Why is a dummy project required?</span></span> <span data-ttu-id="8aa99-158">Jak bylo zmíněno dříve, nástroje musí spustit kód aplikace v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="8aa99-159">K tomu je potřeba použít modul runtime .NET Core nebo .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aa99-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="8aa99-160">Když je model EF Core v projektu cíleném na rozhraní .NET Core nebo .NET Framework, EF Core nástroje vypůjčí modul runtime z projektu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="8aa99-161">Nemůžou to dělat, pokud je model EF Core v knihovně tříd .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8aa99-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="8aa99-162">.NET Standard není skutečná implementace rozhraní .NET; je to specifikace sady rozhraní API, které musí implementace rozhraní .NET podporovat.</span><span class="sxs-lookup"><span data-stu-id="8aa99-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="8aa99-163">Proto .NET Standard není dostačující pro EF Core nástroje pro spouštění kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="8aa99-164">Fiktivní projekt, který vytvoříte pro použití jako spouštěný projekt, poskytuje konkrétní cílovou platformu, do které mohou nástroje načíst .NET Standard knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="8aa99-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="8aa99-165">ASP.NET Core prostředí</span><span class="sxs-lookup"><span data-stu-id="8aa99-165">ASP.NET Core environment</span></span>

<span data-ttu-id="8aa99-166">Chcete-li určit prostředí pro ASP.NET Core projekty, nastavte **obálku ENV: ASPNETCORE_ENVIRONMENT** před spuštěním příkazů.</span><span class="sxs-lookup"><span data-stu-id="8aa99-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="8aa99-167">Společné parametry</span><span class="sxs-lookup"><span data-stu-id="8aa99-167">Common parameters</span></span>

<span data-ttu-id="8aa99-168">V následující tabulce jsou uvedeny parametry, které jsou společné pro všechny EF Core příkazy:</span><span class="sxs-lookup"><span data-stu-id="8aa99-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="8aa99-169">Parametr</span><span class="sxs-lookup"><span data-stu-id="8aa99-169">Parameter</span></span>                 | <span data-ttu-id="8aa99-170">Popis</span><span class="sxs-lookup"><span data-stu-id="8aa99-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8aa99-171">-Context @no__t – > 0String</span><span class="sxs-lookup"><span data-stu-id="8aa99-171">-Context \<String></span></span>        | <span data-ttu-id="8aa99-172">Třída `DbContext`, která se má použít.</span><span class="sxs-lookup"><span data-stu-id="8aa99-172">The `DbContext` class to use.</span></span> <span data-ttu-id="8aa99-173">Pouze název třídy nebo plně kvalifikovaný obory názvů.</span><span class="sxs-lookup"><span data-stu-id="8aa99-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="8aa99-174">Pokud je tento parametr vynechán, EF Core najde kontextovou třídu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="8aa99-175">Pokud existuje více kontextových tříd, je tento parametr povinný.</span><span class="sxs-lookup"><span data-stu-id="8aa99-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="8aa99-176">-Project @no__t – 0String ></span><span class="sxs-lookup"><span data-stu-id="8aa99-176">-Project \<String></span></span>        | <span data-ttu-id="8aa99-177">Cílový projekt.</span><span class="sxs-lookup"><span data-stu-id="8aa99-177">The target project.</span></span> <span data-ttu-id="8aa99-178">Pokud je tento parametr vynechán, použije se jako cílový projekt **výchozí projekt** pro **konzolu Správce balíčků** .</span><span class="sxs-lookup"><span data-stu-id="8aa99-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="8aa99-179">-StartupProject \<String ></span><span class="sxs-lookup"><span data-stu-id="8aa99-179">-StartupProject \<String></span></span> | <span data-ttu-id="8aa99-180">Spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="8aa99-180">The startup project.</span></span> <span data-ttu-id="8aa99-181">Pokud je tento parametr vynechán, použije se jako cílový projekt **spouštěcí projekt** ve **vlastnostech řešení** .</span><span class="sxs-lookup"><span data-stu-id="8aa99-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="8aa99-182">– Verbose</span><span class="sxs-lookup"><span data-stu-id="8aa99-182">-Verbose</span></span>                  | <span data-ttu-id="8aa99-183">Zobrazit podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="8aa99-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="8aa99-184">Pokud chcete zobrazit informace o nápovědě k příkazu, použijte příkaz `Get-Help` prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8aa99-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="8aa99-185">Kontext, projekt a parametry StartupProject podporují rozšíření karty.</span><span class="sxs-lookup"><span data-stu-id="8aa99-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="8aa99-186">Přidání – migrace</span><span class="sxs-lookup"><span data-stu-id="8aa99-186">Add-Migration</span></span>

<span data-ttu-id="8aa99-187">Přidá novou migraci.</span><span class="sxs-lookup"><span data-stu-id="8aa99-187">Adds a new migration.</span></span>

<span data-ttu-id="8aa99-188">Parametry:</span><span class="sxs-lookup"><span data-stu-id="8aa99-188">Parameters:</span></span>

| <span data-ttu-id="8aa99-189">Parametr</span><span class="sxs-lookup"><span data-stu-id="8aa99-189">Parameter</span></span>                         | <span data-ttu-id="8aa99-190">Popis</span><span class="sxs-lookup"><span data-stu-id="8aa99-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8aa99-191">@no__t -0-Name \<String > <nobr></span><span class="sxs-lookup"><span data-stu-id="8aa99-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="8aa99-192">Název migrace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-192">The name of the migration.</span></span> <span data-ttu-id="8aa99-193">Toto je poziční parametr a je povinný.</span><span class="sxs-lookup"><span data-stu-id="8aa99-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="8aa99-194"><nobr>-OutputDir \<String ></nobr></span><span class="sxs-lookup"><span data-stu-id="8aa99-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="8aa99-195">Adresář (a dílčí obor názvů), který se má použít.</span><span class="sxs-lookup"><span data-stu-id="8aa99-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="8aa99-196">Cesty jsou relativní vzhledem k cílovému adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="8aa99-197">Ve výchozím nastavení se jedná o "migrace".</span><span class="sxs-lookup"><span data-stu-id="8aa99-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="8aa99-198">Vyřadit z databáze</span><span class="sxs-lookup"><span data-stu-id="8aa99-198">Drop-Database</span></span>

<span data-ttu-id="8aa99-199">Zruší databázi.</span><span class="sxs-lookup"><span data-stu-id="8aa99-199">Drops the database.</span></span>

<span data-ttu-id="8aa99-200">Parametry:</span><span class="sxs-lookup"><span data-stu-id="8aa99-200">Parameters:</span></span>

| <span data-ttu-id="8aa99-201">Parametr</span><span class="sxs-lookup"><span data-stu-id="8aa99-201">Parameter</span></span> | <span data-ttu-id="8aa99-202">Popis</span><span class="sxs-lookup"><span data-stu-id="8aa99-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="8aa99-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="8aa99-203">-WhatIf</span></span>   | <span data-ttu-id="8aa99-204">Zobrazit, která databáze bude vyřazena, ale neodstraňovat ji.</span><span class="sxs-lookup"><span data-stu-id="8aa99-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="8aa99-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="8aa99-205">Get-DbContext</span></span>

<span data-ttu-id="8aa99-206">Získá informace o typu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8aa99-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="8aa99-207">Odebrání – migrace</span><span class="sxs-lookup"><span data-stu-id="8aa99-207">Remove-Migration</span></span>

<span data-ttu-id="8aa99-208">Odebere poslední migraci (vrátí zpět změny kódu, které byly provedeny pro migraci).</span><span class="sxs-lookup"><span data-stu-id="8aa99-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="8aa99-209">Parametry:</span><span class="sxs-lookup"><span data-stu-id="8aa99-209">Parameters:</span></span>

| <span data-ttu-id="8aa99-210">Parametr</span><span class="sxs-lookup"><span data-stu-id="8aa99-210">Parameter</span></span> | <span data-ttu-id="8aa99-211">Popis</span><span class="sxs-lookup"><span data-stu-id="8aa99-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="8aa99-212">-Force</span><span class="sxs-lookup"><span data-stu-id="8aa99-212">-Force</span></span>    | <span data-ttu-id="8aa99-213">Obnovte migraci (vraťte zpět změny, které byly pro databázi aplikovány).</span><span class="sxs-lookup"><span data-stu-id="8aa99-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="8aa99-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="8aa99-214">Scaffold-DbContext</span></span>

<span data-ttu-id="8aa99-215">Generuje kód pro `DbContext` a typy entit pro databázi.</span><span class="sxs-lookup"><span data-stu-id="8aa99-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="8aa99-216">Aby `Scaffold-DbContext` generovat typ entity, musí mít databázová tabulka primární klíč.</span><span class="sxs-lookup"><span data-stu-id="8aa99-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="8aa99-217">Parametry:</span><span class="sxs-lookup"><span data-stu-id="8aa99-217">Parameters:</span></span>

| <span data-ttu-id="8aa99-218">Parametr</span><span class="sxs-lookup"><span data-stu-id="8aa99-218">Parameter</span></span>                          | <span data-ttu-id="8aa99-219">Popis</span><span class="sxs-lookup"><span data-stu-id="8aa99-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8aa99-220"><nobr>-Connection @no__t – > 1String</nobr></span><span class="sxs-lookup"><span data-stu-id="8aa99-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="8aa99-221">Připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="8aa99-221">The connection string to the database.</span></span> <span data-ttu-id="8aa99-222">Pro projekty ASP.NET Core 2. x může být hodnota *Name = \<Name připojovacího řetězce >* .</span><span class="sxs-lookup"><span data-stu-id="8aa99-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="8aa99-223">V takovém případě název pochází ze zdrojů konfigurace, které jsou nastaveny pro projekt.</span><span class="sxs-lookup"><span data-stu-id="8aa99-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="8aa99-224">Toto je poziční parametr a je povinný.</span><span class="sxs-lookup"><span data-stu-id="8aa99-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="8aa99-225"><nobr>-Provider @no__t – > 1String</nobr></span><span class="sxs-lookup"><span data-stu-id="8aa99-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="8aa99-226">Poskytovatel, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="8aa99-226">The provider to use.</span></span> <span data-ttu-id="8aa99-227">Obvykle se jedná o název balíčku NuGet, například: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="8aa99-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="8aa99-228">Toto je poziční parametr a je povinný.</span><span class="sxs-lookup"><span data-stu-id="8aa99-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="8aa99-229">-OutputDir \<String ></span><span class="sxs-lookup"><span data-stu-id="8aa99-229">-OutputDir \<String></span></span>               | <span data-ttu-id="8aa99-230">Adresář, do kterého se mají umístit soubory</span><span class="sxs-lookup"><span data-stu-id="8aa99-230">The directory to put files in.</span></span> <span data-ttu-id="8aa99-231">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="8aa99-232">-ContextDir \<String ></span><span class="sxs-lookup"><span data-stu-id="8aa99-232">-ContextDir \<String></span></span>              | <span data-ttu-id="8aa99-233">Adresář, do kterého se má umístit soubor `DbContext`</span><span class="sxs-lookup"><span data-stu-id="8aa99-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="8aa99-234">Cesty jsou relativní vzhledem k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="8aa99-235">-Context @no__t – > 0String</span><span class="sxs-lookup"><span data-stu-id="8aa99-235">-Context \<String></span></span>                 | <span data-ttu-id="8aa99-236">Název třídy `DbContext`, která má být vygenerována.</span><span class="sxs-lookup"><span data-stu-id="8aa99-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="8aa99-237">-Schemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="8aa99-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="8aa99-238">Schémata tabulek, pro které se mají generovat typy entit</span><span class="sxs-lookup"><span data-stu-id="8aa99-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="8aa99-239">Pokud je tento parametr vynechán, jsou zahrnuty všechna schémata.</span><span class="sxs-lookup"><span data-stu-id="8aa99-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="8aa99-240">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="8aa99-240">-Tables \<String[]></span></span>                | <span data-ttu-id="8aa99-241">Tabulky, pro které se mají generovat typy entit</span><span class="sxs-lookup"><span data-stu-id="8aa99-241">The tables to generate entity types for.</span></span> <span data-ttu-id="8aa99-242">Pokud je tento parametr vynechán, jsou zahrnuty všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="8aa99-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="8aa99-243">– Dataanotace</span><span class="sxs-lookup"><span data-stu-id="8aa99-243">-DataAnnotations</span></span>                   | <span data-ttu-id="8aa99-244">Použijte atributy ke konfiguraci modelu (Pokud je to možné).</span><span class="sxs-lookup"><span data-stu-id="8aa99-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="8aa99-245">Pokud tento parametr vynecháte, použije se jenom rozhraní API Fluent.</span><span class="sxs-lookup"><span data-stu-id="8aa99-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="8aa99-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="8aa99-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="8aa99-247">Názvy tabulek a sloupců používejte přesně tak, jak se zobrazí v databázi.</span><span class="sxs-lookup"><span data-stu-id="8aa99-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="8aa99-248">Pokud je tento parametr vynechán, jsou názvy databází změněny, aby lépe odpovídaly konvencím stylu C# názvu.</span><span class="sxs-lookup"><span data-stu-id="8aa99-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="8aa99-249">-Force</span><span class="sxs-lookup"><span data-stu-id="8aa99-249">-Force</span></span>                             | <span data-ttu-id="8aa99-250">Přepsat existující soubory.</span><span class="sxs-lookup"><span data-stu-id="8aa99-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="8aa99-251">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8aa99-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="8aa99-252">Příkladem je, že generování pouze tabulek vybralo pouze tabulky a vytvoří kontext v samostatné složce se zadaným názvem:</span><span class="sxs-lookup"><span data-stu-id="8aa99-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="8aa99-253">Migrace skriptu</span><span class="sxs-lookup"><span data-stu-id="8aa99-253">Script-Migration</span></span>

<span data-ttu-id="8aa99-254">Vygeneruje skript SQL, který aplikuje všechny změny z jedné vybrané migrace na jinou vybranou migraci.</span><span class="sxs-lookup"><span data-stu-id="8aa99-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="8aa99-255">Parametry:</span><span class="sxs-lookup"><span data-stu-id="8aa99-255">Parameters:</span></span>

| <span data-ttu-id="8aa99-256">Parametr</span><span class="sxs-lookup"><span data-stu-id="8aa99-256">Parameter</span></span>                | <span data-ttu-id="8aa99-257">Popis</span><span class="sxs-lookup"><span data-stu-id="8aa99-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8aa99-258">*– Od* @no__t – 1String ></span><span class="sxs-lookup"><span data-stu-id="8aa99-258">*-From* \<String></span></span>        | <span data-ttu-id="8aa99-259">Spouští se migrace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-259">The starting migration.</span></span> <span data-ttu-id="8aa99-260">Migrace může být identifikována podle názvu nebo podle ID.</span><span class="sxs-lookup"><span data-stu-id="8aa99-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="8aa99-261">Číslo 0 je zvláštní případ, který znamená *před první migrací*.</span><span class="sxs-lookup"><span data-stu-id="8aa99-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="8aa99-262">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="8aa99-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="8aa99-263">*-To* \<String ></span><span class="sxs-lookup"><span data-stu-id="8aa99-263">*-To* \<String></span></span>          | <span data-ttu-id="8aa99-264">Koncová migrace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-264">The ending migration.</span></span> <span data-ttu-id="8aa99-265">Výchozí hodnota je poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="8aa99-266"><nobr>– Idempotentní</nobr></span><span class="sxs-lookup"><span data-stu-id="8aa99-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="8aa99-267">Vygenerujte skript, který se dá použít v databázi při libovolné migraci.</span><span class="sxs-lookup"><span data-stu-id="8aa99-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="8aa99-268">-Output @no__t – 0String ></span><span class="sxs-lookup"><span data-stu-id="8aa99-268">-Output \<String></span></span>        | <span data-ttu-id="8aa99-269">Soubor, do kterého se má zapisovat výsledek</span><span class="sxs-lookup"><span data-stu-id="8aa99-269">The file to write the result to.</span></span> <span data-ttu-id="8aa99-270">Pokud je tento parametr vynechán, vytvoří se soubor s vygenerovaným názvem ve stejné složce, ve které jsou vytvořeny běhové soubory aplikace, například: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* .</span><span class="sxs-lookup"><span data-stu-id="8aa99-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="8aa99-271">Parametry do, z a výstup podporují rozšíření na kartě.</span><span class="sxs-lookup"><span data-stu-id="8aa99-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="8aa99-272">Následující příklad vytvoří skript pro migraci InitialCreate pomocí názvu migrace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="8aa99-273">Následující příklad vytvoří skript pro všechny migrace po migraci InitialCreate pomocí ID migrace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="8aa99-274">Aktualizace – databáze</span><span class="sxs-lookup"><span data-stu-id="8aa99-274">Update-Database</span></span>

<span data-ttu-id="8aa99-275">Aktualizuje databázi na poslední migraci nebo na určenou migraci.</span><span class="sxs-lookup"><span data-stu-id="8aa99-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="8aa99-276">Parametr</span><span class="sxs-lookup"><span data-stu-id="8aa99-276">Parameter</span></span>                           | <span data-ttu-id="8aa99-277">Popis</span><span class="sxs-lookup"><span data-stu-id="8aa99-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8aa99-278"><nobr> *-@No__t migrace* – 2String ></nobr></span><span class="sxs-lookup"><span data-stu-id="8aa99-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="8aa99-279">Cílová migrace</span><span class="sxs-lookup"><span data-stu-id="8aa99-279">The target migration.</span></span> <span data-ttu-id="8aa99-280">Migrace může být identifikována podle názvu nebo podle ID.</span><span class="sxs-lookup"><span data-stu-id="8aa99-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="8aa99-281">Číslo 0 je zvláštní případ, který znamená *před první migrací* a způsobí, že se všechny migrace vrátí zpět.</span><span class="sxs-lookup"><span data-stu-id="8aa99-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="8aa99-282">Pokud není zadaná žádná migrace, příkaz se nastaví jako výchozí pro poslední migraci.</span><span class="sxs-lookup"><span data-stu-id="8aa99-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="8aa99-283">Parametr Migration podporuje rozšíření karty.</span><span class="sxs-lookup"><span data-stu-id="8aa99-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="8aa99-284">Následující příklad vrátí všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="8aa99-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="8aa99-285">V následujících příkladech je databáze aktualizována na určenou migraci.</span><span class="sxs-lookup"><span data-stu-id="8aa99-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="8aa99-286">První používá název migrace a druhý používá ID migrace:</span><span class="sxs-lookup"><span data-stu-id="8aa99-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="8aa99-287">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8aa99-287">Additional resources</span></span>

* [<span data-ttu-id="8aa99-288">Migrace</span><span class="sxs-lookup"><span data-stu-id="8aa99-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="8aa99-289">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="8aa99-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
