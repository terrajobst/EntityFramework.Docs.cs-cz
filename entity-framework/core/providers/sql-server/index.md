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
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Poskytovatel databáze Microsoft SQL Server EF Core

Tento poskytovatel databáze umožňuje použití Entity Framework Core s Microsoft SQL Server (včetně Azure SQL Database). Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace produktu

Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

## <a name="net-core-clitabdotnet-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Od verze 3.0.0 poskytovatel odkazuje na Microsoft. data. SqlClient (předchozí verze závisejí na System. data. SqlClient). Pokud váš projekt používá přímou závislost na SqlClient, ujistěte se, že odkazuje na balíček Microsoft. data. SqlClient.

## <a name="supported-database-engines"></a>Podporované databázové stroje

* Microsoft SQL Server (2012 a vyšší)
