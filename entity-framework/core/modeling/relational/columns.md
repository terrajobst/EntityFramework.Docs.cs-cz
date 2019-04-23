---
title: Mapování sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929859"
---
# <a name="column-mapping"></a>Mapování sloupce

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Mapování sloupce určuje, jaká data sloupec by měl posílat dotaz z a uloženy v databázi.

## <a name="conventions"></a>Konvence

Podle konvence se k mapování sloupců se stejným názvem jako vlastnost nastavení každou vlastnost.

## <a name="data-annotations"></a>Datové poznámky

Anotací dat můžete použít ke konfiguraci sloupců, ke kterému je namapována vlastnost.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci sloupců, ke kterému je namapována vlastnost.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
