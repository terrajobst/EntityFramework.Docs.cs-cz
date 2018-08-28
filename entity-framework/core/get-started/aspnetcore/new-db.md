---
title: Začínáme v ASP.NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996061"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Začínáme s EF Core v ASP.NET Core s novou databázi

V tomto kurzu sestavíte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core. Vytvoření databáze z vašeho modelu EF Core pomocí migrace.

[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) se tyto úlohy:
  * **Vývoj pro ASP.NET a web** (v části **Web a Cloud**)
  * **Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).

## <a name="create-a-new-project-in-visual-studio-2017"></a>Vytvoření nového projektu v sadě Visual Studio 2017

* Otevřít Visual Studio 2017
* **Soubor > Nový > Projekt**
* V levé nabídce vyberte **nainstalováno > Visual C# > .NET Core**.
* Vyberte **webová aplikace ASP.NET Core**.
* Zadejte **EFGetStarted.AspNetCore.NewDb** název a klikněte na **OK**.
* V **nová webová aplikace ASP.NET Core** dialogové okno:
  * Zkontrolujte možnosti **.NET Core** a **ASP.NET Core 2.1** jsou vybrány v rozevíracích seznamech
  * Vyberte **webové aplikace (Model-View-Controller)** šablony projektu
  * Ujistěte se, že **ověřování** je nastavena na **bez ověřování**
  * Klikněte na tlačítko **OK**

Upozornění: Pokud používáte **jednotlivé uživatelské účty** místo **žádný** pro **ověřování** pak model Entity Framework Core se přidají do vašeho projektu v `Models\IdentityModel.cs`. Pomocí technik, které se naučíte v tomto kurzu, můžete přidat druhý modelu nebo rozšiřte tento existující model tak, aby obsahovala tříd entit.

## <a name="install-entity-framework-core"></a>Nainstalujte Entity Framework Core

Instalace EF Core, nainstalujte balíček vytvořeno EF Core databáze, kterou chcete cílit na pro. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md). 

Pro účely tohoto kurzu není nutné instalovat balíček poskytovatele, protože v tomto kurzu použijete SQL Server. Je součástí balíčku zprostředkovatele SQL Server [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="create-the-model"></a>Vytvoření modelu

Definování třídy kontextu a tříd entit, které tvoří modelu:

* Klikněte pravým tlačítkem na **modely** a pak zvolte položku **Přidat > třída**.
* Zadejte **Model.cs** jako název a klikněte na **OK**.
* Obsah souboru nahraďte následujícím kódem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

V reálné aplikaci obvykle vložíte každá třída z vašeho modelu v samostatném souboru. V tomto kurzu z důvodu zjednodušení, umístí všechny třídy v jednom souboru.

## <a name="register-your-context-with-dependency-injection"></a>Kontext zaregistrovat injektáž závislostí

Služby (například `BloggingContext`) jsou registrovány [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) během spuštění aplikace. Komponenty, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím vlastnosti nebo parametry konstruktoru.

Chcete-li `BloggingContext` k dispozici pro kontrolery MVC, zaregistrujte ho jako služba.

* Otevřít **Startup.cs**
* Přidejte následující `using` příkazy:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Volání `AddDbContext` metody pro registraci kontextu jako služba.

* Přidejte následující zvýrazněný kód do `ConfigureServices` metody:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

Poznámka: Skutečné aplikace obecně vložili připojovací řetězec v konfigurační soubor nebo prostředí proměnnou. Z důvodu zjednodušení v tomto kurzu ji definuje v kódu. Zobrazit [připojovací řetězce](../../miscellaneous/connection-strings.md) Další informace.

## <a name="create-the-database"></a>Vytvoření databáze

Jakmile budete mít modelu, můžete použít [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**
* Spustit `Add-Migration InitialCreate` k migraci na vytvoření počáteční sadu tabulek pro váš model. Pokud se zobrazí chyba `The term 'add-migration' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.
* Spustit `Update-Database` použít novou migraci databáze. Tento příkaz vytvoří databázi před použitím migrace.

## <a name="create-a-controller"></a>Vytvoření kontroleru

Kontroler a zobrazení pro generování uživatelského rozhraní `Blog` entity.

* Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníku řešení** a vyberte **Přidat > kontroler**.
* Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **přidat**.
* Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**.
* Klikněte na tlačítko **přidat**.


## <a name="run-the-application"></a>Spuštění aplikace

Stiskněte klávesu F5 ke spuštění a testování aplikace.

* Přejděte na `/Blogs`
* Pomocí odkazu vytvořit některé položky blogu. Otestujte podrobnosti a odkazy odstranit.

![obrázek](_static/create.png)

![obrázek](_static/index-new-db.png)

## <a name="additional-resources"></a>Další prostředky

* [EF - novou databázi pomocí SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.
* [Úvod do ASP.NET Core MVC v systému Mac nebo Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Úvod do ASP.NET Core MVC se sadou Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
