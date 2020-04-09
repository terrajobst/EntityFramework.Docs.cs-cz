---
title: Základní uložení - EF core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417633"
---
# <a name="basic-save"></a>Základní uložení

Zjistěte, jak přidávat, upravovat a odebírat data pomocí tříd kontextu a entit.

> [!TIP]  
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) můžete zobrazit na GitHubu.

## <a name="adding-data"></a>Přidání dat

Pomocí metody *DbSet.Add* přidejte nové instance tříd entit. Data budou vložena do databáze při volání *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Metody Add, Attach a Update všechny práce na úplný graf entit, které jim byly předány, jak je popsáno v části [Související data.](related-data.md) Alternativně EntityEntry.State vlastnost lze nastavit stav pouze jednu entitu. Například, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualizace dat

EF automaticky rozpozná změny provedené v existující entitě, která je sledována kontextem. To zahrnuje entity, které načtete nebo dotazujete z databáze, a entity, které byly dříve přidány a uloženy do databáze.

Jednoduše upravte hodnoty přiřazené vlastnostem a pak zavolejte *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Odstranění dat

Pomocí metody *DbSet.Remove* odstraňte instance tříd entit.

Pokud entita již v databázi existuje, bude během *savechanges odstraněna*. Pokud entita ještě nebyla uložena do databáze (to znamená, že je sledována jako přidána), bude odebrána z kontextu a již nebude vložena při volání *SaveChanges.*

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Více operací v jednom SaveChanges

Můžete kombinovat více Přidat nebo aktualizovat nebo odebrat operace do jednoho volání *SaveChanges*.

> [!NOTE]  
> Pro většinu poskytovatelů databáze *SaveChanges* je transakční. To znamená, že všechny operace budou úspěšné nebo neúspěšné a operace nebudou nikdy částečně použity.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
