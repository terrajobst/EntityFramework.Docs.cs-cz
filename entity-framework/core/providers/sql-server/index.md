---
title: Zprostředkovatel databáze Microsoft SQL Server – EF Core
description: Dokumentace pro poskytovatele databáze, která umožňuje použití Entity Framework Core s Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: 18a69789ff4ae013c1d60bb6d34ca5c27ee285c2
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824781"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="14430-103">Poskytovatel databáze Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="14430-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="14430-104">Tento poskytovatel databáze umožňuje použití Entity Framework Core s Microsoft SQL Server (včetně Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="14430-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="14430-105">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="14430-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="14430-106">Instalace produktu</span><span class="sxs-lookup"><span data-stu-id="14430-106">Install</span></span>

<span data-ttu-id="14430-107">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="14430-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="14430-108">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="14430-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="14430-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14430-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="14430-110">Od verze 3.0.0 poskytovatel odkazuje na Microsoft. data. SqlClient (předchozí verze závisejí na System. data. SqlClient).</span><span class="sxs-lookup"><span data-stu-id="14430-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="14430-111">Pokud váš projekt používá přímou závislost na SqlClient, ujistěte se, že odkazuje na balíček Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="14430-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="14430-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="14430-112">Supported Database Engines</span></span>

* <span data-ttu-id="14430-113">Microsoft SQL Server (2012 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="14430-113">Microsoft SQL Server (2012 onwards)</span></span>
