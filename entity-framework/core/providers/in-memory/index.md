---
title: "Zprostředkovatel databáze InMemory – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a>Zprostředkovatel EF základní databáze v paměti

Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti. To je užitečné v případě testování kódu, který používá Entity Framework Core. Zprostředkovatel se udržuje v rámci [EntityFramework Githubu projektu](https://github.com/aspnet/EntityFramework).

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
