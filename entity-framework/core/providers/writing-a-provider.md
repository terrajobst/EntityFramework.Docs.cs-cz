---
title: Vytvoření poskytovatele databáze – EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993663"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="d6098-102">Vytvoření poskytovatele databáze</span><span class="sxs-lookup"><span data-stu-id="d6098-102">Writing a Database Provider</span></span>

<span data-ttu-id="d6098-103">Informace o vytváření poskytovatele databáze Entity Framework Core najdete v tématu [tak, že chcete napsat poskytovatele EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) podle [podle Arthur Vickerse](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="d6098-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="d6098-104">Tyto příspěvky nebyly aktualizovány od EF Core 1.1 a od té doby byly provedeny významné změny [problém 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) se ke sledování aktualizací do této dokumentace.</span><span class="sxs-lookup"><span data-stu-id="d6098-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="d6098-105">Základ kódu EF Core je open source a obsahuje několik zprostředkovatelů databáze, které lze použít jako referenci.</span><span class="sxs-lookup"><span data-stu-id="d6098-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="d6098-106">Můžete najít zdrojový kód v https://github.com/aspnet/EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="d6098-106">You can find the source code at https://github.com/aspnet/EntityFrameworkCore.</span></span> <span data-ttu-id="d6098-107">Může být také užitečné si prohlédněte si kód pro běžně používané poskytovatele třetí strany, jako například [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), a [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="d6098-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="d6098-108">Konkrétně se tyto projekty jsou nastavené rozšířit z a spuštění funkčních testů, které budeme publikovat na webu NuGet.</span><span class="sxs-lookup"><span data-stu-id="d6098-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="d6098-109">Důrazně se doporučuje tento typ instalačního programu.</span><span class="sxs-lookup"><span data-stu-id="d6098-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="d6098-110">Udržovat přehled o změnách poskytovatele</span><span class="sxs-lookup"><span data-stu-id="d6098-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="d6098-111">Počínaje práce po vydání 2.1, vytvořili jsme [protokolu změn](provider-log.md) odpovídající změny v kódu poskytovatele, který může být nutné.</span><span class="sxs-lookup"><span data-stu-id="d6098-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="d6098-112">To má pomoci při aktualizaci existujícího poskytovatele pro práci s novou verzi EF Core.</span><span class="sxs-lookup"><span data-stu-id="d6098-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="d6098-113">Před 2.1, které jsme použili [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) a [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) popisky na naše problémy s úložištěm GitHub a žádosti o přijetí změn pro k podobnému účelu.</span><span class="sxs-lookup"><span data-stu-id="d6098-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="d6098-114">Budeme continiue používat tyto se zapnutou problémů poskytnout jako ukazatel toho, které pracovní položky v dané verzi může také vyžadovat práci udělat u zprostředkovatelů.</span><span class="sxs-lookup"><span data-stu-id="d6098-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="d6098-115">A `providers-beware` popisek obvykle znamená, že provádění pracovní položky, mohou přestat fungovat poskytovatelů, zatímco `providers-fyi` popisek obvykle znamená, že poskytovatelů nebude přeruší se, že kód může potřebovat změnit i přesto, třeba, abyste umožnili nové funkce.</span><span class="sxs-lookup"><span data-stu-id="d6098-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="d6098-116">Navrhované názvy poskytovatelů třetích stran</span><span class="sxs-lookup"><span data-stu-id="d6098-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="d6098-117">Doporučujeme používat následující názvy balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="d6098-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="d6098-118">To je konzistentní s názvy balíčků od týmu EF Core.</span><span class="sxs-lookup"><span data-stu-id="d6098-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="d6098-119">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d6098-119">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
