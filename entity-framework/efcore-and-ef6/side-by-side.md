---
title: EF6 a základní EF - jejich používání ve stejné aplikaci
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: f6eb4bf7d99fbc61f8ffbd0dc7c6c17789395303
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054211"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Použití EF Core a EF6 současně ve stejné aplikaci

EF Core a EF6 je možné použít současně ve stejné aplikaci nebo knihovně .NET Frameworku. Je k tomu potřeba mít nainstalované příslušné balíčky NuGet pro EF Core i EF6. 

Některé typy používají v EF Core a EF6 stejné názvy a liší se pouze oborem názvů. Při použítí EF Core i EF6 ve stejném souboru kódu tak mohou nastat komplikace. Nejednoznačné situace je možné snadno vyřešit použitím direktiv aliasů pro obory názvů, například:

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

Pokud přenášíte existující aplikaci s více EF modely, můžete se rozhodnout některé z těchto modelů spravovat pomocí EF Core a jiné nadále spravovat pomocí EF6.
