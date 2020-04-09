---
title: Přehled jádra rámce entity – ef jádro
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416846"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core je lehká, rozšiřitelná, [open source](https://github.com/aspnet/EntityFrameworkCore) a multiplatformní verze populární technologie přístupu k datům Entity Framework.

EF Core může sloužit jako objektrelační mapovač (O/RM), který umožňuje vývojářům rozhraní .NET pracovat s databází pomocí objektů .NET a eliminuje potřebu většiny kódu přístupu k datům, který obvykle potřebují k zápisu.

EF Core podporuje mnoho databázových strojů, podrobnosti najdete v [tématu Zprostředkovatelé databáze.](providers/index.md)

## <a name="the-model"></a>The Model

S EF Core, přístup k datům se provádí pomocí modelu. Model se skládá z tříd entit a objekt kontextu, který představuje relaci s databází, což umožňuje dotazovat a ukládat data. Další informace najdete [v tématu Vytvoření modelu.](modeling/index.md)

Můžete vygenerovat model z existující databáze, ručně kód ovat model tak, aby odpovídal databázi, nebo použít [ef migrace](managing-schemas/migrations/index.md) k vytvoření databáze z modelu a pak ji vyvíjet podle změny modelu v průběhu času.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Dotazování

Instance tříd entity se načítají z databáze pomocí jazyka Integrovaný dotaz (LINQ). Další informace najdete [v tématu Dotazování](querying/index.md) se na data.

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Ukládání dat

Data jsou v databázi vytvářena, odstraňována a upravována pomocí instancí tříd entit. Další informace najdete v [tématu Ukládání dat.](saving/index.md)

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Další kroky

Úvodní kurzy najdete v tématu [Začínáme s jádrem entity frameworku](get-started/index.md).
