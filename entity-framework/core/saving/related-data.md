---
title: Ukládání souvisejících dat – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 45c7b8e4bfa4ce7967ad76ef4a7d4818b0d3aebf
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197891"
---
# <a name="saving-related-data"></a><span data-ttu-id="63c0e-102">Ukládání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="63c0e-102">Saving Related Data</span></span>

<span data-ttu-id="63c0e-103">Navíc k izolovaným entitám můžete také využít vztahy definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="63c0e-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="63c0e-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="63c0e-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="63c0e-105">Přidání grafu nových entit</span><span class="sxs-lookup"><span data-stu-id="63c0e-105">Adding a graph of new entities</span></span>

<span data-ttu-id="63c0e-106">Pokud vytvoříte několik nových souvisejících entit, přidáte jeden z nich do kontextu. tím se přidají i ostatní.</span><span class="sxs-lookup"><span data-stu-id="63c0e-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="63c0e-107">V následujícím příkladu je do databáze vložen blog a tři související příspěvky.</span><span class="sxs-lookup"><span data-stu-id="63c0e-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="63c0e-108">Příspěvky byly nalezeny a přidány, protože jsou dosažitelné prostřednictvím `Blog.Posts` navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="63c0e-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="63c0e-109">Vlastnost EntityEntry. State použijte k nastavení stavu pouze jedné entity.</span><span class="sxs-lookup"><span data-stu-id="63c0e-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="63c0e-110">Například, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="63c0e-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="63c0e-111">Přidání související entity</span><span class="sxs-lookup"><span data-stu-id="63c0e-111">Adding a related entity</span></span>

<span data-ttu-id="63c0e-112">Pokud odkazujete na novou entitu z navigační vlastnosti entity, která je již sledována kontextem, bude entita zjištěna a vložena do databáze.</span><span class="sxs-lookup"><span data-stu-id="63c0e-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="63c0e-113">V následujícím příkladu je `post` entita vložena, protože je přidána `Posts` do vlastnosti `blog` entity, která byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="63c0e-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="63c0e-114">Změna relací</span><span class="sxs-lookup"><span data-stu-id="63c0e-114">Changing relationships</span></span>

<span data-ttu-id="63c0e-115">Změníte-li navigační vlastnost entity, budou provedeny odpovídající změny ve sloupci cizího klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="63c0e-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="63c0e-116">V následujícím příkladu `post` je entita aktualizována tak, aby patřila nové `blog` entitě, protože její `Blog` vlastnost navigace je nastavena na hodnotu Point `blog`.</span><span class="sxs-lookup"><span data-stu-id="63c0e-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="63c0e-117">Všimněte si `blog` , že bude také vložen do databáze, protože se jedná o novou entitu, na kterou odkazuje vlastnost navigace entity, která je již sledována kontextem (`post`).</span><span class="sxs-lookup"><span data-stu-id="63c0e-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="63c0e-118">Odebírání relací</span><span class="sxs-lookup"><span data-stu-id="63c0e-118">Removing relationships</span></span>

<span data-ttu-id="63c0e-119">Relaci můžete odebrat nastavením referenční navigace na `null`nebo odebráním související entity z navigace kolekce.</span><span class="sxs-lookup"><span data-stu-id="63c0e-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="63c0e-120">Odebrání vztahu může mít vedlejší účinky na závislé entitě v závislosti na chování při odstraňování kaskády nakonfigurovaném v relaci.</span><span class="sxs-lookup"><span data-stu-id="63c0e-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="63c0e-121">Ve výchozím nastavení se pro požadované relace nakonfiguruje chování kaskádového odstraňování a entita podřízená/závislá se odstraní z databáze.</span><span class="sxs-lookup"><span data-stu-id="63c0e-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="63c0e-122">U volitelných relací není kaskádové odstranění ve výchozím nastavení nakonfigurováno, ale vlastnost cizího klíče bude nastavena na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="63c0e-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="63c0e-123">Informace o tom, jak se dá nakonfigurovat požadovaná četnost vztahů, najdete v tématu [povinné a volitelné vztahy](../modeling/relationships.md#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="63c0e-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="63c0e-124">Další informace o tom, jak chování při odstraňování chování funguje, najdete v části [kaskádové odstraňování](cascade-delete.md) , jak je možné nakonfigurovat explicitně a jak jsou zvoleni podle konvence.</span><span class="sxs-lookup"><span data-stu-id="63c0e-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="63c0e-125">V následujícím příkladu je kaskádové odstranění nakonfigurované pro relaci mezi `Blog` a `Post`, takže `post` se entita z databáze odstraní.</span><span class="sxs-lookup"><span data-stu-id="63c0e-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
