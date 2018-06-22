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
ms.locfileid: "26054760"
---
# <a name="global-query-filters"></a>Filtry globální dotazu

Dotaz globální filtry jsou predikáty dotaz LINQ (logický výraz obvykle předaný LINQ *kde* – operátor dotazu) u typy entit ve model metadat (obvykle ve *OnModelCreating*). Tyto filtry jsou automaticky použita pro všechny dotazy LINQ zahrnující tyto typy entit, včetně typů entit odkazované nepřímo, jako prostřednictvím zahrnout nebo přímé navigační vlastnost odkazů. Některé běžné aplikace této funkce jsou:

* **Soft odstranit** -definuje typu Entity *IsDeleted* vlastnost.
* **Víceklientský** -definuje typu Entity *TenantId* vlastnost.

## <a name="example"></a>Příklad

Následující příklad ukazuje, jak použít globální filtry dotazu k implementaci chování dotazu konfigurace soft odstranění a víceklientský v modelu jednoduché blogu.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) na Githubu.

Nejprve definujte entity:

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Všimněte si prohlášení o výraz __tenantId_ na _Blog_ entity. Bude se používat pro přidružení každá instance Blog konkrétní klientovi. Je také definován _IsDeleted_ vlastnost _Post_ typ entity. Slouží k zaznamenávat, jestli se _Post_ instance byl "obnovitelné odstranění". Tj. Instance je označena jako odstraněné withouth fyzicky odebrání základní data.

V dalším kroku Nakonfigurujte filtry dotazu v _OnModelCreating_ pomocí ```HasQueryFilter``` rozhraní API.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

Předaný predikátem výrazy _HasQueryFilter_ volání se teď automaticky použijí na všechny dotazy LINQ pro tyto typy.

> [!TIP]
> Všimněte si použití úrovni poli instance DbContext: ```_tenantId``` použít k nastavení aktuální klienta. Filtry na úrovni modelu použije hodnotu z instance správný kontext. Tj. Instance, kterou je zpracování dotazu.

## <a name="disabling-filters"></a>Zakázání filtrů

Může být zakázané filtry pro jednotlivé dotazy LINQ pomocí ```IgnoreQueryFilters()``` operátor.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Omezení

Globální dotaz filtry mají následující omezení:

* Filtry nesmí obsahovat odkazy na navigační vlastnosti.
* Filtry lze definovat pouze pro kořenové typ Entity hierarchie dědičnosti.
