---
title: EF6 a EF Core – použití ve stejné aplikaci
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: ead251c5454473738c2f2bfdac6557aa3e1c5591
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949071"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Ve stejné aplikaci pomocí EF Core a EF6

Je možné použít EF Core a EF6 ve stejné aplikaci rozhraní .NET Framework nebo knihovna nainstalováním oba balíčky NuGet.

Některé typy mají stejné názvy v EF Core a EF6 a liší pouze obor názvů, který může zkomplikovat pomocí EF Core a EF6 ve stejném souboru kódu. Nejednoznačnosti lze snadno odebrat pomocí direktivy alias oboru názvů. Příklad:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Pokud jsou přenesení existující aplikaci, která má více modelů EF, můžete selektivně portu některé z nich na EF Core a EF6 používat pro jiné.
