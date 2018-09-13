---
title: Odkaz na příkazový řádek – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490360"
---
<a name="entity-framework-core-tools"></a>Entity Framework Core Tools
===========================
Entity Framework Core Tools umožňují při vývoji aplikace EF Core. Primárně slouží pro generování uživatelského rozhraní a typy kontext databáze a entity ve zpětné analýze schématu databáze a ke správě migrace.

[EF Core Package Manageru konzoly (PMC) nástroje] [ 1] poskytovat vynikající prostředí v sadě Visual Studio. Spustit pomocí Nugetu [Konzola správce balíčků][2]. Tyto nástroje pracují s projekty rozhraní .NET Framework a .NET Core.

[Nástroje příkazového řádku .NET Core EF] [ 3] jsou rozšíření [nástroje rozhraní příkazového řádku (CLI) pro .NET Core] [ 4] , které jsou různé platformy a spustit mimo sadu Visual Studio. Tyto nástroje vyžadují projektu .NET Core SDK (s `Sdk="Microsoft.NET.Sdk"` nebo podobně jako v souboru projektu).

Oba nástroje zveřejňují stejné funkce. Pokud vyvíjíte v sadě Visual Studio, doporučujeme pomocí PMC nástrojů, které poskytují více integrované prostředí.

<a name="frameworks"></a>Rozhraní
----------
Nástroje podporují projekty cílené na rozhraní .NET Framework nebo .NET Core.

Pokud chcete použít knihovnu tříd, zvažte použití knihovny tříd .NET Core nebo .NET Framework, pokud je to možné. Výsledkem bude nejméně problémy s nástroji .NET. Pokud chcete místo toho použít knihovnu tříd .NET Standard, je potřeba použít spouštěný projekt, který cílí rozhraní .NET Framework nebo .NET Core tak, aby měl cílovou platformu conrete, do kterého je možné načíst knihovny tříd nástroje. Tento projekt po spuštění může být fiktivní projektu bez skutečné kódu – je pouze potřebných k poskytování cíl pro nástroje.

Pokud váš projekt cílí na jinou framework (například Universal Windows nebo Xamarin), je potřeba vytvořit samostatné knihovny tříd .NET Standard. V takovém případě postupujte podle pokynů výše vytvořte také spouštěný projekt, který je možné pomocí nástroje.

<a name="startup-and-target-projects"></a>Spuštění a cílové projekty
---------------------------
Při každém vyvolání příkazu se využívá řada dva projekty: cílový projekt a projekt po spuštění.

Cílový projekt je tam, kde se přidají všechny soubory (nebo v některých případech odebrána).

Projekt po spuštění je emulován nástroji při spuštění kódu projektu.

Cílový projekt a projekt po spuštění může být stejný.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
