---
title: "Zprostředkovatel databáze SQLite – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a>Zprostředkovatel SQLite EF základní databáze

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s SQLite. Zprostředkovatel se udržuje v rámci [EntityFramework Githubu projektu](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Začínáme

Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.
* [Místní SQLite na UWP](../../get-started/uwp/getting-started.md)

* [Aplikace .NET core k nové databázi SQLite](../../get-started/netcore/new-db-sqlite.md)

* [Unicorn Clicker ukázkové aplikace](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Unicorn balírna ukázkové aplikace](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Podporované databázové stroje

* SQLite (3.7 a vyšší)

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (4.5.1 a vyšší)

* .NET Core

* Mono (4.2.0 a vyšší)

* Univerzální platforma pro Windows

## <a name="limitations"></a>Omezení

V tématu [SQLite omezení](limitations.md) pro některé důležité omezení poskytovatele SQLite.
