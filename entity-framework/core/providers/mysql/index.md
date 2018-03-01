---
title: "Databáze MySQL zprostředkovatel – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a>Zprostředkovatel databáze základní EF MySQL

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s MySQL. Zprostředkovatel se udržuje v rámci [MySQL projektu](http://dev.mysql.com).

> [!WARNING]  
> Tento zprostředkovatel je předběžné verze.

> [!NOTE]  
> Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core. Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.

## <a name="install"></a>Instalace

Nainstalujte [balíček MySql.Data.EntityFrameworkCore NuGet](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>Začínáme

V tématu [počínaje MySQL EF základní zprostředkovatel a Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).

## <a name="supported-database-engines"></a>Podporované databázové stroje

* MySQL

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (4.5.1 a vyšší)

* .NET Core

Nezapomeňte si přečíst v dokumentaci MySQL pro informace o kompatibilitě verze [sem](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) a [sem](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)
