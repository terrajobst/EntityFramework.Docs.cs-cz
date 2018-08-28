---
title: Začínáme v rozhraní .NET Framework – existující databáze – EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: edcdc0b76394c4d604cf43fc170424e474532b17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993415"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Začínáme s EF Core na rozhraní .NET Framework s existující databáze

V tomto kurzu vytvoříte konzolovou aplikaci, která provádí základní přístup k datům pro databázi serveru Microsoft SQL Server používá nástroj Entity Framework. Vytvoření modelu Entity Framework ve zpětné analýze existující databázi.

[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Vytvoření databáze blogovací

Tento kurz používá **blogovací** databáze na instanci LocalDb jako existující databázi. Pokud jste již vytvořili **blogovací** databáze jako součást další kurz, přeskočit tyto kroky.

* Otevřít Visual Studio

* **Nástroje > připojení k databázi...**

* Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**

* Zadejte **(localdb) \mssqllocaldb** jako **název serveru**

* Zadejte **hlavní** jako **název_databáze** a klikněte na tlačítko **OK**

* Hlavní databázi se nyní zobrazí v části **datová připojení** v **Průzkumníka serveru**

* Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**

* Zkopírujte skript níže do editoru dotazů

* Klikněte pravým tlačítkem myši na editor dotazů a vyberte **spouštění**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Otevřít Visual Studio 2017

* **Soubor > Nový > projekt...**

* V levé nabídce vyberte **nainstalováno > Visual C# > Windows Desktop**

* Vyberte **Konzolová aplikace (.NET Framework)** šablony projektu

* Ujistěte se, že projekt cílí **rozhraní .NET Framework 4.6.1** nebo novější

* Pojmenujte projekt *ConsoleApp.ExistingDb* a klikněte na tlačítko **OK**

## <a name="install-entity-framework"></a>Nainstalujte rozhraní Entity Framework

Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit. Tento kurz používá systém SQL Server. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

V dalším kroku použijete některé Entity Framework Tools provést zpětnou analýzu databáze. Balíček nástroje tak instalaci.

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-the-model"></a>Zpětná analýza modelu

Nyní je čas vytvořit EF model založený na existující databázi.

* **Balíček NuGet –> nástroje Správce –> Konzola správce balíčků**

* Spusťte následující příkaz pro vytvoření modelu z existující databáze

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> Můžete zadat tabulkami k vygenerování entity pro tak, že přidáte `-Tables` argument výše uvedeného příkazu. Například `-Tables Blog,Post`.

Zpětná analýza procesu vytvoření tříd entit (`Blog` a `Post`) a odvozené kontextu (`BloggingContext`) na základě schématu existující databázi.

Entity třídy jsou jednoduché C# objekty, jež reprezentují data budete dotazování a ukládání. Tady jsou `Blog` a `Post` tříd entit:

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Povolit opožděné načtení, můžete nastavit vlastnosti navigace `virtual` (Blog.Post a Post.Blog).

Kontext představuje relaci s databází. Obsahuje metody, které můžete použít k dotazování a uložit instancí tříd entit.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Použití modelu

Nyní můžete model přístup k datům.

* Otevřít *Program.cs*

* Obsah souboru nahraďte následujícím kódem

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Ladit > Spustit bez ladění

  Uvidíte, že jeden blogu se uloží do databáze a pak se podrobnosti o všech blogy vytisknou na konzole.

  ![obrázek](_static/output-existing-db.png)

## <a name="additional-resources"></a>Další prostředky

* [EF Core na rozhraní .NET Framework s novou databázi](xref:core/get-started/full-dotnet/new-db)
* [EF Core na .NET Core s novou databázi - SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.
