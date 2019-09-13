---
title: Instalace Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 62194d1db4efcdaed53ca0e14f160315f8e3cf03
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921761"
---
# <a name="installing-entity-framework-core"></a>Instalace Entity Framework Core

## <a name="prerequisites"></a>Požadavky

* EF Core je knihovna [.NET Standard 2,0](/dotnet/standard/net-standard) . Proto EF Core vyžaduje implementaci rozhraní .NET, která podporuje spuštění .NET Standard 2,0. Na EF Core můžou být taky odkazovány jinými knihovnami .NET Standard 2,0. 

* Můžete například použít EF Core pro vývoj aplikací, které cílí na .NET Core. Sestavování aplikací .NET Core vyžaduje [.NET Core SDK](https://dotnet.microsoft.com/download). Volitelně můžete také použít vývojové prostředí, jako je Visual Studio, Visual Studio pro Mac nebo Visual Studio Code. Další informace najdete v [Začínáme pomocí .NET Core](/dotnet/core/get-started).

* EF Core můžete použít k vývoji aplikací, které cílí na .NET Framework 4.6.1 nebo novější ve Windows pomocí sady Visual Studio. Doporučuje se nejnovější verze sady [Visual Studio](https://visualstudio.microsoft.com/vs) . Pokud chcete použít starší verzi, jako je například Visual Studio 2015, je třeba upgradovat [klienta NuGet na verzi 3.6.0](https://www.nuget.org/downloads) , aby fungoval s knihovnami .NET Standard 2,0.

* EF Core lze spustit na jiných implementacích rozhraní .NET, jako je [Xamarin](https://dotnet.microsoft.com/apps/xamarin) a .NET Native. V praxi ale tyto implementace mají omezení za běhu, což může mít vliv na to, jak dobře EF Core fungují ve vaší aplikaci. Další informace najdete v tématu [implementace rozhraní .NET podporované nástrojem EF Core](xref:core/platforms/index).

* Nakonec různí poskytovatelé databáze můžou vyžadovat konkrétní verze databázového stroje, implementace .NET nebo operační systémy. Ujistěte se, že je k dispozici [poskytovatel databáze EF Core](xref:core/providers/index) , který podporuje správné prostředí pro vaši aplikaci.

## <a name="get-the-entity-framework-core-runtime"></a>Získat modul runtime Entity Framework Core

Chcete-li přidat EF Core do aplikace, nainstalujte balíček NuGet pro poskytovatele databáze, který chcete použít.

Pokud vytváříte aplikaci ASP.NET Core, nemusíte instalovat poskytovatele v paměti a SQL Server. Tito poskytovatelé jsou zahrnutí v aktuálních verzích ASP.NET Core společně s modulem EF Core Runtime.  

Chcete-li nainstalovat nebo aktualizovat balíčky NuGet, můžete použít rozhraní příkazového řádku (CLI) .NET Core, dialogové okno Správce balíčků sady Visual Studio nebo konzolu správce sady Visual Studio.

### <a name="net-core-cli"></a>Rozhraní příkazového řádku .NET Core

* Chcete-li nainstalovat nebo aktualizovat poskytovatele služby EF Core SQL Server, použijte následující příkaz .NET Core CLI z příkazového řádku operačního systému:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Konkrétní verzi můžete v `dotnet add package` příkazu označit `-v` pomocí modifikátoru. Například pro instalaci balíčků EF Core 2.2.0 přidejte `-v 2.2.0` k příkazu.

Další informace najdete v tématu [nástroje rozhraní příkazového řádku .NET (CLI)](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Dialogové okno Správce balíčků NuGet sady Visual Studio

* V nabídce sady Visual Studio vyberte **projekt > spravovat balíčky NuGet**

* Klikněte na kartu **Procházet** nebo **aktualizace** .

* Pokud chcete nainstalovat nebo aktualizovat poskytovatele SQL Server, vyberte `Microsoft.EntityFrameworkCore.SqlServer` balíček a potvrďte ho.

Další informace najdete v [dialogovém okně Správce balíčků NuGet](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Konzola správce balíčků NuGet sady Visual Studio

* V nabídce aplikace Visual Studio vyberte **nástroje > správce balíčků NuGet > konzola správce balíčků** .

* Chcete-li nainstalovat poskytovatele SQL Server, spusťte následující příkaz v konzole správce balíčků:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Chcete-li aktualizovat poskytovatele, použijte `Update-Package` příkaz.

* K určení konkrétní verze použijte `-Version` modifikátor. Například pro instalaci balíčků EF Core 2.2.0 přidejte `-Version 2.2.0` k příkazům

Další informace najdete v tématu [Konzola správce balíčků](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Získat nástroje pro Entity Framework Core

Můžete nainstalovat nástroje pro provádění úloh souvisejících s EF Core v projektu, jako je vytvoření a použití migrace databáze nebo vytvoření modelu EF Core na základě existující databáze.

K dispozici jsou dvě sady nástrojů:

* [Nástroje rozhraní příkazového řádku .NET Core (CLI)](xref:core/miscellaneous/cli/dotnet) se dají použít v systému Windows, Linux nebo MacOS. Tyto příkazy začínají `dotnet ef`na. 

* [Nástroje konzoly Správce balíčků (PMC)](xref:core/miscellaneous/cli/powershell) se spouštějí v aplikaci Visual Studio ve Windows. Tyto příkazy začínají příkazem, například `Add-Migration`. `Update-Database`

I když můžete použít `dotnet ef` taky příkazy z konzoly Správce balíčků, doporučuje se použít nástroje konzoly Správce balíčků, když používáte Visual Studio:

* Automaticky pracují s aktuálním projektem vybraným v PMC v aplikaci Visual Studio, aniž by museli ručně přepínat adresáře.  

* Po dokončení příkazu automaticky otevřou soubory vygenerované příkazy v aplikaci Visual Studio.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Získat nástroje pro .NET Core CLI

.NET Core CLI nástroje vyžadují .NET Core SDK, zmíněné dříve v části [požadavky](#prerequisites).

Příkazy jsou obsaženy v aktuálních verzích .NET Core SDK, ale pokud chcete povolit příkazy v konkrétním projektu, je nutné `Microsoft.EntityFrameworkCore.Design` nainstalovat balíček: `dotnet ef`

``` Console 
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Pro ASP.NET Core aplikace je tento balíček zahrnutý automaticky.

> [!IMPORTANT]      
> Vždy používejte verzi balíčku nástroje, která odpovídá hlavní verzi běhových balíčků.

### <a name="get-the-package-manager-console-tools"></a>Získat nástroje konzoly Správce balíčků

Chcete-li získat nástroje konzoly Správce balíčků pro EF Core, nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček. Například ze sady Visual Studio:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

Pro ASP.NET Core aplikace je tento balíček zahrnutý automaticky.

## <a name="upgrading-to-the-latest-ef-core"></a>Upgrade na nejnovější EF Core

* Kdykoli vydáte novou verzi EF Core, vydáváme také novou verzi zprostředkovatelů, které jsou součástí projektu EF Core, jako je Microsoft. EntityFrameworkCore. SqlServer, Microsoft. EntityFrameworkCore. sqlite a Microsoft. EntityFrameworkCore. inMemory. Můžete pouze upgradovat na novou verzi zprostředkovatele, abyste získali všechna vylepšení. 

* EF Core společně s SQL Server a poskytovateli v paměti jsou zahrnuti v aktuálních verzích ASP.NET Core. Chcete-li upgradovat existující aplikaci ASP.NET Core na novější verzi EF Core, vždy upgradujte verzi ASP.NET Core.

* Pokud potřebujete aktualizovat aplikaci, která používá poskytovatele databáze třetí strany, vždy vyhledejte aktualizaci zprostředkovatele, která je kompatibilní s verzí EF Core, kterou chcete použít. Například poskytovatelé databáze pro předchozí verze nejsou kompatibilní s verzí 2,0 EF Core Runtime.

* Poskytovatelé třetích stran pro EF Core obvykle nevydávají verze oprav společně s modulem EF Core Runtime. Chcete-li upgradovat aplikaci, která používá poskytovatele třetí strany, na verzi opravy EF Core, může být nutné přidat přímý odkaz na jednotlivé součásti modulu runtime aplikace EF Core, jako je například Microsoft. EntityFrameworkCore a Microsoft. EntityFrameworkCore. relační.

* Pokud upgradujete existující aplikaci na nejnovější verzi EF Core, může být nutné ručně odebrat některé odkazy na starší EF Core balíčky:

  * Balíčky pro dobu návrhu, `Microsoft.EntityFrameworkCore.SqlServer.Design` jako jsou již nejsou vyžadovány nebo podporovány v EF Core 2,0 a novějších, ale nejsou automaticky odebrány při upgradu ostatních balíčků.

  * Nástroje rozhraní příkazového řádku .NET jsou součástí sady .NET SDK od verze 2,1, takže odkaz na tento balíček lze odebrat ze souboru projektu:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* Aplikace, které cílí na .NET Framework, můžou potřebovat změny pro práci s knihovnami .NET Standard 2,0:

  * Upravte soubor projektu a ujistěte se, že se v počáteční skupině vlastností zobrazuje následující položka:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * V případě testovacích projektů se ujistěte také, že je k dispozici následující položka:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
