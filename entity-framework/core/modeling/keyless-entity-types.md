---
title: Typy entit bez klíčů – EF Core
description: Jak nakonfigurovat typy entit bez klíčů pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 129e24b154ba32583435aeb742dbf478350344e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824659"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="8c04b-103">Typy entit bez klíčů</span><span class="sxs-lookup"><span data-stu-id="8c04b-103">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="8c04b-104">Tato funkce byla přidána do EF Core 2,1 pod názvem typů dotazů.</span><span class="sxs-lookup"><span data-stu-id="8c04b-104">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="8c04b-105">V EF Core 3,0 byl koncept přejmenován na bez klíčů entity Types.</span><span class="sxs-lookup"><span data-stu-id="8c04b-105">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="8c04b-106">Kromě běžných typů entit může EF Core model obsahovat _typy entit bez klíčů_, které lze použít k provádění databázových dotazů na data, která neobsahují klíčové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8c04b-106">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="8c04b-107">Vlastnosti typů entit bez klíčů</span><span class="sxs-lookup"><span data-stu-id="8c04b-107">Keyless entity types characteristics</span></span>

<span data-ttu-id="8c04b-108">Typy entit bez klíčů podporují mnoho stejných funkcí mapování jako regulární typy entit, jako je mapování dědičnosti a vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="8c04b-108">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="8c04b-109">Na relační úložiště můžete nakonfigurovat cílové objektů databáze a sloupce pomocí metody fluent API nebo datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="8c04b-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="8c04b-110">Liší se však od regulárních typů entit v tom, že:</span><span class="sxs-lookup"><span data-stu-id="8c04b-110">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="8c04b-111">Nelze definovat klíč.</span><span class="sxs-lookup"><span data-stu-id="8c04b-111">Cannot have a key defined.</span></span>
- <span data-ttu-id="8c04b-112">Nejsou sledovány pro změny v _DbContext_ , a proto nejsou nikdy vloženy, aktualizovány ani smazány v databázi.</span><span class="sxs-lookup"><span data-stu-id="8c04b-112">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="8c04b-113">Nikdy zjištění konvencí.</span><span class="sxs-lookup"><span data-stu-id="8c04b-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="8c04b-114">Podporuje pouze podmnožinu možností mapování navigace, konkrétně:</span><span class="sxs-lookup"><span data-stu-id="8c04b-114">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="8c04b-115">Může se nikdy fungovat jako hlavní konec relace.</span><span class="sxs-lookup"><span data-stu-id="8c04b-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="8c04b-116">Nemusí mít navigace ke vlastněným entitám.</span><span class="sxs-lookup"><span data-stu-id="8c04b-116">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="8c04b-117">Mohou obsahovat pouze referenční navigační vlastnosti ukazující na běžné entity.</span><span class="sxs-lookup"><span data-stu-id="8c04b-117">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="8c04b-118">Entity nemůžou obsahovat navigační vlastnosti bez klíčů typů entit.</span><span class="sxs-lookup"><span data-stu-id="8c04b-118">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="8c04b-119">Je nutné nakonfigurovat `.HasNoKey()` volání metody.</span><span class="sxs-lookup"><span data-stu-id="8c04b-119">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="8c04b-120">Může být mapován na _definiční dotaz_.</span><span class="sxs-lookup"><span data-stu-id="8c04b-120">May be mapped to a _defining query_.</span></span> <span data-ttu-id="8c04b-121">Definiční dotaz je dotaz deklarovaný v modelu, který slouží jako zdroj dat pro typ entity bez klíčů.</span><span class="sxs-lookup"><span data-stu-id="8c04b-121">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="8c04b-122">Scénáře použití</span><span class="sxs-lookup"><span data-stu-id="8c04b-122">Usage scenarios</span></span>

