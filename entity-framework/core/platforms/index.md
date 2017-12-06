---
title: "Podporované implementace rozhraní .NET – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementace rozhraní .NET podporuje EF jádra

Chceme EF základní být k dispozici kdekoli v můžete napsat kód .NET a stále pracujeme vůči této cíli. Následující tabulka obsahuje pokyny pro každý implementace rozhraní .NET, kde chceme povolit EF jádra.

EF základní 2.0 cíle [standardní rozhraní .NET 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) a vyžaduje tudíž implementace rozhraní .NET, které ji podporují.

| Implementace rozhraní .NET | Stav | vyžaduje 1.x | vyžaduje 2.x
|-|-|-|-
| **.NET core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [konzoly](../get-started/netcore/index.md)atd.) | **Plně podporované a doporučené:** Covered automatizované testování a mnoho aplikací ví, že se pomocí úspěšně. | [.NET core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET core SDK 2.x](https://www.microsoft.com/net/core/)
| **Rozhraní .NET framework** (WinForms, WPF, ASP.NET, [konzoly](../get-started/full-dotnet/index.md)atd.) | **Plně podporované a doporučené:** Covered automatizované testování a mnoho aplikací ví, že se pomocí úspěšně. EF 6 také k dispozici v této platformě (najdete v části [porovnat základní EF & EF6](../../efcore-and-ef6/index.md) zvolit správnou technologii). | .NET Framework 4.5.1 | Rozhraní .NET framework 4.6.1
| **Mono & Xamarin** | **V průběhu – mohou být zjištěny problémy:** některé testování provést podle týmu EF jádra a zákazníků. Inovátoři nahlásilo některé úspěšné, ale [byly zjištěny problémy](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) a ostatní budou pravděpodobně neodkrytých jako testování pokračuje. Konkrétně existují omezení v Xamarin.iOS, která může zabránit některé aplikace vyvinuté pomocí EF základní 2.0 fungovala správně. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**Univerzální platforma pro Windows**](../get-started/uwp/index.md) | **V průběhu – mohou být zjištěny problémy:** některé testování provést podle týmu EF jádra a zákazníků. [Problémy](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) mít byla nahlášena, když kompilujete s nativní sada nástrojů .NET, která se obvykle používá při sestavení pro vydání a je vyžadováno pro nasazení na web Windows Store (pokud nejsou pomocí .NET Native nebo jenom chcete experimentovat, mnohé z problémů nebude mít vliv na můžete). | [Nejnovější balíček .NET UWP 5](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [Nejnovější balíček .NET UWP 6](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1) </sup> Tuto verzi rozhraní .NET UWP přidává podporu pro standardní rozhraní .NET 2.0 a obsahuje nativní 2.0 rozhraní .NET, která řeší většinu kompatibility dříve hlášené problémy, ale testování odhalila [několik zbývající problémy](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) s EF jádra 2.0, která jsme plánování adres v o nadcházejících oprava verzi.

Pro libovolnou kombinaci, která nebude fungovat podle očekávání, doporučujeme vytvoření nové problémy v [sledovací modul EF základní problém](https://github.com/aspnet/entityframeworkcore/issues/new), a pro Xamarin související problémy, [sledovací modul problém Xamarin](https://bugzilla.xamarin.com/newbug).
