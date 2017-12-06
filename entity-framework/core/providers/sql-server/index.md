---
title: "Zprostředkovatel databáze Microsoft SQL Server – základní EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Zprostředkovatel databáze Microsoft SQL Server EF jádra

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s Microsoft SQL Server (včetně SQL Azure). Zprostředkovatel se udržuje v rámci [EntityFramework Githubu projektu](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Začínáme

Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.
* [Začínáme v rozhraní .NET Framework (konzole, WinForms, WPF atd.)](../../get-started/full-dotnet/index.md)

* [Začínáme v ASP.NET Core](../../get-started/aspnetcore/index.md)

* [UnicornStore ukázkové aplikace](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Podporované databázové stroje

* Microsoft SQL Server (2008 a vyšší)

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (4.5.1 a vyšší)

* .NET Core

* Mono (4.2.0 a vyšší)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
