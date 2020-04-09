---
title: Nástroje & rozšíření - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634244"
---
# <a name="ef-core-tools--extensions"></a>Základní nástroje EF & rozšíření

Tyto nástroje a rozšíření poskytují další funkce pro entity framework core 2.1 a novější.

> [!IMPORTANT]  
> Rozšíření jsou sestaveny z různých zdrojů a nejsou udržovány jako součást projektu Core entity framework. Při zvažování rozšíření třetí strany nezapomeňte vyhodnotit jeho kvalitu, licencování, kompatibilitu, podporu atd., abyste se ujistili, že splňuje vaše požadavky. Zejména rozšíření vytvořené pro starší verzi EF Core může potřebovat aktualizaci, než bude fungovat s nejnovějšími verzemi.

## <a name="tools"></a>nástroje

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro je řešení modelování entit s podporou entity frameworku a entity framework core. Umožňuje snadno definovat model entity a mapovat jej do databáze, nejprve pomocí databáze nebo modelu, takže můžete začít psát dotazy hned. Pro EF jádro: 2.

[Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Developer entity je výkonný návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access a LINQ to SQL. Podporuje navrhování ef core modely vizuálně, pomocí modelu první nebo první přístupy databáze a Generování kódu Jazyka C# nebo Visual Basic. Pro EF jádro: 2.

[Web](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a>nHydratační orm pro entity framework

ORM, který vytváří silného typu, rozšiřitelné třídy pro entity framework. Generovaný kód je jádro entity frameworku. Není v tom žádný rozdíl. Toto není náhrada za EF nebo vlastní ORM. Jedná se o vizuální vrstvu modelování, která umožňuje týmu spravovat komplexní schémata databáze. Funguje dobře se softwarem SCM, jako je Git, který umožňuje přístup k vašemu modelu pro více uživatelů s minimálními konflikty. Instalační program sleduje změny modelu a vytváří skripty upgradu. Pro EF core: 3.

[Web Githubu](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a>Základní elektrické nástroje EF

EF Core Power Tools je rozšíření sady Visual Studio, které zpřístupňuje různé úlohy návrhu EF Core v jednoduchém uživatelském rozhraní. Zahrnuje reverzní inženýrství DbContext a entity třídy z existujících databází a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), správa migrace databáze a vizualizace modelu. Pro EF Core: 2, 3.

[GitHub wiki](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Vizuální editor Entity Framework

Entity Framework Visual Editor je rozšíření Visual Studio, které přidává návrháře ORM pro vizuální návrh EF 6 a EF Core tříd. Kód je generován pomocí šablon T4, takže jej lze přizpůsobit tak, aby vyhovoval všem potřebám. Podporuje dědičnost, jednosměrné a obousměrné přidružení, výčty a schopnost barevně kódovat vaše třídy a přidávat textové bloky, které vysvětlují potenciálně tajemné části návrhu. Pro EF jádro: 2.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory je modul generování uživatelského rozhraní pro .NET Core, který může automatizovat generování tříd DbContext, entit, konfigurací mapování a tříd úložiště z databáze serveru SQL Server. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft entity framework core generátor

Entity Framework Core Generator (efg) je nástroj .NET Core CLI, který může `dotnet ef dbcontext scaffold`generovat ef core modely z existující databáze, podobně jako , ale také podporuje bezpečné [regeneraci](https://efg.loresoft.com/en/latest/regeneration/) kódu prostřednictvím nahrazení oblasti nebo analýzou mapovacích souborů. Tento nástroj podporuje generování modelů zobrazení, ověřování a kódu mapovače objektů. Pro EF jádro: 2.

[Dokumentace kurzu](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Rozšíření

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Knihovna pluginů, která umožňuje automatické zaznamenávání změn dat provedených EF Core do tabulky historie. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Rozšíření, které umožňuje ukládání výsledků dotazů EF Core do mezipaměti druhé úrovně, takže následné spuštění stejných dotazů se může vyhnout přístupu k databázi a načíst data přímo z mezipaměti. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a>Geco (Geco)

Geco (Generator Console) je jednoduchý generátor kódu založený na projektu konzoly, který běží na .NET Core a používá interpolované řetězce C# pro generování kódu. Geco obsahuje generátor reverzního modelu pro EF Core s podporou pluralizace, singularizace a upravitelných šablon. Poskytuje také generátor skriptů seed, skript ový běh a čistič databáze. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore.Scaffolding.Řídítka 

Umožňuje přizpůsobení tříd zpětně analyzovány z existující databáze pomocí entity framework core toolchain se šablonami řídítek. Pro EF Core: 2, 3.

[Úložiště GitHub](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore 

NeinLinq rozšiřuje linq zprostředkovatele, jako je entity Framework povolit opakované použití funkcí, přepisování dotazů a vytváření dynamických dotazů pomocí přeložitelné predikáty a voliči. Pro EF Core: 2, 3.

[Úložiště GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Plugin pro Microsoft.EntityFrameworkCore pro podporu úložiště, jednotky pracovních vzorů a více databází s podporou distribuovaných transakcí. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Ef Core rozšíření pro hromadné operace (Vložit, Aktualizovat, Odstranit). Pro EF Core: 2, 3.

[Úložiště GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Přidá pluralizaci návrhu. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Obnovení atributu [Index] (s rozšířením pro vytváření modelů). Pro EF Core: 2, 3.

[Úložiště GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Poskytuje obálku kolem zprostředkovatele databáze základního ef v paměti. Díky tomu se chová spíše jako relační poskytovatel. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implementace časové podpory. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>EfCoreTemporalTabulka

Pomocí zavedených rozšiřujících metod můžete snadno provádět `AsTemporalAll()`časové `AsTemporalAsOf(date)` `AsTemporalFrom(startDate, endDate)`dotazy `AsTemporalBetween(startDate, endDate)` `AsTemporalContained(startDate, endDate)`na svou oblíbenou databázi: , , , , . Pro EF core: 3.

[Úložiště GitHub](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a>EFCore.TimeTraveler

Povolit plně doporučené entity Framework Core dotazy proti [SQL Server časové historie](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) pomocí KÓDU EF core, entity a mapování, které jste již definovali.  Cestujte časem zabalením kódu do aplikace `using (TemporalQuery.AsOf(targetDateTime)) {...}`. Pro EF core: 3.

[Úložiště GitHub](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore.TemporalTables

Knihovna rozšíření pro entity framework core, který umožňuje vývojářům, kteří používají SQL Server snadno používat časové tabulky. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Vysoce výkonná mezipaměť dotazů druhé úrovně. Pro EF jádro: 2.

[Úložiště GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Rozšiřuje dbcontext s funkcemi, jako jsou: Zahrnout filtr, auditování, ukládání do mezipaměti, Dotaz budoucí, Dávkové odstranění, Dávková aktualizace a další. Pro EF Core: 2, 3.

[Úložiště GitHub na webu](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Rozšíření rámce entit

Rozšiřuje dbcontext s vysoce výkonné hromadné operace: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkDelete, BulkMerge a další. Pro EF Core: 2, 3.

[Web](https://entityframework-extensions.net/)

### <a name="expressionify"></a>Výrazy

Přidejte podporu pro volání metod rozšíření v linq lambdas. Pro EF Core: 3.1

[Úložiště GitHub](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a>XLinq

Technologie language integrated query (LINQ) pro relační databáze. Umožňuje používat c# k zápisu dotazů silného typu. Pro EF Core: 3.1

- Plná podpora Jazyka C# pro vytváření dotazů: více příkazů uvnitř lambda, proměnné, funkce atd.
- Žádná sémantická mezera s SQL. XLinq deklaruje `SELECT` `FROM`příkazy SQL (jako , , `WHERE`) jako metody c# první třídy, kombinující známou syntaxi s intellisense, bezpečnost typů a refaktoring.

V důsledku toho SQL se stává jen "další" knihovna třídy vystavuje své rozhraní API místně, doslova *"Jazyk integrovaný SQL"*.

[Web](http://xlinq.live/)
