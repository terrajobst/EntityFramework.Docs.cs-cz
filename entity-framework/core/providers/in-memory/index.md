---
title: Zprostředkovatel databáze InMemory – jádro EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417219"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="1c5f3-102">Zprostředkovatel základní databáze EF Core In-Memory</span><span class="sxs-lookup"><span data-stu-id="1c5f3-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="1c5f3-103">Tento poskytovatel databáze umožňuje entity Framework Core pro použití s databází v paměti.</span><span class="sxs-lookup"><span data-stu-id="1c5f3-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="1c5f3-104">To může být užitečné pro testování, i když zprostředkovatel SQLite v režimu v paměti může být vhodnější nahrazení testu pro relační databáze.</span><span class="sxs-lookup"><span data-stu-id="1c5f3-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="1c5f3-105">Zprostředkovatel je udržován jako součást [základního projektu entity frameworku](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="1c5f3-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="1c5f3-106">Instalace</span><span class="sxs-lookup"><span data-stu-id="1c5f3-106">Install</span></span>

<span data-ttu-id="1c5f3-107">Nainstalujte [balíček Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="1c5f3-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="1c5f3-108">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="1c5f3-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="1c5f3-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c5f3-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="1c5f3-110">Začínáme</span><span class="sxs-lookup"><span data-stu-id="1c5f3-110">Get Started</span></span>

<span data-ttu-id="1c5f3-111">Následující prostředky vám pomohou začít s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="1c5f3-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="1c5f3-112">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="1c5f3-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="1c5f3-113">Testy ukázkových aplikací UnicornStore</span><span class="sxs-lookup"><span data-stu-id="1c5f3-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="1c5f3-114">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="1c5f3-114">Supported Database Engines</span></span>

<span data-ttu-id="1c5f3-115">Databáze in-process paměti (určená pouze pro testovací účely)</span><span class="sxs-lookup"><span data-stu-id="1c5f3-115">In-process memory database (designed for testing purposes only)</span></span>
