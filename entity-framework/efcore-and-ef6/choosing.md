---
title: "Základní EF a EF6 – což jeden je pro vás nejvhodnější"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/20/2018
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="ad95e-102">Základní EF a EF6: který z nich je pro vás nejvhodnější</span><span class="sxs-lookup"><span data-stu-id="ad95e-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="ad95e-103">Následující informace vám pomůže vybrat mezi Entity Framework Core a Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ad95e-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="ad95e-104">Pokyny pro nové aplikace</span><span class="sxs-lookup"><span data-stu-id="ad95e-104">Guidance for new applications</span></span>

<span data-ttu-id="ad95e-105">Zvažte použití EF jádra pro nové aplikace, pokud chcete využít výhod všechny funkce EF jádra a aplikace nevyžaduje žádné funkce, které zatím nejsou implementované v EF jádra.</span><span class="sxs-lookup"><span data-stu-id="ad95e-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="ad95e-106">EF6 vyžaduje rozhraní .NET Framework 4.0 (nebo novější verzi) a je podporován pouze v systému Windows (tj, že nefunguje na .NET Core a není podporováno v jiných operačních systémech), ale je stále vhodným řešením pro nové aplikace jsou přijatelné dlouho těchto omezení a aplikačních nevyžaduje nové funkce v EF jádra, které nejsou k dispozici pro EF6.</span><span class="sxs-lookup"><span data-stu-id="ad95e-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (i.e. it does not run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="ad95e-107">Zkontrolujte [porovnání funkcí](features.md) chcete zobrazit, pokud základní EF může být vhodné volbu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ad95e-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="ad95e-108">Pokyny pro existující aplikace EF6</span><span class="sxs-lookup"><span data-stu-id="ad95e-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="ad95e-109">Z důvodu zásadní změny v EF základní nedoporučujeme pokusu o přesunutí EF6 aplikace do základní EF, pokud nemáte přesvědčivý důvod k provedení změny.</span><span class="sxs-lookup"><span data-stu-id="ad95e-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="ad95e-110">Pokud chcete přesunout do základní EF nutné používat nové funkce, pak se ujistěte se, že jste si vědomi její omezení před zahájením.</span><span class="sxs-lookup"><span data-stu-id="ad95e-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="ad95e-111">Zkontrolujte [porovnání funkcí](features.md) chcete zobrazit, pokud základní EF může být vhodné volbu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ad95e-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="ad95e-112">**Přesunutí ze EF6 na jádro EF jako port místo upgradu by měl zobrazit.**</span><span class="sxs-lookup"><span data-stu-id="ad95e-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="ad95e-113">V tématu [Portování ze EF6 na jádro EF](porting/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ad95e-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
