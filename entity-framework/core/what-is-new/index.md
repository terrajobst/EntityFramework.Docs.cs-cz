---
title: Co je nového – EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 2ca4915fca515b4bdbfeb77bc7b02f15ce1704b6
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197723"
---
# <a name="whats-new-in-ef-core"></a>Co je nového v EF Core

## <a name="recent-releases"></a>Poslední verze

- **EF Core 3,0** (nejnovější stabilní verze) 
  - [Nové funkce](xref:core/what-is-new/ef-core-3.0/index) 
  - Zásadní [změny](xref:core/what-is-new/ef-core-3.0/breaking-changes) , o kterých byste měli vědět při upgradu
- [EF Core 2.2](xref:core/what-is-new/ef-core-2.2)
- [EF Core 2,1](xref:core/what-is-new/ef-core-2.1) (nejnovější dlouhodobá verze podpory)

## <a name="product-roadmap"></a>Plán produktu

> [!IMPORTANT]
> Počítejte s tím, že sady funkcí a plány budoucích verzí se vždycky mění, a i když se pokusíme tuto stránku uchovávat v aktuálním stavu, nemusí se po celou dobu projevit naše nejnovější plány.

### <a name="future-releases"></a>Budoucí verze

- **EF Core 3,1**  
  - V části aktivní vývoj
  - Bude obsahovat [vylepšení malého výkonu, kvality a stability](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.1.0+sort%3Areactions-desc) .
  - Plánováno jako vydaná Dlouhodobá podpora (LTS)
  - Aktuálně plánováno pro prosinec 2019
- **EF Core "vNext"**   
  - Další hlavní verze EF Core pro dodávání společně s .NET 5
  - Plánování této verze ještě není spuštěné a neoznámily se žádné funkce.  

### <a name="schedule"></a>Plán

Plán vydaných verzí pro EF Core je synchronizovaný s plánem vydaných [verzí .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="backlog"></a>Nevyřízená položka

[Milník nevyřízených položek](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) v našem sledování problémů obsahuje problémy, které očekáváme pracovat na Someday, nebo se domníváme, že by se mohl vypořádat někdo z komunity.
Zákazníci jsou připraveni na tyto problémy odeslat komentáře a hlasy.
Přispěvatelé, kteří chtějí pracovat na jakémkoli z těchto problémů, doporučujeme nejprve začít diskusi o tom, jak se k nim přiblíží.

Nikdy nezaručujeme, že budeme pracovat na všech daných funkcích v konkrétní verzi EF Core.
Stejně jako u všech softwarových projektů, priorit, plánů vydání a dostupných prostředků se můžou kdykoli změnit.
Pokud ale máme v úmyslu vyřešit problém v konkrétním časovém období, přiřadíme ho k milníku vydání namísto milníku nevyřízených položek.
V rámci našeho [procesu plánování](#release-planning-process)vydaných verzí se rutinním způsobem přesouváme problémy mezi nevyřízenými položkami a milníky vydání.

Můžeme problém uzavřít, pokud ho neplánujeme na jejich řešení.
Můžeme ale znovu posuzovat problém, který jsme dřív uzavřeli, když získáme nové informace.

### <a name="release-planning-process"></a>Proces plánování vydaných verzí

Často se dostanete na to, jak si vybíráme konkrétní funkce, které se dají použít k přechodu do konkrétní verze.
Naše nevyřízené položky si nemusí automaticky překládat do plánů vydání.
Přítomnost funkce v EF6 také automaticky neznamená, že je nutné implementovat funkci v EF Core.

Je obtížné podrobně pořídit celý proces, který se používá k naplánování vydání verze.
Většina z nich se zabývá konkrétními funkcemi, příležitostmi a prioritami a samotný proces se také vyvíjí v každé verzi.
Můžeme ale shrnout běžné otázky, které se snažíme zodpovědět při rozhodování o tom, co se má v následujících případech pracovat:

1. **Kolik vývojářů se domníváme, že tuto funkci budou používat a kolik lepších funkcí bude mít aplikace nebo prostředí?** K zodpovězení této otázky shromažďujeme názory z mnoha zdrojů – komentáře a hlasy na problémech jsou jedním z těchto zdrojů.

2. **Jaká řešení můžou lidé využít, pokud tuto funkci ještě neimplementujete?** Mnoho vývojářů může například namapovat spojovací tabulku tak, aby fungovala s nedostatečným množstvím nativních podpor. Zjevně ne všichni vývojáři to dělají, ale mnoho z nich se může v našem rozhodnutí počítat jako faktor.

3. **Implementuje Tato funkce architekturu EF Core tak, aby se nám přesunula blíž k implementaci dalších funkcí?** Snažíme se upřednostnit funkce, které fungují jako stavební bloky pro další funkce. Například entity kontejneru objektů a dat můžou přispět k přesunu na více než mnoho podpory a konstruktory entit mají povolené naše opožděné načítání.

4. **Je funkce bodem rozšiřitelnosti?** Doporučujeme upřednostnit body rozšiřitelnosti oproti normálním funkcím, protože umožňují vývojářům připojovat vlastní chování a kompenzovat všechny chybějící funkce.

5. **Jaká je součinnost funkce při použití v kombinaci s jinými produkty?** Dáváme jim funkce, které umožňují nebo významně vylepšit možnosti použití EF Core s jinými produkty, jako je například .NET Core, nejnovější verze sady Visual Studio, Microsoft Azure a tak dále.

6. **Jaké jsou dovednosti, které mají lidé k dispozici pro práci na funkci, a jak tyto prostředky nejlépe využít?** Každý člen týmu EF a naši Přispěvatelé komunity mají různé úrovně zkušeností v různých oblastech, takže je nutné odpovídajícím způsobem naplánovat. I v případě, že chceme mít "veškerou práci na palubě", která bude fungovat na konkrétní funkci, jako jsou překlady GroupBy, nebo m:n, což by nebylo praktické.

Jak bylo zmíněno dříve, proces se vyvíjí v každé verzi.
V budoucnu se pokusíme přidat další příležitosti pro členy komunity, které budou poskytovat vstupy do našich plánů vydávání.
Například chceme, aby bylo snazší zkontrolovat návrhy konceptů funkcí a samotného plánu vydání.