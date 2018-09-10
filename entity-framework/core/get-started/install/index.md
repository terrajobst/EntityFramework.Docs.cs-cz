---
title: Instalace Entiy Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250319"
---
# <a name="installing-entity-framework-core"></a>Instalace Entity Framework Core

## <a name="prerequisites"></a>Požadavky

* Pro vývoj aplikací určených pro .NET Core 2.1, nainstalujte [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core). Sada SDK musí být nainstalovaný, i v případě, že máte nejnovější verzi sady Visual Studio 2017.

* Použití sady Visual Studio pro vývoj aplikací určených pro .NET Core 2.1, nainstalujte Visual Studio 2017 verze 15.7 nebo novější.

* Entity Framework 2.1 v aplikacích ASP.NET Core, použití technologie ASP.NET Core 2.1. Aplikace, které používají starší verze technologie ASP.NET Core, musí být aktualizovány na verzi 2.1.

* Visual Studio 2015 můžete použít pro aplikace, které cílí .NET Framework 4.6.1 nebo novější. Ale potřebujete verzi balíčku nuget, který si je vědoma .NET Standard 2.0 a jeho rozhraní kompatibilní. Chcete-li získat, který v sadě Visual Studio 2015, [upgradovat klienta NuGet verzi 3.6.0](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Získat modul runtime Entity Framework Core

Přidání knihovny runtime EF Core do aplikace, nainstalujte balíček NuGet pro poskytovatele databáze, kterou chcete použít. Seznam podporovaných zprostředkovatelů a jejich názvy balíčků NuGet, naleznete v tématu [databáze poskytovatelé](../../providers/index.md).

Pokud chcete nainstalovat nebo aktualizovat balíčky NuGet, pomocí rozhraní příkazového řádku .NET Core, Visual Studio pomocí dialogové okno Správce balíčků nebo konzole Správce balíčků Visual Studio.

Pro aplikace ASP.NET Core 2.1 v paměti a poskytovatelé serveru SQL Server budou zahrnuty automaticky, takže není nutné je instalovat samostatně.

> [!TIP]  
> Pokud je potřeba aktualizovat aplikaci, která používá databázi jiného poskytovatele, vždy vyhledejte aktualizace zprostředkovatele, který je kompatibilní s verzí EF Core, kterou chcete použít. Poskytovatelé databází pro předchozí verze například nejsou kompatibilní s verzí 2.1 runtime EF Core.  

### <a name="net-core-cli"></a>.NET core CLI

Následující příkaz rozhraní příkazového řádku .NET Core nainstaluje nebo aktualizuje zprostředkovatele SQL Server:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Můžete určit konkrétní verzi `dotnet add package` příkaz a `-v` modifikátor. Například chcete-li nainstalovat balíčky EF Core 2.1.0, přidejte `-v 2.1.0` k příkazu.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Dialogové okno Správce balíčků NuGet sady Visual Studio

* V nabídce vyberte **Projekt > spravovat balíčky NuGet**

* Klikněte na **Procházet** nebo **aktualizace** kartu

* Chcete-li nainstalovat nebo aktualizovat zprostředkovatele SQL Server, vyberte `Microsoft.EntityFrameworkCore.SqlServer` balení a potvrďte.

Další informace najdete v tématu [dialogové okno Správce balíčků NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Konzola správce balíčků NuGet sady Visual Studio

* V nabídce vyberte **nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Pokud chcete nainstalovat zprostředkovatele SQL Server, spusťte následující příkaz v konzole Správce balíčků:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Chcete-li aktualizovat poskytovatele, použijte `Update-Package` příkazu.

* Chcete-li zadat konkrétní verzi, použijte `-Version` modifikátor. Například chcete-li nainstalovat balíčky EF Core 2.1.0, přidejte `-Version 2.1.0` k příkazům

Další informace najdete v tématu [Konzola správce balíčků](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Získat nástroje Entity Framework Core

Kromě knihovny runtime můžete nainstalovat nástroje, které můžete provádět některé úkoly související s EF Core ve vašem projektu v době návrhu. Můžete například vytvořit migrace, migrace použít a vytvořit model založený na existující databázi.

K dispozici jsou dvě sady nástrojů:
* .NET Core [nástroje rozhraní příkazového řádku (CLI)](../../miscellaneous/cli/dotnet.md) lze použít na Windows, Linux nebo macOS. Tyto příkazy začínají `dotnet ef`. 
* [Konzola správce balíčků nástroje](../../miscellaneous/cli/powershell.md) spustit ve Visual Studio 2017 pro Windows. Tyto příkazy spustit s operací, například `Add-Migration`, `Update-Database`.

Přestože lze použít `dotnet ef` příkazy z konzoly Správce balíčků, je vhodnější použít nástroje konzoly Správce balíčků při používání sady Visual Studio:
* Automaticky fungují s aktuální projekt vybraný v konzole Správce balíčků bez nutnosti ručně přepnout adresáře.  
* Automaticky otevřou soubory generované záznamem pro příkazy v sadě Visual Studio po dokončení příkazu.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>Získání nástrojů rozhraní příkazového řádku

`dotnet ef` Příkazy jsou součástí sady .NET Core SDK, ale chcete-li povolit příkazy budete muset nainstalovat `Microsoft.EntityFrameworkCore.Design` balíčku:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Pro aplikace ASP.NET Core 2.1 pro tento balíček je přiložena automaticky.

Jak je vysvětleno výše v [požadavky](#prerequisites), budete také muset nainstalovat .NET Core 2.1 SDK.

> [!IMPORTANT]      
> Vždy používejte verzi sady nástrojů, který odpovídá hlavní verzi balíčky runtime.

### <a name="get-the-package-manager-console-tools"></a>Získat nástroje konzoly Správce balíčků

Chcete-li získat Konzola správce balíčků nástroje na EF Core, nainstalovat `Microsoft.EntityFrameworkCore.Tools` balíčku:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

Pro aplikace ASP.NET Core 2.1 pro tento balíček je přiložena automaticky.

## <a name="upgrading-to-ef-core-21"></a>Upgrade na EF Core 2.1

Pokud upgradujete existující aplikaci do EF Core 2.1, některé odkazy na balíčky starší EF Core může být potřeba odebrat ručně:

* Databáze jako poskytovatel návrhu balíčky `Microsoft.EntityFrameworkCore.SqlServer.Design` jsou již požadované nebo nejsou podporovány v EF Core 2.1, ale odebrána nebudou automaticky při upgradu balíčky.

* Nástroje rozhraní příkazového řádku .NET jsou teď součástí sady .NET SDK, můžete z odebrat odkaz na tento balíček *.csproj* souboru:

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

U aplikací, které se zaměřují na rozhraní .NET Framework a byly vytvořeny v dřívějších verzích sady Visual Studio Ujistěte se, že jsou kompatibilní s knihovnami .NET Standard 2.0:

  * Upravte soubor projektu a ujistěte se, že ve skupině počáteční vlastnosti se zobrazí následující položky:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Projekty testů ujistěte, že je k dispozici následující položky:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
