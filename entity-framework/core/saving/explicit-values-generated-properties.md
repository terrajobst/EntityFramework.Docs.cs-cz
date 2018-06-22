---
title: Explicitní hodnoty vlastností generovaného - EF základní nastavení
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054610"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Explicitní hodnoty nastavení pro generovaný vlastnosti

Vygenerovaný vlastnost je vlastnost, jehož hodnota je generována (buď EF nebo databázi) Pokud je entita přidat nebo aktualizovat. V tématu [generované vlastnosti](../modeling/generated-properties.md) Další informace.

Mohou nastat situace, ve které chcete nastavit explicitní hodnotu pro generovaný vlastnost, místo aby se vytvořen.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) na Githubu.

## <a name="the-model"></a>Model

Model použitý v tomto článku obsahuje jediný `Employee` entity.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Ukládání explicitní hodnotu během přidat

`Employee.EmploymentStarted` Je nakonfigurovaný tak, aby mají hodnoty generované databáze pro nové entity (s výchozí hodnotou).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Následující kód vloží dva zaměstnanci do databáze.
* Pro první, není přiřazena žádná hodnota k `Employee.EmploymentStarted` vlastnost, tak zůstane nastavená na výchozí hodnotu CLR pro `DateTime`.
* Pro druhý, jsme nastavili explicitní hodnotu `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Výstup ukazuje, že databáze generuje hodnotu pro první zaměstnance a naše explicitní hodnotu byl použit pro druhý.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Explicitní hodnoty do sloupce IDENTITY serveru SQL

Podle konvence `Employee.EmployeeId` vlastnost je úložiště vygeneruje `IDENTITY` sloupce.

Většině situací bude přístup výše uvedeném fungovat pro vlastnosti klíče. Ale vložit explicitní hodnoty do systému SQL Server `IDENTITY` sloupec, je nutné ručně povolit `IDENTITY_INSERT` před voláním `SaveChanges()`.

> [!NOTE]  
> Máme [žádost o funkce](https://github.com/aspnet/EntityFramework/issues/703) na našem nevyřízených položek k tomu automaticky v rámci zprostředkovatele SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Výstup ukazuje, že zadané ID byly uloženy do databáze.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Při aktualizaci nastavení explicitní hodnotu

`Employee.LastPayRaise` Je nakonfigurovaný tak, aby mají hodnoty generované databáze během aktualizace.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Ve výchozím nastavení bude EF základní vyvolat výjimku, pokud se pokusíte uložit explicitní hodnotu pro vlastnost nakonfigurovaný tak, aby se vygeneroval během aktualizace. Abyste tomu předešli, budete muset rozevírací nabídku na nižší úrovni metadat rozhraní API a nastavte `AfterSaveBehavior` (jak je uvedeno výše).

> [!NOTE]  
> **Změny v EF základní 2.0:** v předchozích verzích se řídí chování následnou uložit prostřednictvím `IsReadOnlyAfterSave` příznak. Tento příznak je zastaralá a nahrazuje `AfterSaveBehavior`.

V databázi ke generování hodnot pro je také aktivační událost `LastPayRaise` sloupec během `UPDATE` operace.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Následující kód zvyšuje mzda dvě zaměstnanců v databázi.
* Pro první, není přiřazena žádná hodnota k `Employee.LastPayRaise` vlastnost, tak zůstane nastavený na hodnotu null.
* Pro druhý jsme nastavili explicitní hodnotu jeden týden před (zpět z roku mzda vyvolat).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Výstup ukazuje, že databáze generuje hodnotu pro první zaměstnance a naše explicitní hodnotu byl použit pro druhý.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
