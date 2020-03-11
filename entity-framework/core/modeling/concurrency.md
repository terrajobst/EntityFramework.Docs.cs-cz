---
title: Tokeny souběžnosti – EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: bfeb611f222f7195fe22d920b452b40cc4addf90
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417286"
---
# <a name="concurrency-tokens"></a>Tokeny souběžnosti

> [!NOTE]
> Na této stránce jsou dokumenty, jak nakonfigurovat tokeny souběžnosti. Podrobné vysvětlení toho, jak řízení souběžnosti funguje na EF Core a příklady, jak zpracovávat konflikty souběžnosti ve vaší aplikaci, najdete v tématu [zpracování konfliktů](../saving/concurrency.md) souběžnosti.

Vlastnosti konfigurované jako tokeny souběžnosti slouží k implementaci optimistického řízení souběžnosti.

## <a name="configuration"></a>Konfigurace

### <a name="data-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Časové razítko/rowversion

Timestamp/rowversion je vlastnost, pro kterou je nová hodnota automaticky generovaná databází pokaždé, když se řádek vloží nebo aktualizuje. Vlastnost je také zpracována jako token souběžnosti, což zajistí, že dojde k výjimce v případě, že se řádek, který aktualizujete, od svého dotazu změnil. Přesné podrobnosti závisí na použitém poskytovateli databáze. pro SQL Server se obvykle používá vlastnost *Byte []* , která se nastaví jako sloupec *rowversion* v databázi.

Vlastnost lze nastavit jako časové razítko/rowversion následujícím způsobem:

### <a name="data-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs?name=Timestamp&highlight=9,17)]

***
