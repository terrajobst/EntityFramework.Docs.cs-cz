---
title: EF Core vydání a plánování
author: ajcvickers
ms.date: 01/14/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 8d74c24021fd62c5c5d944eaf3973b344fdb1e9c
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124402"
---
# <a name="ef-core-releases-and-planning"></a>EF Core vydání a plánování

## <a name="stable-releases"></a>Stabilní verze

| Vydaná verze | Cílová architektura | Platnost do | Odkazy
|:--------|------------------|-----------------|------
| [EF Core 3,1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.1) | .NET Standard 2.0 | 3\. prosince 2022 (LTS) | [Ohlášen](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| [EF Core 3,0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.1) | .NET Standard 2.1 | 3\. března 2020 | [Oznámení](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [přerušující změny](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2,2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Konec platnosti 23. prosince 2019 | [Ohlášen](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21. srpna 2021 (LTS) | [Ohlášen](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2,0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Platnost od 1. října 2018 | [Ohlášen](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1,1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1,3 | Vypršení od června 27 2019 | [Ohlášen](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1,0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1,3 | Vypršení od června 27 2019 | [Ohlášen](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Informace o konkrétních platformách podporovaných jednotlivými EF Core vydání najdete v tématu [podporované platformy](../platforms/index.md) .

Informace o vypršení platnosti podpory a verzích LTS (dlouhodobá podpora) najdete v tématu [zásady podpory rozhraní .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) .

## <a name="guidance-on-updating-to-new-releases"></a>Pokyny k aktualizaci nových verzí

* Podporované verze jsou opravené z důvodu zabezpečení a dalších kritických chyb. Vždy používejte nejnovější opravu dané verze. Například pro EF Core 2,1 použijte 2.1.14.
* Aktualizace hlavních verzí (například z EF Core 2 na EF Core 3) často přerušují změny. Důkladné testování se doporučuje při aktualizaci v různých hlavních verzích. Odkazy na předchozí změny najdete na stránce s pokyny pro práci s zásadními změnami.
* Aktualizace podverze obvykle neobsahují závažné změny. Důkladné testování však stále doporučujeme, protože nové funkce mohou zavádět regrese.

## <a name="ef-core-50"></a>EF Core 5,0

EF Core vydání se zarovnají s [plánem dodávek .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md). Další plánovaná stabilní verze je **EF Core 5,0**naplánovaná na 2020. listopadu.

[Plán vysoké úrovně pro EF Core 5,0](ef-core-5.0/plan.md) byl vytvořen pomocí podrobného [procesu plánování](release-planning.md)vydaných verzí.

Váš názor na plánování je důležitý. Nejlepším způsobem, jak určit důležitost problému, je hlasovat (palec) pro daný problém na GitHubu. Tato data se pak budou předávat do procesu plánování pro další vydání.

### <a name="get-it-now"></a>Získejte ho hned teď!

Balíčky EF Core 5,0 jsou **nyní k dispozici** jako [každodenní sestavení](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md). 

Použití denních buildů představuje skvělý způsob, jak najít problémy a co nejdříve poskytnout zpětnou vazbu. Dřív získáme takovou zpětnou vazbu, což je pravděpodobnější, že se bude jednat ještě před dalším oficiálním vydáním. Pracujeme na tom, abychom každodenní sestavení v dobrém tvaru zachovali spuštěním více než 55 000 testů na jednu platformu pro každé sestavení.

Balíčky Preview se budou do NuGet dodávat později v roce.
