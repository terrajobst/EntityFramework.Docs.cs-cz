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
# <a name="net-implementations-supported-by-ef-core"></a>Implementace .NET podporované EF Core

Chceme EF Core být k dispozici vývojářům na všech moderních implementací .NET a stále pracujeme na dosažení tohoto cíle. Zatímco ef core podpora na .NET Core je pokryta automatizované testování a mnoho aplikací známo, že jej úspěšně používat, Mono, Xamarin a UPW mají některé problémy.

## <a name="overview"></a>Přehled

Následující tabulka obsahuje pokyny pro každou implementaci rozhraní .NET:

| EF Core                       | 2.1 a 3.1 |
|:------------------------------|:------------|
| .NET Standard                 | 2.0         |
| .NET Core                     | 2.0         |
| Rozhraní .NET<sup>Framework (1)</sup>  | 4.7.2       |
| Mono                          | 5.4         |
| Xamarin.iOS<sup>(2)</sup>     | 10.14       |
| Xamarin.Android<sup>(2)</sup> | 8.0         |
| Upw<sup>(3)</sup>             | 10.0.16299  |
| Jednota<sup>(4)</sup>           | 2018.1      |

<sup>(1)</sup> Viz část [.NET Framework](#net-framework) níže.

<sup>(2)</sup> Existují problémy a známá omezení s Xamarin, které mohou zabránit některé aplikace vyvinuté pomocí EF Core pracovat správně. Zkontrolujte seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) pro řešení.

<sup>(3)</sup> EF Core 2.0.1 a novější doporučeno. Nainstalujte [balíček .NET Core UPW 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/). V tomto článku najdete v části [Univerzální platforma Windows.](#universal-windows-platform)

<sup>(4)</sup> Existují problémy a známá omezení s Unity. Podívejte se na seznam [aktivních problémů](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).

## <a name="net-framework"></a>.NET Framework

Aplikace, které cílí na rozhraní .NET Framework, mohou vyžadovat změny pro práci s knihovnami .NET Standard:

Upravte soubor projektu a ujistěte se, že se v počáteční skupině vlastností zobrazí následující položka:

``` xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

U testovacích projektů se také ujistěte, že je k dispozici následující položka:

``` xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

Pokud chcete použít starší verzi sady Visual Studio, ujistěte se, že [upgrade klienta NuGet na verzi 3.6.0](https://www.nuget.org/downloads) pro práci s knihovnami .NET Standard 2.0.

Doporučujeme také migraci z NuGet packages.config na PackageReference, pokud je to možné. Do souboru projektu přidejte následující vlastnost:

``` xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a>Univerzální platforma Windows

Dřívější verze EF Core a .NET UWP měly mnoho problémů s kompatibilitou, zejména s aplikacemi zkompilovanými pomocí řetězce nástrojů .NET Native. Nová verze .NET UPW přidává podporu pro rozhraní .NET Standard 2.0 a obsahuje rozhraní .NET Native 2.0, které opravuje většinu dříve oznámených problémů s kompatibilitou. EF Core 2.0.1 byl testován důkladněji s UPW, ale testování není automatizováno.

Při použití EF Core na UPW:

* Chcete-li optimalizovat výkon dotazů, vyhněte se anonymní typy v dotazech LINQ. Nasazení aplikace UPW do obchodu s aplikacemi vyžaduje kompilaci aplikace pomocí nativní ho .NET. Dotazy s anonymnítypy mají horší výkon na .NET Native.

* Chcete-li optimalizovat `SaveChanges()` výkon, použijte [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) a implementujte [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx)a [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) v typech entit.

## <a name="report-issues"></a>Nahlášení potíží

Pro každou kombinaci, která nefunguje podle očekávání, doporučujeme vytvářet nové problémy na [ef core problém tracker](https://github.com/aspnet/entityframeworkcore/issues/new). Pro problémy specifické pro Xamarin použijte nástroj pro sledování problémů pro [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) nebo [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
