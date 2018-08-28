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
# <a name="saving-related-data"></a><span data-ttu-id="05bba-102">Ukládání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="05bba-102">Saving Related Data</span></span>

<span data-ttu-id="05bba-103">Kromě izolované entity, můžete provést také pomocí relace v modelu definován.</span><span class="sxs-lookup"><span data-stu-id="05bba-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="05bba-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="05bba-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="05bba-105">Přidání grafu nové entity</span><span class="sxs-lookup"><span data-stu-id="05bba-105">Adding a graph of new entities</span></span>

<span data-ttu-id="05bba-106">Pokud vytvoříte několik nových související entity, jeden z jejich přidávání do kontextu způsobí ostatním uživatelům příliš přidat.</span><span class="sxs-lookup"><span data-stu-id="05bba-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="05bba-107">V následujícím příkladu blogu a tři související příspěvky jsou všechny vložena do databáze.</span><span class="sxs-lookup"><span data-stu-id="05bba-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="05bba-108">Příspěvky jsou vyhledána a přidat, protože jsou dostupné prostřednictvím `Blog.Posts` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="05bba-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="05bba-109">Vlastnost EntityEntry.State použijte k nastavení stavu právě jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="05bba-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="05bba-110">Například `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="05bba-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="05bba-111">Přidat související entity</span><span class="sxs-lookup"><span data-stu-id="05bba-111">Adding a related entity</span></span>

<span data-ttu-id="05bba-112">Pokud odkazujete na nové entity z navigační vlastnosti entity, která je již sledován pomocí funkce kontextu, entity, se zjistí a vloží do databáze.</span><span class="sxs-lookup"><span data-stu-id="05bba-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="05bba-113">V následujícím příkladu `post` je vložena entita, protože se přidal do `Posts` vlastnost `blog` entity, která byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="05bba-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="05bba-114">Změna relací</span><span class="sxs-lookup"><span data-stu-id="05bba-114">Changing relationships</span></span>

<span data-ttu-id="05bba-115">Pokud změníte navigační vlastnost entity, odpovídající změny provedené sloupec cizího klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="05bba-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="05bba-116">V následujícím příkladu `post` entity se aktualizuje a patří k novému `blog` entity protože jeho `Blog` navigační vlastnost je nastavena tak, aby odkazoval na `blog`.</span><span class="sxs-lookup"><span data-stu-id="05bba-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="05bba-117">Všimněte si, že `blog` se také vloží do databáze vzhledem k tomu, že je nová entita, na který odkazuje vlastnost navigace entita, která je již sledován pomocí funkce kontextu (`post`).</span><span class="sxs-lookup"><span data-stu-id="05bba-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="05bba-118">Odebírání relací</span><span class="sxs-lookup"><span data-stu-id="05bba-118">Removing relationships</span></span>

<span data-ttu-id="05bba-119">Vztahu můžete odebrat tak, že nastavíte navigační odkaz `null`, nebo jejich odebrání související entity navigace kolekce.</span><span class="sxs-lookup"><span data-stu-id="05bba-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="05bba-120">Odstranění vztahu můžete mají vedlejší účinky na závislé entity, podle kaskádové odstranění chování nakonfigurovaném v relaci.</span><span class="sxs-lookup"><span data-stu-id="05bba-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="05bba-121">Ve výchozím nastavení pro požadované relace nakonfigurovat chování cascade delete a entity závislé na/podřízený se odstraní z databáze.</span><span class="sxs-lookup"><span data-stu-id="05bba-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="05bba-122">Volitelné vztahů, není ve výchozím nastavení nakonfigurované kaskádové odstranění, ale vlastnost cizího klíče se nastaví na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="05bba-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="05bba-123">V tématu [povinné a nepovinné vztahy](../modeling/relationships.md#required-and-optional-relationships) Další informace o konfiguraci requiredness vztahy.</span><span class="sxs-lookup"><span data-stu-id="05bba-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="05bba-124">Zobrazit [Kaskádové odstraňování](cascade-delete.md) pro další podrobnosti o tom, jak cascade delete chování fungovat, jak se dají konfigurovat explicitně a jak se vybírají podle konvence.</span><span class="sxs-lookup"><span data-stu-id="05bba-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="05bba-125">V následujícím příkladu je nakonfigurovaná kaskádové odstranění na vztah mezi `Blog` a `Post`, takže `post` entitu je odstranit z databáze.</span><span class="sxs-lookup"><span data-stu-id="05bba-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
