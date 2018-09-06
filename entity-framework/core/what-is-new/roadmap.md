---
title: Entity Framework Core – plán
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: fd9086c9911cdb0890117d44c2787780aad9a7cb
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821358"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core – plán

> [!IMPORTANT]
> Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.

## <a name="last-release-ef-core-21"></a>Poslední verze: EF Core 2.1

Stabilní verze EF Core 2.1 vydané 30. května 2018. Můžete najít další informace o této verzi v [Novinky v EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

## <a name="future-releases"></a>Budoucí verze

### <a name="ef-core-22"></a>EF Core 2.2

Tato verze bude zahrnovat řadu oprav chyb a relativně malý počet nových funkcí. Podrobnosti o této verzi jsou součástí [EF Core 2.2 plán oznámení](https://github.com/aspnet/Announcements/issues/308). 

### <a name="ef-core-30"></a>EF Core 3.0

I když jsme dokončili [vydání proces plánování](#release-planning-process) pro další verzi po 2.2, aktuálně plánujeme mít hlavní verze algined .NET Core 3.0 a ASP.NET 3.0. 

Můžete použít [tento dotaz v našich sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) zobrazíte pracovní položky přiřazené tenatively této budoucí verzi.

## <a name="schedule"></a>Plán

Plán pro jádro EF Core je synchronizace s [.NET Core plán](https://github.com/dotnet/core/blob/master/roadmap.md) a [ASP.NET Core plán](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Nevyřízená položka

Používáme [nevyřízených položek milník](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) v našich sledování problémů udržovat podrobný seznam problémů a funkce. Zákazníci mohou přidávat komentáře a nahoru hlas, tyto.

Jsme zpravidla zůstat otevřeno, že to bude přiměřeně Očekáváme, že budeme pracovat na v určitém okamžiku nebo někdo z komunity může řešení, ale který neznamená cílem vyřešit v určitém časovém rámci, dokud jsme přiřadit konkrétní verze jako součást naše problémy [verze procesu plánování](#release-planning-process).

Pokud jsme nemáte v plánu někdy implementovat funkci, jsme tento problém pravděpodobně ukončena. Problém, který jsme uzavřeno, můžete později přezkoumány, pokud jsme získat nové informace o tom.

Všechny možnosti, které ale nutné dodat, nemáme dostatek informací o budoucnosti budete moci Řekněme, že tuto funkci X vyřeší čas nebo vydané verzi Y. Jako všech softwarových projektů můžete kdykoli změnit priority, plány verze a dostupných prostředků.

## <a name="release-planning-process"></a>Plánování proces vydávání verzí

Často získáme dotazy týkající se jak můžeme zvolit konkrétní funkce, které chcete přejít na konkrétní verzi. Backlogu a určitě neprojevuje automaticky uvádění. Přítomnost funkce EF6 také nemusí znamenat automaticky, že funkce by měl být implementována v EF Core.

Je obtížné podrobnosti tady celého procesu, kterou budeme postupovat podle plánování vydání, částečně je to proto diskutuje spoustu konkrétní funkce, příležitostí a priority a částečně, protože samotný proces obvykle vyvíjí s každá vydaná verze. Je však poměrně snadné slouží ke shrnutí běžné otázky, na které jsme pokuste odpovědět při rozhodování o tom, co pro práci na další:

1. **Myslíme si, jak mnoho vývojářů použije funkci a kolik lépe zajistí jejich aplikace/prostředí?** Jsme agregovat zpětné vazby z mnoha zdrojů do tohoto – komentáře a hlasy problémů je jedním z těchto zdrojů.

2. **Co jsou lidé alternativní řešení můžete použít, pokud ještě neimplementujeme tuto funkci?** Například mnoho vývojářů budou moct mapování vazební tabulka za účelem obejít chybějící nativní podpora many-to-many. Samozřejmě ne všichni vývojáři, můžete to provést, ale mnoho můžete, a to je faktorem, který počítá.

3. **Implementaci této funkce vyvíjí architektura EF Core tak, aby nám ještě víc přibližuje přesune k implementaci dalšími funkcemi?** Často Představujeme upřednostnit funkce, které slouží jako stavební bloky pro další funkce – například rozdělení tabulky, který bylo provedeno pro vlastní typy nám pomáhá přesunout TPT podpory.

4. **Je funkce bod rozšiřitelnosti?** Často Představujeme upřednostnit bodů rozšiřitelnosti, protože umožňují vývojářům snadno integrovat do své vlastní chování a některé chybějící funkce získat tímto způsobem. Plánujeme provést některé tuto část berte jako počáteční opožděné načtení práce.

5. **Co je synergii funkce, když použijete v kombinaci s jinými produkty?** Často Představujeme upřednostnit funkce, které EF Core, který se má použít s jinými produkty nebo k výraznému zlepšení používáním další produkty Microsoftu, jako je .NET Core, nejnovější verze sady Visual Studio, Microsoft Azure, atd.

6. **Jaké jsou možnosti lidem pracovat na funkce a jak nejlépe můžete využít tyto prostředky k dispozici?** Každý člen týmu EF a dokonce i naše komunitní přispěvatelé mají různé úrovně prostředí v různých oblastech a musíme podle toho naplánujte. I v případě, že chceme mít "Brigáda na palubě" práci na konkrétní funkce, jako jsou překlady GroupBy nebo many-to-many, který by praktické.

Jak jsme zmínili, tento proces vyvíjí na každá vydaná verze, a rádi v budoucnu přidat více příležitostí pro členy komunity vývojářů k poskytnutí vstupy do uvádění, například tak, což usnadňuje revizemi navrhované funkce a vydané verze naplánovat samotný.
