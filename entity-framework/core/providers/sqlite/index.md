---
title: "Zprostředkovatel databáze SQLite – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="80652-102">Zprostředkovatel SQLite EF základní databáze</span><span class="sxs-lookup"><span data-stu-id="80652-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="80652-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s SQLite.</span><span class="sxs-lookup"><span data-stu-id="80652-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="80652-104">Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="80652-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="80652-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="80652-105">Install</span></span>

<span data-ttu-id="80652-106">Nainstalujte [balíček Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="80652-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="80652-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="80652-107">Get Started</span></span>

<span data-ttu-id="80652-108">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="80652-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="80652-109">Místní SQLite na UWP</span><span class="sxs-lookup"><span data-stu-id="80652-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="80652-110">Aplikace .NET core k nové databázi SQLite</span><span class="sxs-lookup"><span data-stu-id="80652-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="80652-111">Unicorn Clicker ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="80652-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="80652-112">Unicorn balírna ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="80652-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="80652-113">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="80652-113">Supported Database Engines</span></span>

* <span data-ttu-id="80652-114">SQLite (3.7 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="80652-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="80652-115">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="80652-115">Supported Platforms</span></span>

* <span data-ttu-id="80652-116">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="80652-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="80652-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="80652-117">.NET Core</span></span>

* <span data-ttu-id="80652-118">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="80652-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="80652-119">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="80652-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="80652-120">Omezení</span><span class="sxs-lookup"><span data-stu-id="80652-120">Limitations</span></span>

<span data-ttu-id="80652-121">V tématu [SQLite omezení](limitations.md) pro některé důležité omezení poskytovatele SQLite.</span><span class="sxs-lookup"><span data-stu-id="80652-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
