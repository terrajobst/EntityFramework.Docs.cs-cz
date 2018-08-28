---
title: Tokeny souběžnosti – EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994223"
---
# <a name="concurrency-tokens"></a>Tokeny souběžnosti

> [!NOTE]
> Tato stránka dokumenty konfigurace tokeny souběžnosti. Zobrazit [zpracování konfliktů souběžnosti](../saving/concurrency.md) podrobné vysvětlení toho, jak funguje řízení souběžnosti na EF Core a příklady toho, jak zpracování konfliktů souběžnosti v aplikaci.

Implementace optimistického řízení souběžnosti se používají nakonfigurovaných jako tokeny souběžnosti.

## <a name="conventions"></a>Konvence

Podle konvence jsou vlastnosti nakonfigurovány jako tokeny souběžnosti.

## <a name="data-annotations"></a>Datové poznámky

Datové poznámky můžete nakonfigurovat vlastnosti jako tokenem souběžnosti.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete nakonfigurovat vlastnosti jako tokenem souběžnosti.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Časové razítko a klíče řádku verze

Časové razítko je vlastnost, pokud nová hodnota je generován databází pokaždé, když se vloží nebo aktualizuje řádek. Vlastnost je také považováno za tokenem souběžnosti. Tím se zajistí, že výjimky se zobrazí, když se někdo jiný změnil řádek, který se pokoušíte aktualizovat, protože jste se dotázali data.

Až si zprostředkovatele databáze používá jak toho dosáhnout je. Pro SQL Server, se obvykle používá časové razítko na *byte []* nastavit vlastnost, která bude jako *ROWVERSION* sloupec v databázi.

### <a name="conventions"></a>Konvence

Podle konvence jsou vlastnosti nakonfigurovány jako časová razítka.

### <a name="data-annotations"></a>Datové poznámky

Datové poznámky můžete nakonfigurovat vlastnosti jako časové razítko.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete nakonfigurovat vlastnosti jako časové razítko.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
