---
title: Globální filtry dotazů – EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417727"
---
# <a name="global-query-filters"></a><span data-ttu-id="476a2-102">Globální filtry dotazů</span><span class="sxs-lookup"><span data-stu-id="476a2-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="476a2-103">Tato funkce byla představena v EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="476a2-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="476a2-104">Globální filtry dotazů jsou predikáty dotazů LINQ (logický výraz obvykle předaný operátoru dotazu *LINQ)* , který je použit na typy entit v modelu metadat (obvykle v *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="476a2-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="476a2-105">Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="476a2-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="476a2-106">Jsou některé běžné aplikace tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="476a2-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="476a2-107">**Obnovitelné odstranění** – typ entity definuje vlastnost *IsDeleted* .</span><span class="sxs-lookup"><span data-stu-id="476a2-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="476a2-108">**Víceklientská** architektura – typ entity definuje vlastnost *TenantId* .</span><span class="sxs-lookup"><span data-stu-id="476a2-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="476a2-109">Příklad</span><span class="sxs-lookup"><span data-stu-id="476a2-109">Example</span></span>

<span data-ttu-id="476a2-110">Následující příklad ukazuje, jak použít globální filtry dotazů k implementaci chování dotazu obnovitelného odstranění a více tenantů v modelu jednoduché blogů.</span><span class="sxs-lookup"><span data-stu-id="476a2-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="476a2-111">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="476a2-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="476a2-112">Nejprve definujte entity:</span><span class="sxs-lookup"><span data-stu-id="476a2-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="476a2-113">Všimněte si deklarace pole _tenantId_ v entitě _blogu_ .</span><span class="sxs-lookup"><span data-stu-id="476a2-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="476a2-114">Ten se použije pro každou instanci blogu přidružit konkrétního tenanta.</span><span class="sxs-lookup"><span data-stu-id="476a2-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="476a2-115">Také definováno je vlastnost _IsDeleted_ v typu entity _post_ .</span><span class="sxs-lookup"><span data-stu-id="476a2-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="476a2-116">Slouží k udržení přehledu o tom, zda byla instance _příspěvku_ "soft-Deleted".</span><span class="sxs-lookup"><span data-stu-id="476a2-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="476a2-117">To znamená, že instance je označen jako odstraněný bez fyzickým odebráním podkladová data.</span><span class="sxs-lookup"><span data-stu-id="476a2-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="476a2-118">Dále nakonfigurujte filtry dotazů v _OnModelCreating_ pomocí rozhraní API pro `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="476a2-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="476a2-119">Výrazy predikátu předané voláním _HasQueryFilter_ se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.</span><span class="sxs-lookup"><span data-stu-id="476a2-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="476a2-120">Všimněte si použití pole na úrovni instance DbContext: `_tenantId` slouží k nastavení aktuálního tenanta.</span><span class="sxs-lookup"><span data-stu-id="476a2-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="476a2-121">Filtry na úrovni modelu bude používat hodnotu z instance správného kontextu (to znamená, instance, který spouští dotaz).</span><span class="sxs-lookup"><span data-stu-id="476a2-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="476a2-122">V současné době není možné definovat více filtrů dotazů na stejnou entitu. použije se jenom poslední z nich.</span><span class="sxs-lookup"><span data-stu-id="476a2-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="476a2-123">Můžete však definovat jeden filtr s více podmínkami pomocí logického operátoru _and_ ([`&&` in C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span><span class="sxs-lookup"><span data-stu-id="476a2-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="476a2-124">Zakázání filtrů</span><span class="sxs-lookup"><span data-stu-id="476a2-124">Disabling Filters</span></span>

<span data-ttu-id="476a2-125">Filtry mohou být zakázány pro jednotlivé dotazy LINQ pomocí operátoru `IgnoreQueryFilters()`.</span><span class="sxs-lookup"><span data-stu-id="476a2-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="476a2-126">Omezení</span><span class="sxs-lookup"><span data-stu-id="476a2-126">Limitations</span></span>

<span data-ttu-id="476a2-127">Globální filtry dotazů mají následující omezení:</span><span class="sxs-lookup"><span data-stu-id="476a2-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="476a2-128">Filtry je možné definovat pouze pro kořenový typ Entity hierarchie dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="476a2-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
