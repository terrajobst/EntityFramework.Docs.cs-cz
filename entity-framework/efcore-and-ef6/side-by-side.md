---
title: EF6 a EF Core – jejich použití ve stejné aplikaci
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 8bf9f51c0e5c4b1b3adf4a6a9a894689dc13d2d9
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149293"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Použití EF Core a EF6 ve stejné aplikaci

Je možné použít EF Core a EF6 ve stejné aplikaci nebo knihovně instalací balíčků NuGet.

Některé typy mají stejné názvy v EF Core a EF6 a liší se pouze oborem názvů, což může zkomplikovat použití EF Core a EF6 ve stejném souboru kódu. Nejednoznačnost se dá snadno odebrat pomocí direktiv aliasu oboru názvů. Příklad:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Pokud předáváte existující aplikaci, která má více modelů EF, můžete zvolit selektivní port některých z nich pro EF Core a nadále používat EF6 pro ostatní.
