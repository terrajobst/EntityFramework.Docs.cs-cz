---
title: "Zprostředkovatel Npgsql databáze pro PostgreSQL - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="37f77-102">Npgsql EF základní zprostředkovatel databáze pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="37f77-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="37f77-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="37f77-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="37f77-104">Zprostředkovatel se udržuje v rámci [Npgsql projektu](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="37f77-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="37f77-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="37f77-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="37f77-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="37f77-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="37f77-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="37f77-107">Install</span></span>

<span data-ttu-id="37f77-108">Nainstalujte [balíček Npgsql.EntityFrameworkCore.PostgreSQL NuGet](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="37f77-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="37f77-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="37f77-109">Get Started</span></span>

<span data-ttu-id="37f77-110">Najdete v článku [Npgsql dokumentace](http://www.npgsql.org/efcore/index.html) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="37f77-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="37f77-111">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="37f77-111">Supported Database Engines</span></span>

* <span data-ttu-id="37f77-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="37f77-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="37f77-113">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="37f77-113">Supported Platforms</span></span>

* <span data-ttu-id="37f77-114">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="37f77-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="37f77-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="37f77-115">.NET Core</span></span>

* <span data-ttu-id="37f77-116">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="37f77-116">Mono (4.2.0 onwards)</span></span>
