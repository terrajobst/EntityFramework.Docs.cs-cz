---
title: Poskytovatel databáze Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995666"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="f07a3-102">Poskytovatel databáze Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="f07a3-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="f07a3-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s Microsoft SQL Server (včetně SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="f07a3-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="f07a3-104">Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="f07a3-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="f07a3-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="f07a3-105">Install</span></span>

<span data-ttu-id="f07a3-106">Nainstalujte [balíček Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="f07a3-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="f07a3-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f07a3-107">Get Started</span></span>

<span data-ttu-id="f07a3-108">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="f07a3-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="f07a3-109">Začínáme v rozhraní .NET Framework (konzola, WinForms, WPF atd.)</span><span class="sxs-lookup"><span data-stu-id="f07a3-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="f07a3-110">Začínáme v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f07a3-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="f07a3-111">UnicornStore ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="f07a3-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="f07a3-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="f07a3-112">Supported Database Engines</span></span>

* <span data-ttu-id="f07a3-113">Microsoft SQL Server (2008 a novější)</span><span class="sxs-lookup"><span data-stu-id="f07a3-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="f07a3-114">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="f07a3-114">Supported Platforms</span></span>

* <span data-ttu-id="f07a3-115">Rozhraní .NET framework (verze 4.5.1)</span><span class="sxs-lookup"><span data-stu-id="f07a3-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="f07a3-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="f07a3-116">.NET Core</span></span>

* <span data-ttu-id="f07a3-117">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="f07a3-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
