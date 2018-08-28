---
title: EF Core a EF6 – které jedna je pro vás nejvhodnější
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 83ae754f899b624d322c48e8de8432b65519546e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993830"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="2dc87-102">EF Core a EF6: který z nich je pro vás nejvhodnější</span><span class="sxs-lookup"><span data-stu-id="2dc87-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="2dc87-103">Následující informace vám pomohou při výběru mezi Entity Framework Core a Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2dc87-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="2dc87-104">Doporučení pro nové aplikace</span><span class="sxs-lookup"><span data-stu-id="2dc87-104">Guidance for new applications</span></span>

<span data-ttu-id="2dc87-105">EF Core můžete použít pro vývoj nových aplikací, pokud chcete využít všech možností, které EF Core poskytuje, a vaše aplikace nevyžaduje funkce, které ještě nejsou v EF Core implementované.</span><span class="sxs-lookup"><span data-stu-id="2dc87-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="2dc87-106">EF6 vyžaduje rozhraní .NET Framework 4.0 (nebo novější verzi) a je podporován pouze na Windows (to znamená, EF6 momentálně neběží v .NET Core a není podporována v jiných operačních systémech), ale stále je přijatelné volbou pro nové aplikace jsou dlouho těchto omezení přijatelné a aplikace nevyžaduje, aby nové funkce v EF Core, které nejsou k dispozici pro EF6.</span><span class="sxs-lookup"><span data-stu-id="2dc87-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="2dc87-107">Přečtěte si [článek s porovnáním funkcí](features.md), kde zjistíte, jestli je EF Core vhodná volba pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2dc87-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="2dc87-108">Doporučení pro existující aplikace s EF6</span><span class="sxs-lookup"><span data-stu-id="2dc87-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="2dc87-109">Z důvodu zásadních změn v EF Core nedoporučujeme migraci z EF6 na EF Core, pokud pro takovou změnu nemáte dobrý důvod.</span><span class="sxs-lookup"><span data-stu-id="2dc87-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="2dc87-110">Pokud chcete migrovat stávající řešení na EF Core například kvůli novým funkcím, nejdříve se ujistěte, že jste si vědomi případných omezení EF Core.</span><span class="sxs-lookup"><span data-stu-id="2dc87-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="2dc87-111">Přečtěte si [článek s porovnáním funkcí](features.md), kde zjistíte, jestli je EF Core vhodná volba pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2dc87-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="2dc87-112">**Přechod z EF6 na EF Core je potřeba chápat spíše jako migraci než jako upgrade.**</span><span class="sxs-lookup"><span data-stu-id="2dc87-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="2dc87-113">Další informace najdete v [článku o přechodu z EF6 na EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="2dc87-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
