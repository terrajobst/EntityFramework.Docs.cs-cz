---
title: Vytvoření modelu – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 78a8ffd2393a914edf737104f14e41f8a9074ad5
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929895"
---
# <a name="creating-and-configuring-a-model"></a>Vytváření a konfiguraci modelu

Entity Framework používá sadu konvence sestavit model založený na obrazec tříd entit. Můžete zadat další konfiguraci k doplnění a/nebo přepsat, co bylo zjištěno konvencí.

Tento článek se týká konfigurace, který lze použít k modelu, který cílí na jakékoli úložiště dat a který, který je možné použít při cílení na jakoukoli relační databázi. Poskytovatelé může také umožnit konfiguraci, která je specifická pro konkrétní datové úložiště. Dokumentace ke službě na konkrétní konfiguraci poskytovatele najdete v článku [poskytovatelé databází](../providers/index.md) oddílu.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) na Githubu.

## <a name="use-fluent-api-to-configure-a-model"></a>Použití rozhraní fluent API pro konfiguraci modelu

Je možné přepsat `OnModelCreating` metoda v odvozené kontextu a použití `ModelBuilder API` ke konfiguraci modelu. Toto je nejvýkonnější metody konfigurace a umožňuje konfiguraci zadat bez změny vašich tříd entit. Konfigurace Fluent API má nejvyšší prioritu a přepíše poznámky konvence a data.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a>Použití anotací dat při konfiguraci modelu

Můžete také použít atributy (označuje se jako datové poznámky) do vaší třídy a vlastnosti. Datové poznámky přepíše konvence, ale rozhraní Fluent API konfigurace se přepíše.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]
