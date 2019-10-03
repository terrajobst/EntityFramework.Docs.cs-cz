---
title: Indexy (relační databáze) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7bb74d0bfa6090b597eb988a46f00494e25f233e
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813636"
---
# <a name="indexes-relational-database"></a>Indexy (relační databáze)

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Index v relační databázi se mapuje na stejný koncept jako index v jádru Entity Framework.

## <a name="conventions"></a>Konvence

Podle konvence jsou indexy pojmenovány `IX_<type name>_<property name>`. U složených indexů `<property name>` se jako seznam názvů vlastností oddělí podtržítko.

## <a name="data-annotations"></a>Datové poznámky

Indexy nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Ke konfiguraci názvu indexu můžete použít rozhraní Fluent API.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Můžete také zadat filtr.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Při použití poskytovatele SQL Server EF Přidá filtr IS NOT NULL pro všechny sloupce s možnou hodnotou null, které jsou součástí jedinečného indexu. K přepsání této konvence můžete uvést `null` hodnotu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Zahrnout sloupce do indexů SQL Server

Pokud jsou všechny sloupce v dotazu zahrnuté do indexu jako klíčové nebo neklíčové sloupce, můžete nakonfigurovat [indexy se zahrnutými sloupci](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) a významně tak zvýšit výkon dotazů.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
