---
title: "Základní EF a EF6 – což jeden je pro vás nejvhodnější"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="a0672-102">Základní EF a EF6: který z nich je pro vás nejvhodnější</span><span class="sxs-lookup"><span data-stu-id="a0672-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="a0672-103">Následující informace vám pomůže vybrat mezi Entity Framework Core a Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a0672-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="a0672-104">Pokyny pro nové aplikace</span><span class="sxs-lookup"><span data-stu-id="a0672-104">Guidance for new applications</span></span>

<span data-ttu-id="a0672-105">Protože základní EF je novém produktu a stále chybí některé důležité funkce O/RM, zůstanou EF6 nejvhodnější volbou pro mnoho aplikací.</span><span class="sxs-lookup"><span data-stu-id="a0672-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="a0672-106">**Toto jsou typy aplikací, které by doporučujeme používat jádro EF pro:**</span><span class="sxs-lookup"><span data-stu-id="a0672-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="a0672-107">Nové aplikace, které nevyžadují funkce, které zatím nejsou implementované v EF jádra.</span><span class="sxs-lookup"><span data-stu-id="a0672-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="a0672-108">Zkontrolujte [porovnání funkcí](features.md) chcete zobrazit, pokud základní EF může být vhodné volbu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0672-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="a0672-109">Aplikace, které cílí na .NET Core, jako jsou například aplikace pro univerzální platformu Windows (UWP) a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a0672-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="a0672-110">Tyto aplikace nemůžou používat EF6, jak to vyžaduje rozhraní .NET Framework (tj. rozhraní .NET Framework 4.5).</span><span class="sxs-lookup"><span data-stu-id="a0672-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="a0672-111">Pokyny pro existující aplikace EF6</span><span class="sxs-lookup"><span data-stu-id="a0672-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="a0672-112">Z důvodu zásadní změny v EF základní nedoporučujeme pokusu o přesunutí EF6 aplikace do základní EF, pokud nemáte přesvědčivý důvod k provedení změny.</span><span class="sxs-lookup"><span data-stu-id="a0672-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="a0672-113">Pokud chcete přesunout do základní EF nutné používat nové funkce, pak se ujistěte se, že jste si vědomi její omezení před zahájením.</span><span class="sxs-lookup"><span data-stu-id="a0672-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="a0672-114">Zkontrolujte [porovnání funkcí](features.md) chcete zobrazit, pokud základní EF může být vhodné volbu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0672-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="a0672-115">**Přesunutí ze EF6 na jádro EF jako port místo upgradu by měl zobrazit.**</span><span class="sxs-lookup"><span data-stu-id="a0672-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="a0672-116">V tématu [Portování ze EF6 na jádro EF](porting/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a0672-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
