---
title: Začínáme na UPW – nová databáze – EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022373"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="65ea7-102">Začínáme s EF Core na Universal Windows Platform (UWP) s novou databázi</span><span class="sxs-lookup"><span data-stu-id="65ea7-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="65ea7-103">V tomto kurzu vytvoříte aplikaci univerzální platformy Windows (UPW), který provádí základní přístup k datům s použitím Entity Framework Core místní databázi SQLite.</span><span class="sxs-lookup"><span data-stu-id="65ea7-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="65ea7-104">[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="65ea7-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65ea7-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="65ea7-105">Prerequisites</span></span>

* <span data-ttu-id="65ea7-106">[Windows 10 Fall Creators Update (10.0; Sestavení 16299) nebo novější](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="65ea7-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="65ea7-107">[Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/) s **vývoj pro univerzální platformu Windows** pracovního vytížení.</span><span class="sxs-lookup"><span data-stu-id="65ea7-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="65ea7-108">[Sady SDK .NET core 2.1 nebo novější](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="65ea7-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65ea7-109">Tento kurz používá Entity Framework Core [migrace](xref:core/managing-schemas/migrations/index) příkazy k vytvoření a aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="65ea7-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="65ea7-110">Tyto příkazy nechcete pracovat přímo s projekty UWP.</span><span class="sxs-lookup"><span data-stu-id="65ea7-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="65ea7-111">Z tohoto důvodu aplikace datový model je umístěn ve sdílené knihovně projektu a samostatné konzolovou aplikaci .NET Core se používá ke spuštění příkazů.</span><span class="sxs-lookup"><span data-stu-id="65ea7-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="65ea7-112">Vytvořte projekt knihovny pro uchování datového modelu</span><span class="sxs-lookup"><span data-stu-id="65ea7-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="65ea7-113">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65ea7-113">Open Visual Studio</span></span>

* <span data-ttu-id="65ea7-114">**Soubor > Nový > Projekt**</span><span class="sxs-lookup"><span data-stu-id="65ea7-114">**File > New > Project**</span></span>

* <span data-ttu-id="65ea7-115">V levé nabídce vyberte **nainstalováno > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="65ea7-116">Vyberte **knihovna tříd (.NET Standard)** šablony.</span><span class="sxs-lookup"><span data-stu-id="65ea7-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="65ea7-117">Pojmenujte projekt *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="65ea7-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="65ea7-118">Pojmenujte řešení *blogovací*.</span><span class="sxs-lookup"><span data-stu-id="65ea7-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="65ea7-119">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="65ea7-120">Nainstalovat modul runtime Entity Framework Core v datovém modelu projektu</span><span class="sxs-lookup"><span data-stu-id="65ea7-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="65ea7-121">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="65ea7-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="65ea7-122">Tento kurz používá SQLite.</span><span class="sxs-lookup"><span data-stu-id="65ea7-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="65ea7-123">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="65ea7-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="65ea7-124">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="65ea7-125">Ujistěte se, že projekt knihovny *Blogging.Model* je zvolen jako **výchozí projekt** v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="65ea7-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="65ea7-126">Spustit `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="65ea7-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="65ea7-127">Vytvoření datového modelu</span><span class="sxs-lookup"><span data-stu-id="65ea7-127">Create the data model</span></span>

<span data-ttu-id="65ea7-128">Nyní je možné definovat *DbContext* a tříd entit, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="65ea7-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="65ea7-129">Odstranit *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="65ea7-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="65ea7-130">Vytvoření *Model.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="65ea7-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="65ea7-131">Vytvořte nový projekt konzoly pro spuštění příkazů migrace</span><span class="sxs-lookup"><span data-stu-id="65ea7-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="65ea7-132">V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **Přidat > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="65ea7-133">V levé nabídce vyberte **nainstalováno > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="65ea7-134">Vyberte **Konzolová aplikace (.NET Core)** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="65ea7-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="65ea7-135">Pojmenujte projekt *Blogging.Migrations.Startup*a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="65ea7-136">Přidat odkaz na projekt z *Blogging.Migrations.Startup* projektu *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="65ea7-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="65ea7-137">Instalace nástroje Entity Framework Core v projektu po spuštění migrace</span><span class="sxs-lookup"><span data-stu-id="65ea7-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="65ea7-138">Povolit příkazy EF Core migrace v konzole Správce balíčků, nainstalujte balíček nástroje EF Core v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="65ea7-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="65ea7-139">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="65ea7-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="65ea7-140">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="65ea7-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="65ea7-141">Vytvoření počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="65ea7-141">Create the initial migration</span></span>

 <span data-ttu-id="65ea7-142">Vytvořte počáteční migraci zadáte jako spouštěný projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="65ea7-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="65ea7-143">Spustit `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="65ea7-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="65ea7-144">Tento příkaz vygeneruje uživatelské rozhraní migrace, které vytvoří počáteční sada databázových tabulek datového modelu.</span><span class="sxs-lookup"><span data-stu-id="65ea7-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="65ea7-145">Vytvoření projektu UPW</span><span class="sxs-lookup"><span data-stu-id="65ea7-145">Create the UWP project</span></span>

* <span data-ttu-id="65ea7-146">V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **Přidat > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="65ea7-147">V levé nabídce vyberte **nainstalováno > Visual C# > Windows Universal**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="65ea7-148">Vyberte **prázdná aplikace (Universal Windows)** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="65ea7-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="65ea7-149">Pojmenujte projekt *Blogging.UWP*a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="65ea7-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65ea7-150">Nastavte cílovou a minimální verze na alespoň **Windows 10 Fall Creators Update (10.0; sestavení 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="65ea7-151">Předchozí verze Windows 10 nepodporuje .NET Standard 2.0, které je požadované Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="65ea7-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="65ea7-152">Přidejte kód k vytvoření databáze při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="65ea7-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="65ea7-153">Protože chcete, aby databáze, kterou chcete vytvořit pro zařízení, na které aplikace poběží, přidejte kód pro použití žádné čekající migrace do místní databáze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="65ea7-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="65ea7-154">Při prvním spuštění aplikace, postará se vytvoří místní databázi.</span><span class="sxs-lookup"><span data-stu-id="65ea7-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="65ea7-155">Přidat odkaz na projekt z *Blogging.UWP* projektu *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="65ea7-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="65ea7-156">Otevřít *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="65ea7-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="65ea7-157">Přidejte zvýrazněný kód chcete použít všechny probíhající migrace.</span><span class="sxs-lookup"><span data-stu-id="65ea7-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="65ea7-158">Pokud změníte model, použijte `Add-Migration` příkaz scaffold novou migraci použít odpovídající změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="65ea7-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="65ea7-159">Všechny probíhající migrace se použijí k místní databázi na každé zařízení při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="65ea7-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="65ea7-160">Používá EF Core `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.</span><span class="sxs-lookup"><span data-stu-id="65ea7-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="65ea7-161">Datový model</span><span class="sxs-lookup"><span data-stu-id="65ea7-161">Use the data model</span></span>

<span data-ttu-id="65ea7-162">EF Core můžete teď použít přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="65ea7-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="65ea7-163">Otevřít *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="65ea7-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="65ea7-164">Přidat obslužnou rutinu načtení stránky a uživatelské rozhraní obsah jejichž přehled najdete níže</span><span class="sxs-lookup"><span data-stu-id="65ea7-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="65ea7-165">Teď přidejte kód, který propojí uživatelského rozhraní pomocí databáze</span><span class="sxs-lookup"><span data-stu-id="65ea7-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="65ea7-166">Otevřít *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="65ea7-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="65ea7-167">V následujícím seznamu přidejte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="65ea7-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="65ea7-168">Nyní můžete spustit aplikaci sledujte v akci.</span><span class="sxs-lookup"><span data-stu-id="65ea7-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="65ea7-169">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Blogging.UWP* projektu a pak vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="65ea7-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="65ea7-170">Nastavte *Blogging.UWP* jako spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="65ea7-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="65ea7-171">**Ladit > Spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="65ea7-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="65ea7-172">Aplikace vytvoří a spustí.</span><span class="sxs-lookup"><span data-stu-id="65ea7-172">The app builds and runs.</span></span>

* <span data-ttu-id="65ea7-173">Zadejte adresu URL a klikněte na tlačítko **přidat** tlačítko</span><span class="sxs-lookup"><span data-stu-id="65ea7-173">Enter a URL and click the **Add** button</span></span>

  ![obrázek](_static/create.png)

  ![obrázek](_static/list.png)

  <span data-ttu-id="65ea7-176">Tada!</span><span class="sxs-lookup"><span data-stu-id="65ea7-176">Tada!</span></span> <span data-ttu-id="65ea7-177">Teď máte jednoduché aplikace pro UPW s Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="65ea7-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="65ea7-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65ea7-178">Next steps</span></span>

<span data-ttu-id="65ea7-179">Kompatibilitu a výkon informace, které byste měli znát při používání EF Core s UWP, naleznete v tématu [implementacím rozhraní .NET, které EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="65ea7-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="65ea7-180">Přečtěte si další články v této dokumentaci se dozvíte další informace o funkcích Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="65ea7-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
