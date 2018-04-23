---
title: Ukládání souvisejících dat – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a>Ukládání souvisejících dat

Kromě izolované entity, můžete provést také použít vztazích definovaných v modelu.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) na Githubu.

## <a name="adding-a-graph-of-new-entities"></a>Přidání graf nové entity

Pokud vytváříte několik entit v nové relaci, přidání jeden z nich do kontextu způsobí ostatním uživatelům příliš přidat.

V následujícím příkladu blogu a tři související příspěvky jsou všechny vloženy do databáze. V příspěvcích jsou vyhledána a přidat, protože jsou dostupné prostřednictvím `Blog.Posts` navigační vlastnost.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Použijte EntityEntry.State vlastnost pro nastavení stavu právě jednu entitu. Například `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Přidání související entity

Pokud odkazujete nové entity z navigační vlastnosti typu entity, která je již sledován pomocí kontextu, entita, se zjistí a vložit do databáze.

V následujícím příkladu `post` entity je vložit, protože je přidán do `Posts` vlastnost `blog` entity, která byla načtena z databáze.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Změna vztahy

Pokud změníte navigační vlastnost entity, budou provedeny změny odpovídající sloupec cizího klíče v databázi.

V následujícím příkladu `post` entity se aktualizuje na patří do nového `blog` entity protože jeho `Blog` navigační vlastnost je nastavena tak, aby odkazoval na `blog`. Všimněte si, že `blog` bude také vložit do databáze vzhledem k tomu, že je nové entity, který odkazuje navigační vlastnost z entity, která je již sledován pomocí kontextu (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Odstranění relace

Vztahu můžete odebrat nastavením navigační odkaz na `null`, nebo odebrání z kolekce navigační související entity.

Odebrání vztahu může mít vedlejší účinky na závislé entity, podle kaskádové odstranění chování nakonfigurovaném v relaci.

Ve výchozím nastavení pro požadované relace je nakonfigurován chování cascade delete a podřízené/závislé entity se odstraní z databáze. Pro volitelné relace, není ve výchozím nastavení nakonfigurované kaskádové odstranění, avšak bude nastavena vlastnost cizího klíče na hodnotu null.

V tématu [požadované a volitelné vztahy](../modeling/relationships.md#required-and-optional-relationships) Další informace o tom, jak lze nakonfigurovat requiredness relací.

V tématu [Cascade Delete](cascade-delete.md) pro další informace o tom, jak odstranit cascade chování fungovat, jak se dá nakonfigurovat explicitně a jak jsou vybrané podle konvence.

V následujícím příkladu je kaskádové odstranění nastaven na vztah mezi `Blog` a `Post`, proto `post` entity se odstraní z databáze.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
