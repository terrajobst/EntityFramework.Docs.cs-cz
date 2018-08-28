---
title: Začínáme na UPW – nová databáze – EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996907"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="9e087-102">Začínáme s EF Core na Universal Windows Platform (UWP) s novou databázi</span><span class="sxs-lookup"><span data-stu-id="9e087-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="9e087-103">V tomto kurzu vytvoříte aplikaci univerzální platformy Windows (UPW), který provádí základní přístup k datům s použitím Entity Framework Core místní databázi SQLite.</span><span class="sxs-lookup"><span data-stu-id="9e087-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="9e087-104">[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="9e087-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e087-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9e087-105">Prerequisites</span></span>

* <span data-ttu-id="9e087-106">[Windows 10 Fall Creators Update (10.0; Sestavení 16299) nebo novější](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="9e087-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="9e087-107">[Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/) s **vývoj pro univerzální platformu Windows** pracovního vytížení.</span><span class="sxs-lookup"><span data-stu-id="9e087-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="9e087-108">[Sady SDK .NET core 2.1 nebo novější](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9e087-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="9e087-109">Vytvoření projektu s modelem</span><span class="sxs-lookup"><span data-stu-id="9e087-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9e087-110">Vzhledem k omezením způsobem nástroje .NET Core pracovat s projekty UWP v modelu musí být umístěny v projektu bez UPW budete moci spouštět příkazy migrace **Konzola správce balíčků** (PMC)</span><span class="sxs-lookup"><span data-stu-id="9e087-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="9e087-111">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e087-111">Open Visual Studio</span></span>

* <span data-ttu-id="9e087-112">**Soubor > Nový > Projekt**</span><span class="sxs-lookup"><span data-stu-id="9e087-112">**File > New > Project**</span></span>

* <span data-ttu-id="9e087-113">V levé nabídce vyberte **nainstalováno > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="9e087-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="9e087-114">Vyberte **knihovna tříd (.NET Standard)** šablony.</span><span class="sxs-lookup"><span data-stu-id="9e087-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="9e087-115">Pojmenujte projekt *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="9e087-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="9e087-116">Pojmenujte řešení *blogovací*.</span><span class="sxs-lookup"><span data-stu-id="9e087-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="9e087-117">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e087-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="9e087-118">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9e087-118">Install Entity Framework Core</span></span>

<span data-ttu-id="9e087-119">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="9e087-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="9e087-120">Tento kurz používá SQLite.</span><span class="sxs-lookup"><span data-stu-id="9e087-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="9e087-121">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9e087-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="9e087-122">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="9e087-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="9e087-123">Spustit `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="9e087-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="9e087-124">Později v tomto kurzu budete používat některé nástroje Entity Framework Core pro správnou údržbu databáze.</span><span class="sxs-lookup"><span data-stu-id="9e087-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="9e087-125">Balíček nástroje tak instalaci.</span><span class="sxs-lookup"><span data-stu-id="9e087-125">So install the tools package as well.</span></span>

* <span data-ttu-id="9e087-126">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="9e087-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9e087-127">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="9e087-127">Create the model</span></span>

<span data-ttu-id="9e087-128">Nyní je možné definovat kontext a entity třídy, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="9e087-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="9e087-129">Odstranit *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e087-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="9e087-130">Vytvoření *Model.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="9e087-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="9e087-131">Vytvoření nového projektu pro UPW</span><span class="sxs-lookup"><span data-stu-id="9e087-131">Create a new UWP project</span></span>

* <span data-ttu-id="9e087-132">V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **Přidat > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="9e087-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="9e087-133">V levé nabídce vyberte **nainstalováno > Visual C# > Windows Universal**.</span><span class="sxs-lookup"><span data-stu-id="9e087-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="9e087-134">Vyberte **prázdná aplikace (Universal Windows)** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="9e087-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="9e087-135">Pojmenujte projekt *Blogging.UWP*a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="9e087-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="9e087-136">Nastavte cílovou a minimální verze na alespoň **Windows 10 Fall Creators Update (10.0; sestavení 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="9e087-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="9e087-137">Vytvoření počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="9e087-137">Create the initial migration</span></span>

<span data-ttu-id="9e087-138">Teď, když máte model, nastavte pro vytvoření databáze při prvním spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e087-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="9e087-139">V této části vytvoříte počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="9e087-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="9e087-140">V následující části přidáte kód, který použije tuto migraci při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e087-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="9e087-141">Nástroje pro migraci vyžadovat při spuštění projektu bez UPW, takže nyní vytvořte.</span><span class="sxs-lookup"><span data-stu-id="9e087-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="9e087-142">V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **Přidat > Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="9e087-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="9e087-143">V levé nabídce vyberte **nainstalováno > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9e087-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="9e087-144">Vyberte **Konzolová aplikace (.NET Core)** šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="9e087-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="9e087-145">Pojmenujte projekt *Blogging.Migrations.Startup*a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e087-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="9e087-146">Přidat odkaz na projekt z *Blogging.Migrations.Startup* projektu *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="9e087-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="9e087-147">Nyní můžete vytvořit počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="9e087-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="9e087-148">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="9e087-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="9e087-149">Vyberte *Blogging.Model* projektu jako **výchozí projekt**.</span><span class="sxs-lookup"><span data-stu-id="9e087-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="9e087-150">V **Průzkumníka řešení**, nastavte *Blogging.Migrations.Startup* projekt jako spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="9e087-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="9e087-151">Spustit `Add-Migration InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="9e087-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="9e087-152">Tento příkaz vygeneruje uživatelské rozhraní, které vytvoří počáteční sadu tabulek pro váš model migrace.</span><span class="sxs-lookup"><span data-stu-id="9e087-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="9e087-153">Vytvoření databáze při spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="9e087-153">Create the database on app startup</span></span>

<span data-ttu-id="9e087-154">Protože chcete, aby databáze, kterou chcete vytvořit pro zařízení, na které aplikace poběží, přidejte kód pro použití žádné čekající migrace do místní databáze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e087-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="9e087-155">Při prvním spuštění aplikace, postará se vytvoří místní databázi.</span><span class="sxs-lookup"><span data-stu-id="9e087-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="9e087-156">Přidat odkaz na projekt z *Blogging.UWP* projektu *Blogging.Model* projektu.</span><span class="sxs-lookup"><span data-stu-id="9e087-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="9e087-157">Otevřít *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e087-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="9e087-158">Přidejte zvýrazněný kód chcete použít všechny probíhající migrace.</span><span class="sxs-lookup"><span data-stu-id="9e087-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="9e087-159">Pokud změníte model, použijte `Add-Migration` příkaz scaffold novou migraci použít odpovídající změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="9e087-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="9e087-160">Všechny probíhající migrace se použijí k místní databázi na každé zařízení při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e087-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="9e087-161">Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.</span><span class="sxs-lookup"><span data-stu-id="9e087-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="9e087-162">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="9e087-162">Use the model</span></span>

<span data-ttu-id="9e087-163">Nyní můžete model přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="9e087-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="9e087-164">Otevřít *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="9e087-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="9e087-165">Přidat obslužnou rutinu načtení stránky a uživatelské rozhraní obsah jejichž přehled najdete níže</span><span class="sxs-lookup"><span data-stu-id="9e087-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="9e087-166">Teď přidejte kód, který propojí uživatelského rozhraní pomocí databáze</span><span class="sxs-lookup"><span data-stu-id="9e087-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="9e087-167">Otevřít *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e087-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="9e087-168">V následujícím seznamu přidejte zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="9e087-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="9e087-169">Nyní můžete spustit aplikaci sledujte v akci.</span><span class="sxs-lookup"><span data-stu-id="9e087-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="9e087-170">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Blogging.UWP* projektu a pak vyberte **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="9e087-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="9e087-171">Nastavte *Blogging.UWP* jako spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="9e087-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="9e087-172">**Ladit > Spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="9e087-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="9e087-173">Aplikace vytvoří a spustí.</span><span class="sxs-lookup"><span data-stu-id="9e087-173">The app builds and runs.</span></span>

* <span data-ttu-id="9e087-174">Zadejte adresu URL a klikněte na tlačítko **přidat** tlačítko</span><span class="sxs-lookup"><span data-stu-id="9e087-174">Enter a URL and click the **Add** button</span></span>

  ![obrázek](_static/create.png)

  ![obrázek](_static/list.png)

  <span data-ttu-id="9e087-177">Tada!</span><span class="sxs-lookup"><span data-stu-id="9e087-177">Tada!</span></span> <span data-ttu-id="9e087-178">Teď máte jednoduché aplikace pro UPW s Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9e087-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e087-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e087-179">Next steps</span></span>

<span data-ttu-id="9e087-180">Kompatibilitu a výkon informace, které byste měli znát při používání EF Core s UWP, naleznete v tématu [implementacím rozhraní .NET, které EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="9e087-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="9e087-181">Přečtěte si další články v této dokumentaci se dozvíte další informace o funkcích Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9e087-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
