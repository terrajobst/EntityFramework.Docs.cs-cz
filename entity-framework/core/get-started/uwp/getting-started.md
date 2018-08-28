---
title: Začínáme na UPW – nová databáze – EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996907"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Začínáme s EF Core na Universal Windows Platform (UWP) s novou databázi

V tomto kurzu vytvoříte aplikaci univerzální platformy Windows (UPW), který provádí základní přístup k datům s použitím Entity Framework Core místní databázi SQLite.

[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Požadavky

* [Windows 10 Fall Creators Update (10.0; Sestavení 16299) nebo novější](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/) s **vývoj pro univerzální platformu Windows** pracovního vytížení.

* [Sady SDK .NET core 2.1 nebo novější](https://www.microsoft.com/net/core) nebo novější.

## <a name="create-a-model-project"></a>Vytvoření projektu s modelem

> [!IMPORTANT]
> Vzhledem k omezením způsobem nástroje .NET Core pracovat s projekty UWP v modelu musí být umístěny v projektu bez UPW budete moci spouštět příkazy migrace **Konzola správce balíčků** (PMC)

* Otevřít Visual Studio

* **Soubor > Nový > Projekt**

* V levé nabídce vyberte **nainstalováno > Visual C# > .NET Standard**.

* Vyberte **knihovna tříd (.NET Standard)** šablony.

* Pojmenujte projekt *Blogging.Model*.

* Pojmenujte řešení *blogovací*.

* Klikněte na tlačítko **OK**.

## <a name="install-entity-framework-core"></a>Nainstalujte Entity Framework Core

Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit. Tento kurz používá SQLite. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**.

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

Později v tomto kurzu budete používat některé nástroje Entity Framework Core pro správnou údržbu databáze. Balíček nástroje tak instalaci.

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Vytvoření modelu

Nyní je možné definovat kontext a entity třídy, které tvoří modelu.

* Odstranit *Class1.cs*.

* Vytvoření *Model.cs* následujícím kódem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Vytvoření nového projektu pro UPW

* V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **Přidat > Nový projekt**.

* V levé nabídce vyberte **nainstalováno > Visual C# > Windows Universal**.

* Vyberte **prázdná aplikace (Universal Windows)** šablony projektu.

* Pojmenujte projekt *Blogging.UWP*a klikněte na tlačítko **OK**

* Nastavte cílovou a minimální verze na alespoň **Windows 10 Fall Creators Update (10.0; sestavení 16299.0)**.

## <a name="create-the-initial-migration"></a>Vytvoření počáteční migraci

Teď, když máte model, nastavte pro vytvoření databáze při prvním spuštění aplikace. V této části vytvoříte počáteční migraci. V následující části přidáte kód, který použije tuto migraci při spuštění aplikace.

Nástroje pro migraci vyžadovat při spuštění projektu bez UPW, takže nyní vytvořte.

* V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **Přidat > Nový projekt**.

* V levé nabídce vyberte **nainstalováno > Visual C# > .NET Core**.

* Vyberte **Konzolová aplikace (.NET Core)** šablony projektu.

* Pojmenujte projekt *Blogging.Migrations.Startup*a klikněte na tlačítko **OK**.

* Přidat odkaz na projekt z *Blogging.Migrations.Startup* projektu *Blogging.Model* projektu.

Nyní můžete vytvořit počáteční migraci.

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Vyberte *Blogging.Model* projektu jako **výchozí projekt**.

* V **Průzkumníka řešení**, nastavte *Blogging.Migrations.Startup* projekt jako spouštěný projekt.

* Spustit `Add-Migration InitialCreate`.

  Tento příkaz vygeneruje uživatelské rozhraní, které vytvoří počáteční sadu tabulek pro váš model migrace.

## <a name="create-the-database-on-app-startup"></a>Vytvoření databáze při spuštění aplikace

Protože chcete, aby databáze, kterou chcete vytvořit pro zařízení, na které aplikace poběží, přidejte kód pro použití žádné čekající migrace do místní databáze při spuštění aplikace. Při prvním spuštění aplikace, postará se vytvoří místní databázi.

* Přidat odkaz na projekt z *Blogging.UWP* projektu *Blogging.Model* projektu.

* Otevřít *App.xaml.cs*.

* Přidejte zvýrazněný kód chcete použít všechny probíhající migrace.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Pokud změníte model, použijte `Add-Migration` příkaz scaffold novou migraci použít odpovídající změn v databázi. Všechny probíhající migrace se použijí k místní databázi na každé zařízení při spuštění aplikace.
>
>Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.

## <a name="use-the-model"></a>Použití modelu

Nyní můžete model přístup k datům.

* Otevřít *MainPage.xaml*.

* Přidat obslužnou rutinu načtení stránky a uživatelské rozhraní obsah jejichž přehled najdete níže

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Teď přidejte kód, který propojí uživatelského rozhraní pomocí databáze

* Otevřít *MainPage.xaml.cs*.

* V následujícím seznamu přidejte zvýrazněný kód:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

Nyní můžete spustit aplikaci sledujte v akci.

* V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Blogging.UWP* projektu a pak vyberte **nasadit**.

* Nastavte *Blogging.UWP* jako spouštěný projekt.

* **Ladit > Spustit bez ladění**

  Aplikace vytvoří a spustí.

* Zadejte adresu URL a klikněte na tlačítko **přidat** tlačítko

  ![obrázek](_static/create.png)

  ![obrázek](_static/list.png)

  Tada! Teď máte jednoduché aplikace pro UPW s Entity Framework Core.

## <a name="next-steps"></a>Další kroky

Kompatibilitu a výkon informace, které byste měli znát při používání EF Core s UWP, naleznete v tématu [implementacím rozhraní .NET, které EF Core](../../platforms/index.md#universal-windows-platform).

Přečtěte si další články v této dokumentaci se dozvíte další informace o funkcích Entity Framework Core.
