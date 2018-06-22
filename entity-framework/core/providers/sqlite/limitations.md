---
title: Databáze SQLite zprostředkovatele - omezení - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 8a60ccfc61a5757df8ebedf257379d4d3dbffbf6
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
ms.locfileid: "29719482"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Omezení poskytovatele SQLite EF základní databáze

Zprostředkovatel SQLite má několik omezení migrace. Většinu těchto omezení jsou výsledkem omezení v příslušný modul databáze SQLite a nejsou specifické pro EF.

## <a name="modeling-limitations"></a>Omezení modelování

Běžné knihovny relační (sdílené zprostředkovatelé relační databáze Entity Framework) definuje rozhraní API pro modelování koncepty, které jsou společné pro většinu modulů relační databáze. Pár těchto pojmech nejsou podporována zprostředkovatelem SQLite.

* Schémata
* Sekvence

## <a name="migrations-limitations"></a>Omezení migrace

Databázový stroj SQLite nepodporuje počet operací schématu, které jsou podporovány většinou jiných relačních databází. Pokud se pokusíte provést některé z nepodporované operací k databázi SQLite pak `NotSupportedException` bude vyvolána.

| Operace            | Podporovány? | Vyžaduje verzi |
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

Alternativní řešení můžete některé z těchto omezení podle ručně psaní kódu v vaše migrace k provedení tabulku znovu sestavit. Opětovné sestavení tabulky zahrnuje přejmenování existující tabulky, vytvořit novou tabulku, kopírování dat do nové tabulky a vyřadit staré tabulky. Budete muset použít `Sql(string)` metoda provést některé z těchto kroků.

V tématu [provedení další typy z změny schématu tabulky](http://sqlite.org/lang_altertable.html#otheralter) v dokumentaci k SQLite další podrobnosti.

V budoucnu EF může podporovat některé z těchto operací pomocí přístup opětovné sestavení tabulky v pozadí. Můžete [sledovat tuto funkci na našem projektu Githubu](https://github.com/aspnet/EntityFrameworkCore/issues/329).
