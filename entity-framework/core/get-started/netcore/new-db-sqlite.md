---
title: Začínáme na základní EF .NET Core - novou databázi –
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Začínáme s .NET Core pomocí Entity Framework Core
keywords: .NET core, Entity Framework Core, VS kód, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: fcace3c0f259b1a456d9ca1086e6a1549c070d57
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Začínáme s EF základní na konzolové aplikace .NET Core s novou databázi

V tomto návodu vytvoříte konzolovou aplikaci .NET Core, která provádí základní přístup k datům s použitím Entity Framework Core databáze SQLite. Migrace použije k vytvoření databáze z modelu. V tématu [ASP.NET Core - novou databázi](xref:core/get-started/aspnetcore/new-db) pro verzi Visual Studio pomocí technologie ASP.NET MVC jádra.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) na Githubu.

## <a name="prerequisites"></a>Požadavky

Následující požadované součásti jsou nutné k dokončení tohoto názorného postupu:
* Operační systém, který podporuje .NET Core.
* [.NET Core SDK](https://www.microsoft.com/net/core) 2.0 (i když pokyny slouží k vytvoření aplikace s předchozí verzí s velmi málo změny).

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Vytvořte novou `ConsoleApp.SQLite` složky pro váš projekt a použití `dotnet` příkaz k naplnění s aplikací .NET Core.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Instalace jádra Entity Framework

Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit. Tento návod používá SQLite. Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).

* Instalace Microsoft.EntityFrameworkCore.Sqlite a Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Ručně upravte `ConsoleApp.SQLite.csproj` přidat DotNetCliToolReference do Microsoft.EntityFrameworkCore.Tools.DotNet:

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

`ConsoleApp.SQLite.csproj` má teď obsahují následující:

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Poznámka: Byly čísla verzí používá výše v době publikování správně.

*  Spustit `dotnet restore` k instalaci nové balíčky.

## <a name="create-the-model"></a>Vytvoření modelu

Definujte kontextu a entity třídy, které tvoří modelu.

* Vytvořte novou *Model.cs* soubor s tímto obsahem.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Tip: V reálné aplikaci jste by uložte každá třída v samostatném souboru a připojovací řetězec v konfiguračním souboru. Zjednodušení tento kurz, jsme jsou uvedení vše v jednom souboru.

## <a name="create-the-database"></a>Vytvoření databáze

Jakmile máte modelu, můžete použít [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.

* Spustit `dotnet ef migrations add InitialCreate` vygenerovat migrace a vytvořit počáteční sadu tabulek pro model.
* Spustit `dotnet ef database update` použít nové migrace do databáze. Tento příkaz vytvoří databázi před použitím migrace.

> [!NOTE]  
> Při použití relativní cesty s SQLite, cesta bude relativně k hlavní sestavení aplikace. V této ukázce je hlavní binární `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, aby se v databázi SQLite `bin/Debug/netcoreapp2.0/blogging.db`.

## <a name="use-your-model"></a>Použití modelu

* Otevřete *Program.cs* a nahraďte jeho obsah následujícím kódem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testovací aplikace:

  `dotnet run`

  Blogů je uložený na databázi a podrobnosti o všech blogy se zobrazují v konzole.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Změna modelu:

- Pokud provedete změny modelu, můžete použít `dotnet ef migrations add` příkaz chcete vygenerovat nový [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) aby odpovídající schéma změn v databázi. Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny provedené), můžete použít `dotnet ef database update` příkaz k použití změn do databáze.
- Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, která již byla do databáze použít.
- SQLite nepodporuje všechny migrace (změny schématu) z důvodu omezení v SQLite. V tématu [SQLite omezení](../../providers/sqlite/limitations.md). Pro nový vývoj zvažte vyřazení databáze a vytvoření nového než pomocí migrace, když se změní modelu.

## <a name="additional-resources"></a>Další prostředky

* [.NET core - novou databázi pomocí SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzoly napříč platformami.
* [Úvod do základní ASP.NET MVC v Mac nebo Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Úvod do základní ASP.NET MVC pomocí sady Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
