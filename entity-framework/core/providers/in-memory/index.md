---
title: Poskytovatel databáze InMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993317"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="f3a0c-102">Poskytovatel databáze v paměti EF Core</span><span class="sxs-lookup"><span data-stu-id="f3a0c-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="f3a0c-103">Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s databází v paměti.</span><span class="sxs-lookup"><span data-stu-id="f3a0c-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="f3a0c-104">To může být užitečné pro testování, i když poskytovatele SQLite v režimu in-memory může být vhodnější nahrazení testů pro relační databáze.</span><span class="sxs-lookup"><span data-stu-id="f3a0c-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="f3a0c-105">Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="f3a0c-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="f3a0c-106">Instalace</span><span class="sxs-lookup"><span data-stu-id="f3a0c-106">Install</span></span>

<span data-ttu-id="f3a0c-107">Nainstalujte [balíček Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="f3a0c-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="f3a0c-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f3a0c-108">Get Started</span></span>

<span data-ttu-id="f3a0c-109">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="f3a0c-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="f3a0c-110">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="f3a0c-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="f3a0c-111">Testy UnicornStore ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="f3a0c-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="f3a0c-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="f3a0c-112">Supported Database Engines</span></span>

* <span data-ttu-id="f3a0c-113">Integrované databáze v paměti (určená pouze pro účely testování)</span><span class="sxs-lookup"><span data-stu-id="f3a0c-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="f3a0c-114">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="f3a0c-114">Supported Platforms</span></span>

* <span data-ttu-id="f3a0c-115">Rozhraní .NET framework (verze 4.5.1)</span><span class="sxs-lookup"><span data-stu-id="f3a0c-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="f3a0c-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3a0c-116">.NET Core</span></span>

* <span data-ttu-id="f3a0c-117">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="f3a0c-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="f3a0c-118">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="f3a0c-118">Universal Windows Platform</span></span>
