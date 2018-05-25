---
title: Dotaz globální filtry - EF jádra
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="5a39e-102">Filtry globální dotazu</span><span class="sxs-lookup"><span data-stu-id="5a39e-102">Global Query Filters</span></span>

<span data-ttu-id="5a39e-103">Dotaz globální filtry jsou predikáty dotaz LINQ (logický výraz obvykle předaný LINQ *kde* – operátor dotazu) u typy entit ve model metadat (obvykle ve *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="5a39e-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="5a39e-104">Tyto filtry jsou automaticky použita pro všechny dotazy LINQ zahrnující tyto typy entit, včetně typů entit odkazované nepřímo, jako prostřednictvím zahrnout nebo přímé navigační vlastnost odkazů.</span><span class="sxs-lookup"><span data-stu-id="5a39e-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="5a39e-105">Některé běžné aplikace této funkce jsou:</span><span class="sxs-lookup"><span data-stu-id="5a39e-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="5a39e-106">**Soft odstranit** -definuje typu Entity *IsDeleted* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5a39e-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="5a39e-107">**Víceklientský** -definuje typu Entity *TenantId* vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5a39e-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="5a39e-108">Příklad</span><span class="sxs-lookup"><span data-stu-id="5a39e-108">Example</span></span>

<span data-ttu-id="5a39e-109">Následující příklad ukazuje, jak použít globální filtry dotazu k implementaci chování dotazu konfigurace soft odstranění a víceklientský v modelu jednoduché blogu.</span><span class="sxs-lookup"><span data-stu-id="5a39e-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="5a39e-110">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="5a39e-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="5a39e-111">Nejprve definujte entity:</span><span class="sxs-lookup"><span data-stu-id="5a39e-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="5a39e-112">Všimněte si prohlášení o výraz __tenantId_ na _Blog_ entity.</span><span class="sxs-lookup"><span data-stu-id="5a39e-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="5a39e-113">Bude se používat pro přidružení každá instance Blog konkrétní klientovi.</span><span class="sxs-lookup"><span data-stu-id="5a39e-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="5a39e-114">Je také definován _IsDeleted_ vlastnost _Post_ typ entity.</span><span class="sxs-lookup"><span data-stu-id="5a39e-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="5a39e-115">Slouží k zaznamenávat, jestli se _Post_ instance byl "obnovitelné odstranění".</span><span class="sxs-lookup"><span data-stu-id="5a39e-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="5a39e-116">Tj. Instance je označena jako odstraněné withouth fyzicky odebrání základní data.</span><span class="sxs-lookup"><span data-stu-id="5a39e-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="5a39e-117">V dalším kroku Nakonfigurujte filtry dotazu v _OnModelCreating_ pomocí ```HasQueryFilter``` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5a39e-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="5a39e-118">Předaný predikátem výrazy _HasQueryFilter_ volání se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.</span><span class="sxs-lookup"><span data-stu-id="5a39e-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="5a39e-119">Všimněte si použití úrovni poli instance DbContext: ```_tenantId``` použít k nastavení aktuální klienta.</span><span class="sxs-lookup"><span data-stu-id="5a39e-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="5a39e-120">Filtry na úrovni modelu použije hodnotu z instance správný kontext.</span><span class="sxs-lookup"><span data-stu-id="5a39e-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="5a39e-121">Tj. Instance, kterou je zpracování dotazu.</span><span class="sxs-lookup"><span data-stu-id="5a39e-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="5a39e-122">Zakázání filtrů</span><span class="sxs-lookup"><span data-stu-id="5a39e-122">Disabling Filters</span></span>

<span data-ttu-id="5a39e-123">Může být zakázané filtry pro jednotlivé dotazy LINQ pomocí ```IgnoreQueryFilters()``` operátor.</span><span class="sxs-lookup"><span data-stu-id="5a39e-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="5a39e-124">Omezení</span><span class="sxs-lookup"><span data-stu-id="5a39e-124">Limitations</span></span>

<span data-ttu-id="5a39e-125">Globální dotaz filtry mají následující omezení:</span><span class="sxs-lookup"><span data-stu-id="5a39e-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="5a39e-126">Filtry nesmí obsahovat odkazy na navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5a39e-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="5a39e-127">Filtry lze definovat pouze pro kořenové typ Entity hierarchie dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="5a39e-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
