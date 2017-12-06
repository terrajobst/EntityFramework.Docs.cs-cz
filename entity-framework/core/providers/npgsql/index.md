---
title: "Zprostředkovatel Npgsql databáze pro PostgreSQL - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="a8009-102">Npgsql EF základní zprostředkovatel databáze pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="a8009-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="a8009-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="a8009-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="a8009-104">Zprostředkovatel se udržuje v rámci [Npgsql projektu](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="a8009-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="a8009-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a8009-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="a8009-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="a8009-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="a8009-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="a8009-107">Install</span></span>

<span data-ttu-id="a8009-108">Nainstalujte [balíček Npgsql.EntityFrameworkCore.PostgreSQL NuGet](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="a8009-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="a8009-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="a8009-109">Get Started</span></span>

<span data-ttu-id="a8009-110">Najdete v článku [Npgsql dokumentace](http://www.npgsql.org/efcore/index.html) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="a8009-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="a8009-111">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="a8009-111">Supported Database Engines</span></span>

* <span data-ttu-id="a8009-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="a8009-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a8009-113">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="a8009-113">Supported Platforms</span></span>

* <span data-ttu-id="a8009-114">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="a8009-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="a8009-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8009-115">.NET Core</span></span>

* <span data-ttu-id="a8009-116">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="a8009-116">Mono (4.2.0 onwards)</span></span>
