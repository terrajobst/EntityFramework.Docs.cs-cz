---
title: Typy dotazů - EF jádra
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163171"
---
# <a name="query-types"></a><span data-ttu-id="cc56f-102">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="cc56f-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="cc56f-103">Tato funkce je nového v EF základní 2.1</span><span class="sxs-lookup"><span data-stu-id="cc56f-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="cc56f-104">Kromě typy entit, může obsahovat model EF základní _typy dotazů_, které lze provádět dotazy na databázi pro data, která není namapován na typy entit.</span><span class="sxs-lookup"><span data-stu-id="cc56f-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="cc56f-105">Typy dotazů mít mnoho společného s typy entit:</span><span class="sxs-lookup"><span data-stu-id="cc56f-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="cc56f-106">Mohou také být přidány do modelu buď v `OnModelCreating`, nebo prostřednictvím vlastnost "nastavení" na odvozeným _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="cc56f-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="cc56f-107">Podporují řadu stejné mapování funkce, jako je dědičnosti mapování navigační vlastnosti (viz níže omezení) a na relační úložiště umožňuje konfiguraci cílové databázové objekty a sloupce prostřednictvím metody fluent API nebo pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="cc56f-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="cc56f-108">Ale liší se od entity typy v tom, že se:</span><span class="sxs-lookup"><span data-stu-id="cc56f-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="cc56f-109">Klíč, který se má definovat nevyžadují.</span><span class="sxs-lookup"><span data-stu-id="cc56f-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="cc56f-110">Se nikdy sledují změny na _DbContext_ a proto se nikdy vložit, aktualizovat nebo odstranit v databázi.</span><span class="sxs-lookup"><span data-stu-id="cc56f-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="cc56f-111">Nikdy zjistí konvence.</span><span class="sxs-lookup"><span data-stu-id="cc56f-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="cc56f-112">Podporují pouze podmnožinu možnosti mapování navigační – konkrétně:</span><span class="sxs-lookup"><span data-stu-id="cc56f-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="cc56f-113">Může se nikdy fungují jako hlavní konec relace.</span><span class="sxs-lookup"><span data-stu-id="cc56f-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="cc56f-114">Může obsahovat pouze navigační vlastnosti odkazu přejdete na entity.</span><span class="sxs-lookup"><span data-stu-id="cc56f-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="cc56f-115">Entity nemůže obsahovat navigačních vlastností pro typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="cc56f-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="cc56f-116">Řešeny na _ModelBuilder_ pomocí `Query` metoda místo `Entity` metoda.</span><span class="sxs-lookup"><span data-stu-id="cc56f-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="cc56f-117">Jsou namapované na _DbContext_ prostřednictvím vlastnosti typu `DbQuery<T>` místo `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="cc56f-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="cc56f-118">Jsou namapované na databázových objektů pomocí `ToView` metoda, místo `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="cc56f-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="cc56f-119">Může být namapovaný na _definice dotazu_ – definice dotazu je sekundární dotazu deklarované v modelu, který se chová zdroje dat pro typ dotazu.</span><span class="sxs-lookup"><span data-stu-id="cc56f-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="cc56f-120">Některé scénáře hlavní využití pro typy dotazů jsou:</span><span class="sxs-lookup"><span data-stu-id="cc56f-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="cc56f-121">Slouží jako návratový typ pro ad hoc `FromSql()` dotazy.</span><span class="sxs-lookup"><span data-stu-id="cc56f-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="cc56f-122">Mapování pro zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="cc56f-122">Mapping to database views.</span></span>
- <span data-ttu-id="cc56f-123">Mapování tabulek, které nemají definovaný primární klíč.</span><span class="sxs-lookup"><span data-stu-id="cc56f-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="cc56f-124">Mapování na dotazy, které jsou definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="cc56f-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="cc56f-125">Mapování typu dotaz na objekt databáze je dosaženo pomocí `ToView` rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="cc56f-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="cc56f-126">Z pohledu EF jádra, je v této metodě zadaného objektu databáze _zobrazení_, což znamená, že se zpracovává jako zdroj dotazu jen pro čtení a nemůže být cílem aktualizace, vložit nebo operace odstranění.</span><span class="sxs-lookup"><span data-stu-id="cc56f-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="cc56f-127">Však neznamená to, že databázový objekt je ve skutečnosti nemusí být zobrazení databáze – může být případně tabulku databáze, která budou zpracovány jako jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="cc56f-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="cc56f-128">Naopak pro typy entit, EF základní předpokládá, že databázový objekt zadaný v `ToTable` metoda lze zacházet jako _tabulky_, což znamená, že lze použít jako zdroj dotazu, ale také napojeno aktualizaci, odstranění a vložení operace.</span><span class="sxs-lookup"><span data-stu-id="cc56f-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="cc56f-129">Ve skutečnosti můžete zadat název zobrazení databáze v `ToTable` a všechno, co by měla fungovat správně, dokud zobrazení nakonfigurovaný tak, aby byl aktualizovat v databázi.</span><span class="sxs-lookup"><span data-stu-id="cc56f-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="cc56f-130">Příklad</span><span class="sxs-lookup"><span data-stu-id="cc56f-130">Example</span></span>

<span data-ttu-id="cc56f-131">Následující příklad ukazuje, jak chcete použít typ dotazu do databáze zobrazení dotazu.</span><span class="sxs-lookup"><span data-stu-id="cc56f-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="cc56f-132">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="cc56f-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="cc56f-133">Nejprve definujeme jednoduchého modelu Blog a Post:</span><span class="sxs-lookup"><span data-stu-id="cc56f-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="cc56f-134">V dalším kroku jsme definovali zobrazení jednoduché databáze, které vám umožní nám dotaz počet příspěvcích spojené s každou blogu:</span><span class="sxs-lookup"><span data-stu-id="cc56f-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="cc56f-135">V dalším kroku jsme definovali třída pro uložení výsledek z pohledu databáze:</span><span class="sxs-lookup"><span data-stu-id="cc56f-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="cc56f-136">V dalším kroku nakonfigurujeme typ dotazu v _OnModelCreating_ pomocí `modelBuilder.Query<T>` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cc56f-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="cc56f-137">Můžeme použít standardní fluent konfigurace rozhraní API pro konfiguraci mapování pro typ dotazu:</span><span class="sxs-lookup"><span data-stu-id="cc56f-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="cc56f-138">Nakonec jsme dotazu na zobrazení databáze ve standardním způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc56f-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="cc56f-139">Všimněte si, že jsme definovali také vlastnost úrovni dotazu (DbQuery) tak, aby fungoval jako kořenový adresář pro dotazy pro tento typ kontextu.</span><span class="sxs-lookup"><span data-stu-id="cc56f-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
