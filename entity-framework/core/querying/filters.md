---
title: Globální filtry dotazů – EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: f4ee9b77411290249e763f9cb8492eea61803e91
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124389"
---
# <a name="global-query-filters"></a>Globální filtry dotazů

> [!NOTE]
> Tato funkce byla představena v EF Core 2,0.

Globální filtry dotazů jsou predikáty dotazu LINQ (obvykle předaný logický výraz LINQ *kde* – operátor dotazu) použít pro typy entit v modelu metadat (obvykle v *OnModelCreating*). Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti. Jsou některé běžné aplikace tuto funkci:

* **Obnovitelné odstranění** – definuje typu Entity *IsDeleted* vlastnost.
* **Víceklientská architektura** – definuje typu Entity *TenantId* vlastnost.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak použít globální filtry dotazů k implementaci chování dotazu obnovitelného odstranění a více tenantů v modelu jednoduché blogů.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) na Githubu.

Nejprve definujte entity:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Všimněte si deklarace pole _tenantId_ v entitě _blogu_ . Ten se použije pro každou instanci blogu přidružit konkrétního tenanta. Je také definováno _IsDeleted_ vlastnost _příspěvek_ typu entity. To se používá ke sledování toho, jestli _příspěvek_ instance byla "obnovitelně odstraněný". To znamená, že instance je označen jako odstraněný bez fyzickým odebráním podkladová data.

V dalším kroku Nakonfigurujte filtry dotazu v _OnModelCreating_ pomocí `HasQueryFilter` rozhraní API.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Predikátu výraz předaný _HasQueryFilter_ volání se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.

> [!TIP]
> Všimněte si použití úrovně pole instance DbContext: `_tenantId` slouží k nastavení aktuálního tenanta. Filtry na úrovni modelu bude používat hodnotu z instance správného kontextu (to znamená, instance, který spouští dotaz).

> [!NOTE]
> V současné době není možné definovat více filtrů dotazů na stejnou entitu. použije se jenom poslední z nich. Můžete však definovat jeden filtr s více podmínkami pomocí logického operátoru _and_ ([`&&` in C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).

## <a name="disabling-filters"></a>Zakázání filtrů

Může být zakázané filtry pro jednotlivé dotazy LINQ pomocí `IgnoreQueryFilters()` operátor.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Omezení

Globální filtry dotazů mají následující omezení:

* Filtry je možné definovat pouze pro kořenový typ Entity hierarchie dědičnosti.
