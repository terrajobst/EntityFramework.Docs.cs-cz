---
title: Začínáme – EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: fca1b532b34e20aeea1968939af96c692d60d738
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813609"
---
# <a name="getting-started-with-ef-core"></a>Začínáme s EF Core

V tomto kurzu vytvoříte konzolovou aplikaci .NET Core, která provádí přístup k datům v databázi SQLite pomocí Entity Framework Core.

Můžete postupovat podle kurzu pomocí sady Visual Studio ve Windows nebo pomocí .NET Core CLI ve Windows, macOS nebo Linux.

[Podívejte se na ukázku tohoto článku na GitHubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

## <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* [Sada .NET Core 3,0 SDK](https://www.microsoft.com/net/download/core).

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019 verze 16,3 nebo novější](https://www.visualstudio.com/downloads/) s touto úlohou:
  * **Vývoj pro různé platformy .NET Core** (v **jiných sad nástrojů**)

---

## <a name="create-a-new-project"></a>Vytvoření nového projektu

## <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

``` Console
dotnet new console -o EFGetStarted
cd EFGetStarted
```

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Otevřít Visual Studio
* Klikněte na **vytvořit nový projekt** .
* Vyberte možnost **aplikace konzoly (.NET Core)** se **C#** značkou a klikněte na **Další** .
* Jako název zadejte **EFGetStarted** a klikněte na **vytvořit** .

---

## <a name="install-entity-framework-core"></a>Nainstalovat Entity Framework Core

Chcete-li nainstalovat EF Core, nainstalujte balíček pro zprostředkovatele EF Corech databází, které chcete cílit. V tomto kurzu se používá SQLite, protože běží na všech platformách, které .NET Core podporuje. Seznam dostupných zprostředkovatelů najdete v tématu [poskytovatelé databází](../providers/index.md).

## <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Nástroje > Správce balíčků NuGet > konzole správce balíčků**
* Spusťte následující příkazy:

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

Tip: Balíčky můžete nainstalovat také tak, že kliknete pravým tlačítkem na projekt a vyberete **Spravovat balíčky NuGet** .

---

## <a name="create-the-model"></a>Vytvoření modelu

Definujte třídu kontextu a třídy entit, které tvoří model.

## <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* V adresáři projektu vytvořte **model.cs** s následujícím kódem

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klikněte pravým tlačítkem na projekt a vyberte **přidat > třídy** .
* Jako název zadejte **model.cs** a klikněte na **Přidat** .
* Obsah souboru nahraďte následujícím kódem.

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

EF Core může také provádět [zpětnou analýzu](../managing-schemas/scaffolding.md) modelu z existující databáze.

Tip: V reálné aplikaci vložíte každou třídu do samostatného souboru a [připojovací řetězec](../miscellaneous/connection-strings.md) vložíte do konfiguračního souboru nebo proměnné prostředí. V případě jednoduchého kurzu je vše obsaženo v jednom souboru.

## <a name="create-the-database"></a>Vytvoření databáze

V následujících krocích se k vytvoření databáze používají [migrace](xref:core/managing-schemas/migrations/index) .

## <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* Spusťte následující příkazy:

  ``` Console
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Tím se nainstaluje [dotnet EF](../miscellaneous/cli/dotnet.md) a návrhový balíček, který je nutný ke spuštění příkazu na projektu. `migrations` Příkaz vytvoří z tohoto příkazu pro vytvoření počáteční sadu tabulek pro tento model. `database update` Příkaz vytvoří databázi a použije novou migraci na ni.

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* V **konzole správce balíčků** spusťte následující příkazy.

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  Tím se nainstaluje [nástroje PMC pro EF Core](../miscellaneous/cli/powershell.md). `Add-Migration` Příkaz vytvoří z tohoto příkazu pro vytvoření počáteční sadu tabulek pro tento model. `Update-Database` Příkaz vytvoří databázi a použije novou migraci na ni.

---

## <a name="create-read-update--delete"></a>Vytvořit, číst, aktualizovat & Odstranit

* Otevřete *program.cs* a nahraďte obsah následujícím kódem:

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a>Spuštění aplikace

## <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

``` Console
dotnet run
```

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio používá nekonzistentní pracovní adresář při spuštění konzolových aplikací .NET Core. (viz [dotnet/Project-System # 3619](https://github.com/dotnet/project-system/issues/3619)) Výsledkem je vyjímka vyvolané výjimky: *žádná taková tabulka: Blogy*. Aktualizace pracovního adresáře:

* Klikněte pravým tlačítkem na projekt a vyberte **Upravit soubor projektu** .
* Hned pod vlastností *targetFramework* přidejte následující:

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* Uložte soubor.

Teď můžete aplikaci spustit:

* **Ladit > Spustit bez ladění**

---

## <a name="next-steps"></a>Další kroky

* Pokud chcete používat EF Core ve webové aplikaci, postupujte podle [kurzu ASP.NET Core](/aspnet/core/data/ef-rp/intro) .
* Další informace o [výrazech dotazů LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
* [Nakonfigurujte svůj model](xref:core/modeling/index) tak, aby určoval například [požadované](xref:core/modeling/required-optional) a [maximální délku](xref:core/modeling/max-length) .
* Aktualizace schématu databáze po změně modelu pomocí [migrace](xref:core/managing-schemas/migrations/index)
