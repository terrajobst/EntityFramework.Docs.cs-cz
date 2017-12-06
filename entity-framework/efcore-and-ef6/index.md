---
title: "Porovnání EF základní & EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="e242b-102">Porovnání EF základní & EF6</span><span class="sxs-lookup"><span data-stu-id="e242b-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="e242b-103">Existují dvě verze Entity Framework, Entity Framework Core a Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="e242b-103">There are two versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="e242b-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e242b-104">Entity Framework 6</span></span>

<span data-ttu-id="e242b-105">Rozhraní Entity Framework 6 (EF6) je technologie pokusů a otestovaná data přístup s mnoha let funkcí a ustálení.</span><span class="sxs-lookup"><span data-stu-id="e242b-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="e242b-106">Ho prvního vydání v 2008, v rámci rozhraní .NET Framework 3.5 SP1 a Visual Studio 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="e242b-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="e242b-107">Od verze EF4.1 je dodávána jako [balíček EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) -aktuálně nejoblíbenější balíček v NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="e242b-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently the most popular package on NuGet.org.</span></span>

<span data-ttu-id="e242b-108">EF6 nadále podporované produktu a bude dál zobrazovat oprav chyb a menší vylepšení nějakou dobu pochází.</span><span class="sxs-lookup"><span data-stu-id="e242b-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="e242b-109">Základní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e242b-109">Entity Framework Core</span></span>

<span data-ttu-id="e242b-110">Základní Entity Framework (EF jader) je jednoduché, rozšiřitelný a napříč platformami verzi rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e242b-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="e242b-111">Základní EF představuje mnoho vylepšení a nové funkce ve srovnání s EF6.</span><span class="sxs-lookup"><span data-stu-id="e242b-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="e242b-112">Ve stejnou dobu je základní EF nový kód základní a není vyspělá jako EF6.</span><span class="sxs-lookup"><span data-stu-id="e242b-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="e242b-113">Základní EF zajišťuje možnosti vývojáře z EF6 a většina nejvyšší úrovně rozhraní API, zůstane příliš, tak bude působí povědomý zaměstnance, kteří použili EF6 EF jádra.</span><span class="sxs-lookup"><span data-stu-id="e242b-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="e242b-114">Ve stejnou dobu EF základní je sestavena úplně novou sadu základní součásti služby.</span><span class="sxs-lookup"><span data-stu-id="e242b-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="e242b-115">To znamená, že základní EF nedědí automaticky všechny funkce z EF6.</span><span class="sxs-lookup"><span data-stu-id="e242b-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="e242b-116">Některé z těchto funkcí se zobrazí v budoucích verzích (například opožděného načítání a připojení odolnosti), dalších méně často používány, že funkce nebudou implementované v EF jádra.</span><span class="sxs-lookup"><span data-stu-id="e242b-116">Some of these features will show up in future releases (such as lazy loading and connection resiliency), other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="e242b-117">Nový rozšiřitelný a lightweight základní má také povolené nám přidat některé funkce do EF jádra, která nebude implementována v EF6 (například alternativní klíče a vyhodnocení smíšený klienta a databáze v dotazech LINQ).</span><span class="sxs-lookup"><span data-stu-id="e242b-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys and mixed client/database evaluation in LINQ queries).</span></span>
