---
title: "Zprostředkovatel databáze MyCat pomelo – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 02/27/2017
ms.assetid: ad798f02-a7a4-45c1-b0a9-8e92f5dc6ff0
ms.technology: entity-framework-core
uid: core/providers/my-cat/index
ms.openlocfilehash: e13da3ab47e56d1b869e445f2398eda6ea84a79f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="pomelo-mycat-ef-core-database-provider"></a><span data-ttu-id="13084-102">Zprostředkovatel databáze pomelo MyCat EF jádra</span><span class="sxs-lookup"><span data-stu-id="13084-102">Pomelo MyCat EF Core Database Provider</span></span>

<span data-ttu-id="13084-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s [MyCat](https://github.com/MyCATApache/Mycat-Server).</span><span class="sxs-lookup"><span data-stu-id="13084-103">This database provider allows Entity Framework Core to be used with [MyCat](https://github.com/MyCATApache/Mycat-Server).</span></span> <span data-ttu-id="13084-104">Zprostředkovatel se udržuje v rámci [Pomelo Foundation projektu](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span><span class="sxs-lookup"><span data-stu-id="13084-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span></span>

> [!NOTE]  
> <span data-ttu-id="13084-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="13084-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="13084-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="13084-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="13084-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="13084-107">Install</span></span>

<span data-ttu-id="13084-108">Stažení a spuštění [nejnovější verzi z webu projektu](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span><span class="sxs-lookup"><span data-stu-id="13084-108">Download and run [the latest release from the project site](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span></span>

## <a name="get-started"></a><span data-ttu-id="13084-109">Začínáme</span><span class="sxs-lookup"><span data-stu-id="13084-109">Get Started</span></span>

<span data-ttu-id="13084-110">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="13084-110">The following resources will help you get started with this provider.</span></span>
 * [<span data-ttu-id="13084-111">Postup instalace</span><span class="sxs-lookup"><span data-stu-id="13084-111">Setup steps</span></span>](https://github.com/aspnet/EntityFramework.Docs/issues/252)
 * [<span data-ttu-id="13084-112">Úvodní video</span><span class="sxs-lookup"><span data-stu-id="13084-112">Getting started video</span></span>](https://www.youtube.com/watch?v=q0CXfFNtMZo)

## <a name="supported-database-engines"></a><span data-ttu-id="13084-113">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="13084-113">Supported Database Engines</span></span>

* <span data-ttu-id="13084-114">MyCat</span><span class="sxs-lookup"><span data-stu-id="13084-114">MyCat</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="13084-115">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="13084-115">Supported Platforms</span></span>

* <span data-ttu-id="13084-116">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="13084-116">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="13084-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="13084-117">.NET Core</span></span>
* <span data-ttu-id="13084-118">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="13084-118">Mono (4.2.0 onwards)</span></span>
