---
title: Tokeny souběžnosti – EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 8a5f3aa09c2a83d5be0998a11ef2ee8100437514
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781141"
---
# <a name="concurrency-tokens"></a>Tokeny souběžnosti

> [!NOTE]
> Na této stránce jsou dokumenty, jak nakonfigurovat tokeny souběžnosti. Podrobné vysvětlení toho, jak řízení souběžnosti funguje na EF Core a příklady, jak zpracovávat konflikty souběžnosti ve vaší aplikaci, najdete v tématu [zpracování konfliktů](../saving/concurrency.md) souběžnosti.

Vlastnosti konfigurované jako tokeny souběžnosti slouží k implementaci optimistického řízení souběžnosti.

## <a name="configuration"></a>Konfigurace

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Časové razítko/rowversion

Timestamp/rowversion je vlastnost, pro kterou je nová hodnota automaticky generovaná databází pokaždé, když se řádek vloží nebo aktualizuje. Vlastnost je také zpracována jako token souběžnosti, což zajistí, že dojde k výjimce v případě, že se řádek, který aktualizujete, od svého dotazu změnil. Přesné podrobnosti závisí na použitém poskytovateli databáze. pro SQL Server se obvykle používá vlastnost *Byte []* , která se nastaví jako sloupec *rowversion* v databázi.

Vlastnost lze nastavit jako časové razítko/rowversion následujícím způsobem:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[! Code-CSharp [main] (.. /.. /.. /samples/core/Modeling/FluentAPI/Timestamp.cs? název = časové razítko zvýraznění & = 9, 17]

***
