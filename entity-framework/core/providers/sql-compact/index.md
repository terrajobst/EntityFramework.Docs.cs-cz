---
title: "Zprostředkovatel databáze Microsoft SQL Server Compact Edition - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Zprostředkovatel Microsoft SQL Server Compact Edition EF základní databáze

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s SQL Server Compact Edition. Zprostředkovatel se udržuje v rámci [ErikEJ/EntityFramework.SqlServerCompact Githubu projektu](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).

> [!NOTE]  
> Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core. Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.

## <a name="install"></a>Instalace

Chcete-li pracovat s SQL Server Compact Edition 4.0, nainstalujte [balíček EntityFrameworkCore.SqlServerCompact40 NuGet](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

Chcete-li pracovat s SQL Server Compact Edition 3.5, nainstalujte [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>Začínáme

Najdete v článku [Začínáme dokumentaci na webu projektu](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)

## <a name="supported-database-engines"></a>Podporované databázové stroje

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (4.5.1 a vyšší)
