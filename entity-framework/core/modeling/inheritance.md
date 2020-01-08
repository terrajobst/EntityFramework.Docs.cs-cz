---
title: Dědičnost – EF Core
description: Jak nakonfigurovat dědičnost typu entity pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502162"
---
# <a name="inheritance"></a><span data-ttu-id="72612-103">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="72612-103">Inheritance</span></span>

<span data-ttu-id="72612-104">EF může mapovat hierarchii typů .NET na databázi.</span><span class="sxs-lookup"><span data-stu-id="72612-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="72612-105">To vám umožňuje psát entity .NET v kódu jako obvykle pomocí základních a odvozených typů a mít EF bez problémů vytvořit příslušné schéma databáze, vydávat dotazy atd. Skutečné podrobnosti o mapování hierarchie typů jsou závislé na poskytovateli. Tato stránka popisuje podporu dědičnosti v kontextu relační databáze.</span><span class="sxs-lookup"><span data-stu-id="72612-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

<span data-ttu-id="72612-106">V tuto chvíli EF Core podporuje jenom vzor typu tabulka na hierarchii (TPH).</span><span class="sxs-lookup"><span data-stu-id="72612-106">At the moment, EF Core only supports the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="72612-107">Typ TPH používá jedinou tabulku k uložení dat pro všechny typy v hierarchii a sloupec diskriminátoru slouží k identifikaci, který typ každý řádek představuje.</span><span class="sxs-lookup"><span data-stu-id="72612-107">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

> [!NOTE]
> <span data-ttu-id="72612-108">TPT se zatím nepodporují EF Core pomocí tabulky pro typ (TPC) a tabulky podle konkrétního typu (), které podporuje EF6.</span><span class="sxs-lookup"><span data-stu-id="72612-108">The table-per-type (TPT) and table-per-concrete-type (TPC), which are supported by EF6, are not yet supported by EF Core.</span></span> <span data-ttu-id="72612-109">TPT je hlavní funkce naplánovaná pro EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="72612-109">TPT is a major feature planned for EF Core 5.0.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="72612-110">Mapování hierarchie typu entity</span><span class="sxs-lookup"><span data-stu-id="72612-110">Entity type hierarchy mapping</span></span>

<span data-ttu-id="72612-111">Podle konvence, EF nastaví dědění pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů.</span><span class="sxs-lookup"><span data-stu-id="72612-111">By convention, EF will only set up inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="72612-112">EF nebude automaticky vyhledávat základní nebo odvozené typy, které nejsou jinak zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="72612-112">EF will not automatically scan for base or derived types that are not otherwise included in the model.</span></span>

<span data-ttu-id="72612-113">Do modelu můžete zahrnout typy tím, že vystavíte Negenerickými pro každý typ v hierarchii dědičnosti:</span><span class="sxs-lookup"><span data-stu-id="72612-113">You can include types in the model by exposing a DbSet for each type in the inheritance hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

<span data-ttu-id="72612-114">Tento model je namapován na následující schéma databáze (Poznamenejte si implicitně vytvořený sloupec *diskriminátorů* , který určuje, který typ *blogu* je uložený v jednotlivých řádcích):</span><span class="sxs-lookup"><span data-stu-id="72612-114">This model be mapped to the following database schema (note the implicitly-created *Discriminator* column, which identifies which type of *Blog* is stored in each row):</span></span>

![obrázek](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="72612-116">Sloupce databáze se při použití mapování typu TPH automaticky v případě potřeby nepovolují s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="72612-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="72612-117">Například sloupec *RssUrl* může mít hodnotu null, protože běžné instance *blogu* tuto vlastnost nemají.</span><span class="sxs-lookup"><span data-stu-id="72612-117">For example, the *RssUrl* column is nullable because regular *Blog* instances do not have that property.</span></span>

<span data-ttu-id="72612-118">Pokud nechcete zveřejnit Negenerickými pro jednu nebo více entit v hierarchii, můžete také použít rozhraní Fluent API, abyste zajistili, že jsou zahrnuté v modelu.</span><span class="sxs-lookup"><span data-stu-id="72612-118">If you don't want to expose a DbSet for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="72612-119">Pokud nespoléháte na konvence, můžete základní typ explicitně zadat pomocí `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="72612-119">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="72612-120">K odebrání typu entity z hierarchie můžete použít taky `.HasBaseType((Type)null)`.</span><span class="sxs-lookup"><span data-stu-id="72612-120">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="discriminator-configuration"></a><span data-ttu-id="72612-121">Konfigurace diskriminátorů</span><span class="sxs-lookup"><span data-stu-id="72612-121">Discriminator configuration</span></span>

<span data-ttu-id="72612-122">Můžete nakonfigurovat název a typ sloupce diskriminátoru a hodnoty, které se použijí k identifikaci každého typu v hierarchii:</span><span class="sxs-lookup"><span data-stu-id="72612-122">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="72612-123">V předchozích příkladech byl EF přidán diskriminátor implicitně jako [vlastnost Shadow](xref:core/modeling/shadow-properties) v základní entitě hierarchie.</span><span class="sxs-lookup"><span data-stu-id="72612-123">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="72612-124">Tato vlastnost se dá nakonfigurovat jako jakákoli jiná:</span><span class="sxs-lookup"><span data-stu-id="72612-124">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="72612-125">A konečně může být diskriminátor také mapován na regulární vlastnost .NET v entitě:</span><span class="sxs-lookup"><span data-stu-id="72612-125">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a><span data-ttu-id="72612-126">Sdílené sloupce</span><span class="sxs-lookup"><span data-stu-id="72612-126">Shared columns</span></span>

<span data-ttu-id="72612-127">Ve výchozím nastavení, když dva typy entit na stejné úrovni v hierarchii mají vlastnost se stejným názvem, budou namapovány na dva samostatné sloupce.</span><span class="sxs-lookup"><span data-stu-id="72612-127">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="72612-128">Pokud je však jejich typ identický, lze je namapovat na stejný sloupec databáze:</span><span class="sxs-lookup"><span data-stu-id="72612-128">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
