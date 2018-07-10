---
title: Začínáme na .NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Začínáme s .NET Core pomocí Entity Framework Core
keywords: .NET core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911499"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Začínáme s EF Core na aplikace konzoly .NET Core s novou databázi

V tomto návodu vytvoříte konzolovou aplikaci .NET Core, který provádí přístup k datům s použitím Entity Framework Core databáze SQLite. Migrace použijete k vytvoření databáze z modelu. Zobrazit [ASP.NET Core – nová databáze](xref:core/get-started/aspnetcore/new-db) pro verzi sady Visual Studio pomocí ASP.NET Core MVC.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) na Githubu.

## <a name="prerequisites"></a>Požadavky

[.NET Core SDK](https://www.microsoft.com/net/core) 2.1

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Vytvořte nový projekt konzoly:

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a>Nainstalujte Entity Framework Core

Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit. Tento návod používá SQLite. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).

* Instalace Microsoft.EntityFrameworkCore.Sqlite a Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Spustit `dotnet restore` nainstalovat nové balíčky.

## <a name="create-the-model"></a>Vytvoření modelu

Definování kontextu a entity třídy, které tvoří modelu.

* Vytvořte nový *Model.cs* soubor s tímto obsahem.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Tip: V reálné aplikaci můžete vložit každá třída v samostatném souboru a vložit připojovací řetězec do konfiguračního souboru. Pro zjednodušení tento kurz, všechno, co je obsažen v jednom souboru.

## <a name="create-the-database"></a>Vytvoření databáze

Jakmile budete mít modelu, použijete [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.

* Spustit `dotnet ef migrations add InitialCreate` generování uživatelského rozhraní migrace a vytvářet počáteční sadu tabulek pro model.
* Spustit `dotnet ef database update` použít novou migraci databáze. Tento příkaz vytvoří databázi před použitím migrace.

*Blogging.db** je databáze SQLite v adresáři projektu.

## <a name="use-your-model"></a>Použití modelu

* Otevřít *Program.cs* a nahraďte jeho obsah následujícím kódem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testování aplikace:

  `dotnet run`

  Blogů se uloží do databáze a podrobnosti o všech blogy se zobrazují v konzole.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Změna modelu:

- Pokud provedete změny modelu, můžete použít `dotnet ef migrations add` příkaz scaffold nový [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) provést odpovídající schématu změn v databázi. Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny), můžete použít `dotnet ef database update` příkaz změny se projeví do databáze.
- Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.
- SQLite nepodporuje všechny migrace (změny schématu) z důvodu omezení v SQLite. Zobrazit [omezení SQLite](../../providers/sqlite/limitations.md). Pro nový vývoj zvažte vyřazení databáze a vytvořením nového spíš než migrace při změně vašeho modelu.

## <a name="additional-resources"></a>Další prostředky

* [.NET core – nová databáze s SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.
* [Úvod do ASP.NET Core MVC v systému Mac nebo Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Úvod do ASP.NET Core MVC se sadou Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
