---
title: "Reference k příkazovému řádku - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
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

Pokud je cílem vašeho projektu je jiný framework (například Universal Windows nebo Xamarin), doporučujeme vytvořit samostatnou .NET Standard projektu a cílení na mezi jeden z podporovaných architektur.

Cíl mezi .NET Core, například klikněte pravým tlačítkem na projekt a vyberte **upravit \*.csproj**. Aktualizace `TargetFramework` vlastnost následujícím způsobem. (Všimněte si, název vlastnosti stane množném čísle.)

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

Pokud používáte .NET Standard knihovny tříd, nemusíte mezi cíl, pokud je cílem vašeho projektu spuštění rozhraní .NET Framework nebo .NET Core.

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
