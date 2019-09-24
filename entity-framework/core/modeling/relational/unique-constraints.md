---
title: Alternativní klíče (jedinečná omezení) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197612"
---
# <a name="alternate-keys-unique-constraints"></a>Alternativní klíče (jedinečná omezení)

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Pro každý alternativní klíč v modelu se zavádí jedinečné omezení.

## <a name="conventions"></a>Konvence

Podle konvence se pojmenuje `AK_<type name>_<property name>`index a omezení, které jsou představené pro alternativní klíč. U složených alternativních klíčů `<property name>` se jako seznam názvů vlastností oddělí znak podtržení.

## <a name="data-annotations"></a>Datové poznámky

Jedinečná omezení nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci indexu a názvu omezení pro alternativní klíč.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
