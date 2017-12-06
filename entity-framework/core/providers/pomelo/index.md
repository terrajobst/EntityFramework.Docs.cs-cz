---
title: "Zprostředkovatel databáze MySQL pomelo – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="7e397-102">Pomelo EF základní zprostředkovatel databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="7e397-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="7e397-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s MySQL.</span><span class="sxs-lookup"><span data-stu-id="7e397-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="7e397-104">Zprostředkovatel se udržuje v rámci [Pomelo Foundation projektu](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="7e397-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="7e397-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7e397-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="7e397-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="7e397-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="7e397-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="7e397-107">Install</span></span>

<span data-ttu-id="7e397-108">Nainstalujte [balíček Pomelo.EntityFrameworkCore.MySql NuGet](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="7e397-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="7e397-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7e397-109">Get Started</span></span>

<span data-ttu-id="7e397-110">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="7e397-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="7e397-111">Získávání dokumentaci Začínáme</span><span class="sxs-lookup"><span data-stu-id="7e397-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="7e397-112">Yuuko Blog ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="7e397-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="7e397-113">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="7e397-113">Supported Database Engines</span></span>

* <span data-ttu-id="7e397-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="7e397-114">MySQL</span></span>
* <span data-ttu-id="7e397-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="7e397-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="7e397-116">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="7e397-116">Supported Platforms</span></span>

* <span data-ttu-id="7e397-117">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="7e397-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="7e397-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e397-118">.NET Core</span></span>

* <span data-ttu-id="7e397-119">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="7e397-119">Mono (4.2.0 onwards)</span></span>
