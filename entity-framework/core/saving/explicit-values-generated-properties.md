---
title: Nastavení explicitní hodnoty pro generované vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: 00abef4d1208400ff68ced0a241b98b8dc9be5c0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997850"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Nastavení explicitní hodnoty pro generované vlastnosti

Generované vlastnosti je vlastnost, jejíž hodnota je generována (buď EF, nebo databáze) při je entita přidat nebo aktualizovat. Zobrazit [vygenerovaným vlastnostem](../modeling/generated-properties.md) Další informace.

Může nastat situace, ve kterém chcete nastavit explicitní hodnotu pro generované vlastnosti namísto nutnosti vytvořen.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) na Githubu.

## <a name="the-model"></a>Model

Model použitý v tomto článku obsahuje jediný `Employee` entity.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Ukládá se během přidat explicitní hodnotu

`Employee.EmploymentStarted` Vlastnost má nakonfigurovanou hodnoty generován databází pro nové entity (s výchozí hodnotou).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Následující kód vloží dva zaměstnance do databáze.
* Pro funkce first, není přiřazena žádná hodnota pro `Employee.EmploymentStarted` , tak zůstane nastavenou na výchozí hodnotu CLR `DateTime`.
* Pro druhý, jsme nastavili explicitní hodnotu `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Výstup ukazuje, že databáze vygenerovala hodnotu prvního zaměstnance a naše explicitní hodnotu byl použit pro druhý.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Explicitní hodnoty do sloupce IDENTITY serveru SQL

Podle konvence `Employee.EmployeeId` vlastností je úložiště vygeneruje `IDENTITY` sloupce.

Pro většinu situací bude přístup výše uvedené fungovat pro vlastnosti klíče. Ale chcete-li vložit explicitní hodnoty do systému SQL Server `IDENTITY` sloupec, je nutné ručně povolit `IDENTITY_INSERT` před voláním `SaveChanges()`.

> [!NOTE]  
> Máme [žádost o funkci](https://github.com/aspnet/EntityFramework/issues/703) v backlogu a k tomu automaticky v rámci zprostředkovatele SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Výstup ukazuje, že zadané ID byly uloženy do databáze.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Během aktualizace nastavení explicitní hodnotu

`Employee.LastPayRaise` Vlastnost má nakonfigurovanou hodnoty generován databází během aktualizace.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Ve výchozím nastavení bude EF Core vyvolat výjimku, pokud se pokusíte uložit explicitní hodnotu pro vlastnost, která je nakonfigurovaná vygenerování během aktualizace. Abyste tomu předešli, budete muset rozevírací seznam na nižší úrovni metadat rozhraní API a nastavit `AfterSaveBehavior` (jak je uvedeno výše).

> [!NOTE]  
> **Změny v EF Core 2.0:** v předchozích verzích byla ovládaná chování následné uložení `IsReadOnlyAfterSave` příznak. Tento příznak má zastaralé a nahrazuje `AfterSaveBehavior`.

V databázi pro generování hodnot je také aktivační události `LastPayRaise` sloupec během `UPDATE` operace.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Následující kód zvyšuje salary dva zaměstnanci v databázi.
* Pro funkce first, není přiřazena žádná hodnota pro `Employee.LastPayRaise` vlastnosti, tak zůstane nastavený na hodnotu null.
* Pro druhý jsme nastavili explicitní hodnotu o jeden týden před (raise už z roku placené zpět).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Výstup ukazuje, že databáze vygenerovala hodnotu prvního zaměstnance a naše explicitní hodnotu byl použit pro druhý.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
