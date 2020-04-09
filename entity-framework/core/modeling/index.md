---
title: Vytvoření a konfigurace modelu - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416415"
---
# <a name="creating-and-configuring-a-model"></a>Vytvoření a konfigurace modelu

Entity Framework používá sadu konvencí k vytvoření modelu na základě tvaru tříd entity. Můžete zadat další konfiguraci k doplnění nebo přepsání toho, co bylo zjištěno konvencí.

Tento článek popisuje konfiguraci, kterou lze použít u modelu, který cílí na jakékoli úložiště dat, a na konfiguraci, kterou lze použít při cílení na libovolnou relační databázi. Zprostředkovatelé mohou také povolit konfiguraci, která je specifická pro konkrétní úložiště dat. Dokumentace ke konfiguraci specifické pro zprostředkovatele naleznete v části [Zprostředkovatelé](../providers/index.md) databáze.

> [!TIP]  
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) můžete zobrazit na GitHubu.

## <a name="use-fluent-api-to-configure-a-model"></a>Konfigurace modelu pomocí plynulého rozhraní API

Můžete přepsat metodu `OnModelCreating` v odvozeném kontextu `ModelBuilder API` a použít ke konfiguraci modelu. Toto je nejvýkonnější způsob konfigurace a umožňuje konfiguraci, která má být zadána bez úprav tříd entit. Konfigurace rozhraní Fluent API má nejvyšší prioritu a přepíše konvence a poznámky k datům.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>Konfigurace modelu pomocí datových anotací

Můžete také použít atributy (označované jako datové poznámky) na vaše třídy a vlastnosti. Anotace dat budou přepsat konvence, ale budou přepsány konfigurací rozhraní Fluent API.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
