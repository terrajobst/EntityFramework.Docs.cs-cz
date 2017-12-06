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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="5fbe2-102">Pomocí EF jádra a EF6 ve stejné aplikaci</span><span class="sxs-lookup"><span data-stu-id="5fbe2-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="5fbe2-103">Je možné použít EF jádra a EF6 ve stejném aplikace rozhraní .NET Framework nebo knihovna nainstalováním oba balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="5fbe2-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span> 

<span data-ttu-id="5fbe2-104">Některé typy se stejnými názvy v EF jádra a EF6 a liší pouze v oboru názvů, který může zkomplikovat pomocí EF jádra a EF6 ve stejném souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="5fbe2-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="5fbe2-105">Nejednoznačnosti lze snadno odebrat pomocí direktiv alias oboru názvů, například:</span><span class="sxs-lookup"><span data-stu-id="5fbe2-105">The ambiguity can be easily removed using namespace alias directives, e.g.:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

<span data-ttu-id="5fbe2-106">Pokud provádíte přenos existující aplikace, která má více EF modelů, můžete selektivně portu některá z nich na jádro EF a pokračovat v používání EF6 pro jiné.</span><span class="sxs-lookup"><span data-stu-id="5fbe2-106">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
