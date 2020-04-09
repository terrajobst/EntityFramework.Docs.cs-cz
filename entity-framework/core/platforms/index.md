---
title: Podporované implementace rozhraní .NET – ef core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417322"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="974e3-102">Implementace .NET podporované EF Core</span><span class="sxs-lookup"><span data-stu-id="974e3-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="974e3-103">Chceme EF Core být k dispozici vývojářům na všech moderních implementací .NET a stále pracujeme na dosažení tohoto cíle.</span><span class="sxs-lookup"><span data-stu-id="974e3-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="974e3-104">Zatímco ef core podpora na .NET Core je pokryta automatizované testování a mnoho aplikací známo, že jej úspěšně používat, Mono, Xamarin a UPW mají některé problémy.</span><span class="sxs-lookup"><span data-stu-id="974e3-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="974e3-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="974e3-105">Overview</span></span>

<span data-ttu-id="974e3-106">Následující tabulka obsahuje pokyny pro každou implementaci rozhraní .NET:</span><span class="sxs-lookup"><span data-stu-id="974e3-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="974e3-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="974e3-107">EF Core</span></span>                       | <span data-ttu-id="974e3-108">2.1 a 3.1</span><span class="sxs-lookup"><span data-stu-id="974e3-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="974e3-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="974e3-109">.NET Standard</span></span>                 | <span data-ttu-id="974e3-110">2.0</span><span class="sxs-lookup"><span data-stu-id="974e3-110">2.0</span></span>         |
| <span data-ttu-id="974e3-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="974e3-111">.NET Core</span></span>                     | <span data-ttu-id="974e3-112">2.0</span><span class="sxs-lookup"><span data-stu-id="974e3-112">2.0</span></span>         |
| <span data-ttu-id="974e3-113">Rozhraní .NET<sup>Framework (1)</sup></span><span class="sxs-lookup"><span data-stu-id="974e3-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="974e3-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="974e3-114">4.7.2</span></span>       |
| <span data-ttu-id="974e3-115">Mono</span><span class="sxs-lookup"><span data-stu-id="974e3-115">Mono</span></span>                          | <span data-ttu-id="974e3-116">5.4</span><span class="sxs-lookup"><span data-stu-id="974e3-116">5.4</span></span>         |
| <span data-ttu-id="974e3-117">Xamarin.iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="974e3-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="974e3-118">10.14</span><span class="sxs-lookup"><span data-stu-id="974e3-118">10.14</span></span>       |
| <span data-ttu-id="974e3-119">Xamarin.Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="974e3-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="974e3-120">8.0</span><span class="sxs-lookup"><span data-stu-id="974e3-120">8.0</span></span>         |
| <span data-ttu-id="974e3-121">Upw<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="974e3-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="974e3-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="974e3-122">10.0.16299</span></span>  |
| <span data-ttu-id="974e3-123">Jednota<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="974e3-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="974e3-124">2018.1</span><span class="sxs-lookup"><span data-stu-id="974e3-124">2018.1</span></span>      |

<span data-ttu-id="974e3-125"><sup>(1)</sup> Viz část [.NET Framework](#net-framework) níže.</span><span class="sxs-lookup"><span data-stu-id="974e3-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="974e3-126"><sup>(2)</sup> Existují problémy a známá omezení s Xamarin, které mohou zabránit některé aplikace vyvinuté pomocí EF Core pracovat správně.</span><span class="sxs-lookup"><span data-stu-id="974e3-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="974e3-127">Zkontrolujte seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pro řešení.</span><span class="sxs-lookup"><span data-stu-id="974e3-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="974e3-128"><sup>(3)</sup> EF Core 2.0.1 a novější doporučeno.</span><span class="sxs-lookup"><span data-stu-id="974e3-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="974e3-129">Nainstalujte [balíček .NET Core UPW 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span><span class="sxs-lookup"><span data-stu-id="974e3-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="974e3-130">V tomto článku najdete v části [Univerzální platforma Windows.](#universal-windows-platform)</span><span class="sxs-lookup"><span data-stu-id="974e3-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="974e3-131"><sup>(4)</sup> Existují problémy a známá omezení s Unity.</span><span class="sxs-lookup"><span data-stu-id="974e3-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="974e3-132">Podívejte se na seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span><span class="sxs-lookup"><span data-stu-id="974e3-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="974e3-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="974e3-133">.NET Framework</span></span>

<span data-ttu-id="974e3-134">Aplikace, které cílí na rozhraní .NET Framework, mohou vyžadovat změny pro práci s knihovnami .NET Standard:</span><span class="sxs-lookup"><span data-stu-id="974e3-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="974e3-135">Upravte soubor projektu a ujistěte se, že se v počáteční skupině vlastností zobrazí následující položka:</span><span class="sxs-lookup"><span data-stu-id="974e3-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="974e3-136">U testovacích projektů se také ujistěte, že je k dispozici následující položka:</span><span class="sxs-lookup"><span data-stu-id="974e3-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="974e3-137">Pokud chcete použít starší verzi sady Visual Studio, ujistěte se, že [upgrade klienta NuGet na verzi 3.6.0](https://www.nuget.org/downloads) pro práci s knihovnami .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="974e3-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="974e3-138">Doporučujeme také migraci z NuGet packages.config na PackageReference, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="974e3-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="974e3-139">Do souboru projektu přidejte následující vlastnost:</span><span class="sxs-lookup"><span data-stu-id="974e3-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="974e3-140">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="974e3-140">Universal Windows Platform</span></span>

<span data-ttu-id="974e3-141">Dřívější verze EF Core a .NET UWP měly mnoho problémů s kompatibilitou, zejména s aplikacemi zkompilovanými pomocí řetězce nástrojů .NET Native.</span><span class="sxs-lookup"><span data-stu-id="974e3-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="974e3-142">Nová verze .NET UPW přidává podporu pro rozhraní .NET Standard 2.0 a obsahuje rozhraní .NET Native 2.0, které opravuje většinu dříve oznámených problémů s kompatibilitou.</span><span class="sxs-lookup"><span data-stu-id="974e3-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="974e3-143">EF Core 2.0.1 byl testován důkladněji s UPW, ale testování není automatizováno.</span><span class="sxs-lookup"><span data-stu-id="974e3-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="974e3-144">Při použití EF Core na UPW:</span><span class="sxs-lookup"><span data-stu-id="974e3-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="974e3-145">Chcete-li optimalizovat výkon dotazů, vyhněte se anonymní typy v dotazech LINQ.</span><span class="sxs-lookup"><span data-stu-id="974e3-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="974e3-146">Nasazení aplikace UPW do obchodu s aplikacemi vyžaduje kompilaci aplikace pomocí nativní ho .NET.</span><span class="sxs-lookup"><span data-stu-id="974e3-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="974e3-147">Dotazy s anonymnítypy mají horší výkon na .NET Native.</span><span class="sxs-lookup"><span data-stu-id="974e3-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="974e3-148">Chcete-li optimalizovat `SaveChanges()` výkon, použijte [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) a implementujte [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)a [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) v typech entit.</span><span class="sxs-lookup"><span data-stu-id="974e3-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="974e3-149">Nahlášení potíží</span><span class="sxs-lookup"><span data-stu-id="974e3-149">Report issues</span></span>

<span data-ttu-id="974e3-150">Pro každou kombinaci, která nefunguje podle očekávání, doporučujeme vytvářet nové problémy na [ef core problém tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="974e3-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="974e3-151">Pro problémy specifické pro Xamarin použijte nástroj pro sledování problémů pro [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) nebo [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span><span class="sxs-lookup"><span data-stu-id="974e3-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
