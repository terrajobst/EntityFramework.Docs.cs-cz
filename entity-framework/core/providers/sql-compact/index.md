---
title: "Zprostředkovatel databáze Microsoft SQL Server Compact Edition - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="54a64-102">Zprostředkovatel Microsoft SQL Server Compact Edition EF základní databáze</span><span class="sxs-lookup"><span data-stu-id="54a64-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="54a64-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="54a64-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="54a64-104">Zprostředkovatel se udržuje v rámci [ErikEJ/EntityFramework.SqlServerCompact Githubu projektu](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="54a64-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="54a64-105">Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="54a64-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="54a64-106">Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="54a64-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="54a64-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="54a64-107">Install</span></span>

<span data-ttu-id="54a64-108">Chcete-li pracovat s SQL Server Compact Edition 4.0, nainstalujte [balíček EntityFrameworkCore.SqlServerCompact40 NuGet](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span><span class="sxs-lookup"><span data-stu-id="54a64-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="54a64-109">Chcete-li pracovat s SQL Server Compact Edition 3.5, nainstalujte [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span><span class="sxs-lookup"><span data-stu-id="54a64-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="54a64-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="54a64-110">Get Started</span></span>

<span data-ttu-id="54a64-111">Najdete v článku [Začínáme dokumentaci na webu projektu](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span><span class="sxs-lookup"><span data-stu-id="54a64-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="54a64-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="54a64-112">Supported Database Engines</span></span>

* <span data-ttu-id="54a64-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="54a64-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="54a64-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="54a64-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="54a64-115">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="54a64-115">Supported Platforms</span></span>

* <span data-ttu-id="54a64-116">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="54a64-116">.NET Framework (4.5.1 onwards)</span></span>
