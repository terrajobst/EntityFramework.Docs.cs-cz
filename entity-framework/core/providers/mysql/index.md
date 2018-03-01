---
title: "Databáze MySQL zprostředkovatel – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="448e5-102">Zprostředkovatel databáze základní EF MySQL</span><span class="sxs-lookup"><span data-stu-id="448e5-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="448e5-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s MySQL.</span><span class="sxs-lookup"><span data-stu-id="448e5-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="448e5-104">Zprostředkovatel se udržuje v rámci [MySQL projektu](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="448e5-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="448e5-105">Tento zprostředkovatel je předběžné verze.</span><span class="sxs-lookup"><span data-stu-id="448e5-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="448e5-106">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="448e5-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="448e5-107">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="448e5-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="448e5-108">Instalace</span><span class="sxs-lookup"><span data-stu-id="448e5-108">Install</span></span>

<span data-ttu-id="448e5-109">Nainstalujte [balíček MySql.Data.EntityFrameworkCore NuGet](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="448e5-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="448e5-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="448e5-110">Get Started</span></span>

<span data-ttu-id="448e5-111">V tématu [počínaje MySQL EF základní zprostředkovatel a Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span><span class="sxs-lookup"><span data-stu-id="448e5-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="448e5-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="448e5-112">Supported Database Engines</span></span>

* <span data-ttu-id="448e5-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="448e5-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="448e5-114">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="448e5-114">Supported Platforms</span></span>

* <span data-ttu-id="448e5-115">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="448e5-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="448e5-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="448e5-116">.NET Core</span></span>

<span data-ttu-id="448e5-117">Nezapomeňte si přečíst v dokumentaci MySQL pro informace o kompatibilitě verze [sem](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) a [sem](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span><span class="sxs-lookup"><span data-stu-id="448e5-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
