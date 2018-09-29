---
title: Referenční dokumentace nástrojů v Entity Framework Core – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 9fcb452c2798a3d07e39cbcc3c34629dca4394ff
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447115"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="c625c-102">Referenční dokumentace nástrojů Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c625c-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="c625c-103">Entity Framework Core nástroje vám pomůžou s vývojem během návrhu úkoly.</span><span class="sxs-lookup"><span data-stu-id="c625c-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="c625c-104">Primárně slouží ke správě migrace a generování uživatelského rozhraní `DbContext` a typy entit ve zpětné analýze schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="c625c-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="c625c-105">Tím [Konzola správce balíčků EF Core tools](powershell.md)) spustit [Konzola správce balíčků](https://docs.microsoft.com/nuget/tools/package-manager-console) v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c625c-105">The [EF Core Package Manager Console tools](powershell.md)) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="c625c-106">Tyto nástroje pracují s projekty rozhraní .NET Framework a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c625c-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

* <span data-ttu-id="c625c-107">[Nástroje rozhraní příkazového řádku (CLI) EF Core .NET](dotnet.md) jsou rozšíření pro různé platformy [nástroje rozhraní příkazového řádku .NET Core](https://docs.microsoft.com/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="c625c-107">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="c625c-108">Tyto nástroje vyžadují projektu .NET Core SDK (s `Sdk="Microsoft.NET.Sdk"` nebo podobně jako v souboru projektu).</span><span class="sxs-lookup"><span data-stu-id="c625c-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="c625c-109">Oba nástroje zveřejňují stejné funkce.</span><span class="sxs-lookup"><span data-stu-id="c625c-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="c625c-110">Pokud vyvíjíte v sadě Visual Studio, doporučujeme použít **Konzola správce balíčků** nástroje, které poskytují více integrované prostředí.</span><span class="sxs-lookup"><span data-stu-id="c625c-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c625c-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c625c-111">Next steps</span></span>

* [<span data-ttu-id="c625c-112">Konzola správce balíčků EF Core tools odkaz</span><span class="sxs-lookup"><span data-stu-id="c625c-112">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="c625c-113">Odkazovat na EF Core .NET nástrojů rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c625c-113">EF Core .NET CLI tools reference</span></span>](dotnet.md)