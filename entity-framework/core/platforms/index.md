---
title: Podporovaná implementace .NET – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 790628c407cc4374fee4ebde8201783955afdcc3
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900327"
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementace .NET podporuje EF Core

Chceme, aby EF Core dalo kdekoli můžete napsat kód .NET a pořád pracujeme směrem k danému cíli. Zatímco podpora EF Core na .NET Core a .NET Framework se bude vztahovat automatizované testování a spoustu aplikací na známé úspěšně ho používat, Mono, Xamarin a UPW mají některé problémy.

Následující tabulka uvádí pokyny k každá implementace .NET:

| Implementace .NET                                                                                                  | Stav                                                             | Požadavky na EF Core 1.x                                                                                | Požadavky na EF Core 2.x <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [konzoly](../get-started/netcore/index.md)atd.) | Plně podporované a doporučené                                    | [Sady SDK .NET core 1.x](https://www.microsoft.com/net/core/)                                                | [.NET core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **Rozhraní .NET framework** (WinForms, WPF, technologii ASP.NET, [konzoly](../get-started/full-dotnet/index.md)atd.)                    | Plně podporované a doporučené. EF6 také k dispozici <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono & Xamarin**                                                                                                   | V průběhu <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | 5.4 mono <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Universal Windows Platform**](../get-started/uwp/index.md)                                                        | EF Core 2.0.1 doporučuje <sup>(4)</sup>                           | [Balíček .NET core UWP 5.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [.NET core UWP 6.x balíčku](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1) </sup> EF Core 2.0, zaměřuje a proto vyžaduje, aby implementace rozhraní .NET, které podporují [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard).

<sup>(2) </sup> Naleznete v tématu [porovnání EF Core a EF6](../../efcore-and-ef6/index.md) vybrat správnou technologickou.

<sup>(3) </sup> Existují problémy a známé omezení s využitím kódu Xamarin, které mohou bránit některé aplikace vyvinuté pomocí EF Core 2.0 správně fungovat. Zkontrolujte seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pro jejich řešení.

<sup>(4) </sup> Starší verze EF Core a .NET UWP má mnoho problémů s kompatibilitou, zejména s aplikace kompilované pomocí nástrojů .NET Native. Nová verze .NET UWP přidává podporu pro .NET Standard 2.0 a obsahuje nativní 2.0 rozhraní .NET, která řeší většinu dříve hlášené problémy s kompatibilitou. EF Core 2.0.1 má neprošla dostatečně důkladnými více s UWP, ale není automatizované testování.

Pro libovolnou kombinaci, která nebude fungovat podle očekávání, doporučujeme vytvořit nové problémy u [sledování problémů EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). Pro Xamarin konkrétní problémy, použijte pro sledování problémů [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) nebo [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
