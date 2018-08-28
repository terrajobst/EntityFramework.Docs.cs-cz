---
title: Poskytovatel databáze Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995666"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Poskytovatel databáze Microsoft SQL Server EF Core

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s Microsoft SQL Server (včetně SQL Azure). Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Začínáme

Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.
* [Začínáme v rozhraní .NET Framework (konzola, WinForms, WPF atd.)](../../get-started/full-dotnet/index.md)

* [Začínáme v ASP.NET Core](../../get-started/aspnetcore/index.md)

* [UnicornStore ukázkové aplikace](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Podporované databázové stroje

* Microsoft SQL Server (2008 a novější)

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (verze 4.5.1)

* .NET Core

* Mono (4.2.0 a vyšší)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
