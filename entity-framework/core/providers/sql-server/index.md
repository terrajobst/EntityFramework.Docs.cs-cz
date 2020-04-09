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
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Zprostředkovatel základní databáze SERVERU Microsoft SQL Server EF

Tento poskytovatel databáze umožňuje entity Framework Core pro použití s Microsoft SQL Server (včetně Azure SQL Database). Zprostředkovatel je udržován jako součást [základního projektu entity frameworku](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Od verze 3.0.0 poskytovatel odkazuje na microsoft.Data.SqlClient (předchozí verze závisely na systému System.Data.SqlClient). Pokud váš projekt trvá přímou závislost na SqlClient, ujistěte se, že odkazuje na Microsoft.Data.SqlClient balíček.

## <a name="supported-database-engines"></a>Podporované databázové stroje

* Microsoft SQL Server (2012 a dále)
