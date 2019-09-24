---
title: Tokeny souběžnosti – EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197462"
---
# <a name="concurrency-tokens"></a>Tokeny souběžnosti

> [!NOTE]
> Na této stránce jsou dokumenty, jak nakonfigurovat tokeny souběžnosti. Podrobné vysvětlení toho, jak řízení souběžnosti funguje na EF Core a příklady, jak zpracovávat konflikty souběžnosti ve vaší aplikaci, najdete v tématu [zpracování konfliktů](../saving/concurrency.md) souběžnosti.

Vlastnosti konfigurované jako tokeny souběžnosti slouží k implementaci optimistického řízení souběžnosti.

## <a name="conventions"></a>Konvence

Podle konvence se vlastnosti nikdy nekonfigurují jako tokeny souběžnosti.

## <a name="data-annotations"></a>Datové poznámky

Můžete použít datové poznámky ke konfiguraci vlastnosti jako tokenu souběžnosti.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci vlastnosti jako tokenu souběžnosti.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Časové razítko nebo verze řádku

Časové razítko je vlastnost, ve které databáze generuje novou hodnotu při každém vložení nebo aktualizaci řádku. Vlastnost je také zpracována jako token souběžnosti. Tím zajistíte výjimku, pokud někdo jiný změnil řádek, který se pokoušíte aktualizovat, protože jste se dostali k datům.

Způsob, jakým je dosaženo, je až pro používaného poskytovatele databáze. Pro SQL Server se časové razítko obvykle používá u vlastnosti *Byte []* , která bude nastavená jako sloupec *rowversion* v databázi.

### <a name="conventions"></a>Konvence

Podle konvence se vlastnosti nikdy nekonfigurují jako časová razítka.

### <a name="data-annotations"></a>Datové poznámky

Pomocí datových poznámek můžete nastavit vlastnost jako časové razítko.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci vlastnosti jako časového razítka.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
