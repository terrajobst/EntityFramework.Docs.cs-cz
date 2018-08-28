---
title: Poskytovatel databáze InMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993317"
---
# <a name="ef-core-in-memory-database-provider"></a>Poskytovatel databáze v paměti EF Core

Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s databází v paměti. To může být užitečné pro testování, i když poskytovatele SQLite v režimu in-memory může být vhodnější nahrazení testů pro relační databáze. Zprostředkovatel se udržuje v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).

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

* Integrované databáze v paměti (určená pouze pro účely testování)

## <a name="supported-platforms"></a>Podporované platformy

* Rozhraní .NET framework (verze 4.5.1)

* .NET Core

* Mono (4.2.0 a vyšší)

* Univerzální platforma pro Windows
