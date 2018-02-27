---
title: "Zprostředkovatel pro databáze serveru (DB2) ze IBM dat – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>IBM dat serveru (DB2) EF základní databáze zprostředkovatelů

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s IBM dat serveru. Zprostředkovatel je možný díky IBM.

> [!NOTE]  
> Tento zprostředkovatel se neuchovávají v rámci projektu Entity Framework Core. Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.

## <a name="install"></a>Instalace

Chcete-li pracovat s IBM dat serveru v systému Windows, nainstalujte [IBM. Balíček EntityFrameworkCore NuGet](https://www.nuget.org/packages/IBM.EntityFrameworkCore).

``` powershell
Install-Package IBM.EntityFrameworkCore
```

Chcete-li pracovat s IBM dat serveru v systému Linux, nainstalujte [IBM. Balíček NuGet EntityFrameworkCore lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>Začínáme

[Začínáme s IBM poskytovatele .NET pro .NET Core](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>Podporované databázové stroje

* zOS
* LUW včetně IBM dashDB
* IBM I
* Informix

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (4.6 a vyšší)
* .NET Core
