---
title: Zprostředkovatel databáze InMemory – jádro EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417219"
---
# <a name="ef-core-in-memory-database-provider"></a>Zprostředkovatel základní databáze EF Core In-Memory

Tento poskytovatel databáze umožňuje entity Framework Core pro použití s databází v paměti. To může být užitečné pro testování, i když zprostředkovatel SQLite v režimu v paměti může být vhodnější nahrazení testu pro relační databáze. Zprostředkovatel je udržován jako součást [základního projektu entity frameworku](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Začínáme

Následující prostředky vám pomohou začít s tímto poskytovatelem.

* [Testování s InMemory](../../miscellaneous/testing/in-memory.md)
* [Testy ukázkových aplikací UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Podporované databázové stroje

Databáze in-process paměti (určená pouze pro testovací účely)
