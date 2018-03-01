---
title: "Indexy – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a>Indexy

> [!NOTE]  
> Obecně se vztahuje na relační databáze konfigurace v této části. Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).

Index v relační databázi se mapuje na stejný koncept jako index v základní rozhraní Entity Framework.

## <a name="conventions"></a>Konvence

Podle konvence, jsou pojmenované indexy `IX_<type name>_<property name>`. Pro složené indexy `<property name>` stane podtržítka oddělený seznam názvů vlastností.

## <a name="data-annotations"></a>Datových poznámek

Indexy nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci název indexu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Můžete také zadat filtr.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Když pomocí zprostředkovatele SQL Server EF přidá filtrovat "IS NOT NULL" pro všechny sloupce s možnou hodnotou Null, které jsou součástí jedinečný index. Chcete-li přepsat touto konvencí, můžete zadat `null` hodnotu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
