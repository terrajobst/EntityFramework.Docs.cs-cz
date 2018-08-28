---
title: EF6 a EF Core – použití ve stejné aplikaci
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 6f95c02f4f24746605794832b1e26744fc554580
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995706"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Ve stejné aplikaci pomocí EF Core a EF6

Je možné použít EF Core a EF6 ve stejné aplikaci rozhraní .NET Framework nebo knihovna nainstalováním oba balíčky NuGet.

Některé typy mají stejné názvy v EF Core a EF6 a liší pouze obor názvů, který může zkomplikovat pomocí EF Core a EF6 ve stejném souboru kódu. Nejednoznačnosti lze snadno odebrat pomocí direktivy alias oboru názvů. Příklad:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Pokud jsou přenesení existující aplikaci, která má více modelů EF, můžete selektivně portu některé z nich na EF Core a EF6 používat pro jiné.
