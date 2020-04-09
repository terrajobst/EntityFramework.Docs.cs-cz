---
title: Ukládání souvisejících dat - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417543"
---
# <a name="saving-related-data"></a>Ukládání souvisejících dat

Kromě izolovaných entit můžete také využít vztahy definované v modelu.

> [!TIP]  
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) můžete zobrazit na GitHubu.

## <a name="adding-a-graph-of-new-entities"></a>Přidání grafu nových entit

Pokud vytvoříte několik nových souvisejících entit, přidání jednoho z nich do kontextu způsobí, že ostatní budou přidány také.

V následujícím příkladu jsou do databáze vloženy blog a tři související příspěvky. Příspěvky jsou nalezeny a přidány, protože `Blog.Posts` jsou dosažitelné prostřednictvím navigačního zařízení.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Vlastnost EntityEntry.State slouží k nastavení stavu pouze jedné entity. Například, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Přidání související entity

Pokud odkazujete na novou entitu z navigační vlastnosti entity, která je již sledována kontextem, bude entita zjištěna a vložena do databáze.

V následujícím příkladu `post` je entita vložena, `Posts` protože `blog` je přidána do vlastnosti entity, která byla načtena z databáze.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Změna vztahů

Pokud změníte vlastnost navigace entity, budou provedeny odpovídající změny ve sloupci cizího klíče v databázi.

V následujícím příkladu `post` je entita aktualizována tak, aby patřila k nové `blog` entitě, protože její `Blog` navigační vlastnost je nastavena na bod na `blog`. Všimněte `blog` si, že bude také vložen do databáze, protože se jedná o novou entitu, na`post`kterou odkazuje navigační vlastnost entity, která je již sledována kontextem ( ).

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Odebrání relací

Vztah můžete odebrat nastavením navigace `null`s odkazem na nebo odebráním související entity z navigace kolekce.

Odebrání relace může mít vedlejší účinky na závislou entitu podle chování kaskádového odstranění nakonfigurovaného ve vztahu.

Ve výchozím nastavení je pro požadované relace nakonfigurováno chování kaskádového odstranění a podřízená/závislá entita bude odstraněna z databáze. U volitelných relací není kaskádové odstranění ve výchozím nastavení nakonfigurováno, ale vlastnost cizího klíče bude nastavena na hodnotu null.

Informace o tom, jak lze konfigurovat požadovanost vztahů, naleznete v [tématu Povinné a volitelné relace.](../modeling/relationships.md#required-and-optional-relationships)

Další podrobnosti o tom, jak funguje chování kaskádového odstranění, jak je lze explicitně konfigurovat a jak jsou vybrány podle konvence, najdete v tématu [Kaskádové odstranění.](cascade-delete.md)

V následujícím příkladu je nakonfigurováno kaskádové `Post`odstranění `post` vztahu mezi `Blog` a , takže entita je odstraněna z databáze.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
