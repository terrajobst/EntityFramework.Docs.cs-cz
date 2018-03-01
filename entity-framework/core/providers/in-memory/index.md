---
title: "Zprostředkovatel databáze InMemory – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="e9777-102">Zprostředkovatel EF základní databáze v paměti</span><span class="sxs-lookup"><span data-stu-id="e9777-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="e9777-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="e9777-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="e9777-104">To může být užitečné pro testování, i když je zprostředkovatel SQLite v režimu v paměti může být vhodnější testovací náhradou za relačních databází.</span><span class="sxs-lookup"><span data-stu-id="e9777-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="e9777-105">Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="e9777-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="e9777-106">Instalace</span><span class="sxs-lookup"><span data-stu-id="e9777-106">Install</span></span>

<span data-ttu-id="e9777-107">Nainstalujte [balíček Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="e9777-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="e9777-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="e9777-108">Get Started</span></span>

<span data-ttu-id="e9777-109">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="e9777-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="e9777-110">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="e9777-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="e9777-111">Testy UnicornStore ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="e9777-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="e9777-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="e9777-112">Supported Database Engines</span></span>

* <span data-ttu-id="e9777-113">Integrovanou databází v paměti (určena pouze pro testovací účely)</span><span class="sxs-lookup"><span data-stu-id="e9777-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="e9777-114">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="e9777-114">Supported Platforms</span></span>

* <span data-ttu-id="e9777-115">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="e9777-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="e9777-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9777-116">.NET Core</span></span>

* <span data-ttu-id="e9777-117">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="e9777-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="e9777-118">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="e9777-118">Universal Windows Platform</span></span>
