---
title: Získat Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 78ef1e7b20bd879c972870552c8f692e153b7abb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996562"
---
# <a name="get-entity-framework"></a><span data-ttu-id="6a9bc-102">Získat Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6a9bc-102">Get Entity Framework</span></span>
<span data-ttu-id="6a9bc-103">Entity Framework se skládá z EF nástrojů pro Visual Studio a modul Runtime EF.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="6a9bc-104">EF Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a9bc-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="6a9bc-105">Entity Framework Tools for Visual Studio zahrnují EF designeru a v Průvodci modelů EF a jsou nejprve nutné pro databázi a model první pracovní postupy.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="6a9bc-106">EF nástroje jsou součástí všechny nejnovější verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="6a9bc-107">-Li provést vlastní instalaci sady Visual Studio, budete muset zajistit, aby položka "Entity Framework 6 Tools" je vybrána volba úlohu, která ji obsahuje nebo výběrem jednotlivých součástí.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="6a9bc-108">U některých starších verzí sady Visual Studio aktualizované EF nástroje jsou k dispozici ke stažení.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="6a9bc-109">Zobrazit [verzí sady Visual Studio](~/ef6/what-is-new/visual-studio.md) pokyny o tom, jak získat nejnovější verzi nástrojů EF dostupných pro vaši verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="6a9bc-110">Modul Runtime EF</span><span class="sxs-lookup"><span data-stu-id="6a9bc-110">EF Runtime</span></span>

<span data-ttu-id="6a9bc-111">Nejnovější verzi rozhraní Entity Framework je k dispozici jako [balíček EntityFramework NuGet](http://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="6a9bc-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="6a9bc-112">Pokud nejste obeznámeni s Správce balíčků NuGet, doporučujeme vám přečíst si téma [NuGet přehled](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="6a9bc-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="6a9bc-113">Instaluje se balíček NuGet EF</span><span class="sxs-lookup"><span data-stu-id="6a9bc-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="6a9bc-114">Můžete nainstalovat balíček EntityFramework kliknutím pravým tlačítkem na **odkazy** složky vašeho projektu a výběrem **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="6a9bc-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="6a9bc-116">Instalace z konzoly Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="6a9bc-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="6a9bc-117">Alternativně můžete nainstalovat EntityFramework spuštěním následujícího příkazu v [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="6a9bc-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="6a9bc-118">Instalace konkrétní verze EF</span><span class="sxs-lookup"><span data-stu-id="6a9bc-118">Installing a specific version of EF</span></span>

<span data-ttu-id="6a9bc-119">Z EF 4.1 a vyšší, byly vydány nové verze modulu EF runtime jako [balíčku NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="6a9bc-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Pacakge](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="6a9bc-120">Některé z těchto verzí můžete přidat do projektu na základě rozhraní .NET Framework, spuštěním následujícího příkazu v sadě Visual Studio [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="6a9bc-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="6a9bc-121">Všimněte si, že `<number>` představuje konkrétní verze EF k instalaci.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="6a9bc-122">Například 6.2.0 je verze číslo pro EF 6.2.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="6a9bc-123">Moduly runtime EF před 4.1 byly součástí rozhraní .NET Framework a se nedá nainstalovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="6a9bc-124">Instalace nejnovější verze Preview</span><span class="sxs-lookup"><span data-stu-id="6a9bc-124">Installing the Latest Preview</span></span>

<span data-ttu-id="6a9bc-125">Výše uvedené metody získáte nejnovější plně podporované verze rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="6a9bc-126">Jsou často předběžné verze dostupná, budeme rádi, kdybyste k vyzkoušení a sdělte nám svůj názor na rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="6a9bc-127">Chcete-li nainstalovat nejnovější verzi preview objektu EntityFramework, můžete vybrat **zahrnout předprodejní verze** v okně Správa balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="6a9bc-128">Pokud jsou k dispozici žádné předběžné verze se automaticky zobrazí nejnovější plně podporovanou verzi rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a9bc-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![IncludePreRelease](~/ef6/media/includeprerelease.png)

<span data-ttu-id="6a9bc-130">Alternativně můžete spustit následující příkaz [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="6a9bc-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
