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
# <a name="net-implementations-supported-by-ef-core"></a>Implementace rozhraní .NET podporované nástrojem EF Core

Chceme, aby bylo EF Core k dispozici pro vývojáře na všech moderních implementacích .NET a pořád pracujeme na tomto cíli. I když je podpora EF Core pro .NET Core pokrytá automatizovaným testováním a řada aplikací, které ji používala, je úspěšně používá, mono, Xamarin a UWP mají problémy.

## <a name="overview"></a>Přehled

Následující tabulka uvádí pokyny pro každou implementaci rozhraní .NET:

| EF Core                       | 2,1 a 3,1 |
|:------------------------------|:------------|
| .NET Standard                 | 2.0         |
| .NET Core                     | 2.0         |
| .NET Framework<sup>(1)</sup>  | 4.7.2       |
| Mono                          | 5.4         |
| Xamarin. iOS<sup>(2)</sup>     | 10,14       |
| Xamarin. Android<sup>(2)</sup> | 8.0         |
| UWP<sup>(3)</sup>             | 10.0.16299  |
| Unity<sup>(4)</sup>           | 2018,1      |

<sup>(1)</sup> viz část [.NET Framework](#net-framework) níže.

<sup>(2)</sup> k dispozici jsou problémy a známá omezení pro Xamarin, které mohou bránit aplikacím vyvinutým pomocí EF Core, aby fungovaly správně. Projděte si seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pro alternativní řešení.

<sup>(3)</sup> doporučuje se EF Core 2.0.1 a novější. Nainstalujte [balíček .NET Core UWP 6. x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/). Přečtěte si část [Univerzální platforma Windows](#universal-windows-platform) tohoto článku.

<sup>(4)</sup> existují problémy a známá omezení s Unity. Podívejte se na seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).

## <a name="net-framework"></a>.NET Framework

Aplikace, které cílí na .NET Framework, můžou potřebovat změny pro práci s .NET Standard knihovnami:

Upravte soubor projektu a ujistěte se, že se v počáteční skupině vlastností zobrazuje následující položka:

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

V případě testovacích projektů se ujistěte také, že je k dispozici následující položka:

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

Pokud chcete použít starší verzi sady Visual Studio, ujistěte se, že [upgradujete klienta NuGet na verzi 3.6.0](https://www.nuget.org/downloads) , aby fungoval s knihovnami .NET Standard 2,0.

Doporučujeme také migrovat z balíčků NuGet. config na PackageReference, pokud je to možné. Do souboru projektu přidejte následující vlastnost:

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a>Univerzální platforma Windows

Starší verze EF Core a prostředí .NET UWP obsahovaly nejrůznější problémy s kompatibilitou, zejména s aplikacemi, které jsou kompilovány s .NET Native sada nástrojů. Nová verze rozhraní .NET UWP přidává podporu pro .NET Standard 2,0 a obsahuje .NET Native 2,0, které řeší většinu problémů s kompatibilitou, které byly dříve hlášeny. EF Core 2.0.1 byl důkladně testován pomocí UWP, ale testování není automatizované.

Při použití EF Core na UWP:

* Chcete-li optimalizovat výkon dotazů, vyhněte se anonymním typům v dotazech LINQ. Nasazení aplikace UWP do obchodu s aplikacemi vyžaduje, aby byla aplikace kompilována s .NET Native. Dotazy s anonymními typy mají horší výkon u .NET Native.

* Pro optimalizaci výkonu `SaveChanges()` použijte [ChangeTrackingStrategy. ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) a implementujte [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)a [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) ve svých typech entit.

## <a name="report-issues"></a>Nahlásit problémy

Pro libovolnou kombinaci, která nefunguje podle očekávání, doporučujeme vytvořit nové problémy na [EF Core sledování problémů](https://github.com/aspnet/entityframeworkcore/issues/new). V případě problémů specifických pro Xamarin použijte modul pro sledování problémů pro [Xamarin. Android](https://github.com/xamarin/xamarin-android/issues/new) nebo [Xamarin. iOS](https://github.com/xamarin/xamarin-macios/issues/new).
