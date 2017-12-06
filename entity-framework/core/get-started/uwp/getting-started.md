---
title: "Začínáme na základní EF UWP - novou databázi –"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="dd893-102">Začínáme s EF základní na univerzální platformu Windows (UWP) s novou databázi</span><span class="sxs-lookup"><span data-stu-id="dd893-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="dd893-103">Tento kurz používá základní EF 2.0.1 (vydané spolu s ASP.NET Core a .NET Core SDK 2.0.3).</span><span class="sxs-lookup"><span data-stu-id="dd893-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="dd893-104">Základní EF 2.0.0 chybí některé zásadní opravy chyb, které jsou potřebné pro dobrý UWP prostředí.</span><span class="sxs-lookup"><span data-stu-id="dd893-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="dd893-105">V tomto návodu vytvoříte univerzální platformu Windows (UWP) aplikace, která provádí základní data přístup k místní databázi SQLite používající rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dd893-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dd893-106">**Zvažte vyloučení anonymní typy v dotazech LINQ na UWP**.</span><span class="sxs-lookup"><span data-stu-id="dd893-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="dd893-107">Nasazení aplikace UWP k obchodu s aplikacemi vyžaduje vaše aplikace a má kompilovat s .NET Native.</span><span class="sxs-lookup"><span data-stu-id="dd893-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="dd893-108">Dotazy s anonymní typy mít zhoršení výkonu .NET Native.</span><span class="sxs-lookup"><span data-stu-id="dd893-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="dd893-109">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="dd893-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd893-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dd893-110">Prerequisites</span></span>

<span data-ttu-id="dd893-111">Následující položky jsou nutné k dokončení tohoto návodu:</span><span class="sxs-lookup"><span data-stu-id="dd893-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="dd893-112">[Aktualizace Windows 10 patří Creators](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="dd893-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="dd893-113">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dd893-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="dd893-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15,4 nebo novější s **Universal Windows Platform Development** zatížení.</span><span class="sxs-lookup"><span data-stu-id="dd893-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="dd893-115">Vytvoření nového projektu modelu</span><span class="sxs-lookup"><span data-stu-id="dd893-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="dd893-116">Z důvodu omezení v nástrojích pro .NET Core způsob, jak pracovat s projekty UWP, které model musí být umístěny v jiných UWP projektu mohli ke spuštění příkazů migrace v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="dd893-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="dd893-117">Otevřete Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd893-117">Open Visual Studio</span></span>

* <span data-ttu-id="dd893-118">Soubor > Nový > projekt...</span><span class="sxs-lookup"><span data-stu-id="dd893-118">File > New > Project...</span></span>

* <span data-ttu-id="dd893-119">V levé nabídce vyberte šablony > Visual C#</span><span class="sxs-lookup"><span data-stu-id="dd893-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="dd893-120">Vyberte **knihovny tříd (.NET Standard)** šablona projektu</span><span class="sxs-lookup"><span data-stu-id="dd893-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="dd893-121">Pojmenujte projekt a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="dd893-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="dd893-122">Nainstalujte rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="dd893-122">Install Entity Framework</span></span>

<span data-ttu-id="dd893-123">Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit.</span><span class="sxs-lookup"><span data-stu-id="dd893-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="dd893-124">Tento návod používá SQLite.</span><span class="sxs-lookup"><span data-stu-id="dd893-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="dd893-125">Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="dd893-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="dd893-126">Nástroje > Správce balíčků NuGet > Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="dd893-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="dd893-127">Spustit`Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="dd893-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="dd893-128">Dále v tomto návodu také použijeme některé nástroje Entity Framework pro správnou údržbu databáze.</span><span class="sxs-lookup"><span data-stu-id="dd893-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="dd893-129">Proto se nainstaluje balíček nástroje.</span><span class="sxs-lookup"><span data-stu-id="dd893-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="dd893-130">Spustit`Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="dd893-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="dd893-131">Upravte soubor .csproj a nahraďte `<TargetFramework>netstandard2.0</TargetFramework>` s`<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span><span class="sxs-lookup"><span data-stu-id="dd893-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="dd893-132">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="dd893-132">Create your model</span></span>

<span data-ttu-id="dd893-133">Nyní je čas k definování kontextu a entity tříd, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="dd893-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="dd893-134">Projekt > přidejte třídu...</span><span class="sxs-lookup"><span data-stu-id="dd893-134">Project > Add Class...</span></span>

* <span data-ttu-id="dd893-135">Zadejte *Model.cs* jako název a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="dd893-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="dd893-136">Obsah souboru nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="dd893-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="dd893-137">Vytvoření nového projektu UPW</span><span class="sxs-lookup"><span data-stu-id="dd893-137">Create a new UWP project</span></span>

* <span data-ttu-id="dd893-138">Otevřete Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd893-138">Open Visual Studio</span></span>

* <span data-ttu-id="dd893-139">Soubor > Nový > projekt...</span><span class="sxs-lookup"><span data-stu-id="dd893-139">File > New > Project...</span></span>

