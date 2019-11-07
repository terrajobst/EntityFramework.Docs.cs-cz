---
title: Zprostředkovatel databáze Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: dd352b81da05fa8ea8970495f20947bd109edf65
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655898"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="7f035-102">Poskytovatel databáze Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="7f035-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="7f035-103">Tento poskytovatel databáze umožňuje použití Entity Framework Core s Microsoft SQL Server (včetně SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="7f035-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="7f035-104">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="7f035-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="7f035-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="7f035-105">Install</span></span>

<span data-ttu-id="7f035-106">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="7f035-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="7f035-107">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="7f035-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="7f035-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f035-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="7f035-109">Od verze 3.0.0 poskytovatel odkazuje na Microsoft. data. SqlClient (předchozí verze závisejí na System. data. SqlClient).</span><span class="sxs-lookup"><span data-stu-id="7f035-109">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="7f035-110">Pokud váš projekt používá přímou závislost na SqlClient, ujistěte se, že odkazuje na správný balíček.</span><span class="sxs-lookup"><span data-stu-id="7f035-110">If your project takes a direct dependency on SqlClient, make sure it references the correct package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="7f035-111">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="7f035-111">Supported Database Engines</span></span>

* <span data-ttu-id="7f035-112">Microsoft SQL Server (2012 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="7f035-112">Microsoft SQL Server (2012 onwards)</span></span>
