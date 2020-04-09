---
title: Globální filtry dotazů – jádro EF
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417727"
---
# <a name="global-query-filters"></a><span data-ttu-id="d3759-102">Globální filtry dotazů</span><span class="sxs-lookup"><span data-stu-id="d3759-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="d3759-103">Tato funkce byla zavedena v EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d3759-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="d3759-104">Globální filtry dotazů jsou predikáty dotazů LINQ (logický výraz obvykle předávaný operátoru linq *where* query) aplikovaný na typy entit v modelu metadat (obvykle v *OnModelCreating).*</span><span class="sxs-lookup"><span data-stu-id="d3759-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="d3759-105">Tyto filtry jsou automaticky použity na všechny dotazy LINQ zahrnující tyto typy entit, včetně typů entit, na které se nepřímo odkazuje, například pomocí odkazů na vlastnosti Include nebo přímé navigace.</span><span class="sxs-lookup"><span data-stu-id="d3759-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="d3759-106">Některé běžné aplikace této funkce jsou:</span><span class="sxs-lookup"><span data-stu-id="d3759-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="d3759-107">**Obnovitelné odstranění** - Typ entity definuje vlastnost *IsDeleted.*</span><span class="sxs-lookup"><span data-stu-id="d3759-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="d3759-108">**Víceklientský** - Typ entity definuje *vlastnost TenantId.*</span><span class="sxs-lookup"><span data-stu-id="d3759-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="d3759-109">Příklad</span><span class="sxs-lookup"><span data-stu-id="d3759-109">Example</span></span>

<span data-ttu-id="d3759-110">Následující příklad ukazuje, jak pomocí globálních filtrů dotazů implementovat chování dotazů s měkkým odstraněním a více nájemců v jednoduchém modelu blogování.</span><span class="sxs-lookup"><span data-stu-id="d3759-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="d3759-111">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="d3759-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="d3759-112">Nejprve definujte entity:</span><span class="sxs-lookup"><span data-stu-id="d3759-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="d3759-113">Poznamenejte si deklaraci pole _tenantId_ v entitě _Blog._</span><span class="sxs-lookup"><span data-stu-id="d3759-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="d3759-114">To se použije k přidružení každé instance blogu k určitému tenantovi.</span><span class="sxs-lookup"><span data-stu-id="d3759-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="d3759-115">Definována je také vlastnost _IsDeleted_ u typu entity _Post._</span><span class="sxs-lookup"><span data-stu-id="d3759-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="d3759-116">Používá se ke sledování, zda _post_ instance byla "obnovitelné odstranění".</span><span class="sxs-lookup"><span data-stu-id="d3759-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="d3759-117">To znamená, že instance je označena jako odstraněná bez fyzického odebrání podkladových dat.</span><span class="sxs-lookup"><span data-stu-id="d3759-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="d3759-118">Dále nakonfigurujte filtry dotazů v _OnModelCreating_ pomocí `HasQueryFilter` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d3759-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="d3759-119">Predikátové výrazy předané volání _HasQueryFilter_ budou nyní automaticky použity pro všechny dotazy LINQ pro tyto typy.</span><span class="sxs-lookup"><span data-stu-id="d3759-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="d3759-120">Všimněte si použití pole úrovně `_tenantId` instance DbContext: slouží k nastavení aktuálního klienta.</span><span class="sxs-lookup"><span data-stu-id="d3759-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="d3759-121">Filtry na úrovni modelu budou používat hodnotu z instance správného kontextu (to znamená, že instance, která je provádění dotazu).</span><span class="sxs-lookup"><span data-stu-id="d3759-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="d3759-122">V současné době není možné definovat více filtrů dotazů na stejné entitě - bude použita pouze poslední.</span><span class="sxs-lookup"><span data-stu-id="d3759-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="d3759-123">Můžete však definovat jeden filtr s více podmínkami pomocí logického operátoru _AND_ ([ `&&` v c#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span><span class="sxs-lookup"><span data-stu-id="d3759-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="d3759-124">Zakázání filtrů</span><span class="sxs-lookup"><span data-stu-id="d3759-124">Disabling Filters</span></span>

<span data-ttu-id="d3759-125">Filtry mohou být zakázány pro jednotlivé `IgnoreQueryFilters()` dotazy LINQ pomocí operátoru.</span><span class="sxs-lookup"><span data-stu-id="d3759-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="d3759-126">Omezení</span><span class="sxs-lookup"><span data-stu-id="d3759-126">Limitations</span></span>

<span data-ttu-id="d3759-127">Globální filtry dotazů mají následující omezení:</span><span class="sxs-lookup"><span data-stu-id="d3759-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="d3759-128">Filtry lze definovat pouze pro kořenový typ entity hierarchie dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="d3759-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
