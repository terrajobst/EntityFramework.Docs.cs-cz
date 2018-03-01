---
title: "Zprostředkovatel databáze MySQL pomelo – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="a7a36-102">Pomelo EF základní zprostředkovatel databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="a7a36-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="a7a36-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s MySQL.</span><span class="sxs-lookup"><span data-stu-id="a7a36-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="a7a36-104">Zprostředkovatel se udržuje v rámci [Pomelo Foundation projektu](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="a7a36-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="a7a36-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a7a36-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="a7a36-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="a7a36-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="a7a36-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="a7a36-107">Install</span></span>

<span data-ttu-id="a7a36-108">Nainstalujte [balíček Pomelo.EntityFrameworkCore.MySql NuGet](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="a7a36-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="a7a36-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="a7a36-109">Get Started</span></span>

<span data-ttu-id="a7a36-110">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="a7a36-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="a7a36-111">Získávání dokumentaci Začínáme</span><span class="sxs-lookup"><span data-stu-id="a7a36-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="a7a36-112">Yuuko Blog ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="a7a36-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="a7a36-113">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="a7a36-113">Supported Database Engines</span></span>

* <span data-ttu-id="a7a36-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="a7a36-114">MySQL</span></span>
* <span data-ttu-id="a7a36-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="a7a36-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a7a36-116">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="a7a36-116">Supported Platforms</span></span>

* <span data-ttu-id="a7a36-117">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="a7a36-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="a7a36-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7a36-118">.NET Core</span></span>

* <span data-ttu-id="a7a36-119">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="a7a36-119">Mono (4.2.0 onwards)</span></span>
