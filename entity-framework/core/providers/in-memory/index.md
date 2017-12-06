---
title: "Zprostředkovatel databáze InMemory – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="7a09d-102">Zprostředkovatel EF základní databáze v paměti</span><span class="sxs-lookup"><span data-stu-id="7a09d-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="7a09d-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="7a09d-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="7a09d-104">To je užitečné v případě testování kódu, který používá Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7a09d-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="7a09d-105">Zprostředkovatel se udržuje v rámci [EntityFramework Githubu projektu](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="7a09d-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="7a09d-106">Instalace</span><span class="sxs-lookup"><span data-stu-id="7a09d-106">Install</span></span>

<span data-ttu-id="7a09d-107">Nainstalujte [balíček Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="7a09d-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="7a09d-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="7a09d-108">Get Started</span></span>

<span data-ttu-id="7a09d-109">Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="7a09d-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="7a09d-110">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="7a09d-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="7a09d-111">Testy UnicornStore ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="7a09d-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="7a09d-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="7a09d-112">Supported Database Engines</span></span>

* <span data-ttu-id="7a09d-113">Integrovanou databází v paměti (určena pouze pro testovací účely)</span><span class="sxs-lookup"><span data-stu-id="7a09d-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="7a09d-114">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="7a09d-114">Supported Platforms</span></span>

* <span data-ttu-id="7a09d-115">Rozhraní .NET framework (4.5.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="7a09d-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="7a09d-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a09d-116">.NET Core</span></span>

* <span data-ttu-id="7a09d-117">Mono (4.2.0 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="7a09d-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="7a09d-118">Univerzální platforma pro Windows</span><span class="sxs-lookup"><span data-stu-id="7a09d-118">Universal Windows Platform</span></span>
