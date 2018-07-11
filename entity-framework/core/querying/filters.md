---
title: Globální filtry dotazů – EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 2bb0666ba7b75a38e44a348aea735e6fe7eadb29
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949418"
---
# <a name="global-query-filters"></a><span data-ttu-id="85c7e-102">Globální filtry dotazů</span><span class="sxs-lookup"><span data-stu-id="85c7e-102">Global Query Filters</span></span>

<span data-ttu-id="85c7e-103">Globální filtry dotazů jsou predikáty dotazu LINQ (obvykle předaný logický výraz LINQ *kde* – operátor dotazu) použít pro typy entit v modelu metadat (obvykle v *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="85c7e-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="85c7e-104">Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="85c7e-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="85c7e-105">Jsou některé běžné aplikace tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="85c7e-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="85c7e-106">**Obnovitelné odstranění** – definuje typu Entity *IsDeleted* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="85c7e-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="85c7e-107">**Víceklientská architektura** – definuje typu Entity *TenantId* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="85c7e-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="85c7e-108">Příklad</span><span class="sxs-lookup"><span data-stu-id="85c7e-108">Example</span></span>

<span data-ttu-id="85c7e-109">Následující příklad ukazuje, jak použít globální filtry dotazů k implementaci chování dotazu obnovitelného odstranění a více tenantů v modelu jednoduché blogů.</span><span class="sxs-lookup"><span data-stu-id="85c7e-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="85c7e-110">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="85c7e-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="85c7e-111">Nejprve definujte entity:</span><span class="sxs-lookup"><span data-stu-id="85c7e-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="85c7e-112">Poznámka: deklarace __tenantId_ pole na _blogu_ entity.</span><span class="sxs-lookup"><span data-stu-id="85c7e-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="85c7e-113">Ten se použije pro každou instanci blogu přidružit konkrétního tenanta.</span><span class="sxs-lookup"><span data-stu-id="85c7e-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="85c7e-114">Je také definováno _IsDeleted_ vlastnost _příspěvek_ typu entity.</span><span class="sxs-lookup"><span data-stu-id="85c7e-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="85c7e-115">To se používá ke sledování toho, jestli _příspěvek_ instance byla "obnovitelně odstraněný".</span><span class="sxs-lookup"><span data-stu-id="85c7e-115">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="85c7e-116">To znamená, že instance je označen jako odstraněný bez fyzickým odebráním podkladová data.</span><span class="sxs-lookup"><span data-stu-id="85c7e-116">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="85c7e-117">V dalším kroku Nakonfigurujte filtry dotazu v _OnModelCreating_ pomocí ```HasQueryFilter``` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="85c7e-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="85c7e-118">Predikátu výraz předaný _HasQueryFilter_ volání se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.</span><span class="sxs-lookup"><span data-stu-id="85c7e-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="85c7e-119">Všimněte si použití úrovně pole instance DbContext: ```_tenantId``` slouží k nastavení aktuálního tenanta.</span><span class="sxs-lookup"><span data-stu-id="85c7e-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="85c7e-120">Filtry na úrovni modelu bude používat hodnotu z instance správného kontextu (to znamená, instance, který spouští dotaz).</span><span class="sxs-lookup"><span data-stu-id="85c7e-120">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="85c7e-121">Zakázání filtrů</span><span class="sxs-lookup"><span data-stu-id="85c7e-121">Disabling Filters</span></span>

<span data-ttu-id="85c7e-122">Může být zakázané filtry pro jednotlivé dotazy LINQ pomocí ```IgnoreQueryFilters()``` operátor.</span><span class="sxs-lookup"><span data-stu-id="85c7e-122">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="85c7e-123">Omezení</span><span class="sxs-lookup"><span data-stu-id="85c7e-123">Limitations</span></span>

<span data-ttu-id="85c7e-124">Globální filtry dotazů mají následující omezení:</span><span class="sxs-lookup"><span data-stu-id="85c7e-124">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="85c7e-125">Filtry nemůže obsahovat odkazy na navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="85c7e-125">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="85c7e-126">Filtry je možné definovat pouze pro kořenový typ Entity hierarchie dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="85c7e-126">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
