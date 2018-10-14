---
title: Začínáme na UPW – nová databáze – EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315617"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="48f15-102">Začínáme s EF Core na Universal Windows Platform (UWP) s novou databázi</span><span class="sxs-lookup"><span data-stu-id="48f15-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="48f15-103">V tomto kurzu vytvoříte aplikaci univerzální platformy Windows (UPW), který provádí základní přístup k datům s použitím Entity Framework Core místní databázi SQLite.</span><span class="sxs-lookup"><span data-stu-id="48f15-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="48f15-104">[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="48f15-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48f15-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="48f15-105">Prerequisites</span></span>

* <span data-ttu-id="48f15-106">[Windows 10 Fall Creators Update (10.0; Sestavení 16299) nebo novější](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="48f15-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="48f15-107">[Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/) s **vývoj pro univerzální platformu Windows** pracovního vytížení.</span><span class="sxs-lookup"><span data-stu-id="48f15-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="48f15-108">[Sady SDK .NET core 2.1 nebo novější](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="48f15-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48f15-109">Tento kurz používá Entity Framework Core [migrace](xref:core/managing-schemas/migrations/index) příkazy k vytvoření a aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="48f15-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="48f15-110">Tyto příkazy nechcete pracovat přímo s projekty UWP.</span><span class="sxs-lookup"><span data-stu-id="48f15-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="48f15-111">Z tohoto důvodu aplikace datový model je umístěn ve sdílené knihovně projektu a samostatné konzolovou aplikaci .NET Core se používá ke spuštění příkazů.</span><span class="sxs-lookup"><span data-stu-id="48f15-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="48f15-112">Vytvořte projekt knihovny pro uchování datového modelu</span><span class="sxs-lookup"><span data-stu-id="48f15-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="48f15-113">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48f15-113">Open Visual Studio</span></span>

* <span data-ttu-id="48f15-114">**Soubor > Nový > Projekt**</span><span class="sxs-lookup"><span data-stu-id="48f15-114">**File > New > Project**</span></span>

* <span data-ttu-id="48f15-115">V levé nabídce vyberte **nainstalováno > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="48f15-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="48f15-116">Vyberte **knihovna tříd (.NET Standard)** šablony.</span><span class="sxs-lookup"><span data-stu-id="48f15-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="48f15-117">Pojmenujte projekt *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="48f15-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="48f15-118">Pojmenujte řešení *blogovací*.</span><span class="sxs-lookup"><span data-stu-id="48f15-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="48f15-119">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="48f15-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="48f15-120">Nainstalovat modul runtime Entity Framework Core v datovém modelu projektu</span><span class="sxs-lookup"><span data-stu-id="48f15-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="48f15-121">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="48f15-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="48f15-122">Tento kurz používá SQLite.</span><span class="sxs-lookup"><span data-stu-id="48f15-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="48f15-123">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="48f15-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="48f15-124">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="48f15-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="48f15-125">Ujistěte se, že projekt knihovny *Blogging.Model* je zvolen jako **výchozí projekt** v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="48f15-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="48f15-126">Spustit `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="48f15-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="48f15-127">Vytvoření datového modelu</span><span class="sxs-lookup"><span data-stu-id="48f15-127">Create the data model</span></span>

<span data-ttu-id="48f15-128">Nyní je možné definovat *DbContext* a tříd entit, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="48f15-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="48f15-129">Odstranit *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="48f15-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="48f15-130">Vytvoření *Model.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="48f15-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="48f15-131">Vytvořte nový projekt konzoly pro spuštění příkazů migrace</span><span class="sxs-lookup"><span data-stu-id="48f15-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="48f15-132">V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **Přidat > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="48f15-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="48f15-133">V levé nabídce vyberte **nainstalováno > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="48f15-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="48f15-134">Vyberte **Konzolová aplikace (.NET Core)** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="48f15-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="48f15-135">Pojmenujte projekt *Blogging.Migrations.Startup*a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="48f15-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="48f15-136">Přidat odkaz na projekt z *Blogging.Migrations.Startup* projektu *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="48f15-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="48f15-137">Instalace nástroje Entity Framework Core v projektu po spuštění migrace</span><span class="sxs-lookup"><span data-stu-id="48f15-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="48f15-138">Povolit příkazy EF Core migrace v konzole Správce balíčků, nainstalujte balíček nástroje EF Core v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48f15-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="48f15-139">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="48f15-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="48f15-140">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="48f15-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="48f15-141">Vytvoření počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="48f15-141">Create the initial migration</span></span>

 <span data-ttu-id="48f15-142">Vytvořte počáteční migraci zadáte jako spouštěný projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="48f15-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="48f15-143">Spustit `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="48f15-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="48f15-144">Tento příkaz vygeneruje uživatelské rozhraní migrace, které vytvoří počáteční sada databázových tabulek datového modelu.</span><span class="sxs-lookup"><span data-stu-id="48f15-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="48f15-145">Vytvoření projektu UPW</span><span class="sxs-lookup"><span data-stu-id="48f15-145">Create the UWP project</span></span>

* <span data-ttu-id="48f15-146">V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **Přidat > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="48f15-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="48f15-147">V levé nabídce vyberte **nainstalováno > Visual C# > Windows Universal**.</span><span class="sxs-lookup"><span data-stu-id="48f15-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="48f15-148">Vyberte **prázdná aplikace (Universal Windows)** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="48f15-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="48f15-149">Pojmenujte projekt *Blogging.UWP*a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="48f15-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48f15-150">Nastavte cílovou a minimální verze na alespoň **Windows 10 Fall Creators Update (10.0; sestavení 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="48f15-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="48f15-151">Předchozí verze Windows 10 nepodporuje .NET Standard 2.0, které je požadované Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="48f15-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="48f15-152">Přidejte kód k vytvoření databáze při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="48f15-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="48f15-153">Protože chcete, aby databáze, kterou chcete vytvořit pro zařízení, na které aplikace poběží, přidejte kód pro použití žádné čekající migrace do místní databáze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="48f15-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="48f15-154">Při prvním spuštění aplikace, postará se vytvoří místní databázi.</span><span class="sxs-lookup"><span data-stu-id="48f15-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="48f15-155">Přidat odkaz na projekt z *Blogging.UWP* projektu *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="48f15-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="48f15-156">Otevřít *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="48f15-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="48f15-157">Přidejte zvýrazněný kód chcete použít všechny probíhající migrace.</span><span class="sxs-lookup"><span data-stu-id="48f15-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="48f15-158">Pokud změníte model, použijte `Add-Migration` příkaz scaffold novou migraci použít odpovídající změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="48f15-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="48f15-159">Všechny probíhající migrace se použijí k místní databázi na každé zařízení při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="48f15-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="48f15-160">Používá EF Core `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.</span><span class="sxs-lookup"><span data-stu-id="48f15-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="48f15-161">Datový model</span><span class="sxs-lookup"><span data-stu-id="48f15-161">Use the data model</span></span>

<span data-ttu-id="48f15-162">EF Core můžete teď použít přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="48f15-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="48f15-163">Otevřít *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="48f15-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="48f15-164">Přidat obslužnou rutinu načtení stránky a uživatelské rozhraní obsah jejichž přehled najdete níže</span><span class="sxs-lookup"><span data-stu-id="48f15-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="48f15-165">Teď přidejte kód, který propojí uživatelského rozhraní pomocí databáze</span><span class="sxs-lookup"><span data-stu-id="48f15-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="48f15-166">Otevřít *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="48f15-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="48f15-167">V následujícím seznamu přidejte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="48f15-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="48f15-168">Nyní můžete spustit aplikaci sledujte v akci.</span><span class="sxs-lookup"><span data-stu-id="48f15-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="48f15-169">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Blogging.UWP* projektu a pak vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="48f15-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="48f15-170">Nastavte *Blogging.UWP* jako spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="48f15-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="48f15-171">**Ladit > Spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="48f15-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="48f15-172">Aplikace vytvoří a spustí.</span><span class="sxs-lookup"><span data-stu-id="48f15-172">The app builds and runs.</span></span>

* <span data-ttu-id="48f15-173">Zadejte adresu URL a klikněte na tlačítko **přidat** tlačítko</span><span class="sxs-lookup"><span data-stu-id="48f15-173">Enter a URL and click the **Add** button</span></span>

  ![obrázek](_static/create.png)

  ![obrázek](_static/list.png)

  <span data-ttu-id="48f15-176">Tada!</span><span class="sxs-lookup"><span data-stu-id="48f15-176">Tada!</span></span> <span data-ttu-id="48f15-177">Teď máte jednoduché aplikace pro UPW s Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="48f15-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48f15-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48f15-178">Next steps</span></span>

<span data-ttu-id="48f15-179">Kompatibilitu a výkon informace, které byste měli znát při používání EF Core s UWP, naleznete v tématu [implementacím rozhraní .NET, které EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="48f15-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="48f15-180">Přečtěte si další články v této dokumentaci se dozvíte další informace o funkcích Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="48f15-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
