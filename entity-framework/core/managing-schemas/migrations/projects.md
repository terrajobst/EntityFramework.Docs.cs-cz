---
title: Použití samostatného projektu migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0c08855db77470d28e23f9ef1d147497dfcdff83
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655554"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="ee01e-102">Použití samostatného projektu migrace</span><span class="sxs-lookup"><span data-stu-id="ee01e-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="ee01e-103">Můžete chtít ukládat migrace v jiném sestavení, než jaké obsahuje vaše `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ee01e-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="ee01e-104">Tuto strategii můžete také použít k údržbě několika skupin migrace, například jednoho pro vývoj a další pro upgrady typu Release-to-Release.</span><span class="sxs-lookup"><span data-stu-id="ee01e-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="ee01e-105">Postup...</span><span class="sxs-lookup"><span data-stu-id="ee01e-105">To do this...</span></span>

1. <span data-ttu-id="ee01e-106">Vytvořte novou knihovnu tříd.</span><span class="sxs-lookup"><span data-stu-id="ee01e-106">Create a new class library.</span></span>

2. <span data-ttu-id="ee01e-107">Přidejte odkaz na své DbContext sestavení.</span><span class="sxs-lookup"><span data-stu-id="ee01e-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="ee01e-108">Přesuňte migrace a soubory snímků modelu do knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="ee01e-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="ee01e-109">Pokud nemáte žádná existující migrace, vygenerujte ji v projektu obsahujícím DbContext a pak ji přesuňte.</span><span class="sxs-lookup"><span data-stu-id="ee01e-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="ee01e-110">To je důležité, protože pokud migrační sestavení neobsahují existující migraci, příkaz Add-Migration nebude schopen najít DbContext.</span><span class="sxs-lookup"><span data-stu-id="ee01e-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="ee01e-111">Nakonfigurujte sestavení migrace:</span><span class="sxs-lookup"><span data-stu-id="ee01e-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="ee01e-112">Přidejte odkaz na vaše sestavení migrace ze sestavení po spuštění.</span><span class="sxs-lookup"><span data-stu-id="ee01e-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="ee01e-113">Pokud to způsobí cyklickou závislost, aktualizujte výstupní cestu knihovny tříd:</span><span class="sxs-lookup"><span data-stu-id="ee01e-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="ee01e-114">Pokud jste provedli vše správně, měli byste být schopni přidat do projektu nové migrace.</span><span class="sxs-lookup"><span data-stu-id="ee01e-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="ee01e-115">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee01e-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="ee01e-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee01e-116">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
