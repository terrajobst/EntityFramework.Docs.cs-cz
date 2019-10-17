---
title: Typy entit bez klíčů – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 3dbc2700fc9bb277eb90885dfc2506c250ae21f1
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445942"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="52b8f-102">Typy entit bez klíčů</span><span class="sxs-lookup"><span data-stu-id="52b8f-102">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="52b8f-103">Tato funkce byla přidána do EF Core 2,1 pod názvem typů dotazů.</span><span class="sxs-lookup"><span data-stu-id="52b8f-103">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="52b8f-104">V EF Core 3,0 byl koncept přejmenován na bez klíčů entity Types.</span><span class="sxs-lookup"><span data-stu-id="52b8f-104">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="52b8f-105">Kromě běžných typů entit může EF Core model obsahovat _typy entit bez klíčů_, které lze použít k provádění databázových dotazů na data, která neobsahují klíčové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="52b8f-105">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="52b8f-106">Vlastnosti typů entit bez klíčů</span><span class="sxs-lookup"><span data-stu-id="52b8f-106">Keyless entity types characteristics</span></span>

<span data-ttu-id="52b8f-107">Typy entit bez klíčů podporují mnoho stejných funkcí mapování jako regulární typy entit, jako je mapování dědičnosti a vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="52b8f-107">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="52b8f-108">V relačních úložištích můžou konfigurovat objekty a sloupce cílové databáze prostřednictvím metod rozhraní Fluent API nebo datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="52b8f-108">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="52b8f-109">Liší se však od regulárních typů entit v tom, že:</span><span class="sxs-lookup"><span data-stu-id="52b8f-109">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="52b8f-110">Nelze definovat klíč.</span><span class="sxs-lookup"><span data-stu-id="52b8f-110">Cannot have a key defined.</span></span>
- <span data-ttu-id="52b8f-111">Nejsou sledovány pro změny v _DbContext_ , a proto nejsou nikdy vloženy, aktualizovány ani smazány v databázi.</span><span class="sxs-lookup"><span data-stu-id="52b8f-111">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="52b8f-112">Nejsou nikdy zjištěny konvencí.</span><span class="sxs-lookup"><span data-stu-id="52b8f-112">Are never discovered by convention.</span></span>
- <span data-ttu-id="52b8f-113">Podporuje pouze podmnožinu možností mapování navigace, konkrétně:</span><span class="sxs-lookup"><span data-stu-id="52b8f-113">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="52b8f-114">Nemusí nikdy fungovat jako hlavní konec relace.</span><span class="sxs-lookup"><span data-stu-id="52b8f-114">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="52b8f-115">Nemusí mít navigace ke vlastněným entitám.</span><span class="sxs-lookup"><span data-stu-id="52b8f-115">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="52b8f-116">Mohou obsahovat pouze referenční navigační vlastnosti ukazující na běžné entity.</span><span class="sxs-lookup"><span data-stu-id="52b8f-116">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="52b8f-117">Entity nemůžou obsahovat navigační vlastnosti bez klíčů typů entit.</span><span class="sxs-lookup"><span data-stu-id="52b8f-117">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="52b8f-118">Je nutné nakonfigurovat s voláním metody `.HasNoKey()`.</span><span class="sxs-lookup"><span data-stu-id="52b8f-118">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="52b8f-119">Může být mapován na _definiční dotaz_.</span><span class="sxs-lookup"><span data-stu-id="52b8f-119">May be mapped to a _defining query_.</span></span> <span data-ttu-id="52b8f-120">Definiční dotaz je dotaz deklarovaný v modelu, který slouží jako zdroj dat pro typ entity bez klíčů.</span><span class="sxs-lookup"><span data-stu-id="52b8f-120">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="52b8f-121">Scénáře použití</span><span class="sxs-lookup"><span data-stu-id="52b8f-121">Usage scenarios</span></span>

