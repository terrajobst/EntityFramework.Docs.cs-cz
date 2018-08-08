---
title: Začínáme v rozhraní .NET Framework – nová databáze – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614425"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Začínáme s EF Core na rozhraní .NET Framework s novou databázi

V tomto kurzu vytvoříte konzolovou aplikaci, která provádí základní přístup k datům pro databázi serveru Microsoft SQL Server používá nástroj Entity Framework. Migrace použijete k vytvoření databáze z modelu.

[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Otevřít Visual Studio 2017

* **Soubor > Nový > projekt...**

* V levé nabídce vyberte **nainstalováno > Visual C# > Windows Desktop**

* Vyberte **Konzolová aplikace (.NET Framework)** šablony projektu

* Ujistěte se, že projekt cílí **rozhraní .NET Framework 4.6.1** nebo novější

* Pojmenujte projekt *ConsoleApp.NewDb* a klikněte na tlačítko **OK**

## <a name="install-entity-framework"></a>Nainstalujte rozhraní Entity Framework

Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit. Tento kurz používá systém SQL Server. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).

* Nástroje > Správce balíčků NuGet > Konzola správce balíčků

* Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Později v tomto kurzu použijete některé Entity Framework Tools pro správnou údržbu databáze. Balíček nástroje tak instalaci.

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Vytvoření modelu

Nyní je možné definovat kontext a entity třídy, které tvoří modelu.

* **Projekt > Přidat třídu...**

* Zadejte *Model.cs* jako název a klikněte na **OK**

* Obsah souboru nahraďte následujícím kódem

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> V reálné aplikaci byste umístit každá třída v samostatném souboru a vložte připojovací řetězec konfigurační soubor nebo prostředí proměnnou. Z důvodu zjednodušení všechno, co je v souboru jednoho kódu pro účely tohoto kurzu.

## <a name="create-the-database"></a>Vytvoření databáze

Teď, když máte model, můžete k vytvoření databáze migrace.

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Spustit `Add-Migration InitialCreate` k migraci na vytvoření počáteční sadu tabulek pro model.

* Spustit `Update-Database` použít novou migraci databáze. Vzhledem k tomu, že databáze ještě neexistuje, vytvoří se před použitím migrace.

> [!TIP]  
> Pokud provedete změny modelu, můžete použít `Add-Migration` příkaz scaffold novou migraci provést odpovídající schématu změn v databázi. Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny), můžete použít `Update-Database` příkaz změny se projeví do databáze.
>
> Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.

## <a name="use-the-model"></a>Použití modelu

Nyní můžete model přístup k datům.

* Otevřít *Program.cs*

* Obsah souboru nahraďte následujícím kódem

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **Ladit > Spustit bez ladění**

  Uvidíte, že jeden blogu se uloží do databáze a pak se podrobnosti o všech blogy vytisknou na konzole.

  ![obrázek](_static/output-new-db.png)

## <a name="additional-resources"></a>Další prostředky

* [EF Core na rozhraní .NET Framework s existující databáze](xref:core/get-started/full-dotnet/existing-db)
* [EF Core na .NET Core s novou databázi - SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.
