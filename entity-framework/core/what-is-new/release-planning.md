---
title: Plánování vydání EF Core
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: c60040aa4acc33ba8b5a54b619539b275690f70e
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125394"
---
# <a name="release-planning-process"></a>Proces plánování vydaných verzí

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

## <a name="backlog"></a>Nevyřízená položka

[Milník nevyřízených položek](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) v našem sledování problémů obsahuje problémy, které očekáváme pracovat na Someday, nebo se domníváme, že by se mohl vypořádat někdo z komunity.
Zákazníci jsou připraveni na tyto problémy odeslat komentáře a hlasy.
Přispěvatelé, kteří chtějí pracovat na jakémkoli z těchto problémů, doporučujeme nejprve začít diskusi o tom, jak se k nim přiblíží.

Nikdy nezaručujeme, že budeme pracovat na všech daných funkcích v konkrétní verzi EF Core.
Stejně jako u všech softwarových projektů, priorit, plánů vydání a dostupných prostředků se můžou kdykoli změnit.
Pokud ale máme v úmyslu vyřešit problém v konkrétním časovém období, přiřadíme ho k milníku vydání namísto milníku nevyřízených položek.

Můžeme problém uzavřít, pokud ho neplánujeme na jejich řešení.
Můžeme ale znovu posuzovat problém, který jsme dřív uzavřeli, když získáme nové informace.
