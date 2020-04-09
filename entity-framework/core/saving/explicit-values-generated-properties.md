---
title: Nastavení explicitních hodnot pro generované vlastnosti – jádro EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: 43c4ab3c2a60645cdeff2a6cc40ce979f832f2fd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417567"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Nastavení explicitních hodnot pro generované vlastnosti

Generovaná vlastnost je vlastnost, jejíž hodnota je generována (ef nebo databáze) při přidání nebo aktualizaci entity. Další informace naleznete [v tématu Generované vlastnosti.](../modeling/generated-properties.md)

Mohou nastat situace, kdy chcete nastavit explicitní hodnotu pro generované vlastnosti, spíše než mít jeden generován.

> [!TIP]  
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) můžete zobrazit na GitHubu.

## <a name="the-model"></a>Model

Model použitý v tomto článku `Employee` obsahuje jednu entitu.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Uložení explicitní hodnoty během přidání

Vlastnost `Employee.EmploymentStarted` je nakonfigurována tak, aby hodnoty generovala databáze pro nové entity (pomocí výchozí hodnoty).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Následující kód vloží dva zaměstnance do databáze.

* Pro první, žádná hodnota `Employee.EmploymentStarted` je přiřazena vlastnost, takže zůstane nastavena na clr výchozí hodnotu pro `DateTime`.
* Pro druhé jsme nastavili explicitní `1-Jan-2000`hodnotu .

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Výstup ukazuje, že databáze vygenerovala hodnotu pro prvního zaměstnance a naše explicitní hodnota byla použita pro druhého.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Explicitní hodnoty do sloupců IDENTITY serveru SQL Server

Podle konvence `Employee.EmployeeId` vlastnost je sloupec `IDENTITY` generovaný úložištěm.

Pro většinu situací bude výše uvedený přístup fungovat pro klíčové vlastnosti. Chcete-li však vložit explicitní `IDENTITY` hodnoty do sloupce serveru `IDENTITY_INSERT` SQL `SaveChanges()`Server, je třeba ručně povolit před voláním .

> [!NOTE]  
> Máme [požadavek](https://github.com/aspnet/EntityFramework/issues/703) na funkci na naše nevyřízené položky k tomu automaticky v rámci zprostředkovatele serveru SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Výstup ukazuje, že zadané ID byly uloženy do databáze.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Nastavení explicitní hodnoty během aktualizace

Vlastnost `Employee.LastPayRaise` je nakonfigurována tak, aby hodnoty generovaly databáze během aktualizací.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Ve výchozím nastavení EF Core vyvolá výjimku, pokud se pokusíte uložit explicitní hodnotu pro vlastnost, která je nakonfigurována tak, aby byla generována během aktualizace. Chcete-li se tomu vyhnout, musíte rozbalit na nižší `AfterSaveBehavior` úroveň metadat API a nastavit (jak je uvedeno výše).

> [!NOTE]  
> **Změny v EF Core 2.0:** V předchozích verzích po uložení chování `IsReadOnlyAfterSave` bylo řízeno prostřednictvím příznaku. Tento příznak byl zastaralý a `AfterSaveBehavior`nahrazen .

V databázi je také aktivační událost `LastPayRaise` pro `UPDATE` generování hodnot pro sloupec během operací.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Následující kód zvyšuje plat dvou zaměstnanců v databázi.

* Pro první, žádná hodnota `Employee.LastPayRaise` je přiřazena vlastnost, takže zůstane nastavena na hodnotu null.
* Za druhé jsme stanovili explicitní hodnotu před týdnem (zpět datování zvýšení platu).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Výstup ukazuje, že databáze vygenerovala hodnotu pro prvního zaměstnance a naše explicitní hodnota byla použita pro druhého.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
