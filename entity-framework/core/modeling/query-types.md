---
title: Typy dotazů – EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 6f0f860c6a4e619e13d55e6207234a8b5261ee09
ms.sourcegitcommit: d1230e34673b8323a227ab37958dfa77f3684728
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2019
ms.locfileid: "68330791"
---
# <a name="query-types"></a><span data-ttu-id="7639b-102">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="7639b-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="7639b-103">Tato funkce je nového v EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="7639b-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="7639b-104">Kromě typů entit může obsahovat modelu EF Core _typy dotazů_, které je možné provádět databáze dotazy na data, která není namapována na typy entit.</span><span class="sxs-lookup"><span data-stu-id="7639b-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

## <a name="compare-query-types-to-entity-types"></a><span data-ttu-id="7639b-105">Porovnání typů dotazu pro typy entit</span><span class="sxs-lookup"><span data-stu-id="7639b-105">Compare query types to entity types</span></span>

<span data-ttu-id="7639b-106">Typy dotazů jsou jako typy entit v, které jsou:</span><span class="sxs-lookup"><span data-stu-id="7639b-106">Query types are like entity types in that they:</span></span>

- <span data-ttu-id="7639b-107">Může být buď přidá do modelu v `OnModelCreating` nebo přes vlastnost "set" na odvozený _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="7639b-107">Can be added to the model either in `OnModelCreating` or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="7639b-108">Podporují mnoho stejných mapování funkcí, jako je dědičnost mapování a navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7639b-108">Support many of the same mapping capabilities, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="7639b-109">Na relační úložiště můžete nakonfigurovat cílové objektů databáze a sloupce pomocí metody fluent API nebo datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="7639b-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="7639b-110">Ale se liší od entitu napíše, která jsou:</span><span class="sxs-lookup"><span data-stu-id="7639b-110">However, they are different from entity types in that they:</span></span>

- <span data-ttu-id="7639b-111">Klíč je definovat nevyžadují.</span><span class="sxs-lookup"><span data-stu-id="7639b-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="7639b-112">Nikdy sledovány změny na _DbContext_ a proto se nikdy přidají, aktualizovat nebo odstranit v databázi.</span><span class="sxs-lookup"><span data-stu-id="7639b-112">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="7639b-113">Nikdy zjištění konvencí.</span><span class="sxs-lookup"><span data-stu-id="7639b-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="7639b-114">Podporují pouze podmnožinu možností navigace map – konkrétně:</span><span class="sxs-lookup"><span data-stu-id="7639b-114">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="7639b-115">Může se nikdy fungovat jako hlavní konec relace.</span><span class="sxs-lookup"><span data-stu-id="7639b-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="7639b-116">Může obsahovat pouze vlastnosti navigace odkazu přejdete na položku entity.</span><span class="sxs-lookup"><span data-stu-id="7639b-116">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="7639b-117">Entity nemůže obsahovat navigačních vlastností pro typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="7639b-117">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="7639b-118">Se podrobněji probírají v _ModelBuilder_ pomocí `Query` metoda místo `Entity` metody.</span><span class="sxs-lookup"><span data-stu-id="7639b-118">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="7639b-119">Jsou mapovány na _DbContext_ prostřednictvím vlastnosti typu `DbQuery<T>` spíše než `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="7639b-119">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="7639b-120">Jsou mapovány na databázových objektů pomocí `ToView` metody spíše než `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="7639b-120">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="7639b-121">Může být namapovány na _definování dotazu_ – definování dotazu je sekundární dotaz deklarované v modelu, který funguje zdroj dat pro typ dotazu.</span><span class="sxs-lookup"><span data-stu-id="7639b-121">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="7639b-122">Scénáře použití</span><span class="sxs-lookup"><span data-stu-id="7639b-122">Usage scenarios</span></span>

