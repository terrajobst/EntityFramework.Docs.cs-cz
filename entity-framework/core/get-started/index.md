---
title: Začínáme – EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416883"
---
# <a name="getting-started-with-ef-core"></a>Začínáme s EF Core

V tomto kurzu vytvoříte konzolovou aplikaci .NET Core, která provádí přístup k datům proti databázi SQLite pomocí jádra entity frameworku.

Kurz můžete sledovat pomocí Visual Studia ve Windows nebo pomocí rozhraní .NET Core CLI ve Windows, macOS nebo Linuxu.

[Zobrazit ukázku tohoto článku na GitHubu](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* [Sada SDK jádra .NET](https://www.microsoft.com/net/download/core).

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019 verze 16.3 nebo novější](https://www.visualstudio.com/downloads/) s tímto zatížením:
  * **Vývoj napříč platformami .NET Core** (v části **Jiné sady nástrojů)**

---

## <a name="create-a-new-project"></a>Vytvoření nového projektu

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Otevřete sadu Visual Studio.
* Klikněte na **Vytvořit nový projekt.**
* Vyberte **Konzolovou aplikaci (.NET Core)** pomocí značky **C#** a klikněte na **Další.**
* Zadejte **EFGetStarted** pro název a klepněte na **vytvořit**

---

## <a name="install-entity-framework-core"></a>Instalace jádra rozhraní entity

Chcete-li nainstalovat EF Core, nainstalujte balíček pro zprostředkovatele databáze EF Core, na které chcete cílit. Tento kurz používá SQLite, protože běží na všech platformách, které podporuje .NET Core. Seznam dostupných zprostředkovatelů naleznete v [tématu Database Providers](../providers/index.md).

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Nástroje >, že správce balíčků NuGet > konzoli správce balíčků**
* Spusťte následující příkazy:

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

Tip: Balíčky můžete nainstalovat také kliknutím pravým tlačítkem myši na projekt a výběrem **možnosti Spravovat balíčky NuGet**

---

## <a name="create-the-model"></a>Vytvoření modelu

Definujte třídu kontextu a třídy entit, které tvoří model.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* V adresáři projektu vytvořte **Model.cs** s následujícím kódem.

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klikněte pravým tlačítkem myši na projekt a vyberte **přidat > třídu.**
* Zadejte **Model.cs** jako název a klikněte na **Přidat.**
* Nahrazení obsahu souboru následujícím kódem

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

EF Core můžete také [zpětnou analýzu](../managing-schemas/scaffolding.md) modelu z existující databáze.

Tip: V reálné aplikaci vložíte každou třídu do samostatného souboru a [vložíte připojovací řetězec](../miscellaneous/connection-strings.md) do konfiguračního souboru nebo proměnné prostředí. Chcete-li, aby byl výukový program jednoduchý, je vše obsaženo v jednom souboru.

## <a name="create-the-database"></a>Vytvoření databáze

Následující kroky používají [migrace](xref:core/managing-schemas/migrations/index) k vytvoření databáze.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

* Spusťte následující příkazy:

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Tím [nainstalujete dotnet ef](../miscellaneous/cli/dotnet.md) a návrhový balíček, který je nutný ke spuštění příkazu v projektu. Příkaz `migrations` přisycování kašovina migrace vytvořit počáteční sadu tabulek pro model. Příkaz `database update` vytvoří databázi a použije na něj novou migraci.

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Spuštění následujících příkazů v **konzole Správce balíčků**

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  Tím se nainstaluje [nástroje PMC pro EF Core](../miscellaneous/cli/powershell.md). Příkaz `Add-Migration` přisycování kašovina migrace vytvořit počáteční sadu tabulek pro model. Příkaz `Update-Database` vytvoří databázi a použije na něj novou migraci.

---

## <a name="create-read-update--delete"></a>Vytvořit, číst, aktualizovat & odstranit

* Otevřete *Program.cs* a nahraďte obsah následujícím kódem:

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a>Spuštění aplikace

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio používá nekonzistentní pracovní adresář při spuštění konzolových aplikací .NET Core. (viz [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) Výsledkem je vyvolání výjimky: *žádná taková tabulka: Blogy*. Aktualizace pracovního adresáře:

* Klikněte pravým tlačítkem myši na projekt a vyberte **Upravit soubor projektu.**
* Těsně pod *TargetFramework* vlastnost, přidejte následující:

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* Soubor uložte

Nyní můžete spustit aplikaci:

* **Ladění > spustit bez ladění**

---

## <a name="next-steps"></a>Další kroky

* Používání ef core ve webové aplikaci pomocí [základního ASP.NET](/aspnet/core/data/ef-rp/intro)
* Další informace o [výrazech dotazů LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
* [Konfigurace modelu](xref:core/modeling/index) pro určení požadovaných [a](xref:core/modeling/entity-properties#required-and-optional-properties) maximální [délky](xref:core/modeling/entity-properties#maximum-length)
* Aktualizace schématu databáze po změně modelu pomocí [migrace](xref:core/managing-schemas/migrations/index)
