---
title: Indexy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655702"
---
# <a name="indexes"></a>Indexy

Indexy představují společný pojem v mnoha úložištích dat. I když se jejich implementace v úložišti dat může lišit, používají se k tomu, aby vyhledávání na základě sloupce (nebo sady sloupců) bylo efektivnější.

## <a name="conventions"></a>Konvence

Podle konvence se v každé vlastnosti (nebo sadě vlastností) vytvoří index, který se používá jako cizí klíč.

## <a name="data-annotations"></a>Datové poznámky

Indexy nelze vytvořit pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení indexu pro jednu vlastnost. Ve výchozím nastavení indexy nejsou jedinečné.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

Můžete také určit, že by měl být index jedinečný, což znamená, že žádné dvě entity nemohou mít stejné hodnoty pro danou vlastnost (y).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

Index můžete zadat také pro více než jeden sloupec.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> Pro jednu jedinečnou sadu vlastností je k dispozici pouze jeden index. Pokud použijete rozhraní Fluent API ke konfiguraci indexu pro sadu vlastností, které už mají definovaný index, buď podle konvence, nebo z předchozí konfigurace, bude změna definice tohoto indexu. To je užitečné, pokud chcete dále konfigurovat index, který byl vytvořen pomocí konvence.
