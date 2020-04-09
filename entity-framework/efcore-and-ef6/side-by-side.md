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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="0f450-102">Použití EF Core a EF6 ve stejné aplikaci</span><span class="sxs-lookup"><span data-stu-id="0f450-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="0f450-103">Je možné použít EF Core a EF6 ve stejné aplikaci nebo knihovně instalací obou balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="0f450-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="0f450-104">Některé typy mají stejné názvy v EF Core a EF6 a liší se pouze podle oboru názvů, což může komplikovat použití EF Core a EF6 ve stejném souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="0f450-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="0f450-105">Nejednoznačnost lze snadno odstranit pomocí směrnic alias ujmenovávací obor.</span><span class="sxs-lookup"><span data-stu-id="0f450-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="0f450-106">Příklad:</span><span class="sxs-lookup"><span data-stu-id="0f450-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="0f450-107">Pokud portujete existující aplikaci, která má více modelů EF, můžete selektivně přenést některé z nich ef core a pokračovat v používání EF6 pro ostatní.</span><span class="sxs-lookup"><span data-stu-id="0f450-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
