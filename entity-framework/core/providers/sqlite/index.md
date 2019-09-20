---
title: Zprostředkovatel databáze SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e4cbdba46f901831892192a343db2920a5760042
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149271"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="2516d-102">Zprostředkovatel databáze EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="2516d-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="2516d-103">Tento poskytovatel databáze umožňuje použití Entity Framework Core s SQLite.</span><span class="sxs-lookup"><span data-stu-id="2516d-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="2516d-104">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2516d-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="2516d-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="2516d-105">Install</span></span>

<span data-ttu-id="2516d-106">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="2516d-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a><span data-ttu-id="2516d-107">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="2516d-107">Supported Database Engines</span></span>

* <span data-ttu-id="2516d-108">SQLite (3,7 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="2516d-108">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="2516d-109">Omezení</span><span class="sxs-lookup"><span data-stu-id="2516d-109">Limitations</span></span>

<span data-ttu-id="2516d-110">V některých důležitých omezeních zprostředkovatele SQLite si můžete prohlédnout [omezení SQLite](limitations.md) .</span><span class="sxs-lookup"><span data-stu-id="2516d-110">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
