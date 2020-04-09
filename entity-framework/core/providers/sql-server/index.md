---
title: Zprostředkovatel databáze serveru Microsoft SQL Server – ef core
description: Dokumentace pro poskytovatele databáze, která umožňuje použití entity Framework Core se serverem Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417794"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="7cc3d-103">Zprostředkovatel základní databáze SERVERU Microsoft SQL Server EF</span><span class="sxs-lookup"><span data-stu-id="7cc3d-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="7cc3d-104">Tento poskytovatel databáze umožňuje entity Framework Core pro použití s Microsoft SQL Server (včetně Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="7cc3d-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="7cc3d-105">Zprostředkovatel je udržován jako součást [základního projektu entity frameworku](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="7cc3d-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="7cc3d-106">Instalace</span><span class="sxs-lookup"><span data-stu-id="7cc3d-106">Install</span></span>

<span data-ttu-id="7cc3d-107">Nainstalujte [balíček Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="7cc3d-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="7cc3d-108">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="7cc3d-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[<span data-ttu-id="7cc3d-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7cc3d-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="7cc3d-110">Od verze 3.0.0 poskytovatel odkazuje na microsoft.Data.SqlClient (předchozí verze závisely na systému System.Data.SqlClient).</span><span class="sxs-lookup"><span data-stu-id="7cc3d-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="7cc3d-111">Pokud váš projekt trvá přímou závislost na SqlClient, ujistěte se, že odkazuje na Microsoft.Data.SqlClient balíček.</span><span class="sxs-lookup"><span data-stu-id="7cc3d-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="7cc3d-112">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="7cc3d-112">Supported Database Engines</span></span>

* <span data-ttu-id="7cc3d-113">Microsoft SQL Server (2012 a dále)</span><span class="sxs-lookup"><span data-stu-id="7cc3d-113">Microsoft SQL Server (2012 onwards)</span></span>
