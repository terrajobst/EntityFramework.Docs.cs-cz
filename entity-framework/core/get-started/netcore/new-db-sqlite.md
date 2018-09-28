---
title: Začínáme na .NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
description: Začínáme s .NET Core pomocí Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: b30800afb63a51ab14aecb559dcc83fd89f71a71
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415767"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Začínáme s EF Core na aplikace konzoly .NET Core s novou databázi

V tomto kurzu vytvoříte konzolovou aplikaci .NET Core, který provádí přístup k datům s použitím Entity Framework Core databáze SQLite. Migrace použijete k vytvoření databáze z modelu. Zobrazit [ASP.NET Core – nová databáze](xref:core/get-started/aspnetcore/new-db) pro verzi sady Visual Studio pomocí ASP.NET Core MVC.

Zobrazit ukázky v tomto článku na Githubu] (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Požadavky

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Vytvořte nový projekt konzoly:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>Změňte aktuální adresář 

V dalších krocích, potřebujeme k vydávání `dotnet` příkazy vzhledem k aplikaci. 

* Můžeme změnit aktuální adresář do adresáře aplikace následujícím způsobem:

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a>Nainstalujte Entity Framework Core

Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit. Tento návod používá SQLite. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).

* Instalace Microsoft.EntityFrameworkCore.Sqlite a Microsoft.EntityFrameworkCore.Design

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Spustit `dotnet restore` nainstalovat nové balíčky.

## <a name="create-the-model"></a>Vytvoření modelu

Definování kontextu a entity třídy, které tvoří modelu.

* Vytvořte nový *Model.cs* soubor s tímto obsahem.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Tip: V reálné aplikaci, můžete vložit každá třída v samostatném souboru a vložit připojovací řetězec konfigurační soubor nebo prostředí proměnnou. Pro zjednodušení tento kurz, všechno, co je obsažen v jednom souboru.

## <a name="create-the-database"></a>Vytvoření databáze

Jakmile budete mít modelu, použijete [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.

* Spustit `dotnet ef migrations add InitialCreate` generování uživatelského rozhraní migrace a vytvářet počáteční sadu tabulek pro model.
* Spustit `dotnet ef database update` použít novou migraci databáze. Tento příkaz vytvoří databázi před použitím migrace.

*Blogging.db** je databáze SQLite v adresáři projektu.

## <a name="use-the-model"></a>Použití modelu

* Otevřít *Program.cs* a nahraďte jeho obsah následujícím kódem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testování aplikace z konzoly. Zobrazit [Poznámka: Visual Studio](#vs) ke spuštění aplikace ze sady Visual Studio.

  `dotnet run`

  Blogů se uloží do databáze a podrobnosti o všech blogy se zobrazují v konzole.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Změna modelu:

- Pokud provedete změny modelu, můžete použít `dotnet ef migrations add` příkaz scaffold nový [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations). Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny), můžete použít `dotnet ef database update` příkaz pro použití schématu změn v databázi.
- Používá EF Core `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.
- Databázový stroj SQLite nepodporuje některé změny schématu, které podporuje většina jiných relačních databází. Například `DropColumn` operace není podporována. Migrace EF Core dojde k vygenerování kódu pro tyto operace. Ale pokud se pokusíte použít na databázi nebo vygenerovat skript, EF Core vyvolá výjimky. Zobrazit [omezení SQLite](../../providers/sqlite/limitations.md). Pro nový vývoj zvažte vyřazení databáze a vytvořením nového spíš než migrace při změně modelu.
- 

<a name="vs"></a>

### <a name="run-from-visual-studio"></a>Spustit ze sady Visual Studio

Tuto ukázku spustit ze sady Visual Studio, musíte nastavit pracovní adresář ručně, aby se použije kořen projektu. Pokud nenastavíte pracovního adresáře a následující `Microsoft.Data.Sqlite.SqliteException` je vyvolána výjimka: `SQLite Error 1: 'no such table: Blogs'`.

Chcete-li nastavit pracovní adresář:

* V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a pak vyberte **vlastnosti**.
* Vyberte **ladění** kartu v levém podokně.
* Nastavte **pracovní adresář** k adresáři projektu.
* Uložte změny.

## <a name="additional-resources"></a>Další prostředky

* [Úvod do ASP.NET Core MVC v systému Mac nebo Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Úvod do ASP.NET Core MVC se sadou Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
