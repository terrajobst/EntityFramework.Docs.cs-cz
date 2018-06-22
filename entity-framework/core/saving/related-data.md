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
ms.locfileid: "31006647"
---
# <a name="saving-related-data"></a><span data-ttu-id="d041b-102">Ukládání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="d041b-102">Saving Related Data</span></span>

<span data-ttu-id="d041b-103">Kromě izolované entity, můžete provést také použít vztazích definovaných v modelu.</span><span class="sxs-lookup"><span data-stu-id="d041b-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="d041b-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d041b-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="d041b-105">Přidání graf nové entity</span><span class="sxs-lookup"><span data-stu-id="d041b-105">Adding a graph of new entities</span></span>

<span data-ttu-id="d041b-106">Pokud vytváříte několik entit v nové relaci, přidání jeden z nich do kontextu způsobí ostatním uživatelům příliš přidat.</span><span class="sxs-lookup"><span data-stu-id="d041b-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="d041b-107">V následujícím příkladu blogu a tři související příspěvky jsou všechny vloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="d041b-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="d041b-108">V příspěvcích jsou vyhledána a přidat, protože jsou dostupné prostřednictvím `Blog.Posts` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d041b-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="d041b-109">Použijte EntityEntry.State vlastnost pro nastavení stavu právě jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="d041b-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="d041b-110">Například `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="d041b-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="d041b-111">Přidání související entity</span><span class="sxs-lookup"><span data-stu-id="d041b-111">Adding a related entity</span></span>

<span data-ttu-id="d041b-112">Pokud odkazujete nové entity z navigační vlastnosti typu entity, která je již sledován pomocí kontextu, entita, se zjistí a vložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="d041b-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="d041b-113">V následujícím příkladu `post` entity je vložit, protože je přidán do `Posts` vlastnost `blog` entity, která byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="d041b-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="d041b-114">Změna vztahy</span><span class="sxs-lookup"><span data-stu-id="d041b-114">Changing relationships</span></span>

<span data-ttu-id="d041b-115">Pokud změníte navigační vlastnost entity, budou provedeny změny odpovídající sloupec cizího klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="d041b-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="d041b-116">V následujícím příkladu `post` entity se aktualizuje na patří do nového `blog` entity protože jeho `Blog` navigační vlastnost je nastavena tak, aby odkazoval na `blog`.</span><span class="sxs-lookup"><span data-stu-id="d041b-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="d041b-117">Všimněte si, že `blog` bude také vložit do databáze vzhledem k tomu, že je nové entity, který odkazuje navigační vlastnost z entity, která je již sledován pomocí kontextu (`post`).</span><span class="sxs-lookup"><span data-stu-id="d041b-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="d041b-118">Odstranění relace</span><span class="sxs-lookup"><span data-stu-id="d041b-118">Removing relationships</span></span>

<span data-ttu-id="d041b-119">Vztahu můžete odebrat nastavením navigační odkaz na `null`, nebo odebrání z kolekce navigační související entity.</span><span class="sxs-lookup"><span data-stu-id="d041b-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="d041b-120">Odebrání vztahu může mít vedlejší účinky na závislé entity, podle kaskádové odstranění chování nakonfigurovaném v relaci.</span><span class="sxs-lookup"><span data-stu-id="d041b-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="d041b-121">Ve výchozím nastavení pro požadované relace je nakonfigurován chování cascade delete a podřízené/závislé entity se odstraní z databáze.</span><span class="sxs-lookup"><span data-stu-id="d041b-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="d041b-122">Pro volitelné relace, není ve výchozím nastavení nakonfigurované kaskádové odstranění, avšak bude nastavena vlastnost cizího klíče na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d041b-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="d041b-123">V tématu [požadované a volitelné vztahy](../modeling/relationships.md#required-and-optional-relationships) Další informace o tom, jak lze nakonfigurovat requiredness relací.</span><span class="sxs-lookup"><span data-stu-id="d041b-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="d041b-124">V tématu [Cascade Delete](cascade-delete.md) pro další informace o tom, jak odstranit cascade chování fungovat, jak se dá nakonfigurovat explicitně a jak jsou vybrané podle konvence.</span><span class="sxs-lookup"><span data-stu-id="d041b-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="d041b-125">V následujícím příkladu je kaskádové odstranění nastaven na vztah mezi `Blog` a `Post`, proto `post` entity se odstraní z databáze.</span><span class="sxs-lookup"><span data-stu-id="d041b-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
