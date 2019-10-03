---
title: Zprostředkovatel databáze Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: f0aa290e8c5166c278f8c9782c4304de5e91f26b
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813509"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Poskytovatel databáze Microsoft SQL Server EF Core

Tento poskytovatel databáze umožňuje použití Entity Framework Core s Microsoft SQL Server (včetně SQL Azure). Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace

Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

# <a name="net-core-clitabdotnet-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

## <a name="supported-database-engines"></a>Podporované databázové stroje

* Microsoft SQL Server (2012 a vyšší)
