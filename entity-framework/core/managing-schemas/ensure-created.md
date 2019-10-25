---
title: Vytvoření a vyřazení rozhraní API – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811783"
---
# <a name="create-and-drop-apis"></a>Vytvoření a přemístění rozhraní API

Metody EnsureCreated a EnsureDeleted poskytují nezjednodušenou alternativu pro [migrace](migrations/index.md) pro správu schématu databáze. Tyto metody jsou užitečné ve scénářích, kdy jsou data přechodný a můžou být vyhozena při změně schématu. Například při vytváření prototypů, v testech nebo místních mezipamětí.

Někteří poskytovatelé (obzvláště nerelační) nepodporují migrace. Pro tyto poskytovatele je EnsureCreated často nejjednodušší způsob, jak inicializovat schéma databáze.

> [!WARNING]
> EnsureCreated a migrace nefungují dobře společně. Pokud používáte migrace, nepoužívejte k inicializaci schématu EnsureCreated.

Přechod z EnsureCreated na migrace není bezproblémové prostředí. Nejjednodušší způsob, jak to udělat, je vyřadit databázi a znovu ji vytvořit pomocí migrací. Pokud předpokládáte použití migrace v budoucnu, je nejlepší začít s migracemi místo používání EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

Metoda EnsureDeleted vynechá databázi, pokud existuje. Pokud nemáte příslušná oprávnění, je vyvolána výjimka.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated vytvoří databázi, pokud neexistuje, a inicializuje schéma databáze. Pokud existují nějaké tabulky (včetně tabulek pro jinou třídu DbContext), schéma se neinicializuje.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> K dispozici jsou také asynchronní verze těchto metod.

## <a name="sql-script"></a>Skript SQL

K získání SQL používaného v EnsureCreated můžete použít metodu GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Více tříd DbContext

EnsureCreated funguje pouze v případě, že databáze neobsahuje žádné tabulky. V případě potřeby můžete napsat vlastní kontrolu, abyste viděli, jestli se schéma musí inicializovat, a k inicializaci schématu použít základní službu IRelationalDatabaseCreator.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
