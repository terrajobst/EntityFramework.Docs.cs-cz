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
# <a name="saving-related-data"></a><span data-ttu-id="00ab2-102">Ukládání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="00ab2-102">Saving Related Data</span></span>

<span data-ttu-id="00ab2-103">Kromě izolovaných entit můžete také využít vztahy definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="00ab2-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="00ab2-104">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="00ab2-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="00ab2-105">Přidání grafu nových entit</span><span class="sxs-lookup"><span data-stu-id="00ab2-105">Adding a graph of new entities</span></span>

<span data-ttu-id="00ab2-106">Pokud vytvoříte několik nových souvisejících entit, přidání jednoho z nich do kontextu způsobí, že ostatní budou přidány také.</span><span class="sxs-lookup"><span data-stu-id="00ab2-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="00ab2-107">V následujícím příkladu jsou do databáze vloženy blog a tři související příspěvky.</span><span class="sxs-lookup"><span data-stu-id="00ab2-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="00ab2-108">Příspěvky jsou nalezeny a přidány, protože `Blog.Posts` jsou dosažitelné prostřednictvím navigačního zařízení.</span><span class="sxs-lookup"><span data-stu-id="00ab2-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="00ab2-109">Vlastnost EntityEntry.State slouží k nastavení stavu pouze jedné entity.</span><span class="sxs-lookup"><span data-stu-id="00ab2-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="00ab2-110">Například, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="00ab2-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="00ab2-111">Přidání související entity</span><span class="sxs-lookup"><span data-stu-id="00ab2-111">Adding a related entity</span></span>

<span data-ttu-id="00ab2-112">Pokud odkazujete na novou entitu z navigační vlastnosti entity, která je již sledována kontextem, bude entita zjištěna a vložena do databáze.</span><span class="sxs-lookup"><span data-stu-id="00ab2-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="00ab2-113">V následujícím příkladu `post` je entita vložena, `Posts` protože `blog` je přidána do vlastnosti entity, která byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="00ab2-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="00ab2-114">Změna vztahů</span><span class="sxs-lookup"><span data-stu-id="00ab2-114">Changing relationships</span></span>

<span data-ttu-id="00ab2-115">Pokud změníte vlastnost navigace entity, budou provedeny odpovídající změny ve sloupci cizího klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="00ab2-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="00ab2-116">V následujícím příkladu `post` je entita aktualizována tak, aby patřila k nové `blog` entitě, protože její `Blog` navigační vlastnost je nastavena na bod na `blog`.</span><span class="sxs-lookup"><span data-stu-id="00ab2-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="00ab2-117">Všimněte `blog` si, že bude také vložen do databáze, protože se jedná o novou entitu, na`post`kterou odkazuje navigační vlastnost entity, která je již sledována kontextem ( ).</span><span class="sxs-lookup"><span data-stu-id="00ab2-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="00ab2-118">Odebrání relací</span><span class="sxs-lookup"><span data-stu-id="00ab2-118">Removing relationships</span></span>

<span data-ttu-id="00ab2-119">Vztah můžete odebrat nastavením navigace `null`s odkazem na nebo odebráním související entity z navigace kolekce.</span><span class="sxs-lookup"><span data-stu-id="00ab2-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="00ab2-120">Odebrání relace může mít vedlejší účinky na závislou entitu podle chování kaskádového odstranění nakonfigurovaného ve vztahu.</span><span class="sxs-lookup"><span data-stu-id="00ab2-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="00ab2-121">Ve výchozím nastavení je pro požadované relace nakonfigurováno chování kaskádového odstranění a podřízená/závislá entita bude odstraněna z databáze.</span><span class="sxs-lookup"><span data-stu-id="00ab2-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="00ab2-122">U volitelných relací není kaskádové odstranění ve výchozím nastavení nakonfigurováno, ale vlastnost cizího klíče bude nastavena na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="00ab2-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="00ab2-123">Informace o tom, jak lze konfigurovat požadovanost vztahů, naleznete v [tématu Povinné a volitelné relace.](../modeling/relationships.md#required-and-optional-relationships)</span><span class="sxs-lookup"><span data-stu-id="00ab2-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="00ab2-124">Další podrobnosti o tom, jak funguje chování kaskádového odstranění, jak je lze explicitně konfigurovat a jak jsou vybrány podle konvence, najdete v tématu [Kaskádové odstranění.](cascade-delete.md)</span><span class="sxs-lookup"><span data-stu-id="00ab2-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="00ab2-125">V následujícím příkladu je nakonfigurováno kaskádové `Post`odstranění `post` vztahu mezi `Blog` a , takže entita je odstraněna z databáze.</span><span class="sxs-lookup"><span data-stu-id="00ab2-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
