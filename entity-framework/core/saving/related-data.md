---
title: "Ukládání souvisejících dat – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: 078879163002cb66e0f0f439415789963181ec15
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="saving-related-data"></a><span data-ttu-id="e938e-102">Ukládání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="e938e-102">Saving Related Data</span></span>

<span data-ttu-id="e938e-103">Kromě izolované entity, můžete provést také použít vztazích definovaných v modelu.</span><span class="sxs-lookup"><span data-stu-id="e938e-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="e938e-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="e938e-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="e938e-105">Přidání graf nové entity</span><span class="sxs-lookup"><span data-stu-id="e938e-105">Adding a graph of new entities</span></span>

<span data-ttu-id="e938e-106">Pokud vytváříte několik entit v nové relaci, přidání jeden z nich do kontextu způsobí ostatním uživatelům příliš přidat.</span><span class="sxs-lookup"><span data-stu-id="e938e-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="e938e-107">V následujícím příkladu blogu a tři související příspěvky jsou všechny vloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="e938e-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="e938e-108">V příspěvcích jsou vyhledána a přidat, protože jsou dostupné prostřednictvím `Blog.Posts` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e938e-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a><span data-ttu-id="e938e-109">Přidání související entity</span><span class="sxs-lookup"><span data-stu-id="e938e-109">Adding a related entity</span></span>

<span data-ttu-id="e938e-110">Pokud odkazujete nové entity z navigační vlastnosti typu entity, která je již sledován pomocí kontextu, entita, se zjistí a vložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="e938e-110">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="e938e-111">V následujícím příkladu `post` entity je vložit, protože je přidán do `Posts` vlastnost `blog` entity, která byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="e938e-111">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="e938e-112">Změna vztahy</span><span class="sxs-lookup"><span data-stu-id="e938e-112">Changing relationships</span></span>

<span data-ttu-id="e938e-113">Pokud změníte navigační vlastnost entity, budou provedeny změny odpovídající sloupec cizího klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="e938e-113">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="e938e-114">V následujícím příkladu `post` entity se aktualizuje na patří do nového `blog` entity protože jeho `Blog` navigační vlastnost je nastavena tak, aby odkazoval na `blog`.</span><span class="sxs-lookup"><span data-stu-id="e938e-114">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="e938e-115">Všimněte si, že `blog` bude také vložit do databáze vzhledem k tomu, že je nové entity, který odkazuje navigační vlastnost z entity, která je již sledován pomocí kontextu (`post`).</span><span class="sxs-lookup"><span data-stu-id="e938e-115">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="e938e-116">Odstranění relace</span><span class="sxs-lookup"><span data-stu-id="e938e-116">Removing relationships</span></span>

<span data-ttu-id="e938e-117">Vztahu můžete odebrat nastavením navigační odkaz na `null`, nebo odebrání z kolekce navigační související entity.</span><span class="sxs-lookup"><span data-stu-id="e938e-117">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="e938e-118">Odebrání vztahu může mít vedlejší účinky na závislé entity, podle kaskádové odstranění chování nakonfigurovaném v relaci.</span><span class="sxs-lookup"><span data-stu-id="e938e-118">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="e938e-119">Ve výchozím nastavení pro požadované relace je nakonfigurován chování cascade delete a podřízené/závislé entity se odstraní z databáze.</span><span class="sxs-lookup"><span data-stu-id="e938e-119">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="e938e-120">Pro volitelné relace, není ve výchozím nastavení nakonfigurované kaskádové odstranění, avšak bude nastavena vlastnost cizího klíče na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e938e-120">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="e938e-121">V tématu [požadované a volitelné vztahy](../modeling/relationships.md#required-and-optional-relationships) Další informace o tom, jak lze nakonfigurovat requiredness relací.</span><span class="sxs-lookup"><span data-stu-id="e938e-121">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="e938e-122">V tématu [Cascade Delete](cascade-delete.md) pro další informace o tom, jak odstranit cascade chování fungovat, jak se dá nakonfigurovat explicitně a jak jsou vybrané podle konvence.</span><span class="sxs-lookup"><span data-stu-id="e938e-122">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="e938e-123">V následujícím příkladu je kaskádové odstranění nastaven na vztah mezi `Blog` a `Post`, proto `post` entity se odstraní z databáze.</span><span class="sxs-lookup"><span data-stu-id="e938e-123">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
