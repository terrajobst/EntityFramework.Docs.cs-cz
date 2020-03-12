---
title: Podporované implementace .NET – EF Core
author: bricelam
ms.date: 03/03/2020
uid: core/platforms/index
ms.openlocfilehash: 693d4cae85eddf86d01e17084415147c52a008c7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417322"
---
# <a name="net-implementations-supported-by-ef-core"></a><span data-ttu-id="ed85a-102">Implementace rozhraní .NET podporované nástrojem EF Core</span><span class="sxs-lookup"><span data-stu-id="ed85a-102">.NET implementations supported by EF Core</span></span>

<span data-ttu-id="ed85a-103">Chceme, aby bylo EF Core k dispozici pro vývojáře na všech moderních implementacích .NET a pořád pracujeme na tomto cíli.</span><span class="sxs-lookup"><span data-stu-id="ed85a-103">We want EF Core to be available to developers on all modern .NET implementations, and we're still working towards that goal.</span></span> <span data-ttu-id="ed85a-104">I když je podpora EF Core pro .NET Core pokrytá automatizovaným testováním a řada aplikací, které ji používala, je úspěšně používá, mono, Xamarin a UWP mají problémy.</span><span class="sxs-lookup"><span data-stu-id="ed85a-104">While EF Core's support on .NET Core is covered by automated testing and many applications known to be using it successfully, Mono, Xamarin and UWP have some issues.</span></span>

## <a name="overview"></a><span data-ttu-id="ed85a-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="ed85a-105">Overview</span></span>

<span data-ttu-id="ed85a-106">Následující tabulka uvádí pokyny pro každou implementaci rozhraní .NET:</span><span class="sxs-lookup"><span data-stu-id="ed85a-106">The following table provides guidance for each .NET implementation:</span></span>

| <span data-ttu-id="ed85a-107">EF Core</span><span class="sxs-lookup"><span data-stu-id="ed85a-107">EF Core</span></span>                       | <span data-ttu-id="ed85a-108">2,1 a 3,1</span><span class="sxs-lookup"><span data-stu-id="ed85a-108">2.1 and 3.1</span></span> |
|:------------------------------|:------------|
| <span data-ttu-id="ed85a-109">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="ed85a-109">.NET Standard</span></span>                 | <span data-ttu-id="ed85a-110">2.0</span><span class="sxs-lookup"><span data-stu-id="ed85a-110">2.0</span></span>         |
| <span data-ttu-id="ed85a-111">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed85a-111">.NET Core</span></span>                     | <span data-ttu-id="ed85a-112">2.0</span><span class="sxs-lookup"><span data-stu-id="ed85a-112">2.0</span></span>         |
| <span data-ttu-id="ed85a-113">.NET Framework<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="ed85a-113">.NET Framework<sup>(1)</sup></span></span>  | <span data-ttu-id="ed85a-114">4.7.2</span><span class="sxs-lookup"><span data-stu-id="ed85a-114">4.7.2</span></span>       |
| <span data-ttu-id="ed85a-115">Mono</span><span class="sxs-lookup"><span data-stu-id="ed85a-115">Mono</span></span>                          | <span data-ttu-id="ed85a-116">5.4</span><span class="sxs-lookup"><span data-stu-id="ed85a-116">5.4</span></span>         |
| <span data-ttu-id="ed85a-117">Xamarin. iOS<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="ed85a-117">Xamarin.iOS<sup>(2)</sup></span></span>     | <span data-ttu-id="ed85a-118">10,14</span><span class="sxs-lookup"><span data-stu-id="ed85a-118">10.14</span></span>       |
| <span data-ttu-id="ed85a-119">Xamarin. Android<sup>(2)</sup></span><span class="sxs-lookup"><span data-stu-id="ed85a-119">Xamarin.Android<sup>(2)</sup></span></span> | <span data-ttu-id="ed85a-120">8.0</span><span class="sxs-lookup"><span data-stu-id="ed85a-120">8.0</span></span>         |
| <span data-ttu-id="ed85a-121">UWP<sup>(3)</sup></span><span class="sxs-lookup"><span data-stu-id="ed85a-121">UWP<sup>(3)</sup></span></span>             | <span data-ttu-id="ed85a-122">10.0.16299</span><span class="sxs-lookup"><span data-stu-id="ed85a-122">10.0.16299</span></span>  |
| <span data-ttu-id="ed85a-123">Unity<sup>(4)</sup></span><span class="sxs-lookup"><span data-stu-id="ed85a-123">Unity<sup>(4)</sup></span></span>           | <span data-ttu-id="ed85a-124">2018,1</span><span class="sxs-lookup"><span data-stu-id="ed85a-124">2018.1</span></span>      |

