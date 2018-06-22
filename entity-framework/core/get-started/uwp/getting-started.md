---
title: Začínáme na základní EF UWP - novou databázi –
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
ms.locfileid: "26054814"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Začínáme s EF základní na univerzální platformu Windows (UWP) s novou databázi

> [!NOTE]
> Tento kurz používá základní EF 2.0.1 (vydané spolu s ASP.NET Core a .NET Core SDK 2.0.3). Základní EF 2.0.0 chybí některé zásadní opravy chyb, které jsou potřebné pro dobrý UWP prostředí.

V tomto návodu vytvoříte univerzální platformu Windows (UWP) aplikace, která provádí základní data přístup k místní databázi SQLite používající rozhraní Entity Framework.

> [!IMPORTANT]
> **Zvažte vyloučení anonymní typy v dotazech LINQ na UWP**. Nasazení aplikace UWP k obchodu s aplikacemi vyžaduje vaše aplikace a má kompilovat s .NET Native. Dotazy s anonymní typy mít zhoršení výkonu .NET Native.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) na Githubu.

## <a name="prerequisites"></a>Požadavky

Následující položky jsou nutné k dokončení tohoto návodu:

* [Aktualizace Windows 10 patří Creators](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) verze 15,4 nebo novější s **Universal Windows Platform Development** zatížení.

## <a name="create-a-new-model-project"></a>Vytvoření nového projektu modelu

> [!WARNING]
> Z důvodu omezení v nástrojích pro .NET Core způsob, jak pracovat s projekty UWP, které model musí být umístěny v jiných UWP projektu mohli ke spuštění příkazů migrace v konzole Správce balíčků

* Otevřete Visual Studio

* Soubor > Nový > projekt...

* V levé nabídce vyberte šablony > Visual C#

* Vyberte **knihovny tříd (.NET Standard)** šablona projektu

* Pojmenujte projekt a klikněte na tlačítko **OK**

## <a name="install-entity-framework"></a>Nainstalujte rozhraní Entity Framework

Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit. Tento návod používá SQLite. Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).

* Nástroje > Správce balíčků NuGet > Konzola správce balíčků

* Spustit`Install-Package Microsoft.EntityFrameworkCore.Sqlite`

Dále v tomto návodu také použijeme některé nástroje Entity Framework pro správnou údržbu databáze. Proto se nainstaluje balíček nástroje.

* Spustit`Install-Package Microsoft.EntityFrameworkCore.Tools`

* Upravte soubor .csproj a nahraďte `<TargetFramework>netstandard2.0</TargetFramework>` s`<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`

## <a name="create-your-model"></a>Vytvoření modelu

Nyní je čas k definování kontextu a entity tříd, které tvoří modelu.

* Projekt > přidejte třídu...

* Zadejte *Model.cs* jako název a klikněte na tlačítko **OK**

* Obsah souboru nahraďte následujícím kódem

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Vytvoření nového projektu UPW

* Otevřete Visual Studio

* Soubor > Nový > projekt...

* V levé nabídce vyberte šablony > Visual C# > univerzální pro Windows

* Vyberte **prázdná aplikace (univerzální pro Windows)** šablona projektu

* Pojmenujte projekt a klikněte na tlačítko **OK**

* Nastavte alespoň cíl a minimální verze`Windows 10 Fall Creators Update (10.0; build 16299.0)`

## <a name="create-your-database"></a>Vytvoření databáze

Teď, když máte model, můžete k vytvoření databáze pro vás migrace.

* Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků

* Vyberte model projekt jako výchozí projekt a nastavte jej jako projekt po spuštění

* Spustit `Add-Migration MyFirstMigration` chcete vygenerovat migraci k vytvoření počáteční sadu tabulek pro váš model.

Vzhledem k tomu, že chceme databáze má být vytvořena na zařízení, které aplikace běží na, přidáme některé kódu pro použití žádné čekající migrace do místní databáze při spuštění aplikace. Při prvním spuštění aplikace, to se postará o vytvoření místní databázi pro nás.

* Klikněte pravým tlačítkem na **App.xaml** v **Průzkumníku řešení** a vyberte **zobrazení kódu**

* Na začátek souboru přidejte zvýrazněná using

* Přidejte zvýrazněný kód použít všechny čekající migrace

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Pokud provedete budoucí změny modelu, můžete použít `Add-Migration` příkaz chcete vygenerovat nový migraci použít k odpovídající položce změn v databázi. Žádné čekající migrace se použijí k místní databázi na každé zařízení při spuštění aplikace.
>
>Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, která již byla do databáze použít.

## <a name="use-your-model"></a>Použití modelu

Nyní můžete modelu provádí přístup k datům.

* Otevřete *MainPage.xaml*

* Přidejte obslužné rutiny zatížení stránky a uživatelského rozhraní obsahu zvýrazněná níže

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

Teď přidáme kód pro propojit vytvořit uživatelské rozhraní s databází

* Klikněte pravým tlačítkem na **MainPage.xaml** v **Průzkumníku řešení** a vyberte **zobrazení kódu**

* Přidejte zvýrazněný kód z následujícího seznamu

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

Nyní můžete spustit aplikaci najdete v části v akci.

* Ladění > Spustit bez ladění

* Aplikace bude sestavovat a spouštět

* Zadejte adresu URL a klikněte na **přidat** tlačítko

![obrázek](_static/create.png)

![obrázek](_static/list.png)

## <a name="next-steps"></a>Další kroky

> [!TIP]
> `SaveChanges()`výkon lze zvýšit implementací [ `INotifyPropertyChanged` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [ `INotifyPropertyChanging` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [ `INotifyCollectionChanged` ](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) ve vašem typy entit a pomocí `ChangeTrackingStrategy.ChangingAndChangedNotifications`.

Tada! Nyní máte jednoduchou aplikaci UWP systémem Entity Framework.

Podívejte se na další články v této dokumentaci další informace o funkcích rozhraní Entity Framework.
