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
# <a name="saving-related-data"></a><span data-ttu-id="c028f-102">Ukládání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="c028f-102">Saving Related Data</span></span>

<span data-ttu-id="c028f-103">Navíc k izolovaným entitám můžete také využít vztahy definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="c028f-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="c028f-104">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="c028f-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="c028f-105">Přidání grafu nových entit</span><span class="sxs-lookup"><span data-stu-id="c028f-105">Adding a graph of new entities</span></span>

<span data-ttu-id="c028f-106">Pokud vytvoříte několik nových souvisejících entit, přidáte jeden z nich do kontextu. tím se přidají i ostatní.</span><span class="sxs-lookup"><span data-stu-id="c028f-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="c028f-107">V následujícím příkladu je do databáze vložen blog a tři související příspěvky.</span><span class="sxs-lookup"><span data-stu-id="c028f-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="c028f-108">Příspěvky byly nalezeny a přidány, protože jsou dosažitelné prostřednictvím navigační vlastnosti `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="c028f-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="c028f-109">Vlastnost EntityEntry. State použijte k nastavení stavu pouze jedné entity.</span><span class="sxs-lookup"><span data-stu-id="c028f-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="c028f-110">například `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="c028f-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="c028f-111">Přidání související entity</span><span class="sxs-lookup"><span data-stu-id="c028f-111">Adding a related entity</span></span>

<span data-ttu-id="c028f-112">Pokud odkazujete na novou entitu z navigační vlastnosti entity, která je již sledována kontextem, bude entita zjištěna a vložena do databáze.</span><span class="sxs-lookup"><span data-stu-id="c028f-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="c028f-113">V následujícím příkladu je `post` entita vložena, protože je přidána do vlastnosti `Posts` entity `blog`, která byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="c028f-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="c028f-114">Změna relací</span><span class="sxs-lookup"><span data-stu-id="c028f-114">Changing relationships</span></span>

<span data-ttu-id="c028f-115">Změníte-li navigační vlastnost entity, budou provedeny odpovídající změny ve sloupci cizího klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="c028f-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="c028f-116">V následujícím příkladu je entita `post` aktualizována tak, aby patřila nové entitě `blog`, protože její `Blog` navigační vlastnost je nastavena na `blog`.</span><span class="sxs-lookup"><span data-stu-id="c028f-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="c028f-117">Všimněte si, že `blog` budou také vloženy do databáze, protože se jedná o novou entitu, na kterou odkazuje navigační vlastnost entity, která je již sledována kontextem (`post`).</span><span class="sxs-lookup"><span data-stu-id="c028f-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="c028f-118">Odebírání relací</span><span class="sxs-lookup"><span data-stu-id="c028f-118">Removing relationships</span></span>

<span data-ttu-id="c028f-119">Relaci můžete odebrat nastavením referenční navigace na `null`nebo odebráním související entity z navigace kolekce.</span><span class="sxs-lookup"><span data-stu-id="c028f-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="c028f-120">Odebrání vztahu může mít vedlejší účinky na závislé entitě v závislosti na chování při odstraňování kaskády nakonfigurovaném v relaci.</span><span class="sxs-lookup"><span data-stu-id="c028f-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="c028f-121">Ve výchozím nastavení se pro požadované relace nakonfiguruje chování kaskádového odstraňování a entita podřízená/závislá se odstraní z databáze.</span><span class="sxs-lookup"><span data-stu-id="c028f-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="c028f-122">U volitelných relací není kaskádové odstranění ve výchozím nastavení nakonfigurováno, ale vlastnost cizího klíče bude nastavena na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c028f-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="c028f-123">Informace o tom, jak se dá nakonfigurovat požadovaná četnost vztahů, najdete v tématu [povinné a volitelné vztahy](../modeling/relationships.md#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="c028f-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="c028f-124">Další informace o tom, jak chování při odstraňování chování funguje, najdete v části [kaskádové odstraňování](cascade-delete.md) , jak je možné nakonfigurovat explicitně a jak jsou zvoleni podle konvence.</span><span class="sxs-lookup"><span data-stu-id="c028f-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="c028f-125">V následujícím příkladu je kaskádové odstranění nakonfigurované na relaci mezi `Blog` a `Post`, takže se entita `post` z databáze odstraní.</span><span class="sxs-lookup"><span data-stu-id="c028f-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
