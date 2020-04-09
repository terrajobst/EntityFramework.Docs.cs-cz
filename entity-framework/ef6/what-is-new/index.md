---
title: Co je nového - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136140"
---
# <a name="whats-new-in-ef6"></a>Co je nového v EF6

Důrazně doporučujeme použít nejnovější vydanou verzi entity frameworku, abyste zajistili, že získáte nejnovější funkce a nejvyšší stabilitu.
Uvědomujeme si však, že možná budete muset použít předchozí verzi nebo že budete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.
Chcete-li nainstalovat konkrétní verze EF, naleznete v tématu [Získat entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-640"></a>EF 6.4.0

Běhový čas EF 6.4.0 byl vydán nugetv prosinci 2019. Primárním cílem EF 6.4 je vyleštit funkce a scénáře, které byly dodány v EF 6.3. Podívejte se na [seznam důležitých oprav](https://github.com/dotnet/ef6/milestone/14?closed=1) na Githubu.

## <a name="ef-630"></a>EF 6.3.0

Běhový čas EF 6.3.0 byl vydán nugetv září 2019. Hlavním cílem této verze bylo usnadnit migraci existujících aplikací, které používají EF 6 na .NET Core 3.0. Komunita také přispěla několika opravami chyb a vylepšeními. Podrobnosti najdete v problémech uzavřených v každém [milníku](https://github.com/aspnet/EntityFramework6/milestones?state=closed) 6.3.0. Zde jsou některé z nejvýznamnějších z nich:

- Podpora pro rozhraní .NET Core 3.0
  - Balíček EntityFramework nyní cílí na standard .NET Standard 2.1 kromě rozhraní .NET Framework 4.x.
  - To znamená, že EF 6.3 je multiplatformní a podporuje v jiných operačních systémech kromě Windows, jako je Linux a macOS.
  - Příkazy migrace byly přepsány tak, aby byly spuštěny mimo proces a pracovaly s projekty ve stylu sady SDK.
- Podpora pro SQL Server HierarchyId.
- Vylepšená kompatibilita s Roslyn a NuGet PackageReference.
- Byl `ef6.exe` přidán nástroj pro povolení, přidání, skriptování a použití migrace z sestavení. Tím se `migrate.exe`nahradí .

### <a name="ef-designer-support"></a>Podpora návrháře EF

V současné době neexistuje žádná podpora pro použití návrháře EF přímo na projektech .NET Core nebo .NET Standard nebo v projektu rozhraní SDK .NET Framework. 

Toto omezení můžete obejít přidáním souboru EDMX a generovaných tříd pro entity a DbContext jako propojené soubory do projektu .NET Core 3.0 nebo .NET Standard 2.1 ve stejném řešení.

Propojené soubory budou v souboru projektu vypadat takto:

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

Všimněte si, že soubor EDMX je propojen s entityDeploy sestavení akce. Toto je speciální úloha MSBuild (nyní součástí balíčku EF 6.3), která se stará o přidání modelu EF do cílového sestavení jako vložené prostředky (nebo kopírování jako soubory ve výstupní složce, v závislosti na nastavení zpracování artefaktů metadat v EDMX). Další podrobnosti o tom, jak toto nastavení nastavit, naleznete v našem [vzorku EDMX .NET Core](https://aka.ms/EdmxDotNetCoreSample).

Upozornění: ujistěte se, že starý styl (tj. non-SDK-style) .NET Framework projekt definující "skutečný" .edmx soubor je _před_ projektem definující odkaz uvnitř souboru .sln. V opačném případě při otevření souboru .edmx v návrháři se zobrazí chybová zpráva "Entity Framework není k dispozici v cílovém rámci aktuálně určené pro projekt. Můžete změnit cílovou architekturu projektu nebo upravit model v XmlEditor".

## <a name="past-releases"></a>Předchozí verze

Stránka [Minulé verze](past-releases.md) obsahuje archiv všech předchozích verzí EF a hlavních funkcí, které byly zavedeny v každé verzi.
