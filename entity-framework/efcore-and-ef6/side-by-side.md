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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Pomocí EF jádra a EF6 ve stejné aplikaci

Je možné použít EF jádra a EF6 ve stejném aplikace rozhraní .NET Framework nebo knihovna nainstalováním oba balíčky NuGet. 

Některé typy se stejnými názvy v EF jádra a EF6 a liší pouze v oboru názvů, který může zkomplikovat pomocí EF jádra a EF6 ve stejném souboru kódu. Nejednoznačnosti lze snadno odebrat pomocí direktiv alias oboru názvů, například:

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

Pokud provádíte přenos existující aplikace, která má více EF modelů, můžete selektivně portu některá z nich na jádro EF a pokračovat v používání EF6 pro jiné.
