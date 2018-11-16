---
title: Vytvoření a přemístění rozhraní API – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688626"
---
# <a name="create-and-drop-apis"></a>Vytvoření a přemístění rozhraní API

Metody EnsureCreated a EnsureDeleted poskytují jednoduchý alternativou k [migrace](migrations/index.md) pro správu schéma databáze. Tyto metody jsou užitečné v situacích, když je přechodná data a můžete vyřadit, když se změní schéma. Například při vytváření prototypů, v testech, nebo pro místní mezipaměti.

Někteří poskytovatelé (zvlášť ty nerelačních) nepodporují migrace. U těchto poskytovatelů je EnsureCreated často nejjednodušší způsob, jak inicializovat schéma databáze.

> [!WARNING]
> Migrace a EnsureCreated nefungují dobře spolupracovaly. Pokud používáte migraci, nepoužívejte EnsureCreated inicializovat schématu.

Přechod z EnsureCreated pro migrace není bezproblémové prostředí. Nejjednodušší způsob, jak to udělat, je vyřaďte databázi a znovu vytvořit pomocí migrace. Pokud očekáváte, že v budoucnu pomocí migrace, je vhodné pouze začít s migrací namísto použití EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

Metoda EnsureDeleted bude vymazání databáze, pokud existuje. Pokud nemáte příslušná oprávnění, je vyvolána výjimka.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated vytvoří databázi, pokud ho neexistuje a inicializovat schéma databáze. Pokud neexistují žádné tabulky (včetně tabulek pro jiné třídy DbContext), schéma nelze inicializovat.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Asynchronní verze těchto metod jsou také k dispozici.

## <a name="sql-script"></a>Skript SQL

SQL používané EnsureCreated získáte můžete použít metodu GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Vícenásobné třídy DbContext

EnsureCreated funguje pouze v případě v databázi nejsou žádné tabulky. V případě potřeby můžete napsat vlastní zkontrolujte, jestli je potřeba inicializovat schématu a použijte základní službě IRelationalDatabaseCreator inicializovat schématu.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
