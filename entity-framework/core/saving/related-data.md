---
title: Ukládání souvisejících dat – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 7349c57c0dccd3c911178641d3b34a478a4f6194
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994741"
---
# <a name="saving-related-data"></a>Ukládání souvisejících dat

Kromě izolované entity, můžete provést také pomocí relace v modelu definován.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) na Githubu.

## <a name="adding-a-graph-of-new-entities"></a>Přidání grafu nové entity

Pokud vytvoříte několik nových související entity, jeden z jejich přidávání do kontextu způsobí ostatním uživatelům příliš přidat.

V následujícím příkladu blogu a tři související příspěvky jsou všechny vložena do databáze. Příspěvky jsou vyhledána a přidat, protože jsou dostupné prostřednictvím `Blog.Posts` navigační vlastnost.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Vlastnost EntityEntry.State použijte k nastavení stavu právě jednu entitu. Například `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Přidat související entity

Pokud odkazujete na nové entity z navigační vlastnosti entity, která je již sledován pomocí funkce kontextu, entity, se zjistí a vloží do databáze.

V následujícím příkladu `post` je vložena entita, protože se přidal do `Posts` vlastnost `blog` entity, která byla načtena z databáze.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Změna relací

Pokud změníte navigační vlastnost entity, odpovídající změny provedené sloupec cizího klíče v databázi.

V následujícím příkladu `post` entity se aktualizuje a patří k novému `blog` entity protože jeho `Blog` navigační vlastnost je nastavena tak, aby odkazoval na `blog`. Všimněte si, že `blog` se také vloží do databáze vzhledem k tomu, že je nová entita, na který odkazuje vlastnost navigace entita, která je již sledován pomocí funkce kontextu (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Odebírání relací

Vztahu můžete odebrat tak, že nastavíte navigační odkaz `null`, nebo jejich odebrání související entity navigace kolekce.

Odstranění vztahu můžete mají vedlejší účinky na závislé entity, podle kaskádové odstranění chování nakonfigurovaném v relaci.

Ve výchozím nastavení pro požadované relace nakonfigurovat chování cascade delete a entity závislé na/podřízený se odstraní z databáze. Volitelné vztahů, není ve výchozím nastavení nakonfigurované kaskádové odstranění, ale vlastnost cizího klíče se nastaví na hodnotu null.

V tématu [povinné a nepovinné vztahy](../modeling/relationships.md#required-and-optional-relationships) Další informace o konfiguraci requiredness vztahy.

Zobrazit [Kaskádové odstraňování](cascade-delete.md) pro další podrobnosti o tom, jak cascade delete chování fungovat, jak se dají konfigurovat explicitně a jak se vybírají podle konvence.

V následujícím příkladu je nakonfigurovaná kaskádové odstranění na vztah mezi `Blog` a `Post`, takže `post` entitu je odstranit z databáze.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
