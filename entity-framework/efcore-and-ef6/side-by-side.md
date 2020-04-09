---
title: EF6 a EF Core – jejich použití ve stejné aplikaci
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419642"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Použití EF Core a EF6 ve stejné aplikaci

Je možné použít EF Core a EF6 ve stejné aplikaci nebo knihovně instalací obou balíčků NuGet.

Některé typy mají stejné názvy v EF Core a EF6 a liší se pouze podle oboru názvů, což může komplikovat použití EF Core a EF6 ve stejném souboru kódu. Nejednoznačnost lze snadno odstranit pomocí směrnic alias ujmenovávací obor. Příklad:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Pokud portujete existující aplikaci, která má více modelů EF, můžete selektivně přenést některé z nich ef core a pokračovat v používání EF6 pro ostatní.
