---
title: Instalace jádra entity – jádro EF
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 6575b1ac028f8b67b49ca7f4e49d6f19500be98f
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136175"
---
# <a name="installing-entity-framework-core"></a>Instalace jádra entity frameworku

## <a name="prerequisites"></a>Požadavky

* EF Core je knihovna [.NET Standard 2.0.](/dotnet/standard/net-standard) Takže EF Core vyžaduje implementaci .NET, která podporuje .NET Standard 2.0 ke spuštění. Ef Core lze také odkazovat na jiné knihovny .NET Standard 2.0.

* Například můžete použít EF Core k vývoji aplikací, které cílí na .NET Core. Vytváření aplikací .NET Core vyžaduje [.NET Core SDK](https://dotnet.microsoft.com/download). Volitelně můžete také použít vývojové prostředí, jako je [Visual Studio](https://visualstudio.microsoft.com/vs), Visual Studio [pro Mac](https://visualstudio.microsoft.com/vs/mac)nebo Visual [Studio Code](https://code.visualstudio.com). Další informace naleznete v souboru [Začínáme s rozhraním .NET Core](/dotnet/core/get-started).

* EF Core můžete použít k vývoji aplikací v systému Windows pomocí sady Visual Studio. Doporučujeme nejnovější verzi [sady Visual Studio.](https://visualstudio.microsoft.com/vs)

* EF Core lze spustit na jiných implementacích .NET, jako je [Xamarin](https://dotnet.microsoft.com/apps/xamarin) a .NET Native. Ale v praxi tyto implementace mají omezení za běhu, které mohou ovlivnit, jak dobře EF Core funguje na vaší aplikaci. Další informace naleznete [v tématu implementace rozhraní .NET podporované EF Core](xref:core/platforms/index).

* Nakonec mohou různí poskytovatelé databází vyžadovat konkrétní verze databázového stroje, implementace rozhraní .NET nebo operační systémy. Ujistěte se, že je k dispozici [zprostředkovatel databáze EF Core,](xref:core/providers/index) který podporuje správné prostředí pro vaši aplikaci.

## <a name="get-the-entity-framework-core-runtime"></a>Získání runtime jádra entity frameworku

Chcete-li přidat EF Core do aplikace, nainstalujte balíček NuGet pro poskytovatele databáze, který chcete použít.

Pokud vytváříte ASP.NET základní aplikace, není nutné instalovat v paměti a SQL Server zprostředkovatelů. Tito poskytovatelé jsou zahrnuti v aktuálních verzích ASP.NET Core, spolu s runtime EF Core.  

Chcete-li nainstalovat nebo aktualizovat balíčky NuGet, můžete použít rozhraní příkazového řádku .NET Core (CLI), dialogové okno Správce balíčků sady Visual Studio nebo konzolu Správce balíčků sady Visual Studio.

### <a name="net-core-cli"></a>Rozhraní příkazového řádku .NET Core

* K instalaci nebo aktualizaci zprostředkovatele serveru SQL Server EF Core použijte následující příkaz příkazu .NET Core CLI z příkazového řádku operačního systému:

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Pomocí `-v` modifikátoru `dotnet add package` můžete v příkazu určit určitou verzi. Chcete-li například nainstalovat balíčky EF Core `-v 2.2.0` 2.2.0, připojte příkaz.

For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Dialogové okno Správce balíčků aplikace Visual Studio NuGet

* V nabídce Visual Studio vyberte **Project > Manage NuGet Packages .**

* Klikněte na kartu **Procházet** nebo **Aktualizace.**

* Chcete-li nainstalovat nebo aktualizovat zprostředkovatele serveru SQL Server, vyberte `Microsoft.EntityFrameworkCore.SqlServer` balíček a potvrďte.

Další informace naleznete [v tématu NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Konzola Správce balíčků aplikace Visual Studio NuGet

* V nabídce Visual Studio vyberte **Nástroje > Správce balíčků NuGet > konzoli správce balíčků.**

* Chcete-li nainstalovat zprostředkovatele serveru SQL Server, spusťte v konzole Správce balíčků následující příkaz:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Chcete-li aktualizovat zprostředkovatele, použijte `Update-Package` příkaz.

* Chcete-li určit konkrétní `-Version` verzi, použijte modifikátor. Chcete-li například nainstalovat balíčky EF Core `-Version 2.2.0` 2.2.0, připojujte k příkazům

Další informace naleznete v [tématu Package Manager Console](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Získejte základní nástroje frameworku entit

Můžete nainstalovat nástroje pro provádění úloh souvisejících s EF Core v projektu, jako je vytváření a použití migrace databáze nebo vytvoření modelu EF Core na základě existující databáze.

K dispozici jsou dvě sady nástrojů:

* Nástroje [rozhraní příkazového řádku .NET Core (CLI)](xref:core/miscellaneous/cli/dotnet) lze použít ve Windows, Linuxu nebo macOS. Tyto příkazy `dotnet ef`začínají na .

* [Nástroje konzoly Správce balíčků (PMC)](xref:core/miscellaneous/cli/powershell) jsou spuštěny v sadě Visual Studio v systému Windows. Tyto příkazy začínají slovesem, například `Add-Migration`. `Update-Database`

I když můžete `dotnet ef` příkazy použít také z konzoly Správce balíčků, doporučujeme používat nástroje konzoly Správce balíčků, když používáte Visual Studio:

* Automaticky pracují s aktuálním projektem vybraným v PMC v sadě Visual Studio, aniž by bylo nutné ručně přepínat adresáře.  

* Automaticky otevírají soubory generované příkazy v sadě Visual Studio po dokončení příkazu.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Získání nástrojů rozhraní .NET Core CLI

Nástroje rozhraní CLI .NET Core vyžadují sadku .NET Core SDK, která je uvedena dříve v [části Požadavky](#prerequisites).

Příkazy `dotnet ef` jsou zahrnuty v aktuálních verzích sady .NET Core SDK, ale chcete-li `Microsoft.EntityFrameworkCore.Design` povolit příkazy v konkrétním projektu, je nutné nainstalovat balíček:

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]
> Vždy používejte verzi balíčku nástroje, který odpovídá hlavní verzi balíčků runtime.

### <a name="get-the-package-manager-console-tools"></a>Získání nástrojů konzoly Správce balíčků

Chcete-li získat nástroje konzoly Správce `Microsoft.EntityFrameworkCore.Tools` balíčků pro EF Core, nainstalujte balíček. Například z Visual Studia:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Pro ASP.NET aplikace Core je tento balíček součástí automaticky.

## <a name="upgrading-to-the-latest-ef-core"></a>Upgrade na nejnovější ef jádro

* Pokaždé, když vydáme novou verzi EF Core, vydáme také novou verzi zprostředkovatelů, kteří jsou součástí projektu EF Core, jako jsou Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityCore.Sqlite a Microsoft.EntityFrameworkCore.InMemory. Můžete jen upgradovat na novou verzi poskytovatele získat všechna vylepšení.

* EF Core, spolu s SQL Server a zprostředkovatelé v paměti jsou zahrnuty v aktuálních verzích ASP.NET Core. Chcete-li upgradovat existující ASP.NET základní aplikace na novější verzi EF Core, vždy upgradujte verzi ASP.NET Core.

* Pokud potřebujete aktualizovat aplikaci, která používá zprostředkovatele databáze jiného výrobce, vždy zkontrolujte aktualizaci zprostředkovatele, která je kompatibilní s verzí EF Core, kterou chcete použít. Například zprostředkovatelé databáze pro předchozí verze nejsou kompatibilní s verzí 2.0 runtime EF Core.

* Poskytovatelé třetích stran pro EF Core obvykle nevydávají verze oprav vedle runtime EF Core. Chcete-li upgradovat aplikaci, která používá poskytovatele třetí strany na verzi opravy EF Core, budete muset přidat přímý odkaz na jednotlivé komponenty ef core runtime, jako jsou Microsoft.EntityFrameworkCore a Microsoft.EntityFrameworkCore.Relational.

* Pokud upgradujete existující aplikaci na nejnovější verzi EF Core, může být nutné ručně odebrat některé odkazy na starší balíčky EF Core:

  * Balíčky návrhu zprostředkovatele `Microsoft.EntityFrameworkCore.SqlServer.Design` databáze, jako jsou již nejsou požadovány nebo podporovány z EF Core 2.0 a novější, ale nejsou automaticky odebrány při upgradu ostatní balíčky.

  * Nástroje rozhraní .NET CLI jsou zahrnuty v sdk .NET od verze 2.1, takže odkaz na tento balíček lze odebrat ze souboru projektu:

    ``` xml
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```
