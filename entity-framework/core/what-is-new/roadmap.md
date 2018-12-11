---
title: Entity Framework Core – plán
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: f18de8e8cb4fbe81bb2f983a00c9dd2f46be6073
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182017"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core – plán

> [!IMPORTANT]
> Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.

### <a name="ef-core-30"></a>EF Core 3.0

EF Core 2.2 letos uvedli na trh našim hlavním hlavním cílem je nyní EF Core 3.0, které budou zarovnané s .NET Core 3.0 a uvolní 3.0 technologie ASP.NET.

Jsme nebyly dokončeny žádné nové funkce ještě, proto [EF Core 3.0 ve verzi Preview 1 balíčky publikována do Galerie NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1) prosince 2018 pouze obsahovat [opravy chyb, menší vylepšení a změny, které jsme provedli v Příprava 3.0 pracovní](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed).

Ve skutečnosti ještě nutné upřesnit naše [release plánování](#release-planning-process) 3.0, ujistěte se, že jsme mají správnou sadu funkcí, které můžete dokončit v přiděleném čase.
Jsme podělí o informace, jak jsme získali další jasné, ale tady jsou některá základní témata a funkce jsme itend pracovat na:

- **Vylepšení LINQ ([#12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))**: LINQ umožňuje psát dotazy databáze, aniž byste museli opustit váš jazyk podle vlastní volby, využití výhod bohaté zadejte informace, které pomůžou technologie IntelliSense a kontrola typu v době kompilace.
  Ale LINQ také umožňuje psát neomezený počet složité dotazy a, který byl vždy velkým problémem pro zprostředkovatele LINQ.
  V prvních několika verzích EF Core jsme vyřešit, v části tím, jaké části dotazu může být přeloženy na SQL a tím, že zbývající části dotazu ke spuštění v paměti na straně klienta.
  Toto spuštění na straně klienta může být žádoucí v některých situacích ale v mnoha případech může způsobit neefektivní dotazy, které nebyly identifikovány, dokud je aplikace nasazená do produkčního prostředí.
  V EF Core 3.0 plánujeme změnit velký fungování naše implementace LINQ, a jak můžeme otestovat.
  Cíle jsou k němu robustnější (třeba, aby se zabránilo přerušení dotazů v aktualizací), bude moct přeložit dalších výrazů správně do databáze SQL, vygenerujte efektivní dotazy ve více případech a zabránit v přechodu nezjištěné neefektivní dotazy.

- **Podpora služby cosmos DB ([#8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))**: Pracujeme na poskytovatele služby Cosmos DB pro jádro EF Core a umožňuje vývojářům znáte model programování na EF snadno cílit na jako databáze aplikace služby Azure Cosmos DB.
  Cílem je, že některé z výhod Cosmos DB, jako jsou globální distribuce "always on" dostupnost, elastické škálovatelnosti a nízké latenci, ještě více přístupné pro vývojáře na platformě .NET.
  Zprostředkovatel umožní většina EF Core funkcí, jako je sledování automatických změn, LINQ a převody hodnot, s využitím rozhraní SQL API ve službě Cosmos DB. Začali jsme snaha před EF Core 2.2 a [jsme se rozhodli některé verze zprostředkovatele k dispozici ve verzi preview](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  Nový plán má pokračovat ve vývoji poskytovatele spolu s EF Core 3.0.   

- **C#Podpora 8.0 ([#12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))**: Chceme našim zákazníkům umožní využít některé z [nové funkce v C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) , jako jsou asynchronní datové proudy (včetně await pro každou) a typy s možnou hodnotou Null odkazů pomocí EF Core.

- **Reverse engineering zobrazení databáze do typy dotazů ([#1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))**: V EF Core 2.1 přidali jsme podporu pro typy dotazů, které mohou představovat data, která mohou číst z databáze, ale nejde aktualizovat.
  Typy dotazů jsou zobrazení skvěle hodí k mapování databáze, tak v EF Core 3.0, rádi bychom k automatizaci vytváření typů dotazu pro zobrazení databáze.

- **Vlastnosti kontejneru objektů a dat entit ([#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) a [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))**: Tato funkce je o povolení entity, které ukládají data v indexované vlastnosti namísto regulární vlastností a také o bude možné použít instance stejné třídy .NET (potenciálně něco jednoduchého jako `Dictionary<string, object>`) k reprezentaci entit různých typů ve stejném modelu EF Core.
  Tato funkce je odrazový můstek k podpoře many-to-many relací bez připojení k entitě, což je jedna z nejžádanějších vylepšení pro jádro EF Core.

- **EF 6.3 v rozhraní .NET Core ([EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271))**: Chápeme, že mnoho existujících aplikací pomocí předchozí verze EF a že přenesení do EF Core jenom, abyste mohli využívat výhod .NET Core může někdy vyžadovat značné úsilí.
  Z tohoto důvodu jsme se přizpůsobení další verze EF 6 a spustit na .NET Core 3.0.
  To usnadňuje přenos stávající aplikace s minimálními změnami provádíme.
  Existuje budou představovat určitá omezení (například bude vyžadovat nové zprostředkovatele, nepovolí prostorových podporu s SQL serverem) a neexistují žádné nové plánované funkce pro EF 6.

Do té doby můžete použít [tento dotaz v našich sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) zobrazíte pracovní položky přiřazené nezávazně 3.0.

## <a name="schedule"></a>Plán

Plán pro jádro EF Core je synchronizace s [.NET Core plán](https://github.com/dotnet/core/blob/master/roadmap.md) a [ASP.NET Core plán](https://github.com/aspnet/Home/wiki/Roadmap).

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
