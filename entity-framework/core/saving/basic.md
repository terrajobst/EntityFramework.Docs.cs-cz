---
title: Základní uložení – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 23e0e4611f642d59048fca5a808d0782b22caa1e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994798"
---
# <a name="basic-save"></a>Základní uložení

Zjistěte, jak přidávat, upravovat a odebírat dat pomocí třídy kontextu a entity.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) na Githubu.

## <a name="adding-data"></a>Přidání dat

Použití *DbSet.Add* způsob, jak přidat nové instance třídy entity. Data se vloží do databáze při volání *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Metody přidat, připojit a aktualizovat všechny práce na úplný graf entity předaný k nim, jak je popsáno v [souvisejících dat](related-data.md) oddílu. Alternativně EntityEntry.State vlastnost lze nastavit stav pouze jednu entitu. Například `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualizace dat

EF automaticky rozpozná změny provedené na existující entitu, která je sledována podle kontextu. To zahrnuje entity, které můžete načíst/dotazu z databáze a entity, které byly dříve přidány a uloženy do databáze.

Stačí upravit hodnoty přiřazené k vlastnosti a pak vyvolejte *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Odstranění dat

Použití *DbSet.Remove* metodu k odstranění instancí tříd entit.

Pokud entita již existuje v databázi, bude odstraněna během *SaveChanges*. Pokud entita ještě nebyla uložena do databáze (to znamená, ho je sledovat přidávání), se odeberou z kontextu a nebude vložen při *SaveChanges* je volána.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Více operací v jedné SaveChanges

Můžete zkombinovat několik operací přidání/aktualizaci/odebrání do jednoho volání *SaveChanges*.

> [!NOTE]  
> Pro většinu poskytovatelů databáze *SaveChanges* je transakční. To znamená, že všechny operace bude úspěch nebo neúspěch a operace se nikdy vlevo použijí částečně.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
