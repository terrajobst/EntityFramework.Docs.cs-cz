---
title: Zprostředkovatel databáze inMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502120"
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core poskytovatel databáze v paměti

Tento poskytovatel databáze umožňuje použití Entity Framework Core s databází v paměti. To může být užitečné pro testování, přestože poskytovatel SQLite v režimu v paměti může být vhodnějším náhradním testem u relačních databází. Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalace produktu

Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

### <a name="net-core-clitabdotnet-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Začínáme

Následující zdroje vám pomůžou začít s tímto poskytovatelem.

* [Testování s InMemory](../../miscellaneous/testing/in-memory.md)
* [Testy vzorových aplikací UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Podporované databázové stroje

Databáze v procesu v paměti (určená pouze pro účely testování)
