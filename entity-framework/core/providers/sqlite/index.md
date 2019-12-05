---
title: Zprostředkovatel databáze SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 433dedbe1e0e11bf2fd83e259e321ece32b2c188
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824806"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="86c07-102">Zprostředkovatel databáze EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="86c07-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="86c07-103">Tento poskytovatel databáze umožňuje použití Entity Framework Core s SQLite.</span><span class="sxs-lookup"><span data-stu-id="86c07-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="86c07-104">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="86c07-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="86c07-105">Instalace produktu</span><span class="sxs-lookup"><span data-stu-id="86c07-105">Install</span></span>

<span data-ttu-id="86c07-106">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="86c07-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="86c07-107">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="86c07-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="86c07-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86c07-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="86c07-109">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="86c07-109">Supported Database Engines</span></span>

* <span data-ttu-id="86c07-110">SQLite (3,7 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="86c07-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="86c07-111">Omezení</span><span class="sxs-lookup"><span data-stu-id="86c07-111">Limitations</span></span>

<span data-ttu-id="86c07-112">V některých důležitých omezeních zprostředkovatele SQLite si můžete prohlédnout [omezení SQLite](limitations.md) .</span><span class="sxs-lookup"><span data-stu-id="86c07-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
