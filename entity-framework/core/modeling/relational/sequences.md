---
title: Sekvence – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656118"
---
# <a name="sequences"></a>Sekvence

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Sekvence generuje sekvenční číselné hodnoty v databázi. Sekvence nejsou spojeny s konkrétní tabulkou.

## <a name="conventions"></a>Konvence

Podle konvence nejsou sekvence do modelu zavedeny.

## <a name="data-annotations"></a>Datové poznámky

Nelze konfigurovat sekvenci pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k vytvoření sekvence v modelu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

Můžete také nakonfigurovat další aspekt sekvence, například její schéma, počáteční hodnotu a přírůstek.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

Po zavedení sekvence je můžete použít ke generování hodnot vlastností v modelu. Můžete například použít [výchozí hodnoty](default-values.md) pro vložení další hodnoty z sekvence.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