<span data-ttu-id="ed85a-125"><sup>(1)</sup> viz část [.NET Framework](#net-framework) níže.</span><span class="sxs-lookup"><span data-stu-id="ed85a-125"><sup>(1)</sup> See the [.NET Framework](#net-framework) section below.</span></span>

<span data-ttu-id="ed85a-126"><sup>(2)</sup> k dispozici jsou problémy a známá omezení pro Xamarin, které mohou bránit aplikacím vyvinutým pomocí EF Core, aby fungovaly správně.</span><span class="sxs-lookup"><span data-stu-id="ed85a-126"><sup>(2)</sup> There are issues and known limitations with Xamarin which may prevent some applications developed using EF Core from working correctly.</span></span> <span data-ttu-id="ed85a-127">Projděte si seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pro alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="ed85a-127">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) for workarounds.</span></span>

<span data-ttu-id="ed85a-128"><sup>(3)</sup> doporučuje se EF Core 2.0.1 a novější.</span><span class="sxs-lookup"><span data-stu-id="ed85a-128"><sup>(3)</sup> EF Core 2.0.1 and newer recommended.</span></span> <span data-ttu-id="ed85a-129">Nainstalujte [balíček .NET Core UWP 6. x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span><span class="sxs-lookup"><span data-stu-id="ed85a-129">Install the [.NET Core UWP 6.x package](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/).</span></span> <span data-ttu-id="ed85a-130">Přečtěte si část [Univerzální platforma Windows](#universal-windows-platform) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="ed85a-130">See the [Universal Windows Platform](#universal-windows-platform) section of this article.</span></span>

<span data-ttu-id="ed85a-131"><sup>(4)</sup> existují problémy a známá omezení s Unity.</span><span class="sxs-lookup"><span data-stu-id="ed85a-131"><sup>(4)</sup> There are issues and known limitations with Unity.</span></span> <span data-ttu-id="ed85a-132">Podívejte se na seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span><span class="sxs-lookup"><span data-stu-id="ed85a-132">Check the list of [active issues](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).</span></span>

## <a name="net-framework"></a><span data-ttu-id="ed85a-133">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="ed85a-133">.NET Framework</span></span>

<span data-ttu-id="ed85a-134">Aplikace, které cílí na .NET Framework, můžou potřebovat změny pro práci s .NET Standard knihovnami:</span><span class="sxs-lookup"><span data-stu-id="ed85a-134">Applications that target .NET Framework may need changes to work with .NET Standard libraries:</span></span>

<span data-ttu-id="ed85a-135">Upravte soubor projektu a ujistěte se, že se v počáteční skupině vlastností zobrazuje následující položka:</span><span class="sxs-lookup"><span data-stu-id="ed85a-135">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

<span data-ttu-id="ed85a-136">V případě testovacích projektů se ujistěte také, že je k dispozici následující položka:</span><span class="sxs-lookup"><span data-stu-id="ed85a-136">For test projects, also make sure the following entry is present:</span></span>

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

<span data-ttu-id="ed85a-137">Pokud chcete použít starší verzi sady Visual Studio, ujistěte se, že [upgradujete klienta NuGet na verzi 3.6.0](https://www.nuget.org/downloads) , aby fungoval s knihovnami .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="ed85a-137">If you want to use an older version of Visual Studio, make sure you [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads) to work with .NET Standard 2.0 libraries.</span></span>

<span data-ttu-id="ed85a-138">Doporučujeme také migrovat z balíčků NuGet. config na PackageReference, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="ed85a-138">We also recommend migrating from NuGet packages.config to PackageReference if possible.</span></span> <span data-ttu-id="ed85a-139">Do souboru projektu přidejte následující vlastnost:</span><span class="sxs-lookup"><span data-stu-id="ed85a-139">Add the following property to your project file:</span></span>

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a><span data-ttu-id="ed85a-140">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="ed85a-140">Universal Windows Platform</span></span>

<span data-ttu-id="ed85a-141">Starší verze EF Core a prostředí .NET UWP obsahovaly nejrůznější problémy s kompatibilitou, zejména s aplikacemi, které jsou kompilovány s .NET Native sada nástrojů.</span><span class="sxs-lookup"><span data-stu-id="ed85a-141">Earlier versions of EF Core and .NET UWP had numerous compatibility issues, especially with applications compiled with the .NET Native toolchain.</span></span> <span data-ttu-id="ed85a-142">Nová verze rozhraní .NET UWP přidává podporu pro .NET Standard 2,0 a obsahuje .NET Native 2,0, které řeší většinu problémů s kompatibilitou, které byly dříve hlášeny.</span><span class="sxs-lookup"><span data-stu-id="ed85a-142">The new .NET UWP version adds support for .NET Standard 2.0 and contains .NET Native 2.0, which fixes most of the compatibility issues previously reported.</span></span> <span data-ttu-id="ed85a-143">EF Core 2.0.1 byl důkladně testován pomocí UWP, ale testování není automatizované.</span><span class="sxs-lookup"><span data-stu-id="ed85a-143">EF Core 2.0.1 has been tested more thoroughly with UWP but testing is not automated.</span></span>

<span data-ttu-id="ed85a-144">Při použití EF Core na UWP:</span><span class="sxs-lookup"><span data-stu-id="ed85a-144">When using EF Core on UWP:</span></span>

* <span data-ttu-id="ed85a-145">Chcete-li optimalizovat výkon dotazů, vyhněte se anonymním typům v dotazech LINQ.</span><span class="sxs-lookup"><span data-stu-id="ed85a-145">To optimize query performance, avoid anonymous types in LINQ queries.</span></span> <span data-ttu-id="ed85a-146">Nasazení aplikace UWP do obchodu s aplikacemi vyžaduje, aby byla aplikace kompilována s .NET Native.</span><span class="sxs-lookup"><span data-stu-id="ed85a-146">Deploying a UWP application to the app store requires an application to be compiled with .NET Native.</span></span> <span data-ttu-id="ed85a-147">Dotazy s anonymními typy mají horší výkon u .NET Native.</span><span class="sxs-lookup"><span data-stu-id="ed85a-147">Queries with anonymous types have worse performance on .NET Native.</span></span>

* <span data-ttu-id="ed85a-148">Pro optimalizaci výkonu `SaveChanges()` použijte [ChangeTrackingStrategy. ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) a implementujte [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)a [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) ve svých typech entit.</span><span class="sxs-lookup"><span data-stu-id="ed85a-148">To optimize `SaveChanges()` performance, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) and implement [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx), and [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types.</span></span>

## <a name="report-issues"></a><span data-ttu-id="ed85a-149">Nahlásit problémy</span><span class="sxs-lookup"><span data-stu-id="ed85a-149">Report issues</span></span>

<span data-ttu-id="ed85a-150">Pro libovolnou kombinaci, která nefunguje podle očekávání, doporučujeme vytvořit nové problémy na [EF Core sledování problémů](https://github.com/aspnet/entityframeworkcore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="ed85a-150">For any combination that doesn�t work as expected, we encourage creating new issues on the [EF Core issue tracker](https://github.com/aspnet/entityframeworkcore/issues/new).</span></span> <span data-ttu-id="ed85a-151">V případě problémů specifických pro Xamarin použijte modul pro sledování problémů pro [Xamarin. Android](https://github.com/xamarin/xamarin-android/issues/new) nebo [Xamarin. iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span><span class="sxs-lookup"><span data-stu-id="ed85a-151">For Xamarin-specific issues use the issue tracker for [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) or [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).</span></span>
