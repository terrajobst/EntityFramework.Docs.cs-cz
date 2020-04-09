---
title: Zprostředkovatel databáze SQLite – ef jádro
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417777"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="61c7c-102">Základní poskytovatel databáze SQLite EF</span><span class="sxs-lookup"><span data-stu-id="61c7c-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="61c7c-103">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s SQLite.</span><span class="sxs-lookup"><span data-stu-id="61c7c-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="61c7c-104">Zprostředkovatel je udržován jako součást [projektu Core entity frameworku](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="61c7c-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="61c7c-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="61c7c-105">Install</span></span>

<span data-ttu-id="61c7c-106">Nainstalujte [balíček Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="61c7c-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="61c7c-107">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="61c7c-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="61c7c-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61c7c-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="61c7c-109">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="61c7c-109">Supported Database Engines</span></span>

* <span data-ttu-id="61c7c-110">SQLite (3.7 a dále)</span><span class="sxs-lookup"><span data-stu-id="61c7c-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="61c7c-111">Omezení</span><span class="sxs-lookup"><span data-stu-id="61c7c-111">Limitations</span></span>

<span data-ttu-id="61c7c-112">Viz [Omezení SQLite](limitations.md) pro některá důležitá omezení poskytovatele SQLite.</span><span class="sxs-lookup"><span data-stu-id="61c7c-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
