---
title: Indexy (relační databáze) – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824572"
---
# <a name="indexes-relational-database"></a>Indexy (relační databáze)

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Index v relační databázi se mapuje na stejný koncept jako index v jádru Entity Framework.

## <a name="conventions"></a>Konvence

Podle konvence jsou indexy pojmenované `IX_<type name>_<property name>`. U složených indexů se `<property name>` jako podtržítko oddělený seznam názvů vlastností.

## <a name="data-annotations"></a>Datové poznámky

Indexy nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Ke konfiguraci názvu indexu můžete použít rozhraní Fluent API.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Můžete také zadat filtr.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Při použití poskytovatele SQL Server EF přidá `'IS NOT NULL'` filtr pro všechny sloupce s možnou hodnotou null, které jsou součástí jedinečného indexu. K přepsání této konvence můžete uvést `null`ou hodnotu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Zahrnout sloupce do indexů SQL Server

Pokud jsou všechny sloupce v dotazu zahrnuté do indexu jako klíčové nebo neklíčové sloupce, můžete nakonfigurovat [indexy se zahrnutými sloupci](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) a významně tak zvýšit výkon dotazů.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
