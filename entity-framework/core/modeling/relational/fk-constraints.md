---
title: Omezení cizího klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656000"
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
