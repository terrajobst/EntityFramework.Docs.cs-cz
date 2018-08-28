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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="dbc10-102">Ve stejné aplikaci pomocí EF Core a EF6</span><span class="sxs-lookup"><span data-stu-id="dbc10-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="dbc10-103">Je možné použít EF Core a EF6 ve stejné aplikaci rozhraní .NET Framework nebo knihovna nainstalováním oba balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="dbc10-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="dbc10-104">Některé typy mají stejné názvy v EF Core a EF6 a liší pouze obor názvů, který může zkomplikovat pomocí EF Core a EF6 ve stejném souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="dbc10-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="dbc10-105">Nejednoznačnosti lze snadno odebrat pomocí direktivy alias oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="dbc10-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="dbc10-106">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dbc10-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="dbc10-107">Pokud jsou přenesení existující aplikaci, která má více modelů EF, můžete selektivně portu některé z nich na EF Core a EF6 používat pro jiné.</span><span class="sxs-lookup"><span data-stu-id="dbc10-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
