---
title: Alternativní klíče (jedinečná omezení) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994188"
---
# <a name="alternate-keys-unique-constraints"></a>Alternativní klíče (jedinečná omezení)

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Jedinečné omezení se používá pro každý alternativního klíče v modelu.

## <a name="conventions"></a>Konvence

Podle konvence, bude mít název indexu a omezení, které jsou zavedeny pro alternativního klíče `AK_<type name>_<property name>`. Pro složené alternativní klíče `<property name>` stane podtržítka oddělený seznam názvů vlastností.

## <a name="data-annotations"></a>Datové poznámky

Jedinečné omezení nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete nakonfigurovat položku Název indexu a omezení pro alternativního klíče.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
