---
title: Zprostředkovatel databáze inMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 28f5f262b41cbc1f196e41d75c8b88ca60e678fe
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149244"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="423de-102">EF Core poskytovatel databáze v paměti</span><span class="sxs-lookup"><span data-stu-id="423de-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="423de-103">Tento poskytovatel databáze umožňuje použití Entity Framework Core s databází v paměti.</span><span class="sxs-lookup"><span data-stu-id="423de-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="423de-104">To může být užitečné pro testování, přestože poskytovatel SQLite v režimu v paměti může být vhodnějším náhradním testem u relačních databází.</span><span class="sxs-lookup"><span data-stu-id="423de-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="423de-105">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="423de-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="423de-106">Instalace</span><span class="sxs-lookup"><span data-stu-id="423de-106">Install</span></span>

<span data-ttu-id="423de-107">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="423de-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="423de-108">Začínáme</span><span class="sxs-lookup"><span data-stu-id="423de-108">Get Started</span></span>

<span data-ttu-id="423de-109">Následující zdroje vám pomůžou začít s tímto poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="423de-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="423de-110">Testování s InMemory</span><span class="sxs-lookup"><span data-stu-id="423de-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="423de-111">Testy vzorových aplikací UnicornStore</span><span class="sxs-lookup"><span data-stu-id="423de-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="423de-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="423de-112">Supported Database Engines</span></span>

* <span data-ttu-id="423de-113">Integrovaná databáze v paměti (navržena pouze pro účely testování)</span><span class="sxs-lookup"><span data-stu-id="423de-113">Built-in in-memory database (designed for testing purposes only)</span></span>
