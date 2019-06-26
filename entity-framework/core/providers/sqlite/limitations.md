---
title: Databázi SQLite poskytovatele – omezení – EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: eaa7d5b1496172e4f3821433a1cd098ee7e8b737
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394802"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core Database Provider Limitations

Zprostředkovatel SQLite má několik omezení migrace. Většina těchto omezení jsou výsledkem omezení v podkladové databázový stroj SQLite a nejsou specifická pro EF.

## <a name="modeling-limitations"></a>Omezení modelování

Společná knihovna relační (sdílené poskytovateli rozhraní Entity Framework relační databáze) definuje rozhraní API pro modelování koncepty, které jsou společné pro většinu relačních databázových strojů. Několik těchto konceptů nejsou podporována zprostředkovatelem SQLite.

* Schémata
* Sekvence
* Vypočítané sloupce

## <a name="query-limitations"></a>Omezení dotazu

SQLite nenabízí nativní podporu následujících datových typů. EF Core může číst a zapisovat hodnoty těchto typů a dotazování na rovnost (`where e.Property == value`) je také podpora. Další operace, jako jsou však porovnání a řazení se vyžaduje vyhodnocení na straně klienta.

* DateTimeOffset
* Desetinné číslo
* TimeSpan
* UInt64

Místo `DateTimeOffset`, doporučujeme použít hodnoty data a času. Při zpracování více časových pásem, doporučujeme převod hodnoty na standard UTC před uložením a pak převod zpátky na odpovídající časové pásmo.

`Decimal` Typ poskytuje vysokou úroveň přesnosti. Pokud nepotřebujete tuto úroveň přesnosti, však doporučujeme místo toho použít double. Můžete použít [převaděč hodnoty](../../modeling/value-conversions.md) nadále používat desetinné ve třídách.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>Omezení migrace

Databázový stroj SQLite nepodporuje počet operací schématu, které podporuje většinu dalších relačních databází. Pokud se pokusíte použít některé z nepodporované operace databáze SQLite o `NotSupportedException` bude vyvolána výjimka.

| Operace            | Podporuje? | Vyžaduje verzi |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (no-op)  | 2.0              |
| DropSchema           | ✔ (no-op)  | 2.0              |
| Insert               | ✔          | 2.0              |
| Aktualizace               | ✔          | 2.0              |
| Odstranit               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Alternativní řešení omezení migrace

Můžete vyřešit některé z těchto omezení ručně napsáním kódu v vaše migrace provést tabulku znovu sestavit. Tabulka opětovné sestavení zahrnuje přejmenování existující tabulky, vytvářet nové tabulky, kopírování dat do nové tabulky a vyřazení staré tabulky. Budete muset použít `Sql(string)` možností, jak provést některé z těchto kroků.

Zobrazit [provádění jiné typy o změny schématu tabulky](http://sqlite.org/lang_altertable.html#otheralter) v další podrobnosti naleznete v dokumentaci SQLite.

V budoucnu EF může podporovat některé z těchto operací pomocí přístupu tabulku znovu sestavit na pozadí. Je možné [sledování této funkce v našem projektu z Githubu](https://github.com/aspnet/EntityFrameworkCore/issues/329).
