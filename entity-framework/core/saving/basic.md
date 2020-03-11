---
title: EF Core pro ukládání na úrovni Basic
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417633"
---
# <a name="basic-save"></a>Základní uložení

Naučte se přidávat, upravovat a odebírat data pomocí tříd kontextu a entit.

> [!TIP]  
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) tohoto článku můžete zobrazit na GitHubu.

## <a name="adding-data"></a>Přidávání dat

K přidání nových instancí tříd entit použijte metodu *negenerickými. Add* . Data budou vložena do databáze při volání *metody SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Metody přidat, připojit a aktualizovat všechny pracují na úplných grafech entit, které jsou předány do nich, jak je popsáno v části [související data](related-data.md) . Alternativně lze vlastnost EntityEntry. State použít k nastavení stavu pouze jedné entity. například `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualizace dat

EF automaticky zjistí změny provedené u existující entity, která je sledována kontextem. To zahrnuje entity, které načtete nebo dotazují z databáze, a entity, které byly dříve přidány a uloženy do databáze.

Jednoduše upravte hodnoty přiřazené vlastnostem a potom zavolejte metodu *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Odstranění dat

K odstranění instancí tříd entit použijte metodu *negenerickými. Remove* .

Pokud entita v databázi již existuje, bude odstraněna během *metody SaveChanges*. Pokud entita ještě nebyla uložena do databáze (to znamená, že je sledována jako přidaná), bude odebrána z kontextu a nebude již vložena při volání *metody SaveChanges* .

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Více operací v jednom SaveChanges

Můžete zkombinovat více operací Přidat/aktualizovat/odebrat do jednoho volání *metody SaveChanges*.

> [!NOTE]  
> Pro většinu poskytovatelů databáze je *SaveChanges* transakční. To znamená, že všechny operace budou buď úspěšné, nebo selžou a operace se nikdy nepoužijí částečně.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
