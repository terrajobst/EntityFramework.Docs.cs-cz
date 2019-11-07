---
title: Výchozí schéma – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655970"
---
# <a name="default-schema"></a>Výchozí schéma

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Výchozím schématem je schéma databáze, ve kterém budou objekty vytvořeny, pokud není pro daný objekt explicitně nakonfigurováno schéma.

## <a name="conventions"></a>Konvence

Podle konvence poskytovatel databáze zvolí nejvhodnější výchozí schéma. Například Microsoft SQL Server použije schéma `dbo` a SQLite nepoužije schéma (vzhledem k tomu, že schémata nejsou podporovaná v SQLite).

## <a name="data-annotations"></a>Datové poznámky

Výchozí schéma nelze nastavit pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení výchozího schématu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
