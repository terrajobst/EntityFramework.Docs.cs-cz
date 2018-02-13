---
title: "Začínáme na základní EF ASP.NET Core - novou databázi –"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: f6ed19d3c5d2ae8d1f5756558e50c1f0dddd2f07
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Začínáme s EF základní na ASP.NET Core s novou databázi

V tomto návodu vytvoříte aplikaci ASP.NET MVC jádra, která provádí základní přístup k datům používá Entity Framework Core. Migrace použije k vytvoření databáze z modelu EF jádra. V tématu [další prostředky](#additional-resources) pro další kurzy Entity Framework Core.

Tento kurz vyžaduje:
* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) s tyto úlohy:
  * **Vývoj pro ASP.NET a webové** (v části **Web a Cloud**)
  * **Vývoj pro různé platformy .NET core** (v části **ostatní modulové**)
* [Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/download/core).

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) na Githubu.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Vytvořte nový projekt v Visual Studio 2017

* **Soubor > Nový > Projekt**
* V levé nabídce vyberte **nainstalovaná > šablony > Visual C# > .NET Core**.
* Vyberte **webové aplikace ASP.NET Core**.
* Zadejte **EFGetStarted.AspNetCore.NewDb** pro název a klikněte na tlačítko **OK**.
* V **nové webové aplikace ASP.NET Core** dialogové okno:
  * Zkontrolujte možnosti **.NET Core** a **technologii ASP.NET 2.0 základní** jsou vybrány v rozevíracích seznamů
  * Vyberte **webové aplikace (Model-View-Controller)** šablona projektu
  * Ujistěte se, že **ověřování** je nastaven na **bez ověřování**
  * Klikněte na tlačítko **OK**

Upozornění: Pokud používáte **jednotlivé uživatelské účty** místo **žádné** pro **ověřování** pak model Entity Framework Core přidá do projektu v `Models\IdentityModel.cs`. Pomocí technik, které se dozvíte v tomto návodu, můžete přidat druhý modelu nebo rozšířit tento existující model tak, aby obsahovala vaše třídy entity.

## <a name="install-entity-framework-core"></a>Instalace jádra Entity Framework

Nainstalujte balíček pro následující zprostředkovatele databáze EF jádra, které chcete cílit. Tento návod používá SQL Server. Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Použijeme některé Entity Framework jádra nástroje k vytvoření databáze z modelu EF jádra. Proto se nainstaluje balíček nástroje:

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

Nemůžeme vytvořit kontrolery a zobrazení později na pomocí některé nástroje ASP.NET Core generování uživatelského rozhraní. Proto se nainstaluje tohoto návrhu balíčku:

* Spustit `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="create-the-model"></a>Vytvoření modelu

Definujte kontext a entity třídy, které tvoří modelu:

* Klikněte pravým tlačítkem na **modely** složky a vyberte **Přidat > třída**.
* Zadejte **Model.cs** jako název a klikněte na tlačítko **OK**.
* Obsah souboru nahraďte následujícím kódem:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Poznámka: V reálné aplikaci byste obvykle umístili každá třída z modelu v samostatném souboru. Z důvodu zjednodušení jsme jsou uvedení všechny třídy v jednom souboru pro účely tohoto kurzu.

## <a name="register-your-context-with-dependency-injection"></a>Zaregistrovat váš kontext vkládání závislostí

Služby (například `BloggingContext`) jsou registrovány [vkládání závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) během spuštění aplikace. Součásti, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím konstruktor parametry nebo vlastnosti.

V pořadí pro naše řadiče MVC, chcete-li použít `BloggingContext` jsme se registrovat jako službu.

* Otevřete **Startup.cs**
* Přidejte následující `using` příkazy:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Přidat `AddDbContext` metody pro registraci jako služba:

* Přidejte následující kód, který `ConfigureServices` metoda:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Poznámka: Skutečné aplikace obecně přepnutím připojovací řetězec v konfiguračním souboru. Z důvodu zjednodušení jsme se jeho definováním v kódu. V tématu [připojovací řetězce](../../miscellaneous/connection-strings.md) Další informace.

## <a name="create-your-database"></a>Vytvoření databáze

Jakmile máte modelu, můžete použít [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.

* Otevřete pomocí PMC:

  **Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků**
* Spustit `Add-Migration InitialCreate` chcete vygenerovat migraci k vytvoření počáteční sadu tabulek pro váš model. Pokud se zobrazí chybová zpráva, `The term 'add-migration' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.
* Spustit `Update-Database` použít nové migrace do databáze. Tento příkaz vytvoří databázi před použitím migrace.

## <a name="create-a-controller"></a>Vytvořit řadič

Povolte generování uživatelského rozhraní v projektu:

* Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat > řadiče**.
* Vyberte **minimální závislosti** a klikněte na tlačítko **přidat**.
* Můžete ignorovat nebo odstranit *ScaffoldingReadMe.txt* souboru.

Teď, když je povoleno generování uživatelského rozhraní, jsme vygenerovat řadič pro `Blog` entity.

* Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat > řadiče**.
* Vyberte **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **Ok**.
* Nastavit **třída modelu** k **Blog** a **třída kontextu dat** k **BloggingContext**.
* Klikněte na tlačítko **přidat**.


## <a name="run-the-application"></a>Spuštění aplikace

Stisknutím klávesy F5 spusťte a testování aplikací.

* Přejděte na `/Blogs`
* Pomocí odkazu vytvořte vytvořit některé položky blogu. Testování podrobnosti a odstraňte odkazy.

![obrázek](_static/create.png)

![obrázek](_static/index-new-db.png)

## <a name="additional-resources"></a>Další prostředky

* [EF - novou databázi pomocí SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzoly napříč platformami.
* [Úvod do základní ASP.NET MVC v Mac nebo Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Úvod do základní ASP.NET MVC pomocí sady Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
