---
title: "Alternativní klíče (jedinečná omezení) - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys-unique-constraints"></a>Alternativní klíče (jedinečná omezení)

> [!NOTE]  
> Obecně se vztahuje na relační databáze konfigurace v této části. Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).

Pro každý alternativní klíč v modelu uvádíme jedinečné omezení.

## <a name="conventions"></a>Konvence

Podle konvence, bude mít název indexu a omezení, které jsou zavedené pro alternativní klíč `AK_<type name>_<property name>`. Pro složené klíče alternativní `<property name>` stane podtržítka oddělený seznam názvů vlastností.

## <a name="data-annotations"></a>Datových poznámek

Jedinečná omezení nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete nakonfigurovat položku Název indexu a omezení pro alternativní klíč.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
