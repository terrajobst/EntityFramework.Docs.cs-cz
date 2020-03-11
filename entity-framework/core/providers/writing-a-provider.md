---
title: Zápis poskytovatele databáze – EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 2d9e4a6cdfda80d7dfcfb6e7bf0480eb49f8e057
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417881"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="431b0-102">Vytvoření poskytovatele databáze</span><span class="sxs-lookup"><span data-stu-id="431b0-102">Writing a Database Provider</span></span>

<span data-ttu-id="431b0-103">Informace o tom, jak napsat poskytovatele databáze Entity Framework Core, najdete v tématu, chcete [-li vytvořit poskytovatele EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) pomocí [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="431b0-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="431b0-104">Tyto příspěvky nebyly od EF Core 1,1 aktualizovány a došlo k významným změnám od doby, kdy [problém 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) sleduje aktualizace této dokumentace.</span><span class="sxs-lookup"><span data-stu-id="431b0-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="431b0-105">Základ kódu EF Core je open source a obsahuje několik poskytovatelů databáze, které je možné použít jako referenci.</span><span class="sxs-lookup"><span data-stu-id="431b0-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="431b0-106">Zdrojový kód můžete najít na <https://github.com/aspnet/EntityFrameworkCore>.</span><span class="sxs-lookup"><span data-stu-id="431b0-106">You can find the source code at <https://github.com/aspnet/EntityFrameworkCore>.</span></span> <span data-ttu-id="431b0-107">Může být také užitečné si prohlédnout kód pro běžně používané poskytovatele třetích stran, například [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)a [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="431b0-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="431b0-108">Konkrétně jsou tyto projekty instalačním programem, aby rozšířily a spouštěly funkční testy, které publikujeme na NuGet.</span><span class="sxs-lookup"><span data-stu-id="431b0-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="431b0-109">Tento druh instalace se důrazně doporučuje.</span><span class="sxs-lookup"><span data-stu-id="431b0-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="431b0-110">Udržování aktuálnosti se změnami od poskytovatele</span><span class="sxs-lookup"><span data-stu-id="431b0-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="431b0-111">Od verze 2,1 jsme vytvořili [protokol změn](provider-log.md) , které by mohly potřebovat odpovídající změny v kódu poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="431b0-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="431b0-112">Slouží k tomu, aby vám pomohla při aktualizaci existujícího poskytovatele pro práci s novou verzí EF Core.</span><span class="sxs-lookup"><span data-stu-id="431b0-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="431b0-113">Před 2,1 jsme použili [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) a [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) popisky na naše problémy GitHubu a žádosti o přijetí změn pro podobný účel.</span><span class="sxs-lookup"><span data-stu-id="431b0-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="431b0-114">Continiueme, že tyto zapnutouy využijeme k tomu, abychom zjistili, které pracovní položky v dané vydané verzi můžou také vyžadovat, aby se práce prováděla v poskytovatelích.</span><span class="sxs-lookup"><span data-stu-id="431b0-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="431b0-115">`providers-beware` popisek obvykle znamená, že implementace pracovní položky může přerušit poskytovatele, zatímco `providers-fyi` popisek obvykle znamená, že zprostředkovatelé nebudou přerušeny, ale může být nutné změnit kód, například pro povolení nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="431b0-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="431b0-116">Navrhované pojmenování poskytovatelů třetích stran</span><span class="sxs-lookup"><span data-stu-id="431b0-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="431b0-117">Doporučujeme použít následující pojmenování balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="431b0-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="431b0-118">To je konzistentní s názvy balíčků, které doručí tým EF Core.</span><span class="sxs-lookup"><span data-stu-id="431b0-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="431b0-119">Příklad:</span><span class="sxs-lookup"><span data-stu-id="431b0-119">For example:</span></span>

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
