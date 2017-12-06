---
title: "Databáze SQLite zprostředkovatele - omezení - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Omezení poskytovatele SQLite EF základní databáze

Zprostředkovatel SQLite má několik omezení migrace. Většinu těchto omezení jsou výsledkem omezení v příslušný modul databáze SQLite a nejsou specifické pro EF.

## <a name="modeling-limitations"></a>Omezení modelování

Běžné knihovny relační (sdílené zprostředkovatelé relační databáze Entity Framework) definuje rozhraní API pro modelování koncepty, které jsou společné pro většinu modulů relační databáze. Pár těchto pojmech nejsou podporována zprostředkovatelem SQLite.

* Schémata
* Sekvence

## <a name="migrations-limitations"></a>Omezení migrace

Databázový stroj SQLite nepodporuje počet operací schématu, které jsou podporovány většinou jiných relačních databází. Pokud se pokusíte provést některé z nepodporované operací k databázi SQLite pak `NotSupportedException` bude vyvolána.

| Operace            | Podporovány? |
| -------------------- | ---------- |
| AddColumn            | ✔          |
| Addforeignkey a        | ✗          |
| AddPrimaryKey        | ✗          |
| AddUniqueConstraint  | ✗          |
| AlterColumn          | ✗          |
| CreateIndex          | ✔          |
| CreateTable          | ✔          |
| DropColumn           | ✗          |
| DropForeignKey       | ✗          |
| DropIndex            | ✔          |
| DropPrimaryKey       | ✗          |
| DropTable            | ✔          |
| DropUniqueConstraint | ✗          |
| RenameColumn         | ✗          |
| RenameIndex          | ✗          |
| RenameTable          | ✔          |

## <a name="migrations-limitations-workaround"></a>Alternativní řešení omezení migrace

Alternativní řešení můžete některé z těchto omezení podle ručně psaní kódu v vaše migrace k provedení tabulku znovu sestavit. Opětovné sestavení tabulky zahrnuje přejmenování existující tabulky, vytvořit novou tabulku, kopírování dat do nové tabulky a vyřadit staré tabulky. Budete muset použít `Sql(string)` metoda provést některé z těchto kroků.

V tématu [provedení další typy z změny schématu tabulky](http://sqlite.org/lang_altertable.html#otheralter) v dokumentaci k SQLite další podrobnosti.

V budoucnu EF může podporovat některé z těchto operací pomocí přístup opětovné sestavení tabulky v pozadí. Můžete [sledovat tuto funkci na našem projektu Githubu](https://github.com/aspnet/EntityFramework/issues/329).
