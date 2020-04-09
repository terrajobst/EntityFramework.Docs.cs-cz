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
# <a name="sqlite-ef-core-database-provider"></a>Základní poskytovatel databáze SQLite EF

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s SQLite. Zprostředkovatel je udržován jako součást [projektu Core entity frameworku](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Podporované databázové stroje

* SQLite (3.7 a dále)

## <a name="limitations"></a>Omezení

Viz [Omezení SQLite](limitations.md) pro některá důležitá omezení poskytovatele SQLite.
