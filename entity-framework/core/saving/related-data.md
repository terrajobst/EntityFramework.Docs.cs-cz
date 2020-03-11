---
title: Ukládání souvisejících dat – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417543"
---
# <a name="saving-related-data"></a>Ukládání souvisejících dat

Navíc k izolovaným entitám můžete také využít vztahy definované v modelu.

> [!TIP]  
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) tohoto článku můžete zobrazit na GitHubu.

## <a name="adding-a-graph-of-new-entities"></a>Přidání grafu nových entit

Pokud vytvoříte několik nových souvisejících entit, přidáte jeden z nich do kontextu. tím se přidají i ostatní.

V následujícím příkladu je do databáze vložen blog a tři související příspěvky. Příspěvky byly nalezeny a přidány, protože jsou dosažitelné prostřednictvím navigační vlastnosti `Blog.Posts`.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Vlastnost EntityEntry. State použijte k nastavení stavu pouze jedné entity. například `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Přidání související entity

Pokud odkazujete na novou entitu z navigační vlastnosti entity, která je již sledována kontextem, bude entita zjištěna a vložena do databáze.

V následujícím příkladu je `post` entita vložena, protože je přidána do vlastnosti `Posts` entity `blog`, která byla načtena z databáze.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Změna relací

Změníte-li navigační vlastnost entity, budou provedeny odpovídající změny ve sloupci cizího klíče v databázi.

V následujícím příkladu je entita `post` aktualizována tak, aby patřila nové entitě `blog`, protože její `Blog` navigační vlastnost je nastavena na `blog`. Všimněte si, že `blog` budou také vloženy do databáze, protože se jedná o novou entitu, na kterou odkazuje navigační vlastnost entity, která je již sledována kontextem (`post`).

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Odebírání relací

Relaci můžete odebrat nastavením referenční navigace na `null`nebo odebráním související entity z navigace kolekce.

Odebrání vztahu může mít vedlejší účinky na závislé entitě v závislosti na chování při odstraňování kaskády nakonfigurovaném v relaci.

Ve výchozím nastavení se pro požadované relace nakonfiguruje chování kaskádového odstraňování a entita podřízená/závislá se odstraní z databáze. U volitelných relací není kaskádové odstranění ve výchozím nastavení nakonfigurováno, ale vlastnost cizího klíče bude nastavena na hodnotu null.

Informace o tom, jak se dá nakonfigurovat požadovaná četnost vztahů, najdete v tématu [povinné a volitelné vztahy](../modeling/relationships.md#required-and-optional-relationships) .

Další informace o tom, jak chování při odstraňování chování funguje, najdete v části [kaskádové odstraňování](cascade-delete.md) , jak je možné nakonfigurovat explicitně a jak jsou zvoleni podle konvence.

V následujícím příkladu je kaskádové odstranění nakonfigurované na relaci mezi `Blog` a `Post`, takže se entita `post` z databáze odstraní.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
