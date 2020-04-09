---
title: EF Core vydání a plánování
description: Aktuální ef core verze a podrobnosti o plánu/plánování pro budoucí verze
author: ajcvickers
ms.date: 03/03/2020
uid: core/what-is-new/index
ms.openlocfilehash: 89687417685f291b44dcb250c96c5c9fa57da80f
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634263"
---
# <a name="ef-core-releases-and-planning"></a>EF Core vydání a plánování

## <a name="stable-releases"></a>Stabilní verze

| Vydat | Cílový rámec | Platnost do | Odkazy
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.3) | .NET Standard 2.0 | 3. prosince 2022 (LTS) | [Oznámení](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standard 2.1 | Platnost března 3, 2020 | [Změny porušení](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [oznámení](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Platnost prosinec 23, 2019 | [Oznámení](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21. srpna 2021 (LTS) | [Oznámení](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Platnost října 1, 2018 | [Oznámení](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Platnost platnost června 27 2019 | [Oznámení](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Jádro 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Platnost platnost června 27 2019 | [Oznámení](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Informace o konkrétních platformách podporovaných jednotlivými verzemi EF Core najdete na [podporovaných platformách.](../platforms/index.md)

Informace o vydáních vypršení platnosti podpory a vydání dlouhodobé podpory (LTS) naleznete v [zásadách podpory rozhraní .NET.](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)

## <a name="guidance-on-updating-to-new-releases"></a>Pokyny pro aktualizaci na nové verze

* Podporované verze jsou opraveny z důvodu zabezpečení a dalších kritických chyb. Vždy používejte nejnovější náplast dané verze. Například pro EF Core 2.1 použijte 2.1.14.
* Aktualizace hlavních verzí (například z EF Core 2 na EF Core 3) mají často narušující změny. Při aktualizaci v hlavních verzích se doporučuje důkladné testování. Pomocí výše uvedených odkazů narušujících změn můžete poradit s nejnovějšími změnami.
* Dílčí verze aktualizace obvykle neobsahují nejnovější změny. Důkladné testování se však stále doporučuje, protože nové funkce mohou zavést regrese.

## <a name="release-planning-and-schedules"></a>Plánování vydání a plány

Ef Core verze zarovnání s [plánem expedice .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

Patch zprávy obvykle loď měsíčně, ale mají dlouhou dobu vedení.
Pracujeme na tom, abychom to zlepšili.

Další informace o tom, jak se rozhodneme, co máme v každé verzi doručovat, najdete v [procesu plánování vydání.](release-planning.md)
Obvykle neprovádíme podrobné plánování pro další vydání než další hlavní nebo menší verze.

## <a name="ef-core-50"></a>EF Core 5.0

Další plánovaná stabilní verze je **EF Core 5.0**, která je naplánována na listopad 2020.

Plán [vysoké úrovně pro EF Core 5.0](ef-core-5.0/plan.md) byl vytvořen v návaznosti na zdokumentovaný proces plánování [vydání](release-planning.md).

Vaše zpětná vazba na plánování je důležitá.
Nejlepší způsob, jak označit důležitost problému, je hlasovat 👍(palec nahoru) pro tuto otázku na GitHubu.
Tato data se pak budou připojuji k procesu plánování pro další verzi.

### <a name="get-it-now"></a>Získejte to teď!

Balíčky EF Core 5.0 jsou **nyní k dispozici** jako

* [Denní sestavení](https://github.com/dotnet/aspnetcore/blob/master/docs/DailyBuilds.md)
  * Všechny nejnovější funkce a opravy chyb. Obecně velmi stabilní; 57 000+ testů spustit proti každé sestavení.
* [Ukázky na NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
  * Zaostává za denní sestavení, ale jsou testovány pro práci s odpovídajícími ASP.NET core a .NET Core náhledy.

Použití náhledů nebo denních sestavení je skvělý způsob, jak najít problémy a poskytnout zpětnou vazbu co nejdříve.
Čím dříve získáme takovou zpětnou vazbu, tím je pravděpodobnější, že bude žalovatelná před dalším oficiálním vydáním.
