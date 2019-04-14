---
title: Entity Framework Core – plán
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: e9d1a707c643036fa15b1e0e46730eadefd65518
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562465"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core – plán

> [!IMPORTANT]
> Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.

## <a name="ef-core-30"></a>EF Core 3.0

EF Core 2.2 letos uvedli na trh našim hlavním cílem hlavní je nyní EF Core 3.0.
V tématu [Novinky v EF Core 3.0](xref:core/what-is-new/ef-core-3.0/index) informace o plánované [nové funkce](xref:core/what-is-new/ef-core-3.0/features) a úmyslné [rozbíjející změny v](xref:core/what-is-new/ef-core-3.0/breaking-changes) zahrnuta ve vydané verzi.

## <a name="schedule"></a>Plán

Plán vydání pro jádro EF Core je synchronizace s [plán vydané verze .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

## <a name="backlog"></a>Nevyřízená položka

[Nevyřízených položek milník](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) v našich problém obsahuje sledování problémů, které Očekáváme, že pro práci na někdy nebo které by se mohl někdo z komunity tohoto problému.
Zákazníci si mohou odeslat komentáře a hlasy na tyto problémy.
Chcete-li pracovat na některý z těchto problémů přispěvatelům doporučujeme nejprve zahájit diskusi o tom, jak je přístup.

To se nikdy záruku, že budeme pracovat na jakékoli dané funkce v určité verze EF Core.
Jako všech softwarových projektů můžete kdykoli změnit priority, plány verze a dostupných prostředků.
Ale pokud chceme vyřešit problém v konkrétním časovém rámci, přiřadíme je k vydání milníků místo milník nevyřízených položek.
Problémy se pustíme pravidelně mezi nevyřízenými položkami a vydání milníků jako součást naše [vydání proces plánování](#release-planning-process).

Pokud jsme nemáte v úmyslu někdy adres jsme budete pravděpodobně uzavřete problém.
Ale jsme zvažte problém, který dříve zavřena, pokud jsme získat nové informace o něm.

## <a name="release-planning-process"></a>Plánování proces vydávání verzí

Často získáme dotazy týkající se jak můžeme zvolit konkrétní funkce, které chcete přejít na konkrétní verzi.
Backlogu a určitě není převede automaticky na uvádění.
Přítomnost funkce EF6 také neznamená automaticky, že funkce by měl být implementována v EF Core.

Je obtížné podrobností celého procesu budeme postupovat podle plán vydané verze.
Většina článku je diskuze o konkrétní součásti, příležitostí a priority a samotný proces také vyvíjí s každá vydaná verze.
Však můžete Shrneme informace z běžných otázek, na které jsme pokuste odpovědět při rozhodování o tom, co pro práci na další:

1. **Myslíme si, jak mnoho vývojářů použije funkci a kolik lépe zajistí jejich aplikace/prostředí?** Na to, můžeme shromažďovat zpětnou vazbu z mnoha zdrojů, komentáře a hlasy problémů je jedním z těchto zdrojů.

2. **Co jsou lidé alternativní řešení můžete použít, pokud ještě neimplementujeme tuto funkci?** Mnoho vývojářů například můžete namapovat tabulku spojení obejít chybějící nativní podpora many-to-many. Samozřejmě ne všichni vývojáři, chcete to provést, ale mnoho můžete a, které se počítá jako faktor při naše rozhodnutí.

3. **Implementaci této funkce vyvíjí architektura EF Core tak, aby nám ještě víc přibližuje přesune k implementaci dalšími funkcemi?** Často Představujeme upřednostnit funkce, které slouží jako stavební bloky pro další funkce. Například vlastnost kontejner objektů a dat entit může pomoct nám přesunout many-to-many podpory a konstruktory entity povolena podpora naše opožděné načtení.

4. **Je funkce bod rozšiřitelnosti?** Často Představujeme upřednostnil před normální funkce bodů rozšiřitelnosti, protože umožňují vývojářům integrovat svoje vlastní chování a kompenzovat všechny chybějící funkce.

5. **Co je synergii funkce, když použijete v kombinaci s jinými produkty?** Jsme upřednostnit funkce, které umožňují nebo výrazně vylepšit pomocí EF Core s jinými produkty, jako je .NET Core, nejnovější verze sady Visual Studio, Microsoft Azure, atd.

6. **Co jsou dovednosti lidem pracovat na funkce a jak nejlépe můžete využít tyto prostředky k dispozici?** Každý člen týmu EF a naše přispěvatelé komunit má různé úrovně zkušeností v různých oblastech, proto musíme podle toho naplánujte. I v případě, že chceme mít "Brigáda na palubě" práci na konkrétní funkce, jako jsou překlady GroupBy nebo many-to-many, který by praktické.

Jak jsme zmínili, vyvíjí procesu na každá vydaná verze.
Zkusíme v budoucnu přidat více příležitostí pro členy komunity k poskytování vstupy do naše plány na vydání.
Chtěli bychom například usnadňují zkontrolujte návrhy návrh funkcí a samotný plán vydání.
