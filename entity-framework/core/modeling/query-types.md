---
title: "Typy dotazů - EF jádra"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a><span data-ttu-id="3a4b6-102">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="3a4b6-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="3a4b6-103">Tato funkce je nového v EF základní 2.1</span><span class="sxs-lookup"><span data-stu-id="3a4b6-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="3a4b6-104">Typy dotazů jsou typy výsledků dotazu jen pro čtení, které mohou být přidány do model EF jádra.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="3a4b6-105">Typy dotazů povolení dotazů ad-hoc (jako anonymní typy), ale jsou flexibilnější, protože mají zadané konfigurace mapování.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="3a4b6-106">Jsou koncepčně podobá typy entit v tom, že:</span><span class="sxs-lookup"><span data-stu-id="3a4b6-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="3a4b6-107">Jsou typy objektů POCO C#, které jsou přidány do modelu, buď v ```OnModelCreating``` pomocí ```ModelBuilder.Query``` metoda, nebo přes vlastnost DbContext "set" (pro dotaz typy tato vlastnost je zadán jako ```DbQuery<T>``` místo ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="3a4b6-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather than ```DbSet<T>```).</span></span>
- <span data-ttu-id="3a4b6-108">Podporují většinu stejné mapování funkcí jako typy pravidelných entit.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="3a4b6-109">Například mapování dědění, navigací (viz níže limitiations) a na relační úložiště umožňuje konfiguraci cílové databázi objektů schématu prostřednictvím ```ToTable```, ```HasColumn``` metody rozhraní fluent api (nebo pomocí datových poznámek).</span><span class="sxs-lookup"><span data-stu-id="3a4b6-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="3a4b6-110">Typy dotazů se liší od entity typy v tom, že se:</span><span class="sxs-lookup"><span data-stu-id="3a4b6-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="3a4b6-111">Klíč, který se má definovat nevyžadují.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="3a4b6-112">Nikdy jsou sledovány objektem modul sledování změny.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="3a4b6-113">Nikdy zjistí konvence.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="3a4b6-114">Podporují pouze podmnožinu možnosti mapování navigační – konkrétně, nikdy může fungovat jako hlavní konec relace.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="3a4b6-115">Může být namapovaný na _definice dotazu_ -A definice dotazu je sekundární dotaz, který funguje zdroj dat pro typ dotazu.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="3a4b6-116">Některé scénáře hlavní využití pro typy dotazů jsou:</span><span class="sxs-lookup"><span data-stu-id="3a4b6-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="3a4b6-117">Mapování pro zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-117">Mapping to database views.</span></span>
- <span data-ttu-id="3a4b6-118">Mapování tabulek, které nemají definovaný primární klíč.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="3a4b6-119">Slouží jako návratový typ pro ad hoc ```FromSql()``` dotazy.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="3a4b6-120">Mapování na dotazy, které jsou definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="3a4b6-121">Mapování typu dotaz na zobrazení databáze je dosaženo pomocí ```ToTable``` rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="3a4b6-122">Příklad</span><span class="sxs-lookup"><span data-stu-id="3a4b6-122">Example</span></span>

<span data-ttu-id="3a4b6-123">Následující příklad ukazuje, jak chcete použít typ dotazu do databáze zobrazení dotazu.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="3a4b6-124">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="3a4b6-125">Nejprve definujeme jednoduchého modelu Blog a Post:</span><span class="sxs-lookup"><span data-stu-id="3a4b6-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="3a4b6-126">V dalším kroku jsme definovali zobrazení jednoduché databáze, které vám umožní nám dotaz počet příspěvcích spojené s každou blogu:</span><span class="sxs-lookup"><span data-stu-id="3a4b6-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="3a4b6-127">V dalším kroku jsme definovali třída pro uložení výsledek z pohledu databáze:</span><span class="sxs-lookup"><span data-stu-id="3a4b6-127">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="3a4b6-128">V dalším kroku nakonfigurujeme typ dotazu v _OnModelCreating_ pomocí ```modelBuilder.Query<T>``` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-128">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="3a4b6-129">Můžeme použít standardní fluent konfigurace rozhraní API pro konfiguraci mapování pro typ dotazu:</span><span class="sxs-lookup"><span data-stu-id="3a4b6-129">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="3a4b6-130">Nakonec jsme dotazu na zobrazení databáze ve standardním způsobem:</span><span class="sxs-lookup"><span data-stu-id="3a4b6-130">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="3a4b6-131">Všimněte si, že jsme definovali také vlastnost úrovni dotazu (DbQuery) tak, aby fungoval jako kořenový adresář pro dotazy pro tento typ kontextu.</span><span class="sxs-lookup"><span data-stu-id="3a4b6-131">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
