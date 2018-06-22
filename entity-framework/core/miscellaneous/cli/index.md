---
title: Reference k příkazovému řádku - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769403"
---
<a name="entity-framework-core-tools"></a>Základní nástroje Entity Framework
===========================
Nástroje Entity Framework Core vám pomohou při vývoji aplikace EF jádra. Používají se primárně chcete vygenerovat typy DbContext a entity podle zpětnou analýzu schématu databáze a spravovat migrace.

[EF základní balíček správce konzoly (pomocí PMC) nástroje] [ 1] poskytnout vynikající prostředí v sadě Visual Studio. Je spustit pomocí NuGet [Konzola správce balíčků][2]. Tyto nástroje pro práci s projekty rozhraní .NET Framework a .NET Core.

[Nástroje příkazového řádku .NET Core EF] [ 3] jsou rozšíření pro [nástrojů rozhraní příkazového řádku (CLI) .NET Core] [ 4] , které jsou napříč platformami a můžete spustit mimo Visual Studio. Tyto nástroje vyžadují rozhraní .NET Core SDK projektu (s `Sdk="Microsoft.NET.Sdk"` nebo podobné v souboru projektu).

Oba nástroje zveřejňují stejné funkce. Pokud vyvíjíte v sadě Visual Studio, doporučujeme pomocí nástroje pomocí PMC vzhledem k tomu, že poskytují lepší integrace.

<a name="frameworks"></a>rozhraní
----------
Nástroje pro podporu projektech zacílených na rozhraní .NET Framework nebo .NET Core.

Pokud chcete používat knihovny tříd, pak zvažte, pokud je to možné použití knihovny tříd .NET Core nebo rozhraní .NET Framework. Tato akce způsobí alespoň problémy s nástrojů .NET. Pokud chcete místo toho použijte rozhraní .NET standardní knihovny tříd, budete muset použít spouštěný projekt s cílem rozhraní .NET Framework nebo .NET Core tak, aby nástroje Cílová platforma conrete, do kterého je možné načíst knihovny tříd. Tento projekt po spuštění může být fiktivní projektu žádné skutečné kódem – je pouze potřebných k poskytování cíl pro nástrojů.

Pokud cílem vašeho projektu je jiný framework (například Universal Windows nebo Xamarin), pak bude muset vytvořit samostatný .NET standardní knihovny tříd. V takovém případě podle pokynů uvedených výše můžete také vytvořit spouštěný projekt, který lze nástroji.

<a name="startup-and-target-projects"></a>Spuštění a cíl projekty
---------------------------
Při každém vyvolání příkazu, se skládá dva projekty: cílový projekt a projekt po spuštění.

Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat).

Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu.

Cílový projekt a projekt po spuštění může být stejné.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
