---
title: Tokeny souběžnosti - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/05/2018
ms.locfileid: "29745476"
---
# <a name="concurrency-tokens"></a>Tokeny souběžnosti

> [!NOTE]
> Tato stránka dokumenty postup konfigurace tokenů souběžnosti. V tématu [zpracování konfliktů souběžnosti](../saving/concurrency.md) pro podrobné vysvětlení, jak funguje řízení souběžnosti na základní EF a příklady způsobu řešení konfliktů souběžnosti ve vaší aplikaci.

Vlastnosti, které jsou nakonfigurované jako tokenů souběžnosti slouží k implementaci optimistické řízení souběžného.

## <a name="conventions"></a>Konvence

Podle konvence jsou nakonfigurovány jako tokenů souběžnosti nikdy vlastnosti.

## <a name="data-annotations"></a>Datových poznámek

Můžete konfigurovat vlastnosti jako token souběžnosti datových poznámek.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete konfigurovat vlastnosti jako token souběžnosti.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Časové razítko či řádku verze

Časové razítko je vlastnost, kde nová hodnota je generován databázi pokaždé, když se přidají nebo aktualizují řádek. Vlastnost je také považovány za token souběžnosti. Tím se zajistí, že obdržíte výjimku, pokud jiné změnil řádek, který se pokoušíte aktualizovat, protože dotaz pro data.

Jak to se dá dosáhnout závisí používaný zprostředkovatel databáze. Pro systém SQL Server, se obvykle používá časové razítko na *byte []* vlastnosti, která bude instalační program jako *ROWVERSION* sloupec v databázi.

### <a name="conventions"></a>Konvence

Podle konvence jsou vlastnosti nikdy nakonfigurovány jako časová razítka.

### <a name="data-annotations"></a>Datových poznámek

Můžete konfigurovat vlastnosti podobě časového razítka datových poznámek.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete konfigurovat vlastnosti podobě časového razítka.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
