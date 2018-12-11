---
title: Instalace Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 58c79d477d590eea355a922b3e1233bbecb305cc
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/10/2018
ms.locfileid: "53181978"
---
# <a name="installing-entity-framework-core"></a>Instalace Entity Framework Core

## <a name="prerequisites"></a>Požadavky

* Je EF Core [.NET Standard 2.0](/dotnet/standard/net-standard) knihovny. EF Core vyžaduje implementaci rozhraní .NET, která podporuje .NET Standard 2.0 ke spuštění. EF Core můžete odkazovat také jiné knihovny .NET Standard 2.0. 

* Například můžete použít EF Core můžete vyvíjet aplikace, které cílí na .NET Core. Vytvoření aplikace .NET Core vyžaduje [.NET Core SDK](https://dotnet.microsoft.com/download). Volitelně můžete také použít vývojové prostředí, jako je Visual Studio, Visual Studio pro Mac nebo Visual Studio Code. Další informace najdete [Začínáme s .NET Core](/dotnet/core/get-started).

* EF Core můžete použít k vývoji aplikací využívajících rozhraní .NET Framework 4.6.1 nebo později ve Windows pomocí sady Visual Studio. Doporučuje se nejnovější verzi sady Visual Studio. Pokud chcete použít starší verzi, jako je Visual Studio 2015, ujistěte se, že jste [upgradovat klienta NuGet verzi 3.6.0](https://www.nuget.org/downloads) pro práci s knihovnami .NET Standard 2.0.

* EF Core můžete spustit v jiných implementacích rozhraní .NET, jako třeba Xamarin nebo .NET Native. Ale v praxi tyto implementace omezení modulu runtime, které mohou ovlivnit, jak dobře EF Core funguje ve vaší aplikaci. Další informace najdete v tématu [implementacím rozhraní .NET, které EF Core](xref:core/platforms/index).

* Nakonec poskytovatelé různých databází může vyžadovat konkrétní databázi verzí vyhledávacích strojů, implementace .NET nebo operační systémy. Ujistěte se, že [poskytovatele databáze EF Core](xref:core/providers/index) je k dispozici, který podporuje správné prostředí pro vaši aplikaci.

## <a name="get-the-entity-framework-core-runtime"></a>Získat modul runtime Entity Framework Core

EF Core přidat do aplikace, nainstalujte balíček NuGet pro poskytovatele databáze, kterou chcete použít.

Pokud vytváříte aplikaci ASP.NET Core, není nutné instalovat v paměti a poskytovatelé serveru SQL Server. Tyto poskytovatelé jsou součástí aktuálních verzí ASP.NET Core, společně s runtime EF Core.  

Pokud chcete nainstalovat nebo aktualizovat balíčky NuGet, můžete použít [rozhraní .NET Core rozhraní příkazového řádku (CLI), dialogové okno Správce balíčků Visual Studio nebo Konzola správce balíčků Visual Studio.

### <a name="net-core-cli"></a>.NET core CLI

* Použijte následující příkaz rozhraní příkazového řádku .NET Core z příkazového řádku operačního systému pro instalaci nebo aktualizaci zprostředkovatele EF Core SQL Server:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Můžete určit konkrétní verzi `dotnet add package` příkaz a `-v` modifikátor. Například chcete-li instalovat balíčky EF Core 2.2.0, přidejte `-v 2.2.0` k příkazu.

Další informace najdete v tématu [nástroje rozhraní příkazového řádku (CLI) .NET](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Dialogové okno Správce balíčků NuGet sady Visual Studio

* V nabídce sady Visual Studio vyberte **Projekt > spravovat balíčky NuGet**

* Klikněte na **Procházet** nebo **aktualizace** kartu

* Chcete-li nainstalovat nebo aktualizovat zprostředkovatele SQL Server, vyberte `Microsoft.EntityFrameworkCore.SqlServer` balení a potvrďte.

Další informace najdete v tématu [dialogové okno Správce balíčků NuGet](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Konzola správce balíčků NuGet sady Visual Studio

* V nabídce sady Visual Studio vyberte **nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Pokud chcete nainstalovat zprostředkovatele SQL Server, spusťte následující příkaz v konzole Správce balíčků:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Chcete-li aktualizovat poskytovatele, použijte `Update-Package` příkazu.

* Chcete-li zadat konkrétní verzi, použijte `-Version` modifikátor. Například chcete-li instalovat balíčky EF Core 2.2.0, přidejte `-Version 2.2.0` k příkazům

Další informace najdete v tématu [Konzola správce balíčků](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Získat nástroje Entity Framework Core

Můžete nainstalovat nástroje provádět úlohy související s EF Core ve vašem projektu, jako jsou vytvoření a použití migrace databází nebo vytvoření modelu EF Core založené na existující databázi.

K dispozici jsou dvě sady nástrojů:

* [Nástroje rozhraní příkazového řádku (CLI) pro .NET Core](xref:core/miscellaneous/cli/dotnet) lze použít na Windows, Linux nebo macOS. Tyto příkazy začínají `dotnet ef`. 

* [Nástroje konzoly Správce balíčků (PMC)](xref:core/miscellaneous/cli/powershell) spustit v sadě Visual Studio na Windows. Tyto příkazy spustit s operací, například `Add-Migration`, `Update-Database`.

I když můžete použít také `dotnet ef` příkazy z konzoly Správce balíčků, doporučuje se používat nástroje konzoly Správce balíčků při používání sady Visual Studio:

* Automaticky fungují s aktuální projekt vybraný v konzole PMC v sadě Visual Studio bez nutnosti ručně přepnout adresáře.  

* Automaticky otevřou soubory generované záznamem pro příkazy v sadě Visual Studio po dokončení příkazu.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Získání nástrojů rozhraní příkazového řádku .NET Core

Nástroje rozhraní příkazového řádku .NET core vyžaduje .NET Core SDK, uvedené výše v [požadavky](#prerequisites).

`dotnet ef` Příkazy jsou zahrnuté v aktuálních verzích .NET Core SDK, ale aby příkazy v konkrétním projektu, je třeba nainstalovat `Microsoft.EntityFrameworkCore.Design` balíčku:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Pro aplikace ASP.NET Core pro tento balíček je přiložena automaticky.

> [!IMPORTANT]      
> Vždy používejte verzi sady nástrojů, který odpovídá hlavní verzi balíčky runtime.

### <a name="get-the-package-manager-console-tools"></a>Získat nástroje konzoly Správce balíčků

Chcete-li získat Konzola správce balíčků nástroje na EF Core, nainstalovat `Microsoft.EntityFrameworkCore.Tools` balíčku. Například ze sady Visual Studio:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

Pro aplikace ASP.NET Core pro tento balíček je přiložena automaticky.

## <a name="upgrading-to-the-latest-ef-core"></a>Upgrade na nejnovější EF Core

* Kdykoli můžeme vydat novou verzi EF Core, také uvolňujeme nové verze zprostředkovatele, které jsou součástí projektu EF Core, jako je Microsoft.EntityFrameworkCore.SqlServer Microsoft.EntityFrameworkCore.Sqlite, a Microsoft.EntityFrameworkCore.InMemory. Pouze můžete upgradovat na novou verzi poskytovatele za účelem získání všech vylepšení. 

* EF Core, spolu se službou SQL Server a poskytovatele v paměti jsou zahrnuté v aktuálních verzích ASP.NET Core. Upgrade stávající aplikace ASP.NET Core na novější verzi EF Core, vždy upgradujte na verzi ASP.NET Core.

* Pokud je potřeba aktualizovat aplikaci, která používá databázi jiného poskytovatele, vždy vyhledejte aktualizace zprostředkovatele, který je kompatibilní s verzí EF Core, kterou chcete použít. Poskytovatelé databází pro předchozí verze například nejsou kompatibilní s verzí modulu runtime EF Core 2.0.

* Zprostředkovatelé třetí strany pro jádro EF Core obvykle není konečné verze opravy společně s runtime EF Core. Pokud chcete upgradovat aplikaci, která používá poskytovatele třetí strany na verzi opravy EF Core, budete muset přidat přímý odkaz na jednotlivé součásti modulu runtime EF Core, jako je například Microsoft.EntityFrameworkCore a Microsoft.EntityFrameworkCore.Relational.

* Pokud upgradujete existující aplikaci na nejnovější verzi EF Core, některé odkazy na balíčky starší EF Core může být potřeba odebrat ručně:

  * Databáze jako poskytovatel návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` už nejsou požadované nebo podporovaných v EF Core 2.0 a vyšší, ale neodeberou se automaticky při upgradu balíčky.

  * Nástroje rozhraní příkazového řádku .NET jsou zahrnuty v sadě .NET SDK od verze 2.1, tak odkaz na tento balíček je odebrat ze souboru projektu:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* Aplikací určených pro rozhraní .NET Framework možná bude nutné změny pro práci s knihovnami .NET Standard 2.0:

  * Upravte soubor projektu a ujistěte se, že ve skupině počáteční vlastnosti se zobrazí následující položky:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Projekty testů ujistěte, že je k dispozici následující položky:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