* <span data-ttu-id="dd893-140">V levé nabídce vyberte šablony > Visual C# > univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="dd893-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="dd893-141">Vyberte **prázdná aplikace (univerzální pro Windows)** šablona projektu</span><span class="sxs-lookup"><span data-stu-id="dd893-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="dd893-142">Pojmenujte projekt a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="dd893-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="dd893-143">Nastavte alespoň cíl a minimální verze`Windows 10 Fall Creators Update (10.0; build 16299.0)`</span><span class="sxs-lookup"><span data-stu-id="dd893-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="dd893-144">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="dd893-144">Create your database</span></span>

<span data-ttu-id="dd893-145">Teď, když máte model, můžete k vytvoření databáze pro vás migrace.</span><span class="sxs-lookup"><span data-stu-id="dd893-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="dd893-146">Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="dd893-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="dd893-147">Vyberte model projekt jako výchozí projekt a nastavte jej jako projekt po spuštění</span><span class="sxs-lookup"><span data-stu-id="dd893-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="dd893-148">Spustit `Add-Migration MyFirstMigration` chcete vygenerovat migraci k vytvoření počáteční sadu tabulek pro váš model.</span><span class="sxs-lookup"><span data-stu-id="dd893-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="dd893-149">Vzhledem k tomu, že chceme databáze má být vytvořena na zařízení, které aplikace běží na, přidáme některé kódu pro použití žádné čekající migrace do místní databáze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd893-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="dd893-150">Při prvním spuštění aplikace, to se postará o vytvoření místní databázi pro nás.</span><span class="sxs-lookup"><span data-stu-id="dd893-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="dd893-151">Klikněte pravým tlačítkem na **App.xaml** v **Průzkumníku řešení** a vyberte **zobrazení kódu**</span><span class="sxs-lookup"><span data-stu-id="dd893-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="dd893-152">Na začátek souboru přidejte zvýrazněná using</span><span class="sxs-lookup"><span data-stu-id="dd893-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="dd893-153">Přidejte zvýrazněný kód použít všechny čekající migrace</span><span class="sxs-lookup"><span data-stu-id="dd893-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="dd893-154">Pokud provedete budoucí změny modelu, můžete použít `Add-Migration` příkaz chcete vygenerovat nový migraci použít k odpovídající položce změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="dd893-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="dd893-155">Žádné čekající migrace se použijí k místní databázi na každé zařízení při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd893-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="dd893-156">Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, která již byla do databáze použít.</span><span class="sxs-lookup"><span data-stu-id="dd893-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="dd893-157">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="dd893-157">Use your model</span></span>

<span data-ttu-id="dd893-158">Nyní můžete modelu provádí přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="dd893-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="dd893-159">Otevřete *MainPage.xaml*</span><span class="sxs-lookup"><span data-stu-id="dd893-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="dd893-160">Přidejte obslužné rutiny zatížení stránky a uživatelského rozhraní obsahu zvýrazněná níže</span><span class="sxs-lookup"><span data-stu-id="dd893-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="dd893-161">Teď přidáme kód pro propojit vytvořit uživatelské rozhraní s databází</span><span class="sxs-lookup"><span data-stu-id="dd893-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="dd893-162">Klikněte pravým tlačítkem na **MainPage.xaml** v **Průzkumníku řešení** a vyberte **zobrazení kódu**</span><span class="sxs-lookup"><span data-stu-id="dd893-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="dd893-163">Přidejte zvýrazněný kód z následujícího seznamu</span><span class="sxs-lookup"><span data-stu-id="dd893-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="dd893-164">Nyní můžete spustit aplikaci najdete v části v akci.</span><span class="sxs-lookup"><span data-stu-id="dd893-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="dd893-165">Ladění > Spustit bez ladění</span><span class="sxs-lookup"><span data-stu-id="dd893-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="dd893-166">Aplikace bude sestavovat a spouštět</span><span class="sxs-lookup"><span data-stu-id="dd893-166">The application will build and launch</span></span>

* <span data-ttu-id="dd893-167">Zadejte adresu URL a klikněte na **přidat** tlačítko</span><span class="sxs-lookup"><span data-stu-id="dd893-167">Enter a URL and click the **Add** button</span></span>

![obrázek](_static/create.png)

![obrázek](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="dd893-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd893-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="dd893-171">`SaveChanges()`výkon lze zvýšit implementací [ `INotifyPropertyChanged` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [ `INotifyPropertyChanging` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [ `INotifyCollectionChanged` ](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) ve vašem typy entit a pomocí `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span><span class="sxs-lookup"><span data-stu-id="dd893-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="dd893-172">Tada!</span><span class="sxs-lookup"><span data-stu-id="dd893-172">Tada!</span></span> <span data-ttu-id="dd893-173">Nyní máte jednoduchou aplikaci UWP systémem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dd893-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="dd893-174">Podívejte se na další články v této dokumentaci další informace o funkcích rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dd893-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
