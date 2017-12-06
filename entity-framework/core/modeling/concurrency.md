---
title: "Tokeny souběžnosti - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>Tokeny souběžnosti

Pokud vlastnost je nakonfigurovaný jako token souběžnosti pak EF zkontroluje, že žádný jiný uživatel změnil tuto hodnotu v databázi při ukládání změn do záznamů. EF používá optimistickou metodu souběžného vzor, znamená se bude předpokládat, že hodnota nebyla změněna a pokuste se ukládat data, ale výjimku, pokud zjistí, že byla hodnota změněna.

Například může chceme konfigurovat `LastName` na `Person` být token souběžnosti. To znamená, že pokud jeden uživatel se pokusí uložit některé změny `Person`, ale došlo ke změně jiného uživatele `LastName` pak bude vyvolána výjimka. To může být žádoucí, aby vaše aplikace může vyzvat uživatele k Ujistěte se, že tento záznam stále představuje stejná skutečná osoba před uložením jejich změny.

> [!NOTE]
> Tato stránka dokumenty postup konfigurace tokenů souběžnosti. V tématu [zpracování souběžnosti](../saving/concurrency.md) příklady, jak použít optimistickou metodu souběžného v aplikaci.

## <a name="how-concurrency-tokens-work-in-ef"></a>Jak fungují tokenů souběžnosti v EF

Úložiště dat můžete vynutit tokenů souběžnosti kontrolou, že všechny záznam bude aktualizován nebo odstraněn stále má stejnou hodnotu pro concurrency token, který byl přiřazen při kontext původně načtení dat z databáze.

Například relačních databází dosáhnout zahrnutím token souběžnosti v `WHERE` klauzule libovolného `UPDATE` nebo `DELETE` příkazy a kontrola počet řádků, které situace měla vliv na. Pokud token souběžnosti stále shoduje se bude aktualizovat jeden řádek. Pokud se změnila hodnota v databázi, jsou aktualizovány žádné řádky.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

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