<span data-ttu-id="8c04b-123">Některé z hlavních scénářů použití pro typy entit bez klíčů jsou:</span><span class="sxs-lookup"><span data-stu-id="8c04b-123">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="8c04b-124">Slouží jako návratový typ pro [nezpracované dotazy SQL](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="8c04b-124">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="8c04b-125">Mapování na zobrazení databáze, která neobsahují primární klíč.</span><span class="sxs-lookup"><span data-stu-id="8c04b-125">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="8c04b-126">Mapování tabulek, které nemají definován primární klíč.</span><span class="sxs-lookup"><span data-stu-id="8c04b-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="8c04b-127">Mapování pro dotazy definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="8c04b-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="8c04b-128">Mapování databázových objektů</span><span class="sxs-lookup"><span data-stu-id="8c04b-128">Mapping to database objects</span></span>

<span data-ttu-id="8c04b-129">Mapování typu entity bez klíčů k databázovému objektu se dosahuje pomocí rozhraní API pro `ToTable` nebo `ToView` Fluent.</span><span class="sxs-lookup"><span data-stu-id="8c04b-129">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="8c04b-130">Z pohledu EF Core je určený v této metodě objekt databáze _zobrazení_, to znamená, že je považován za zdroj dotazu jen pro čtení a nemůže být cílem příkazu update, insert nebo operace odstranění.</span><span class="sxs-lookup"><span data-stu-id="8c04b-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="8c04b-131">To však neznamená, že objekt databáze je skutečně vyžadován pro zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="8c04b-131">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="8c04b-132">Může se případně jednat o databázovou tabulku, která bude považována za jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8c04b-132">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="8c04b-133">U regulárních typů entit EF Core předpokládá, že databázový objekt zadaný v metodě `ToTable` lze považovat za _tabulku_, což znamená, že se dá použít jako zdroj dotazu, ale také cílený na operace aktualizace, odstranění a vložení.</span><span class="sxs-lookup"><span data-stu-id="8c04b-133">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="8c04b-134">Ve skutečnosti můžete zadat název databáze zobrazení v `ToTable` a všechno, co by mělo fungovat bez problémů jako zobrazení konfigurován tak, aby umožnit aktualizaci modelové databáze.</span><span class="sxs-lookup"><span data-stu-id="8c04b-134">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="8c04b-135">`ToView` předpokládá, že objekt již v databázi existuje a nebude vytvořen migracemi.</span><span class="sxs-lookup"><span data-stu-id="8c04b-135">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="8c04b-136">Příklad</span><span class="sxs-lookup"><span data-stu-id="8c04b-136">Example</span></span>

<span data-ttu-id="8c04b-137">Následující příklad ukazuje, jak použít typy entit bez klíčů k dotazování zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="8c04b-137">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="8c04b-138">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="8c04b-138">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="8c04b-139">Nejprve definujte jsme jednoduchý model blogu a příspěvek:</span><span class="sxs-lookup"><span data-stu-id="8c04b-139">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="8c04b-140">Dále nadefinujeme zobrazení jednoduché databáze, které vám umožní nám zjistit počet příspěvků, které jsou spojené s každou blogu:</span><span class="sxs-lookup"><span data-stu-id="8c04b-140">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="8c04b-141">V dalším kroku budeme definovat třídu pro uchování výsledku ze zobrazení databáze:</span><span class="sxs-lookup"><span data-stu-id="8c04b-141">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="8c04b-142">V dalším kroku nakonfigurujeme typ entity bez klíčů v _OnModelCreating_ pomocí rozhraní API pro `HasNoKey`.</span><span class="sxs-lookup"><span data-stu-id="8c04b-142">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="8c04b-143">Rozhraní API pro konfiguraci Fluent používáme ke konfiguraci mapování pro typ entity bez klíčů:</span><span class="sxs-lookup"><span data-stu-id="8c04b-143">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="8c04b-144">Dále nakonfigurujeme `DbContext`, aby zahrnovala `DbSet<T>`:</span><span class="sxs-lookup"><span data-stu-id="8c04b-144">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="8c04b-145">Nakonec jsme zobrazení databáze můžete dotazovat na standardním způsobem:</span><span class="sxs-lookup"><span data-stu-id="8c04b-145">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="8c04b-146">Poznámka: také jsme definovali vlastnost dotazu na úrovni kontextu (Negenerickými) tak, aby fungovala jako kořen pro dotazy na tento typ.</span><span class="sxs-lookup"><span data-stu-id="8c04b-146">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
