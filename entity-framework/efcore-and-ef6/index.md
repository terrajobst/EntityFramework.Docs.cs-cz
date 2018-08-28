---
title: Porovnání EF Core a EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997121"
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="4c59b-102">Porovnání EF Core a EF6</span><span class="sxs-lookup"><span data-stu-id="4c59b-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="4c59b-103">Existují dvě různé verze rozhraní Entity Framework, Entity Framework Core a Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="4c59b-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="4c59b-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4c59b-104">Entity Framework 6</span></span>

<span data-ttu-id="4c59b-105">Entity Framework 6 (EF6) je technologie přístup vyzkoušená a otestovaná data mnohaletých zkušeností s funkcemi a stabilizací.</span><span class="sxs-lookup"><span data-stu-id="4c59b-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="4c59b-106">To 2008, původně vydaný jako součást rozhraní .NET Framework 3.5 SP1 a Visual Studio 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="4c59b-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="4c59b-107">Od verze EF4.1 byla odeslaná jako [balíček EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) – právě jeden z nejoblíbenějších balíčků na NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="4c59b-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="4c59b-108">EF6 i nadále podporované produkty a bude dál zobrazovat, opravy chyb a menší vylepšení nechystáte nějakou dobu pocházet.</span><span class="sxs-lookup"><span data-stu-id="4c59b-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="4c59b-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4c59b-109">Entity Framework Core</span></span>

<span data-ttu-id="4c59b-110">Entity Framework Core (jádro EF) je jednoduchý, rozšiřitelná a multiplatformní verze Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4c59b-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="4c59b-111">EF Core přináší řadu vylepšení a nových funkcí ve srovnání s EF6.</span><span class="sxs-lookup"><span data-stu-id="4c59b-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="4c59b-112">EF Core ve stejnou dobu, je nový kód pro základní a ne vyspělá jako EF6.</span><span class="sxs-lookup"><span data-stu-id="4c59b-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="4c59b-113">EF Core udržuje prostředí pro vývojáře z EF6 a většina nejvyšší úrovně rozhraní API zůstanou stejné, v tak působí povědomý lidé, kteří použili EF6 EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c59b-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="4c59b-114">EF Core ve stejnou dobu, je sestavena zcela novou sadu základních komponent.</span><span class="sxs-lookup"><span data-stu-id="4c59b-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="4c59b-115">To znamená, že EF Core nedědí automaticky všechny funkce z EF6.</span><span class="sxs-lookup"><span data-stu-id="4c59b-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="4c59b-116">Zatímco některé z těchto funkcí se zobrazí v budoucích verzích, jiné méně často používaným funkcím se nebude implementovat v EF Core.</span><span class="sxs-lookup"><span data-stu-id="4c59b-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="4c59b-117">Nové, rozšiřitelné a zjednodušené core má zároveň nám to umožnilo přidáte některé funkce na EF Core, který se nebude implementovat v EF6 (například alternativní klíče, dávkové aktualizace a smíšené klientské/databáze vyhodnocení v LINQ dotazy).</span><span class="sxs-lookup"><span data-stu-id="4c59b-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
