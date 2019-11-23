---
title: Vytvoření a konfigurace modelu – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 5b886226b16b5b1a1f01e6040e58d92ae8678d29
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197304"
---
# <a name="creating-and-configuring-a-model"></a>Vytvoření a konfigurace modelu

Entity Framework používá sadu konvencí pro sestavování modelu na základě tvaru tříd entit. Můžete zadat další konfiguraci pro doplnění nebo přepsání toho, co bylo zjištěno podle konvence.

Tento článek se zabývá konfigurací, kterou je možné použít pro model, který cílí na jakékoli úložiště dat a který je možné použít při cílení na jakoukoli relační databázi. Poskytovatelé taky můžou povolit konfiguraci, která je specifická pro konkrétní úložiště dat. Dokumentaci ke konkrétní konfiguraci poskytovatele najdete v části [poskytovatelé databáze](../providers/index.md) .

> [!TIP]  
> Na GitHubu můžete zobrazit [ukázkové](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) tohoto článku.

## <a name="use-fluent-api-to-configure-a-model"></a>Použití rozhraní Fluent API ke konfiguraci modelu

V odvozeném kontextu můžete přepsat metodu `OnModelCreating` a pomocí `ModelBuilder API` nakonfigurovat model. Jedná se o nejúčinnější způsob konfigurace a umožňuje zadat konfiguraci bez úprav tříd entit. Konfigurace rozhraní Fluent API má nejvyšší prioritu a přepíše konvence a datové poznámky.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a>Použití datových poznámek ke konfiguraci modelu

Můžete také použít atributy (známé jako datové poznámky) k vašim třídám a vlastnostem. Datové poznámky přepíšou konvence, ale budou přepsány konfigurací Fluent rozhraní API.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
