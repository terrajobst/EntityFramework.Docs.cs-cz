---
title: Získat Entity Framework – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419466"
---
# <a name="get-entity-framework"></a><span data-ttu-id="3ff39-102">Jak získat Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3ff39-102">Get Entity Framework</span></span>
<span data-ttu-id="3ff39-103">Entity Framework se skládá z nástrojů EF pro Visual Studio a modulu runtime EF.</span><span class="sxs-lookup"><span data-stu-id="3ff39-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="3ff39-104">Nástroje EF pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3ff39-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="3ff39-105">Entity Framework Tools pro Visual Studio obsahuje návrháře EF a Průvodce modelem EF a jsou nutné pro databázi nejprve a modelovat první pracovní postupy.</span><span class="sxs-lookup"><span data-stu-id="3ff39-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="3ff39-106">Nástroje EF jsou zahrnuté ve všech nejnovějších verzích sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ff39-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="3ff39-107">Pokud provedete vlastní instalaci sady Visual Studio, bude nutné zajistit, aby se položka "Entity Framework 6 Tools" vybrala buď tak, že vyberete úlohu, která ji obsahuje, nebo ji vyberete jako jednotlivou součást.</span><span class="sxs-lookup"><span data-stu-id="3ff39-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="3ff39-108">Pro některé starší verze sady Visual Studio jsou aktualizované nástroje EF k dispozici jako soubor ke stažení.</span><span class="sxs-lookup"><span data-stu-id="3ff39-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="3ff39-109">Návod, jak získat nejnovější verzi nástrojů EF dostupných pro vaši verzi sady Visual Studio, najdete v tématu [verze sady Visual Studio](~/ef6/what-is-new/visual-studio.md) .</span><span class="sxs-lookup"><span data-stu-id="3ff39-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="3ff39-110">Modul runtime EF</span><span class="sxs-lookup"><span data-stu-id="3ff39-110">EF Runtime</span></span>

<span data-ttu-id="3ff39-111">Nejnovější verze Entity Framework je k dispozici jako [balíček NuGet EntityFramework](https://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="3ff39-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="3ff39-112">Pokud nejste obeznámeni se správcem balíčků NuGet, doporučujeme, abyste si přečetli [Přehled NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="3ff39-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="3ff39-113">Instalace balíčku NuGet pro EF</span><span class="sxs-lookup"><span data-stu-id="3ff39-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="3ff39-114">Balíček EntityFramework můžete nainstalovat tak, že kliknete pravým tlačítkem na složku **odkazy** v projektu a vyberete **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="3ff39-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Správa balíčků NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="3ff39-116">Instalace z konzoly Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="3ff39-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="3ff39-117">Alternativně můžete nainstalovat EntityFramework spuštěním následujícího příkazu v [konzole správce balíčků](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="3ff39-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="3ff39-118">Instalace konkrétní verze EF</span><span class="sxs-lookup"><span data-stu-id="3ff39-118">Installing a specific version of EF</span></span>

<span data-ttu-id="3ff39-119">Od EF 4,1 a vyšší jsou nové verze modulu runtime EF vydány jako [balíček NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="3ff39-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="3ff39-120">Kteroukoli z těchto verzí lze přidat do projektu založeného na .NET Framework spuštěním následujícího příkazu v [konzole správce balíčků](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3ff39-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="3ff39-121">Všimněte si, že `<number>` představuje konkrétní verzi EF, která se má nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="3ff39-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="3ff39-122">Například 6.2.0 je verze čísla pro EF 6,2.</span><span class="sxs-lookup"><span data-stu-id="3ff39-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="3ff39-123">Moduly runtime EF před 4,1 byly součástí .NET Framework a nelze je nainstalovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="3ff39-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="3ff39-124">Instalace nejnovější verze Preview</span><span class="sxs-lookup"><span data-stu-id="3ff39-124">Installing the Latest Preview</span></span>

<span data-ttu-id="3ff39-125">Výše uvedené metody vám poskytnou nejnovější plně podporovanou verzi Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3ff39-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="3ff39-126">K dispozici jsou často předprodejní verze Entity Framework, které bychom rádi si vyzkoušeli a poskytli nám zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="3ff39-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="3ff39-127">Pokud chcete nainstalovat nejnovější verzi Preview nástroje EntityFramework, můžete v okně Spravovat balíčky NuGet vybrat možnost **Zahrnout předprodejní verze** .</span><span class="sxs-lookup"><span data-stu-id="3ff39-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="3ff39-128">Pokud nejsou k dispozici žádné předprodejní verze, bude automaticky získána nejnovější plně podporovaná verze Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3ff39-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Zahrnout předběžné verze](~/ef6/media/includeprerelease.png)

<span data-ttu-id="3ff39-130">Případně můžete spustit následující příkaz v [konzole správce balíčků](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="3ff39-130">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
