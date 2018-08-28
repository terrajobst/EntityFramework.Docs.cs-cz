---
title: Poskytovatel databáze SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993999"
---
# <a name="sqlite-ef-core-database-provider"></a>Poskytovatel databáze EF Core SQLite

Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s SQLite. Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Začínáme

Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.
* [Místní SQLite na UPW](../../get-started/uwp/getting-started.md)

* [Aplikaci .NET core pro novou databázi SQLite](../../get-started/netcore/new-db-sqlite.md)

* [Unicornu Clicker ukázkové aplikace](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Unicornu Packeru ukázkové aplikace](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Podporované databázové stroje

* SQLite (3.7 a vyšší)

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (verze 4.5.1)

* .NET Core

* Mono (4.2.0 a vyšší)

* Univerzální platforma pro Windows

## <a name="limitations"></a>Omezení

Zobrazit [omezení SQLite](limitations.md) pro důležitá omezení SQLite poskytovatele.
