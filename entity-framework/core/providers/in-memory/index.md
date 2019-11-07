---
title: Zprostředkovatel databáze inMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 4b35e8c4b29a951449d4a26c6e274eb3015069bc
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656027"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="10930-102">EF Core poskytovatel databáze v paměti</span><span class="sxs-lookup"><span data-stu-id="10930-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="10930-103">Tento poskytovatel databáze umožňuje použití Entity Framework Core s databází v paměti.</span><span class="sxs-lookup"><span data-stu-id="10930-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="10930-104">To může být užitečné pro testování, přestože poskytovatel SQLite v režimu v paměti může být vhodnějším náhradním testem u relačních databází.</span><span class="sxs-lookup"><span data-stu-id="10930-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="10930-105">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="10930-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="10930-106">Instalace</span><span class="sxs-lookup"><span data-stu-id="10930-106">Install</span></span>

<span data-ttu-id="10930-107">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="10930-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="10930-108">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="10930-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="10930-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10930-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="10930-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="10930-110">Get Started</span></span>

<span data-ttu-id="10930-111">Následující zdroje vám pomůžou začít s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="10930-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="10930-112">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="10930-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="10930-113">Testy vzorových aplikací UnicornStore</span><span class="sxs-lookup"><span data-stu-id="10930-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="10930-114">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="10930-114">Supported Database Engines</span></span>

<span data-ttu-id="10930-115">Databáze v procesu v paměti (určená pouze pro účely testování)</span><span class="sxs-lookup"><span data-stu-id="10930-115">In-process memory database (designed for testing purposes only)</span></span>
