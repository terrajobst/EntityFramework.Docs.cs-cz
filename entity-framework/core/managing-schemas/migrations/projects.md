---
title: Migrace s více projekty – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997984"
---
<a name="using-a-separate-project"></a><span data-ttu-id="3a6e4-102">Použití samostatného projektu</span><span class="sxs-lookup"><span data-stu-id="3a6e4-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="3a6e4-103">Můžete chtít ukládat vaše migrace v jiném sestavení než jeden obsahující vaše `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="3a6e4-104">Můžete také tuto strategii udržovat několik sad migrace, například, jeden pro vývoj a druhý pro upgrady verzí.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="3a6e4-105">Postup...</span><span class="sxs-lookup"><span data-stu-id="3a6e4-105">To do this...</span></span>

1. <span data-ttu-id="3a6e4-106">Vytvořte novou knihovnu tříd.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-106">Create a new class library.</span></span>

2. <span data-ttu-id="3a6e4-107">Přidejte odkaz na sestavení DbContext.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="3a6e4-108">Přesuňte soubory snímků modelu a migrace do knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="3a6e4-109">Pokud máte k dispozici žádné existující migrace, vygenerujte ho projekt, který obsahuje objekt DbContext a přesuňte ho.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="3a6e4-110">To je důležité, protože pokud migrace sestavení neobsahuje existující migrace, příkazu Add-migrace nebude moci najít uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="3a6e4-111">Konfigurace sestavení migrace:</span><span class="sxs-lookup"><span data-stu-id="3a6e4-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="3a6e4-112">Přidejte odkaz na sestavení migrace ze spuštění sestavení.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="3a6e4-113">Pokud to způsobuje cyklickou závislost, aktualizujte výstupní cestu knihovny tříd:</span><span class="sxs-lookup"><span data-stu-id="3a6e4-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="3a6e4-114">Pokud jste to udělali všechno správně, byste měli možnost přidávat nové migrace do projektu.</span><span class="sxs-lookup"><span data-stu-id="3a6e4-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
