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
# <a name="global-query-filters"></a>Globální filtry dotazů

> [!NOTE]
> Tato funkce byla zavedena v EF Core 2.0.

Globální filtry dotazů jsou predikáty dotazů LINQ (logický výraz obvykle předávaný operátoru linq *where* query) aplikovaný na typy entit v modelu metadat (obvykle v *OnModelCreating).* Tyto filtry jsou automaticky použity na všechny dotazy LINQ zahrnující tyto typy entit, včetně typů entit, na které se nepřímo odkazuje, například pomocí odkazů na vlastnosti Include nebo přímé navigace. Některé běžné aplikace této funkce jsou:

* **Obnovitelné odstranění** - Typ entity definuje vlastnost *IsDeleted.*
* **Víceklientský** - Typ entity definuje *vlastnost TenantId.*

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak pomocí globálních filtrů dotazů implementovat chování dotazů s měkkým odstraněním a více nájemců v jednoduchém modelu blogování.

> [!TIP]
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) můžete zobrazit na GitHubu.

Nejprve definujte entity:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Poznamenejte si deklaraci pole _tenantId_ v entitě _Blog._ To se použije k přidružení každé instance blogu k určitému tenantovi. Definována je také vlastnost _IsDeleted_ u typu entity _Post._ Používá se ke sledování, zda _post_ instance byla "obnovitelné odstranění". To znamená, že instance je označena jako odstraněná bez fyzického odebrání podkladových dat.

Dále nakonfigurujte filtry dotazů v _OnModelCreating_ pomocí `HasQueryFilter` rozhraní API.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Predikátové výrazy předané volání _HasQueryFilter_ budou nyní automaticky použity pro všechny dotazy LINQ pro tyto typy.

> [!TIP]
> Všimněte si použití pole úrovně `_tenantId` instance DbContext: slouží k nastavení aktuálního klienta. Filtry na úrovni modelu budou používat hodnotu z instance správného kontextu (to znamená, že instance, která je provádění dotazu).

> [!NOTE]
> V současné době není možné definovat více filtrů dotazů na stejné entitě - bude použita pouze poslední. Můžete však definovat jeden filtr s více podmínkami pomocí logického operátoru _AND_ ([ `&&` v c#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).

## <a name="disabling-filters"></a>Zakázání filtrů

Filtry mohou být zakázány pro jednotlivé `IgnoreQueryFilters()` dotazy LINQ pomocí operátoru.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Omezení

Globální filtry dotazů mají následující omezení:

* Filtry lze definovat pouze pro kořenový typ entity hierarchie dědičnosti.
