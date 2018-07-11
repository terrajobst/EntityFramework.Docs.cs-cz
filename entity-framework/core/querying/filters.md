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

Poznámka: deklarace __tenantId_ pole na _blogu_ entity. Ten se použije pro každou instanci blogu přidružit konkrétního tenanta. Je také definováno _IsDeleted_ vlastnost _příspěvek_ typu entity. To se používá ke sledování toho, jestli _příspěvek_ instance byla "obnovitelně odstraněný". To znamená, že instance je označen jako odstraněný bez fyzickým odebráním podkladová data.

V dalším kroku Nakonfigurujte filtry dotazu v _OnModelCreating_ pomocí ```HasQueryFilter``` rozhraní API.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

Predikátu výraz předaný _HasQueryFilter_ volání se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.

> [!TIP]
> Všimněte si použití úrovně pole instance DbContext: ```_tenantId``` slouží k nastavení aktuálního tenanta. Filtry na úrovni modelu bude používat hodnotu z instance správného kontextu (to znamená, instance, který spouští dotaz).

## <a name="disabling-filters"></a>Zakázání filtrů

Může být zakázané filtry pro jednotlivé dotazy LINQ pomocí ```IgnoreQueryFilters()``` operátor.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Omezení

Globální filtry dotazů mají následující omezení:

* Filtry nemůže obsahovat odkazy na navigační vlastnosti.
* Filtry je možné definovat pouze pro kořenový typ Entity hierarchie dědičnosti.
