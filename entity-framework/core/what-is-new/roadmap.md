---
title: Přehled základní Entity Framework
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: e23f5d7b1ff95bead310fa8e618a88c161a4e10c
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754441"
---
# <a name="entity-framework-core-roadmap"></a>Přehled základní Entity Framework

> [!IMPORTANT]
> Upozorňujeme, že sady funkcí a plány budoucích verzí se vždy mohou změnit a i když se pokusíte průběžně aktualizovat tuto stránku, nemusí odrážet naše nejnovější plány vůbec časů.

Stabilní verze 2.1 základní EF byla vydána 30 může 2018. Můžete najít další informace o tomto vydání v [co je nového v EF základní 2.1](xref:core/what-is-new/ef-core-2.1).

Jsme nedokončili [verze procesu plánování](#release-planning-process) na další vydání po 2.1.

## <a name="schedule"></a>Plán

Plán pro základní EF je synchronizovaná s [.NET Core plán](https://github.com/dotnet/core/blob/master/roadmap.md) a [ASP.NET Core plán](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Nevyřízená položka

Používáme [nevyřízených položek milník](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) v našem sledovací modul problém udržovat podrobný seznam problémů a funkce. Zákazníci mohou komentovat a až hlas, tyto.

Jsme zpravidla zůstat otevřeno, že to bude přiměřeně Očekáváme, že budeme pracovat na v určitém okamžiku nebo někdo z komunity může řešení, ale který neznamená cílem vyřešit v určitém časovém rámci, dokud jsme přiřadit konkrétní verze jako součást naše problémy [verze procesu plánování](#release-planning-process).

Pokud jsme nemáte v plánu někdy implementovat funkci, jsme se zavře pravděpodobně problém. Pokud jsme získat nové informace o tom můžete znovu problém, který jsme uzavřený posouzeno později.

Všechny možnosti, které ale nutné dodat, nemáme dostatek informací o budoucnosti mohly k tomu, že tato funkce X vyřeší čas a verze Y. Stejně jako všechny projekty softwaru můžete kdykoli změnit priority, verze plány a dostupné prostředky.

## <a name="release-planning-process"></a>Verze procesu plánování

Nemůžeme získat často dotazy o tom, jak jsme zvolte konkrétní funkce Přejít na konkrétní verzi. Naše nevyřízených položek určitě neprojevuje automaticky verze plány. Přítomnost funkce v EF6 také neznamená automaticky, musí být implementován v EF základní funkci.

Je obtížné podrobností zde celý proces, který jsme při plánování verze, částečně, protože spoustu se diskutuje specifické funkce, příležitostí a priority, a částečně, protože samotný proces obvykle zpracovaní všechny verze. Je však je poměrně snadné ho shrnout běžné otázky, které jsme zkuste zodpovědět při rozhodování, kterým chcete pracovat další:

1. **Myslíme si, kolik vývojářům používat funkci a kolik lépe budou své aplikace prostředí?** Jsme agregovat zpětné vazby z mnoha zdrojů do této – komentáře a problémy hlasů je jedním z těchto zdrojů.

2. **Co jsou osoby alternativní řešení můžete použít, pokud zatím jsme nemáte implementací této funkce?** Celá řada vývojářů jsou například mohou mapovat tabulku spojení, aby bylo možné obejít nedostatečná nativní podpora m: n. Samozřejmě ne všechny vývojáře můžete to provést, ale mnoho můžete a toto je faktorem, který udává.

3. **Implementací této funkce momentální Architektura jádra EF tak, že nám blíže přesune k implementaci dalších funkcí?** Jsme zpravidla upřednostnit funkce, které fungují jako stavební bloky u jiných funkcí – například rozdělení tabulky, které bylo provedeno pro vlastní typy nám pomáhá přesuňte směrem TPT podpory.

4. **Je funkce bod rozšiřitelnosti?** Jsme zpravidla upřednostnit body rozšiřitelnosti, protože umožňují vývojářům snadno připojit v jejich vlastní chování a získat některé z funkcí, které chybí tímto způsobem. Jsme plánování provést některé z jako počáteční na opožděného načítání fungují.

5. **Co je součinnost funkci, pokud se používá v kombinaci s jinými produkty?** Jsme zpravidla upřednostnit funkce, které umožňují EF jádra, který se má použít s jinými produkty nebo na výrazné zlepšení uživatelského rozhraní pomocí jiné produkty, jako je například .NET Core, nejnovější verzi sady Visual Studio, Microsoft Azure, atd.

6. **Jaké jsou možnosti osob, které jsou k dispozici pro práci na funkce a jak nejlépe využít tyto prostředky?** Každý člen týmu EF a i naše přispěvatelé komunit mají různé úrovně prostředí v různých oblastech a musíme podle toho naplánovat. I v případě, že jsme chtěli tak, aby měl "podlaží všechny rukou" práce na konkrétní funkci jako překlady GroupBy nebo m: n, který nebude praktické.

Jak je uvedeno nahoře, tento proces zpracovaní v každé verzi a v budoucnu jsme chcete přidat další možnosti pro členy komunity vývojářů zadejte vstupy do plánu uvádění, například tím, že zkontrolujte navrhované koncepty funkce a nástroje samotný plán verze.