<span data-ttu-id="7639b-123">Zde jsou některé scénáře hlavní použití pro typy dotazů:</span><span class="sxs-lookup"><span data-stu-id="7639b-123">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="7639b-124">Slouží jako návratový typ pro ad hoc `FromSql()` dotazy.</span><span class="sxs-lookup"><span data-stu-id="7639b-124">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="7639b-125">Mapování na zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="7639b-125">Mapping to database views.</span></span>
- <span data-ttu-id="7639b-126">Mapování tabulek, které nemají definován primární klíč.</span><span class="sxs-lookup"><span data-stu-id="7639b-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="7639b-127">Mapování pro dotazy definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="7639b-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="7639b-128">Mapování databázových objektů</span><span class="sxs-lookup"><span data-stu-id="7639b-128">Mapping to database objects</span></span>

<span data-ttu-id="7639b-129">Mapování typu dotazu k databázovému objektu je dosaženo pomocí `ToView` rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="7639b-129">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="7639b-130">Z pohledu EF Core je určený v této metodě objekt databáze _zobrazení_, to znamená, že je považován za zdroj dotazu jen pro čtení a nemůže být cílem příkazu update, insert nebo operace odstranění.</span><span class="sxs-lookup"><span data-stu-id="7639b-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="7639b-131">Nicméně to neznamená, že databázový objekt je ve skutečnosti musí být zobrazení databáze - může být případně tabulku databáze, která se zpracuje jako jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="7639b-131">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="7639b-132">Naopak pro typy entit, EF Core předpokládá, že databázový objekt zadaný v `ToTable` metody lze zacházet jako _tabulky_, což znamená, že může sloužit jako zdroj dotazu, ale také cílí aktualizace, odstranění a vložení operace.</span><span class="sxs-lookup"><span data-stu-id="7639b-132">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="7639b-133">Ve skutečnosti můžete zadat název databáze zobrazení v `ToTable` a všechno, co by mělo fungovat bez problémů jako zobrazení konfigurován tak, aby umožnit aktualizaci modelové databáze.</span><span class="sxs-lookup"><span data-stu-id="7639b-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="7639b-134">Příklad</span><span class="sxs-lookup"><span data-stu-id="7639b-134">Example</span></span>

<span data-ttu-id="7639b-135">Následující příklad ukazuje, jak používat typ dotazu do databáze zobrazení dotazu.</span><span class="sxs-lookup"><span data-stu-id="7639b-135">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="7639b-136">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="7639b-136">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="7639b-137">Nejprve definujte jsme jednoduchý model blogu a příspěvek:</span><span class="sxs-lookup"><span data-stu-id="7639b-137">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="7639b-138">Dále nadefinujeme zobrazení jednoduché databáze, které vám umožní nám zjistit počet příspěvků, které jsou spojené s každou blogu:</span><span class="sxs-lookup"><span data-stu-id="7639b-138">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

<span data-ttu-id="7639b-139">V dalším kroku budeme definovat třídu pro uchování výsledku ze zobrazení databáze:</span><span class="sxs-lookup"><span data-stu-id="7639b-139">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="7639b-140">V dalším kroku budeme nakonfigurujte typ dotazu v _OnModelCreating_ pomocí `modelBuilder.Query<T>` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7639b-140">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="7639b-141">Můžeme použít standardní konfigurace fluent API pro konfiguraci mapování pro typ dotazu:</span><span class="sxs-lookup"><span data-stu-id="7639b-141">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="7639b-142">Dále nakonfigurujeme `DbContext` tak, aby `DbQuery<T>`zahrnovala:[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]</span><span class="sxs-lookup"><span data-stu-id="7639b-142">Next, we configure the `DbContext` to include the `DbQuery<T>`: [!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]</span></span>

<span data-ttu-id="7639b-143">Nakonec jsme zobrazení databáze můžete dotazovat na standardním způsobem:</span><span class="sxs-lookup"><span data-stu-id="7639b-143">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="7639b-144">Poznámka: také jsme definovali vlastnost úrovně dotazu (DbQuery) tak, aby fungoval jako kořenový adresář pro dotazy na tento typ kontextu.</span><span class="sxs-lookup"><span data-stu-id="7639b-144">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
