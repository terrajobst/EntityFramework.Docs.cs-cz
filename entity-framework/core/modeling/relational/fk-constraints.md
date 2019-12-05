---
title: Omezení cizího klíče – EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824588"
---
# <a name="foreign-key-constraints"></a>Omezení cizího klíče

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Pro každou relaci v modelu je představeno omezení cizího klíče.

## <a name="conventions"></a>Konvence

Podle konvence jsou omezení cizího klíče pojmenována `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Pro složené cizí klíče se `<foreign key property name>` jako podtržítko oddělený seznam názvů vlastností cizích klíčů.

## <a name="data-annotations"></a>Datové poznámky

Názvy omezení cizího klíče nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci názvu omezení cizího klíče pro relaci.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
