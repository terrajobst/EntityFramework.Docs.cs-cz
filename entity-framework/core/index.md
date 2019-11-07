---
title: Přehled Entity Framework Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655623"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Jádro Entity Framework (EF) je odlehčená, rozšiřitelná, [Open zdrojová](https://github.com/aspnet/EntityFrameworkCore) a verze pro různé platformy, která je oblíbená entity Frameworká technologie pro přístup k datům.

EF Core může sloužit jako relační Mapovač (O/RM), což umožňuje vývojářům rozhraní .NET pracovat s databází pomocí objektů .NET a eliminovat nutnost většiny kódu přístupu k datům, které obvykle potřebují napsat.

EF Core podporuje mnoho databázových strojů, další informace najdete v tématu [poskytovatelé databází](providers/index.md) .

## <a name="the-model"></a>Model

Při použití EF Core se přístup k datům provádí pomocí modelu. Model je tvořen třídami entit a kontextový objekt, který představuje relaci s databází, a umožňuje vám dotazování a ukládání dat. Další informace najdete v tématu [Vytvoření modelu](modeling/index.md) .

Můžete vygenerovat model z existující databáze, kód pro psaní rukou modelu, který odpovídá vaší databázi, nebo použít [migrace EF](managing-schemas/migrations/index.md) k vytvoření databáze z modelu a pak ji vyvíjet jako změny modelu v průběhu času.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Dotazování

Instance tříd vaší entity se načítají z databáze pomocí jazyka LINQ (Language Integrated Query). Další informace najdete v tématu [dotazování na data](querying/index.md) .

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Ukládání dat

Data se vytvářejí, odstraňují a upravují v databázi s použitím instancí tříd vaší entity. Další informace najdete v tématu [ukládání dat](saving/index.md) .

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Další kroky

Úvodní kurzy najdete v tématu [Začínáme s Entity Framework Core](get-started/index.md).
