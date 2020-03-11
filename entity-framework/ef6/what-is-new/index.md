---
title: Co je nového – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: 9daae787d0cec0ca536413e6263bb363ba76ff2c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419657"
---
# <a name="whats-new-in-ef6"></a>Co je nového v EF6

Důrazně doporučujeme, abyste používali nejnovější vydanou verzi Entity Framework, abyste měli jistotu, že získáte nejnovější funkce a nejvyšší stabilitu.
Uvědomujeme si ale, že možná budete muset použít předchozí verzi nebo můžete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.
Chcete-li nainstalovat konkrétní verze EF, přečtěte si téma [Get Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

Modul runtime EF 6.3.0 byl vydán do NuGet v září 2019. Hlavním cílem této verze bylo usnadnit migraci stávajících aplikací, které používají EF 6 až .NET Core 3,0. Komunita také přispěla k několika opravám chyb a vylepšením. Podrobnosti najdete v tématu problémy uzavřené v jednotlivých 6.3.0 [milníku](https://github.com/aspnet/EntityFramework6/milestones?state=closed) . Tady jsou některé z důležitějších verzí:

- Podpora pro .NET Core 3,0
  - Balíček EntityFramework se teď zaměřuje .NET Standard 2,1, kromě .NET Framework 4. x.
  - To znamená, že EF 6,3 je platforma pro víc platforem a je podporovaná na jiných operačních systémech než Windows, jako je Linux a macOS.
  - Příkazy migrace byly přepsány, aby vytvářely mimo proces a pracovaly s projekty ve stylu sady SDK.
- Podpora SQL Server HierarchyId.
- Vylepšená kompatibilita s Roslyn a NuGet PackageReference.
- Přidání nástroje `ef6.exe` pro povolování, přidávání, skriptování a instalaci migrace ze sestavení. Tato náhrada nahrazuje `migrate.exe`.

### <a name="ef-designer-support"></a>Podpora návrháře EF

V tuto chvíli není dostupná podpora pro použití návrháře EF přímo v projektech .NET Core nebo .NET Standard. 

Toto omezení můžete obejít tak, že přidáte soubor EDMX a generované třídy pro entity a DbContext jako propojené soubory do projektu .NET Core 3,0 .NET Standard nebo 2,1 ve stejném řešení.

Propojené soubory budou vypadat jako v souboru projektu:

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

Všimněte si, že soubor EDMX je propojený s akcí sestavení EntityDeploy. Jedná se o speciální úlohu MSBuild (teď zahrnutou v balíčku EF 6,3), která postará o přidání modelu EF do cílového sestavení jako vložené prostředky (nebo jejich zkopírování jako soubory ve výstupní složce v závislosti na nastavení zpracování artefaktu metadat v EDMX). Další podrobnosti o tom, jak toto nastavení získat, najdete v našem [příkladu pro edmx .NET Core Sample](https://aka.ms/EdmxDotNetCoreSample).

## <a name="past-releases"></a>Předchozí verze

Stránka [starší](past-releases.md) verze obsahuje archiv všech předchozích verzí EF a hlavní funkce, které byly představeny v každé vydané verzi.
