---
title: Nastavení explicitních hodnot pro vygenerované vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: d6aa9a0a9ce34e09a39026ad7ea9195b6777858c
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197855"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Nastavení explicitních hodnot pro vygenerované vlastnosti

Vygenerovaná vlastnost je vlastnost, jejíž hodnota je vygenerována (buď podle EF, nebo v databázi), když je entita přidána nebo aktualizována. Další informace najdete v tématu [vygenerované vlastnosti](../modeling/generated-properties.md) .

Mohou nastat situace, kdy chcete nastavit explicitní hodnotu pro vygenerovanou vlastnost místo toho, aby vygenerovala jednu z nich.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) na Githubu.

## <a name="the-model"></a>Model

Model použitý v tomto článku obsahuje jednu `Employee` entitu.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Uložení explicitní hodnoty během přidávání

`Employee.EmploymentStarted` Vlastnost je nakonfigurována tak, aby měla hodnoty generované databází pro nové entity (s použitím výchozí hodnoty).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Následující kód vloží dva zaměstnance do databáze.
* Pro první není přiřazena `Employee.EmploymentStarted` žádná hodnota, takže zůstane vlastnost nastavena na výchozí hodnotu CLR pro. `DateTime`
* Pro druhý jsme nastavili explicitní hodnotu `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Výstup ukazuje, že databáze vygenerovala hodnotu pro prvního zaměstnance a naše explicitní hodnota byla použita pro druhý.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Explicitní hodnoty do SQL Server sloupců IDENTITY

Podle úmluvy `Employee.EmployeeId` je vlastnost sloupcem generovaným `IDENTITY` úložištěm.

Ve většině případů bude výše uvedený přístup pro klíčové vlastnosti fungovat. Chcete-li však vložit explicitní hodnoty do sloupce `IDENTITY` SQL Server, je nutné ručně povolit `IDENTITY_INSERT` před voláním `SaveChanges()`.

> [!NOTE]  
> V našich backlogu máme [žádost o funkci](https://github.com/aspnet/EntityFramework/issues/703) , která to provede automaticky v rámci poskytovatele SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Výstup ukazuje, že dodaná ID byla uložena do databáze.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Nastavení explicitní hodnoty během aktualizace

`Employee.LastPayRaise` Vlastnost je nakonfigurována tak, aby při aktualizacích generovala hodnoty generované databází.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Ve výchozím nastavení EF Core vyvolá výjimku, pokud se pokusíte uložit explicitní hodnotu vlastnosti, která je nakonfigurována k vygenerování během aktualizace. Aby k tomu nedocházelo, je nutné vyřadit dolů na nižší úroveň rozhraní API metadat a `AfterSaveBehavior` nastavit (jak je uvedeno výše).

> [!NOTE]  
> **Změny v EF Core 2,0:** V předchozích verzích bylo chování po uložení řízeno pomocí `IsReadOnlyAfterSave` příznaku. Tento příznak byl zastaralý a nahrazuje ho `AfterSaveBehavior`.

V databázi je také Trigger, který generuje hodnoty pro `LastPayRaise` sloupec během `UPDATE` operací.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Následující kód zvyšuje plat dvou zaměstnanců v databázi.
* Pro první není přiřazena `Employee.LastPayRaise` žádná hodnota, takže vlastnost zůstane nastavena na hodnotu null.
* Pro druhý jsme nastavili explicitní hodnotu před jedním týdnem (back dating se vyzvednutím platby).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Výstup ukazuje, že databáze vygenerovala hodnotu pro prvního zaměstnance a naše explicitní hodnota byla použita pro druhý.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
