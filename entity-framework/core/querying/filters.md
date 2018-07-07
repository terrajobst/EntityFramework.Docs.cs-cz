---
title: Globální filtry dotazů – EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 0c7858d660665b4f17aedea2101452048f9aff25
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900295"
---
# <a name="global-query-filters"></a>Globální filtry dotazů

Globální filtry dotazů jsou predikáty dotazu LINQ (obvykle předaný logický výraz LINQ *kde* – operátor dotazu) použít pro typy entit v modelu metadat (obvykle v *OnModelCreating*). Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti. Jsou některé běžné aplikace tuto funkci:

* **Obnovitelné odstranění** – definuje typu Entity *IsDeleted* vlastnost.
* **Víceklientská architektura** – definuje typu Entity *TenantId* vlastnost.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak použít globální filtry dotazů k implementaci chování dotazu obnovitelného odstranění a více tenantů v modelu jednoduché blogů.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) na Githubu.

Nejprve definujte entity:

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

Poznámka: deklarace __tenantId_ pole na _blogu_ entity. Ten se použije pro každou instanci blogu přidružit konkrétního tenanta. Je také definováno _IsDeleted_ vlastnost _příspěvek_ typu entity. Slouží ke sledování toho, jestli to _příspěvek_ instance byla "obnovitelně odstraněný". To znamená Instance je označen jako odstraněný withouth fyzickým odebráním podkladová data.

V dalším kroku Nakonfigurujte filtry dotazu v _OnModelCreating_ pomocí ```HasQueryFilter``` rozhraní API.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

Predikátu výraz předaný _HasQueryFilter_ volání se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.

> [!TIP]
> Všimněte si použití úrovně pole instance DbContext: ```_tenantId``` slouží k nastavení aktuálního tenanta. Filtry na úrovni modelu bude používat hodnotu z instance správný kontext. To znamená Instance, který spouští dotaz.

## <a name="disabling-filters"></a>Zakázání filtrů

Může být zakázané filtry pro jednotlivé dotazy LINQ pomocí ```IgnoreQueryFilters()``` operátor.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Omezení

Globální filtry dotazů mají následující omezení:

* Filtry nemůže obsahovat odkazy na navigační vlastnosti.
* Filtry je možné definovat pouze pro kořenový typ Entity hierarchie dědičnosti.
