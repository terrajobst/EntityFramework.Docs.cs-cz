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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="ce0b7-102">Použití EF Core a EF6 ve stejné aplikaci</span><span class="sxs-lookup"><span data-stu-id="ce0b7-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="ce0b7-103">Je možné použít EF Core a EF6 ve stejné aplikaci nebo knihovně instalací balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="ce0b7-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="ce0b7-104">Některé typy mají stejné názvy v EF Core a EF6 a liší se pouze oborem názvů, což může zkomplikovat použití EF Core a EF6 ve stejném souboru kódu.</span><span class="sxs-lookup"><span data-stu-id="ce0b7-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="ce0b7-105">Nejednoznačnost se dá snadno odebrat pomocí direktiv aliasu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ce0b7-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="ce0b7-106">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ce0b7-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="ce0b7-107">Pokud předáváte existující aplikaci, která má více modelů EF, můžete zvolit selektivní port některých z nich pro EF Core a nadále používat EF6 pro ostatní.</span><span class="sxs-lookup"><span data-stu-id="ce0b7-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
