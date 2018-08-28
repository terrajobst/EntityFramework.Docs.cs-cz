---
title: Poskytovatel databáze SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993999"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="b6212-102">Poskytovatel databáze EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="b6212-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="b6212-103">Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s SQLite.</span><span class="sxs-lookup"><span data-stu-id="b6212-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="b6212-104">Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="b6212-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="b6212-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="b6212-105">Install</span></span>

<span data-ttu-id="b6212-106">Nainstalujte [balíček Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="b6212-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="b6212-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="b6212-107">Get Started</span></span>

<span data-ttu-id="b6212-108">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="b6212-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="b6212-109">Místní SQLite na UPW</span><span class="sxs-lookup"><span data-stu-id="b6212-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="b6212-110">Aplikaci .NET core pro novou databázi SQLite</span><span class="sxs-lookup"><span data-stu-id="b6212-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="b6212-111">Unicornu Clicker ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="b6212-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="b6212-112">Unicornu Packeru ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="b6212-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="b6212-113">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="b6212-113">Supported Database Engines</span></span>

* <span data-ttu-id="b6212-114">SQLite (3.7 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="b6212-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="b6212-115">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="b6212-115">Supported Platforms</span></span>

* <span data-ttu-id="b6212-116">Rozhraní .NET framework (verze 4.5.1)</span><span class="sxs-lookup"><span data-stu-id="b6212-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="b6212-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6212-117">.NET Core</span></span>

* <span data-ttu-id="b6212-118">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="b6212-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="b6212-119">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="b6212-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="b6212-120">Omezení</span><span class="sxs-lookup"><span data-stu-id="b6212-120">Limitations</span></span>

<span data-ttu-id="b6212-121">Zobrazit [omezení SQLite](limitations.md) pro důležitá omezení SQLite poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="b6212-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
