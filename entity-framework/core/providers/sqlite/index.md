---
title: Zprostředkovatel databáze SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417777"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="475f9-102">Zprostředkovatel databáze EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="475f9-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="475f9-103">Tento poskytovatel databáze umožňuje použití Entity Framework Core s SQLite.</span><span class="sxs-lookup"><span data-stu-id="475f9-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="475f9-104">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="475f9-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="475f9-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="475f9-105">Install</span></span>

<span data-ttu-id="475f9-106">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="475f9-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="475f9-107">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="475f9-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="475f9-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="475f9-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="475f9-109">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="475f9-109">Supported Database Engines</span></span>

* <span data-ttu-id="475f9-110">SQLite (3,7 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="475f9-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="475f9-111">Omezení</span><span class="sxs-lookup"><span data-stu-id="475f9-111">Limitations</span></span>

<span data-ttu-id="475f9-112">V některých důležitých omezeních zprostředkovatele SQLite si můžete prohlédnout [omezení SQLite](limitations.md) .</span><span class="sxs-lookup"><span data-stu-id="475f9-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
