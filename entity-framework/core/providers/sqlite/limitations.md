---
title: Zprostředkovatel databáze SQLite – omezení – EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 2f80dc195265787318ac4925dd937da45ffad011
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179774"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Omezení zprostředkovatele databáze EF Core SQLite

Zprostředkovatel SQLite má několik omezení migrace. Většina těchto omezení je výsledkem omezení v podkladovém databázovém stroji SQLite a není specifická pro EF.

## <a name="modeling-limitations"></a>Omezení modelování

Společná relační knihovna (sdílená poskytovateli relačních databází Entity Framework) definuje rozhraní API pro koncepty modelování, které jsou společné pro většinu relačních databázových strojů. Zprostředkovatel SQLite nepodporuje několik těchto konceptů.

* Schémata
* Sekvence
* Vypočítané sloupce

## <a name="query-limitations"></a>Omezení dotazů

SQLite netivně podporuje následující datové typy. EF Core mohou číst a zapisovat hodnoty těchto typů a dotazování na rovnost (`where e.Property == value`) je také podporováno. Jiné operace, například porovnání a řazení, budou vyžadovat vyhodnocení u klienta.

* DateTimeOffset
* Decimal
* TimeSpan
* UInt64

Místo `DateTimeOffset` doporučujeme použít hodnoty DateTime. Při zpracování několika časových pásem doporučujeme před uložením a převodem zpět na příslušné časové pásmo převést hodnoty na čas UTC.

Typ `Decimal` poskytuje vysokou úroveň přesnosti. Pokud ale tuto úroveň přesnosti nepotřebujete, doporučujeme místo toho použít Double. Pomocí [převaděče hodnot](../../modeling/value-conversions.md) můžete v třídách dál používat desetinné číslo.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>Omezení migrace

Databázový stroj SQLite nepodporuje řadu operací schématu, které jsou podporovány většinou ostatních relačních databází. Pokud se pokusíte použít jednu z nepodporovaných operací na databázi SQLite, bude vyvolána `NotSupportedException`.

| Operace            | Doložen? | Vyžaduje verzi |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| Vytvořit          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DROPS            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| Přejmenovat          | ✔          | 1.0              |
| EnsureSchema         | ✔ (No-OP)  | 2.0              |
| DropSchema           | ✔ (No-OP)  | 2.0              |
| Vložit               | ✔          | 2.0              |
| Aktualizace               | ✔          | 2.0              |
| Odstranění               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Omezení migrace – alternativní řešení

Některá tato omezení můžete vyřešit ručním psaním kódu v migracích k provedení opětovného sestavení tabulky. Sestavování tabulky zahrnuje přejmenování existující tabulky, vytvoření nové tabulky, zkopírování dat do nové tabulky a vyřazení staré tabulky. K provedení některých z těchto kroků budete muset použít metodu `Sql(string)`.

Další podrobnosti najdete v dokumentaci k [jinému druhu změn schématu tabulky](https://sqlite.org/lang_altertable.html#otheralter) v dokumentaci k sqlite.

V budoucnu může EF podporovat některé z těchto operací pomocí přístupu k opakovanému sestavení tabulky v rámci pokrývání. [Tuto funkci můžete sledovat v našem projektu GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues/329).
