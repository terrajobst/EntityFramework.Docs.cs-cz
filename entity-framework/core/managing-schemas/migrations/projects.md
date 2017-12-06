---
title: "Migrace s více projekty - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="using-a-separate-project"></a><span data-ttu-id="eff01-102">Pomocí samostatného projektu</span><span class="sxs-lookup"><span data-stu-id="eff01-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="eff01-103">Můžete chtít uložit vaše migrace v jiném sestavení než jeden obsahující vaše `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="eff01-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="eff01-104">Můžete také tuto strategii zachování více sad migrace, například, jeden pro vývoj a druhý pro upgrade verze verze.</span><span class="sxs-lookup"><span data-stu-id="eff01-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="eff01-105">Chcete-li to provést...</span><span class="sxs-lookup"><span data-stu-id="eff01-105">To do this...</span></span>

1. <span data-ttu-id="eff01-106">Vytvořte nové knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="eff01-106">Create a new class library.</span></span>

2. <span data-ttu-id="eff01-107">Přidáte odkaz na sestavení vaší DbContext.</span><span class="sxs-lookup"><span data-stu-id="eff01-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="eff01-108">Přesuňte soubory snímků modelu a migrace do knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="eff01-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="eff01-109">Pokud nepřidali jste žádné, přidat do projektu DbContext a přesunout ho.</span><span class="sxs-lookup"><span data-stu-id="eff01-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="eff01-110">Konfigurace sestavení migrace:</span><span class="sxs-lookup"><span data-stu-id="eff01-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="eff01-111">Přidáte odkaz na vaše migrace sestavení ze sestavení po spuštění.</span><span class="sxs-lookup"><span data-stu-id="eff01-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="eff01-112">Pokud to způsobuje cyklickou závislost, aktualizujte výstupní cesta knihovny tříd:</span><span class="sxs-lookup"><span data-stu-id="eff01-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="eff01-113">Pokud jste to udělali všechno správně, byste měli přidávat nové migrace do projektu.</span><span class="sxs-lookup"><span data-stu-id="eff01-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
