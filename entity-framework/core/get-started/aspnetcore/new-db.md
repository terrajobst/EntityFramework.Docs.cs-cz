---
title: Začínáme v ASP.NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 2eb1668b8c077fabc9cb21088452fd1bead7ff22
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452246"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Začínáme s EF Core v ASP.NET Core s novou databázi

V tomto kurzu sestavíte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core. Tento kurz používá k vytvoření databáze z modelu migrace.

Tento kurz provedením pomocí sady Visual Studio 2017 na Windows nebo pomocí rozhraní příkazového řádku .NET Core ve Windows, macOS nebo Linuxu.

Zobrazte ukázky v tomto článku na Githubu:
* [Visual Studio 2017 with SQL Server](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [.NET core CLI s SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/) se tyto úlohy:
  * **Vývoj pro ASP.NET a web** (v části **Web a Cloud**)
  * **Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

---

## <a name="create-a-new-project"></a>Vytvoření nového projektu

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Open Visual Studio 2017
* **Soubor > Nový > Projekt**
* V nabídce vlevo vyberte **nainstalováno > Visual C# > .NET Core**.
* Vyberte **webová aplikace ASP.NET Core**.
* Zadejte **EFGetStarted.AspNetCore.NewDb** název a klikněte na **OK**.
* V **nová webová aplikace ASP.NET Core** dialogové okno:
  * Ujistěte se, že **.NET Core** a **ASP.NET Core 2.1** jsou vybrány v rozevíracích seznamech
  * Vyberte **webové aplikace (Model-View-Controller)** šablony projektu
  * Ujistěte se, že **ověřování** je nastavena na **bez ověřování**
  * Klikněte na tlačítko **OK**

Upozornění: Pokud používáte **jednotlivé uživatelské účty** místo **žádný** pro **ověřování** pak model Entity Framework Core se přidá do projektu `Models\IdentityModel.cs`. Pomocí technik, které se naučíte v tomto kurzu, můžete přidat druhý modelu nebo rozšiřte tento existující model tak, aby obsahovala tříd entit.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* Spusťte následující příkaz pro vytvoření projektu MVC:

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Přejděte do adresáře projektu. Další příkazy, které zadáte nutné ke spuštění nového projektu.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Nainstalujte Entity Framework Core

Instalace EF Core, nainstalujte balíček vytvořeno EF Core databáze, kterou chcete cílit na pro. Seznam dostupných zprostředkovatelů najdete v tématu [poskytovatelé databází](../../providers/index.md).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Pro účely tohoto kurzu není nutné instalovat balíček poskytovatele, protože v tomto kurzu použijete SQL Server. Je součástí balíčku zprostředkovatele SQL Server [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Tento kurz používá SQLite, protože běží na všech platformách, které podporuje .NET Core.

* Spusťte následující příkaz k instalaci zprostředkovatele SQLite:

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Vytvoření modelu

Definování třídy kontextu a tříd entit, které tvoří modelu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klikněte pravým tlačítkem na **modely** a pak zvolte položku **Přidat > třída**.
* Zadejte **Model.cs** jako název a klikněte na **OK**.
* Obsah souboru nahraďte následujícím kódem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* V **modely** vytvořit složku **Model.cs** následujícím kódem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Produkční aplikace obvykle vložili každá třída v samostatném souboru. V tomto kurzu z důvodu zjednodušení, vloží tyto třídy v jednom souboru.

## <a name="register-the-context-with-dependency-injection"></a>Zaregistrovat kontext pomocí vkládání závislostí

Chcete-li `BloggingContext` k dispozici pro kontrolery MVC registrovat jako službu v `Startup.cs`.

Služby (například `BloggingContext`) jsou registrovány [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) během spuštění aplikace tak, aby se je možné poskytnout automaticky součásti, které využívají služeb (jako jsou řadiče MVC) prostřednictvím konstruktoru parametry a vlastnosti.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* V **Startup.cs** přidejte následující `using` příkazy:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Přidejte následující zvýrazněný kód do `ConfigureServices` metody:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* V **Startup.cs** přidejte následující `using` příkazy:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Přidejte následující zvýrazněný kód do `ConfigureServices` metody:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

Produkční aplikace obvykle vložili připojovací řetězec v konfigurační soubor nebo prostředí proměnnou. Z důvodu zjednodušení v tomto kurzu ji definuje v kódu. Zobrazit [připojovací řetězce](../../miscellaneous/connection-strings.md) Další informace.

## <a name="create-the-database"></a>Vytvoření databáze

Následující kroky použijte [migrace](xref:core/managing-schemas/migrations/index) k vytvoření databáze.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**
* Spusťte následující příkazy:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Pokud se zobrazí chyba `The term 'add-migration' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.

  `Add-Migration` Příkaz vygeneruje uživatelské rozhraní migrace vytvořte počáteční sadu tabulek pro model. `Update-Database` Příkaz vytvoří databázi a použije novou migraci na něj.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* Spusťte následující příkazy:

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  `migrations` Příkaz vygeneruje uživatelské rozhraní migrace vytvořte počáteční sadu tabulek pro model. `database update` Příkaz vytvoří databázi a použije novou migraci na něj.

---

## <a name="create-a-controller"></a>Vytvoření kontroleru

Kontroler a zobrazení pro generování uživatelského rozhraní `Blog` entity.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníku řešení** a vyberte **Přidat > kontroler**.
* Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **přidat**.
* Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**.
* Klikněte na **Přidat**.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* Spusťte následující příkazy:

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  `tool install` a `add package` příkazy instalace nástrojů, můžete vygenerovat kontrolerů a zobrazení. `restore` Příkaz zajistí, že se stáhnou všechny balíčky v projektu a `aspnet-codegenerator` příkaz dělá základní kostry aplikace.
---

Modul generování uživatelského rozhraní vytvoří následující soubory:

* Kontroler (*Controllers/BlogsController.cs*)
* Zobrazení syntaxe Razor pro vytvořit, odstranit, podrobností, úpravy a Index stránky (_Views/Blogs/*.cshtml_)

## <a name="run-the-application"></a>Spuštění aplikace

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Ladění** > **spustit bez ladění**

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```cli
dotnet run
```
---

* Přejděte na `/Blogs`

* Použití **vytvořit nový** odkaz můžete vytvořit některé položky blogu.

  ![Vytvoření stránky](_static/create.png)

* Test **podrobnosti**, **upravit**, a **odstranit** odkazy.

  ![Indexová stránka](_static/index-new-db.png)

## <a name="additional-tutorials"></a>Další kurzy

* [Začínáme s EF Core na .NET Core s novou databázi pomocí SQLite](xref:core/get-started/netcore/new-db-sqlite)
* ASP.NET Core MVC:
  * [Začínáme s ASP.NET Core MVC](/aspnet/core/tutorials/first-mvc-app/start-mvc)
  * [Začínáme s EF Core ve webové aplikaci ASP.NET MVC](/aspnet/core/data/ef-mvc/intro)
* [Stránky Razor](/aspnet/core/razor-pages/index):
  * [Začínáme se stránkami Razor v ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start)
  * [Stránky Razor pomocí Entity Framework Core v ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
