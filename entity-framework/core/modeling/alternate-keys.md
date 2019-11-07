---
title: Alternativní klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655778"
---
# <a name="alternate-keys"></a>Alternativní klíče

Alternativní klíč slouží jako alternativní jedinečný identifikátor pro každou instanci entity kromě primárního klíče. Alternativní klíče lze použít jako cíl relace. Při použití relační databáze se tato mapa mapuje na koncept jedinečného indexu nebo omezení pro sloupce nebo sloupce alternativních klíčů a jedno nebo více omezení cizího klíče, které odkazují na sloupce.

> [!TIP]  
> Pokud chcete vymáhat jedinečnost sloupce a chcete místo alternativního klíče vyhovět jedinečným indexům, přečtěte si téma [indexy](indexes.md). V EF poskytují alternativní klíče větší funkce než jedinečné indexy, protože je lze použít jako cíl cizího klíče.

V případě potřeby jsou obvykle zavedeny alternativní klíče a není nutné je ručně konfigurovat. Další podrobnosti najdete v tématu věnovaném [konvencím](#conventions) .

## <a name="conventions"></a>Konvence

Podle konvence se pro vás zavádí alternativní klíč, pokud identifikujete vlastnost, která není primárním klíčem, jako cíl relace.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a>Datové poznámky

Alternativní klíče nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Pomocí rozhraní Fluent API můžete nakonfigurovat jednu vlastnost jako alternativní klíč.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, aby byl alternativním klíčem (označovaným jako složený alternativní klíč).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
