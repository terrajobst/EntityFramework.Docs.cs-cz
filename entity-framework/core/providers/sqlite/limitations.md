---
title: Databázi SQLite poskytovatele – omezení – EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 69c40fcd8b7ddb925728b1bad9992ad2a81e7540
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994661"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Omezení poskytovatele pro databázi SQLite EF Core

Zprostředkovatel SQLite má několik omezení migrace. Většina těchto omezení jsou výsledkem omezení v podkladové databázový stroj SQLite a nejsou specifická pro EF.

## <a name="modeling-limitations"></a>Omezení modelování

Společná knihovna relační (sdílené poskytovateli rozhraní Entity Framework relační databáze) definuje rozhraní API pro modelování koncepty, které jsou společné pro většinu relačních databázových strojů. Několik těchto konceptů nejsou podporována zprostředkovatelem SQLite.

* Schémata
* Sekvence

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
| RenameColumn         | ✗          |                  |
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
