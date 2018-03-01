---
title: "Zprostředkovatel databáze FirebirdSQL – EF jádra"
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="ad4e5-102">Zprostředkovatel firebird EF základní databáze</span><span class="sxs-lookup"><span data-stu-id="ad4e5-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="ad4e5-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s FirebirdSQL.</span><span class="sxs-lookup"><span data-stu-id="ad4e5-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="ad4e5-104">Zprostředkovatel se udržuje v rámci [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL Githubu projektu](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="ad4e5-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="ad4e5-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ad4e5-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ad4e5-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="ad4e5-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="ad4e5-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="ad4e5-107">Install</span></span>

<span data-ttu-id="ad4e5-108">Nainstalujte [balíček EntityFrameworkCore.FirebirdSQL NuGet](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="ad4e5-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="ad4e5-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="ad4e5-109">Get Started</span></span>

<span data-ttu-id="ad4e5-110">Najdete v článku [Začínáme dokumentaci na webu projektu](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span><span class="sxs-lookup"><span data-stu-id="ad4e5-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="ad4e5-111">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="ad4e5-111">Supported Database Engines</span></span>

* <span data-ttu-id="ad4e5-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="ad4e5-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="ad4e5-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="ad4e5-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ad4e5-114">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="ad4e5-114">Supported Platforms</span></span>

* <span data-ttu-id="ad4e5-115">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="ad4e5-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="ad4e5-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad4e5-116">.NET Core</span></span>

* <span data-ttu-id="ad4e5-117">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="ad4e5-117">Mono (4.2.0 onwards)</span></span>
