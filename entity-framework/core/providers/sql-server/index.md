---
title: "Zprostředkovatel databáze Microsoft SQL Server – základní EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="a0de9-102">Zprostředkovatel databáze Microsoft SQL Server EF jádra</span><span class="sxs-lookup"><span data-stu-id="a0de9-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="a0de9-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s Microsoft SQL Server (včetně SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="a0de9-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="a0de9-104">Zprostředkovatel se udržuje v rámci [EntityFramework Githubu projektu](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="a0de9-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="a0de9-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="a0de9-105">Install</span></span>

<span data-ttu-id="a0de9-106">Nainstalujte [balíček Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="a0de9-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="a0de9-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="a0de9-107">Get Started</span></span>

<span data-ttu-id="a0de9-108">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="a0de9-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="a0de9-109">Začínáme v rozhraní .NET Framework (konzole, WinForms, WPF atd.)</span><span class="sxs-lookup"><span data-stu-id="a0de9-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="a0de9-110">Začínáme v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0de9-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="a0de9-111">UnicornStore ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="a0de9-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="a0de9-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="a0de9-112">Supported Database Engines</span></span>

* <span data-ttu-id="a0de9-113">Microsoft SQL Server (2008 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="a0de9-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a0de9-114">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="a0de9-114">Supported Platforms</span></span>

* <span data-ttu-id="a0de9-115">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="a0de9-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="a0de9-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0de9-116">.NET Core</span></span>

* <span data-ttu-id="a0de9-117">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="a0de9-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
