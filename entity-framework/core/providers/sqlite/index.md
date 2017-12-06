---
title: "Zprostředkovatel databáze SQLite – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="eb977-102">Zprostředkovatel SQLite EF základní databáze</span><span class="sxs-lookup"><span data-stu-id="eb977-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="eb977-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s SQLite.</span><span class="sxs-lookup"><span data-stu-id="eb977-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="eb977-104">Zprostředkovatel se udržuje v rámci [EntityFramework Githubu projektu](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="eb977-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="eb977-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="eb977-105">Install</span></span>

<span data-ttu-id="eb977-106">Nainstalujte [balíček Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="eb977-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="eb977-107">Začínáme</span><span class="sxs-lookup"><span data-stu-id="eb977-107">Get Started</span></span>

<span data-ttu-id="eb977-108">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="eb977-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="eb977-109">Místní SQLite na UWP</span><span class="sxs-lookup"><span data-stu-id="eb977-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="eb977-110">Aplikace .NET core k nové databázi SQLite</span><span class="sxs-lookup"><span data-stu-id="eb977-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="eb977-111">Unicorn Clicker ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="eb977-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="eb977-112">Unicorn balírna ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="eb977-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="eb977-113">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="eb977-113">Supported Database Engines</span></span>

* <span data-ttu-id="eb977-114">SQLite (3.7 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="eb977-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="eb977-115">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="eb977-115">Supported Platforms</span></span>

* <span data-ttu-id="eb977-116">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="eb977-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="eb977-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="eb977-117">.NET Core</span></span>

* <span data-ttu-id="eb977-118">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="eb977-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="eb977-119">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="eb977-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="eb977-120">Omezení</span><span class="sxs-lookup"><span data-stu-id="eb977-120">Limitations</span></span>

<span data-ttu-id="eb977-121">V tématu [SQLite omezení](limitations.md) pro některé důležité omezení poskytovatele SQLite.</span><span class="sxs-lookup"><span data-stu-id="eb977-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
