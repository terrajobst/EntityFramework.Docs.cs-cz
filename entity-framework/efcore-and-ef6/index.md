---
title: "Porovnání EF Core a EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4609ecbc9e24d8a359694d256523c64141b5ff62
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/20/2018
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="5cb31-102">Porovnání EF Core a EF6</span><span class="sxs-lookup"><span data-stu-id="5cb31-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="5cb31-103">Existují dvě různé verze Entity Framework, Entity Framework Core a Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="5cb31-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="5cb31-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5cb31-104">Entity Framework 6</span></span>

<span data-ttu-id="5cb31-105">Rozhraní Entity Framework 6 (EF6) je technologie pokusů a otestovaná data přístup s mnoha let funkcí a ustálení.</span><span class="sxs-lookup"><span data-stu-id="5cb31-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="5cb31-106">Ho prvního vydání v 2008, v rámci rozhraní .NET Framework 3.5 SP1 a Visual Studio 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="5cb31-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="5cb31-107">Od verze EF4.1 je dodávána jako [balíček EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) -aktuálně jeden z nejoblíbenější balíčky na NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="5cb31-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="5cb31-108">EF6 nadále podporované produktu a bude dál zobrazovat oprav chyb a menší vylepšení nějakou dobu pochází.</span><span class="sxs-lookup"><span data-stu-id="5cb31-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="5cb31-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5cb31-109">Entity Framework Core</span></span>

<span data-ttu-id="5cb31-110">Základní Entity Framework (EF jader) je jednoduché, rozšiřitelný a napříč platformami verzi rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5cb31-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="5cb31-111">Základní EF představuje mnoho vylepšení a nové funkce ve srovnání s EF6.</span><span class="sxs-lookup"><span data-stu-id="5cb31-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="5cb31-112">Ve stejnou dobu je základní EF nový kód základní a není vyspělá jako EF6.</span><span class="sxs-lookup"><span data-stu-id="5cb31-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="5cb31-113">Základní EF zajišťuje možnosti vývojáře z EF6 a většina nejvyšší úrovně rozhraní API, zůstane příliš, tak bude působí povědomý zaměstnance, kteří použili EF6 EF jádra.</span><span class="sxs-lookup"><span data-stu-id="5cb31-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="5cb31-114">Ve stejnou dobu EF základní je sestavena úplně novou sadu základní součásti služby.</span><span class="sxs-lookup"><span data-stu-id="5cb31-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="5cb31-115">To znamená, že základní EF nedědí automaticky všechny funkce z EF6.</span><span class="sxs-lookup"><span data-stu-id="5cb31-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="5cb31-116">Když některé z těchto funkcí se zobrazí v budoucích verzích, nebude další funkce, ne tak často používaná implementovat v EF jádra.</span><span class="sxs-lookup"><span data-stu-id="5cb31-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="5cb31-117">Nový rozšiřitelný a lightweight základní má také povolené nám přidat některé funkce do EF jádra, která nebude implementována v EF6 (například alternativní klíče, dávková aktualizace a vyhodnocení smíšený klienta a databáze v dotazech LINQ).</span><span class="sxs-lookup"><span data-stu-id="5cb31-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