<span data-ttu-id="52b8f-122">Některé z hlavních scénářů použití pro typy entit bez klíčů jsou:</span><span class="sxs-lookup"><span data-stu-id="52b8f-122">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="52b8f-123">Slouží jako návratový typ pro [nezpracované dotazy SQL](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="52b8f-123">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="52b8f-124">Mapování na zobrazení databáze, která neobsahují primární klíč.</span><span class="sxs-lookup"><span data-stu-id="52b8f-124">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="52b8f-125">Mapování na tabulky, ve kterých není definován primární klíč.</span><span class="sxs-lookup"><span data-stu-id="52b8f-125">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="52b8f-126">Mapování na dotazy definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="52b8f-126">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="52b8f-127">Mapování na databázové objekty</span><span class="sxs-lookup"><span data-stu-id="52b8f-127">Mapping to database objects</span></span>

<span data-ttu-id="52b8f-128">Mapování typu entity bez klíčů na databázový objekt se dosahuje pomocí rozhraní API `ToTable` nebo `ToView` Fluent.</span><span class="sxs-lookup"><span data-stu-id="52b8f-128">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="52b8f-129">Z perspektivy EF Core je databázový objekt zadaný v této metodě _zobrazení_, což znamená, že je považován za zdroj dotazu jen pro čtení a nemůže být cílem operace Update, INSERT nebo DELETE.</span><span class="sxs-lookup"><span data-stu-id="52b8f-129">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="52b8f-130">To však neznamená, že objekt databáze je skutečně vyžadován pro zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="52b8f-130">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="52b8f-131">Může se případně jednat o databázovou tabulku, která bude považována za jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="52b8f-131">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="52b8f-132">U regulárních typů entit EF Core předpokládá, že databázový objekt zadaný v metodě `ToTable` lze považovat za _tabulku_, což znamená, že se dá použít jako zdroj dotazu, ale také cílený na operace aktualizace, odstranění a vložení.</span><span class="sxs-lookup"><span data-stu-id="52b8f-132">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="52b8f-133">Ve skutečnosti můžete zadat název zobrazení databáze v `ToTable` a vše by mělo fungovat, dokud je v databázi nakonfigurováno, aby bylo možné aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="52b8f-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="52b8f-134">`ToView` předpokládá, že objekt již v databázi existuje a nebude vytvořen migracemi.</span><span class="sxs-lookup"><span data-stu-id="52b8f-134">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="52b8f-135">Příklad</span><span class="sxs-lookup"><span data-stu-id="52b8f-135">Example</span></span>

<span data-ttu-id="52b8f-136">Následující příklad ukazuje, jak použít typy entit bez klíčů k dotazování zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="52b8f-136">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="52b8f-137">[Ukázku](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="52b8f-137">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="52b8f-138">Nejdřív definujeme jednoduchý blog a model post:</span><span class="sxs-lookup"><span data-stu-id="52b8f-138">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="52b8f-139">V dalším kroku definujeme jednoduché zobrazení databáze, které nám umožní dotazovat se na počet příspěvků přidružených ke každému blogu:</span><span class="sxs-lookup"><span data-stu-id="52b8f-139">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="52b8f-140">Dále definujeme třídu, která bude uchovávat výsledek z pohledu databáze:</span><span class="sxs-lookup"><span data-stu-id="52b8f-140">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="52b8f-141">V dalším kroku nakonfigurujeme typ entity bez klíčů v _OnModelCreating_ pomocí rozhraní API `HasNoKey`.</span><span class="sxs-lookup"><span data-stu-id="52b8f-141">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="52b8f-142">Rozhraní API pro konfiguraci Fluent používáme ke konfiguraci mapování pro typ entity bez klíčů:</span><span class="sxs-lookup"><span data-stu-id="52b8f-142">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="52b8f-143">Dále nakonfigurujte `DbContext` tak, aby zahrnovala `DbSet<T>`:</span><span class="sxs-lookup"><span data-stu-id="52b8f-143">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="52b8f-144">Nakonec můžeme dotazovat zobrazení databáze standardním způsobem:</span><span class="sxs-lookup"><span data-stu-id="52b8f-144">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="52b8f-145">Poznámka: také jsme definovali vlastnost dotazu na úrovni kontextu (Negenerickými) tak, aby fungovala jako kořen pro dotazy na tento typ.</span><span class="sxs-lookup"><span data-stu-id="52b8f-145">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
