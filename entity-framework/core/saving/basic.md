---
title: Základní uložit - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: 35bf14af43289ad6308a49482d3f45a7a8be9067
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754393"
---
# <a name="basic-save"></a>Základní uložit

Naučte se přidávat, upravovat a odebírat dat pomocí třídy kontextu a entity.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) na Githubu.

## <a name="adding-data"></a>Přidávání dat

Použití *DbSet.Add* metoda pro přidání nové instance třídy entity. Data se vloží v databázi při volání *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Metody přidat, připojit a aktualizace všech pracovních na úplné graf entit předaný, jak je popsáno v [související Data](related-data.md) části. Alternativně vlastnost EntityEntry.State slouží k nastavení stavu právě jednu entitu. Například `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualizace dat

EF automaticky zjistí změny provedené v existující entita, která se sleduje kontextu. To zahrnuje entity, které můžete načíst nebo dotazu z databáze a entity, které byly dříve přidány a uloženy do databáze.

Stačí upravit hodnot přiřazených vlastnosti a pak zavolají *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Odstraňování dat

Použití *DbSet.Remove* metoda odstranění instancí tříd entity.

Pokud entita již existuje v databázi, se odstraní při *SaveChanges*. Pokud entita ještě nebyl uložen do databáze (tj. jeho sledování jak byl přidán) pak ji bude odebrána z kontextu a bude vložen při *SaveChanges* je volána.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Více operací v jednom SaveChanges

Můžete kombinovat více operací přidat/aktualizovat nebo odebrat do jediné volání *SaveChanges*.

> [!NOTE]  
> Pro většinu poskytovatelů databáze *SaveChanges* transakční. To znamená, že všechny operace bude úspěch nebo neúspěch a operace se nikdy vlevo použijí částečně.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
