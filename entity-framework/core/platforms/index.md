---
title: "Podporované implementace rozhraní .NET – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 02e9450cb0ead1701da9f58c51bef3031a3be4ed
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementace rozhraní .NET podporuje EF jádra

Chceme EF základní být k dispozici kdekoli v můžete napsat kód .NET a stále pracujeme vůči této cíli. Během EF základní podpora na .NET Core a rozhraní .NET Framework je předmětem automatizované testování a mnoho aplikací ví, že se pomocí úspěšně, Mono, Xamarin a UWP mají některé problémy.

Následující tabulka obsahuje pokyny pro každou implementaci rozhraní .NET:

| Implementace rozhraní .NET                                                                                                  | Stav                                                             | EF základní 1.x požadavky                                                                                | EF základní 2.x požadavky <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [konzoly](../get-started/netcore/index.md)atd.) | Plně podporované a doporučené                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **Rozhraní .NET framework** (WinForms, WPF, ASP.NET, [konzoly](../get-started/full-dotnet/index.md)atd.)                    | Plně podporované a doporučené. EF6 také k dispozici <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono & Xamarin**                                                                                                   | V průběhu <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Universal Windows Platform**](../get-started/uwp/index.md)                                                        | Základní EF 2.0.1 doporučená <sup>(4)</sup>                           | [.NET core UWP 5.x balíčku](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [.NET core UWP 6.x balíčku](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1) </sup> EF základní 2.0 cílí a vyžaduje tudíž implementace rozhraní .NET, které podporují [standardní rozhraní .NET 2.0](https://docs.microsoft.com/dotnet/standard/net-standard).

<sup>(2) </sup> Najdete v části [porovnat základní EF & EF6](../../efcore-and-ef6/index.md) zvolit správnou technologii.

<sup>(3) </sup> Existují problémy a známá omezení s Xamarinem, která může zabránit některé aplikace vyvinuté pomocí EF základní 2.0 fungovala správně. Zkontrolujte seznam [aktivní problémy] ([ ](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pro alternativní řešení.

<sup>(4) </sup> Dřívějších verzích EF jádra a .NET UWP měl množství problémy s kompatibilitou, zejména s aplikacemi, kompilovat s .NET Native nástrojů. Nová verze rozhraní .NET UWP přidává podporu pro standardní rozhraní .NET 2.0 a obsahuje nativní 2.0 rozhraní .NET, která řeší většinu dříve hlášené problémy s kompatibilitou. Základní EF 2.0.1 byl testovaný více důkladně s UPW, ale není automatizované testování.

Pro libovolnou kombinaci, která nebude fungovat podle očekávání, doporučujeme vytvořit nové problémy v [sledovací modul EF základní problém](https://github.com/aspnet/entityframeworkcore/issues/new). Pro Xamarin specifické problémy pomocí nástroje Sledování problém pro [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) nebo [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
