---
title: Globální filtry dotazů – EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: e1cb9f5afc54aaa12e5880ace606277b00911c06
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306476"
---
# <a name="global-query-filters"></a><span data-ttu-id="d5105-102">Globální filtry dotazů</span><span class="sxs-lookup"><span data-stu-id="d5105-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="d5105-103">Tato funkce byla představena v EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="d5105-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="d5105-104">Globální filtry dotazů jsou predikáty dotazu LINQ (obvykle předaný logický výraz LINQ *kde* – operátor dotazu) použít pro typy entit v modelu metadat (obvykle v *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="d5105-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="d5105-105">Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5105-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="d5105-106">Jsou některé běžné aplikace tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="d5105-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="d5105-107">**Obnovitelné odstranění** – definuje typu Entity *IsDeleted* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d5105-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="d5105-108">**Víceklientská architektura** – definuje typu Entity *TenantId* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d5105-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="d5105-109">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5105-109">Example</span></span>

<span data-ttu-id="d5105-110">Následující příklad ukazuje, jak použít globální filtry dotazů k implementaci chování dotazu obnovitelného odstranění a více tenantů v modelu jednoduché blogů.</span><span class="sxs-lookup"><span data-stu-id="d5105-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="d5105-111">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d5105-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="d5105-112">Nejprve definujte entity:</span><span class="sxs-lookup"><span data-stu-id="d5105-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="d5105-113">Všimněte si deklarace pole _tenantId_ v entitě _blogu_ .</span><span class="sxs-lookup"><span data-stu-id="d5105-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="d5105-114">Ten se použije pro každou instanci blogu přidružit konkrétního tenanta.</span><span class="sxs-lookup"><span data-stu-id="d5105-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="d5105-115">Je také definováno _IsDeleted_ vlastnost _příspěvek_ typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5105-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="d5105-116">To se používá ke sledování toho, jestli _příspěvek_ instance byla "obnovitelně odstraněný".</span><span class="sxs-lookup"><span data-stu-id="d5105-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="d5105-117">To znamená, že instance je označen jako odstraněný bez fyzickým odebráním podkladová data.</span><span class="sxs-lookup"><span data-stu-id="d5105-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="d5105-118">V dalším kroku Nakonfigurujte filtry dotazu v _OnModelCreating_ pomocí `HasQueryFilter` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d5105-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="d5105-119">Predikátu výraz předaný _HasQueryFilter_ volání se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.</span><span class="sxs-lookup"><span data-stu-id="d5105-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="d5105-120">Všimněte si použití úrovně pole instance DbContext: `_tenantId` slouží k nastavení aktuálního tenanta.</span><span class="sxs-lookup"><span data-stu-id="d5105-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="d5105-121">Filtry na úrovni modelu bude používat hodnotu z instance správného kontextu (to znamená, instance, který spouští dotaz).</span><span class="sxs-lookup"><span data-stu-id="d5105-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="d5105-122">Zakázání filtrů</span><span class="sxs-lookup"><span data-stu-id="d5105-122">Disabling Filters</span></span>

<span data-ttu-id="d5105-123">Může být zakázané filtry pro jednotlivé dotazy LINQ pomocí `IgnoreQueryFilters()` operátor.</span><span class="sxs-lookup"><span data-stu-id="d5105-123">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="d5105-124">Omezení</span><span class="sxs-lookup"><span data-stu-id="d5105-124">Limitations</span></span>

<span data-ttu-id="d5105-125">Globální filtry dotazů mají následující omezení:</span><span class="sxs-lookup"><span data-stu-id="d5105-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="d5105-126">Filtry nemůže obsahovat odkazy na navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5105-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="d5105-127">Filtry je možné definovat pouze pro kořenový typ Entity hierarchie dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="d5105-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
