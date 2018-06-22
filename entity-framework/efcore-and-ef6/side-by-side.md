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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="5ce19-102">Použití EF Core a EF6 současně ve stejné aplikaci</span><span class="sxs-lookup"><span data-stu-id="5ce19-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="5ce19-103">EF Core a EF6 je možné použít současně ve stejné aplikaci nebo knihovně .NET Frameworku. Je k tomu potřeba mít nainstalované příslušné balíčky NuGet pro EF Core i EF6.</span><span class="sxs-lookup"><span data-stu-id="5ce19-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span> 

<span data-ttu-id="5ce19-104">Některé typy používají v EF Core a EF6 stejné názvy a liší se pouze oborem názvů. Při použítí EF Core i EF6 ve stejném souboru kódu tak mohou nastat komplikace.</span><span class="sxs-lookup"><span data-stu-id="5ce19-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="5ce19-105">Nejednoznačné situace je možné snadno vyřešit použitím direktiv aliasů pro obory názvů, například:</span><span class="sxs-lookup"><span data-stu-id="5ce19-105">The ambiguity can be easily removed using namespace alias directives, e.g.:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

<span data-ttu-id="5ce19-106">Pokud přenášíte existující aplikaci s více EF modely, můžete se rozhodnout některé z těchto modelů spravovat pomocí EF Core a jiné nadále spravovat pomocí EF6.</span><span class="sxs-lookup"><span data-stu-id="5ce19-106">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
