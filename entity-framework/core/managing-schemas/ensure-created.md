---
title: Vytvoření a přemístění rozhraní API – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285636"
---
# <a name="create-and-drop-apis"></a>Vytvoření a přemístění rozhraní API

Metody EnsureCreated a EnsureDeleted poskytují jednoduchý alternativou k [migrace](migrations/index.md) pro správu schéma databáze. To je užitečné v situacích, když data je přechodná a můžete vyřadit, když se změní schéma. Například při vytváření prototypů, v testech, nebo pro místní mezipaměti.

Někteří poskytovatelé (zvlášť ty nerelačních) nepodporují migrace. Pro tyto EnsureCreated je často nejjednodušší způsob, jak inicializovat schéma databáze.

> [!WARNING]
> Migrace a EnsureCreated nefungují dobře spolupracovaly. Pokud používáte migraci, nepoužívejte EnsureCreated inicializovat schématu.

Přechod z EnsureCreated pro migrace není bezproblémové prostředí. Je simpelest způsob, jak toho dosáhnout, je vyřaďte databázi a znovu vytvořit pomocí migrace. Pokud očekáváte, že v budoucnu pomocí migrace, je vhodné pouze začít s migrací namísto použití EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

Metoda EnsureDeleted bude vymazání databáze, pokud existuje. Pokud nemáte odpovídající oprávnění, je vyvolána výjimka.

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
