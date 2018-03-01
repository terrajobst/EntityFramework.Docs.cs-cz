---
title: "Zprostředkovatel pro databáze serveru (DB2) ze IBM dat – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="55c63-102">IBM dat serveru (DB2) EF základní databáze zprostředkovatelů</span><span class="sxs-lookup"><span data-stu-id="55c63-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="55c63-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s IBM dat serveru.</span><span class="sxs-lookup"><span data-stu-id="55c63-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="55c63-104">Zprostředkovatel je možný díky IBM.</span><span class="sxs-lookup"><span data-stu-id="55c63-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="55c63-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="55c63-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="55c63-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="55c63-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="55c63-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="55c63-107">Install</span></span>

<span data-ttu-id="55c63-108">Chcete-li pracovat s IBM dat serveru v systému Windows, nainstalujte [IBM. Balíček EntityFrameworkCore NuGet](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="55c63-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="55c63-109">Chcete-li pracovat s IBM dat serveru v systému Linux, nainstalujte [IBM. Balíček NuGet EntityFrameworkCore lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span><span class="sxs-lookup"><span data-stu-id="55c63-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="55c63-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="55c63-110">Get Started</span></span>

[<span data-ttu-id="55c63-111">Začínáme s IBM poskytovatele .NET pro .NET Core</span><span class="sxs-lookup"><span data-stu-id="55c63-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="55c63-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="55c63-112">Supported Database Engines</span></span>

* <span data-ttu-id="55c63-113">zOS</span><span class="sxs-lookup"><span data-stu-id="55c63-113">zOS</span></span>
* <span data-ttu-id="55c63-114">LUW včetně IBM dashDB</span><span class="sxs-lookup"><span data-stu-id="55c63-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="55c63-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="55c63-115">IBM I</span></span>
* <span data-ttu-id="55c63-116">Informix</span><span class="sxs-lookup"><span data-stu-id="55c63-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="55c63-117">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="55c63-117">Supported Platforms</span></span>

* <span data-ttu-id="55c63-118">Rozhraní .NET framework (4.6 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="55c63-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="55c63-119">.NET Core</span><span class="sxs-lookup"><span data-stu-id="55c63-119">.NET Core</span></span>
