---
title: Výchozí hodnoty – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655907"
---
# <a name="default-values"></a>Výchozí hodnoty

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Výchozí hodnota sloupce je hodnota, která bude vložena při vložení nového řádku, ale pro sloupec není zadána žádná hodnota.

## <a name="conventions"></a>Konvence

Podle konvence není výchozí hodnota nakonfigurovaná.

## <a name="data-annotations"></a>Datové poznámky

Výchozí hodnotu nelze nastavit pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení výchozí hodnoty pro vlastnost.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

Můžete také zadat fragment SQL, který se použije k výpočtu výchozí hodnoty.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
