---
title: "EF6 a základní EF - jejich používání ve stejné aplikaci"
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
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>EF Core a EF6 současně ve stejné aplikaci

V případě .NET Frameworku je možné použít současně EF Core a EF6 ve stejné aplikaci. Za tímto účelem je nutné mít nainstalované příslušné NuGet balíčky pro EF Core a EF6. 

Mějte na vědomí, že některé typy (třídy) jsou obsažené v EF Core i v EF6 a mohou tak zkomplikovat práci s oběma ORM současně. Nejednoznačné a konfliktní části lze snadno vyřešit použitím aliasů pro obory názvů (namespaces), např.:

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

Pokud migrujete existující databázi, která má v aplikaci více EF modelů, můžete se rozhodnout některé z těchto modelů spravovat pomocí EF Core a jiné nadále spravovat pomocí EF6.
