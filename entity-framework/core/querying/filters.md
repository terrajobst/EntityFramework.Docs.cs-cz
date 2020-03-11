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
# <a name="global-query-filters"></a>Globální filtry dotazů

> [!NOTE]
> Tato funkce byla představena v EF Core 2,0.

Globální filtry dotazů jsou predikáty dotazů LINQ (logický výraz obvykle předaný operátoru dotazu *LINQ)* , který je použit na typy entit v modelu metadat (obvykle v *OnModelCreating*). Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti. Jsou některé běžné aplikace tuto funkci:

* **Obnovitelné odstranění** – typ entity definuje vlastnost *IsDeleted* .
* **Víceklientská** architektura – typ entity definuje vlastnost *TenantId* .

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak použít globální filtry dotazů k implementaci chování dotazu obnovitelného odstranění a více tenantů v modelu jednoduché blogů.

> [!TIP]
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) tohoto článku můžete zobrazit na GitHubu.

Nejprve definujte entity:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Všimněte si deklarace pole _tenantId_ v entitě _blogu_ . Ten se použije pro každou instanci blogu přidružit konkrétního tenanta. Také definováno je vlastnost _IsDeleted_ v typu entity _post_ . Slouží k udržení přehledu o tom, zda byla instance _příspěvku_ "soft-Deleted". To znamená, že instance je označen jako odstraněný bez fyzickým odebráním podkladová data.

Dále nakonfigurujte filtry dotazů v _OnModelCreating_ pomocí rozhraní API pro `HasQueryFilter`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Výrazy predikátu předané voláním _HasQueryFilter_ se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.

> [!TIP]
> Všimněte si použití pole na úrovni instance DbContext: `_tenantId` slouží k nastavení aktuálního tenanta. Filtry na úrovni modelu bude používat hodnotu z instance správného kontextu (to znamená, instance, který spouští dotaz).

> [!NOTE]
> V současné době není možné definovat více filtrů dotazů na stejnou entitu. použije se jenom poslední z nich. Můžete však definovat jeden filtr s více podmínkami pomocí logického operátoru _and_ ([`&&` in C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).

## <a name="disabling-filters"></a>Zakázání filtrů

Filtry mohou být zakázány pro jednotlivé dotazy LINQ pomocí operátoru `IgnoreQueryFilters()`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Omezení

Globální filtry dotazů mají následující omezení:

* Filtry je možné definovat pouze pro kořenový typ Entity hierarchie dědičnosti.
