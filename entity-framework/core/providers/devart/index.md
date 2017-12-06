---
title: "Zprostředkovatelé Devart databáze - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aad70a75-d04d-4d63-a489-7f9062a3c73d
ms.technology: entity-framework-core
uid: core/providers/devart/index
ms.openlocfilehash: 04de917b3327a6f544292781bca42a1170c199ad
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="devart-ef-core-database-providers"></a><span data-ttu-id="0208f-102">Devart EF základní databáze zprostředkovatelů</span><span class="sxs-lookup"><span data-stu-id="0208f-102">Devart EF Core Database Providers</span></span>

<span data-ttu-id="0208f-103">Devart je zapisovač zprostředkovatele třetí strany, který nabízí zprostředkovatelé databáze pro řadu různých databází.</span><span class="sxs-lookup"><span data-stu-id="0208f-103">Devart is a third party provider writer that offers database providers for a wide range of databases.</span></span> <span data-ttu-id="0208f-104">Další informace v [devart.com/dotconnect](https://www.devart.com/dotconnect/).</span><span class="sxs-lookup"><span data-stu-id="0208f-104">Find out more at [devart.com/dotconnect](https://www.devart.com/dotconnect/).</span></span>

> [!NOTE]  
> <span data-ttu-id="0208f-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0208f-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="0208f-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="0208f-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="paid-versions-only"></a><span data-ttu-id="0208f-107">Pouze placené verze</span><span class="sxs-lookup"><span data-stu-id="0208f-107">Paid Versions Only</span></span>

<span data-ttu-id="0208f-108">Devart dotConnect je zprostředkovatele komerční třetí strany.</span><span class="sxs-lookup"><span data-stu-id="0208f-108">Devart dotConnect is a commercial third party provider.</span></span> <span data-ttu-id="0208f-109">Entity Framework podpora je k dispozici v placené verzích dotConnect pouze.</span><span class="sxs-lookup"><span data-stu-id="0208f-109">Entity Framework support is only available in paid versions of dotConnect.</span></span>

## <a name="install"></a><span data-ttu-id="0208f-110">Instalace</span><span class="sxs-lookup"><span data-stu-id="0208f-110">Install</span></span>

<span data-ttu-id="0208f-111">Najdete v článku [Devart dotConnect dokumentace](https://www.devart.com/dotconnect/) pokyny k instalaci.</span><span class="sxs-lookup"><span data-stu-id="0208f-111">See the [Devart dotConnect documentation](https://www.devart.com/dotconnect/) for installation instructions.</span></span>

## <a name="get-started"></a><span data-ttu-id="0208f-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="0208f-112">Get Started</span></span>

<span data-ttu-id="0208f-113">Najdete v článku [Devart dotConnect dokumentaci k rozhraní Entity Framework](https://www.devart.com/dotconnect/entityframework.html) a [článek na blogu o Entity Framework Core 1 podpoře](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="0208f-113">See the [Devart dotConnect Entity Framework documentation](https://www.devart.com/dotconnect/entityframework.html) and [blog article about Entity Framework Core 1 Support](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="0208f-114">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="0208f-114">Supported Database Engines</span></span>

* <span data-ttu-id="0208f-115">MySQL</span><span class="sxs-lookup"><span data-stu-id="0208f-115">MySQL</span></span>

* <span data-ttu-id="0208f-116">Oracle</span><span class="sxs-lookup"><span data-stu-id="0208f-116">Oracle</span></span>

* <span data-ttu-id="0208f-117">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0208f-117">PostgreSQL</span></span>

* <span data-ttu-id="0208f-118">SQLite</span><span class="sxs-lookup"><span data-stu-id="0208f-118">SQLite</span></span>

* <span data-ttu-id="0208f-119">DB2</span><span class="sxs-lookup"><span data-stu-id="0208f-119">DB2</span></span>

* [<span data-ttu-id="0208f-120">Cloudové aplikace</span><span class="sxs-lookup"><span data-stu-id="0208f-120">Cloud apps</span></span>](https://www.devart.com/dotconnect/#cloud)

## <a name="supported-platforms"></a><span data-ttu-id="0208f-121">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="0208f-121">Supported Platforms</span></span>

* <span data-ttu-id="0208f-122">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="0208f-122">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="0208f-123">.NET core 1.0 a vyšší (zprostředkovatele Oracle, MySQL, PostgreSQL, SQLite)</span><span class="sxs-lookup"><span data-stu-id="0208f-123">.NET Core 1.0 and higher (providers for Oracle, MySQL, PostgreSQL, SQLite)</span></span>
