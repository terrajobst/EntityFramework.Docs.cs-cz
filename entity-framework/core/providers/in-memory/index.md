---
title: Zprostředkovatel databáze InMemory – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678983"
---
# <a name="ef-core-in-memory-database-provider"></a>Zprostředkovatel EF základní databáze v paměti

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti. To může být užitečné pro testování, i když je zprostředkovatel SQLite v režimu v paměti může být vhodnější testovací náhradou za relačních databází. Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Začínáme

Následující prostředky vám pomůže začít pracovat s tímto poskytovatelem.
* [Testování s InMemory](../../miscellaneous/testing/in-memory.md)

* [Testy UnicornStore ukázkové aplikace](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Podporované databázové stroje

* Integrovanou databází v paměti (určena pouze pro testovací účely)

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (4.5.1 a vyšší)

* .NET Core

* Mono (4.2.0 a vyšší)

* Univerzální platforma pro Windows
