---
title: Rozšíření & nástrojů – EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: bab725afffe1fbf9f8c0abeef58579ac9dc842d2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502079"
---
# <a name="ef-core-tools--extensions"></a>Rozšíření & nástrojů pro EF Core

Tyto nástroje a rozšíření poskytují další funkce pro Entity Framework Core 2,1 a novější.

> [!IMPORTANT]  
> Rozšíření jsou sestavená z nejrůznějších zdrojů, která nejsou udržována jako součást projektu Entity Framework Core. Při zvažování rozšíření jiného výrobce nezapomeňte vyhodnotit jeho kvalitu, licencování, kompatibilitu, podporu atd., abyste zajistili, že splňuje vaše požadavky. Konkrétně rozšíření sestavené pro starší verzi EF Core může vyžadovat aktualizaci před tím, než bude fungovat s nejnovějšími verzemi.

## <a name="tools"></a>Nástroje

### <a name="llblgen-pro"></a>LLBLGen pro

LLBLGen pro je řešení modelování entit s podporou Entity Framework a Entity Framework Core. Umožňuje snadno definovat model entity a namapovat ho do vaší databáze, použít nejprve databázi nebo model, abyste mohli začít psát dotazy hned. Pro EF Core: 2.

[Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Vývojář entit Devart

Vývojář entit je výkonný Návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik přístup k datům a LINQ to SQL. Podporuje navrhování EF Corech modelů vizuálně, použití prvního nebo databáze prvního přístupového C# modelu nebo generování kódu Visual Basic. Pro EF Core: 2.

[Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core nástroje Power Tools

EF Core Power Tools je rozšíření sady Visual Studio, které zpřístupňuje různé úlohy EF Core v době návrhu v jednoduchém uživatelském rozhraní. Zahrnuje zpětnou přípravu tříd DbContext a entit z existujících databází a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), správu migrací databáze a vizualizace modelů. Pro EF Core: 2, 3.

[Wiki GitHubu](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework vizuální Editor

Entity Framework Visual Editor je rozšíření sady Visual Studio, které přidává návrháře ORM pro vizuální návrh z EF 6 a EF Core třídy. Kód se generuje pomocí šablon T4, takže ho můžete přizpůsobit podle potřeby. Podporuje dědičnost, jednosměrná a obousměrná přidružení, výčty a možnost barevně kódovat vaše třídy a přidat textové bloky, které vysvětlují potenciálně učit složité části návrhu. Pro EF Core: 2.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory je modul pro generování uživatelského rozhraní pro .NET Core, který může automatizovat generování tříd DbContext, entit, mapování konfigurace a tříd úložiště z databáze SQL Server. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Generátor Entity Framework Core LoreSoft

Generátor Entity Framework Core (EFG) je .NET Core CLI nástroj, který může generovat EF Core modely z existující databáze, podobně jako `dotnet ef dbcontext scaffold`, ale také podporuje bezpečné [obnovení](https://efg.loresoft.com/en/latest/regeneration/) kódu prostřednictvím nahrazení oblastí nebo analýzou souborů mapování. Tento nástroj podporuje generování modelů zobrazení, ověřování a kódu mapovače objektů. Pro EF Core: 2.

[Dokumentace](https://efg.loresoft.com/en/latest/)
[kurzu](https://www.loresoft.com/Generate-ASP-NET-Web-API)

## <a name="extensions"></a>Rozšíření

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Knihovna modulů plug-in, která umožňuje automatické zaznamenávání změn dat provedených EF Core do tabulky historie. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Rozšíření, které umožňuje ukládat výsledky EF Core dotazů do mezipaměti druhé úrovně, aby následné spouštění stejných dotazů se mohli vyhnout přístupu k databázi a načíst data přímo z mezipaměti. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a>Geco

GECO (konzola generátoru) je jednoduchý generátor kódu založený na projektu konzoly, který běží na .NET Core a používá C# interpolované řetězce pro generování kódu. GECO zahrnuje generátor reverzních modelů pro EF Core s podporou pro pluralitování, jednotné navýšení a upravitelnou šablonu. Poskytuje také generátor počátečních dat, spouštěč skriptů a čistič databáze. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore. Lešeníing. handlebars 

Umožňuje přizpůsobit třídy zpětnou analýzou z existující databáze pomocí Entity Framework Core sada nástrojů s handlebars šablonami. Pro EF Core: 2, 3.

[Úložiště GitHub](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore 

NeinLinq rozšiřuje zprostředkovatele LINQ, jako je například Entity Framework, aby umožnila opakované použití funkcí, přepisování dotazů a sestavování dynamických dotazů pomocí přeložitelných predikátů a selektorů. Pro EF Core: 2, 3.

[Úložiště GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Modul plug-in pro Microsoft. EntityFrameworkCore pro podporu úložiště, pracovních schémat a více databází s podporou distribuované transakce. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Rozšíření EF Core pro hromadné operace (vložení, aktualizace, odstranění). Pro EF Core: 2, 3.

[Úložiště GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Přidá pluralitu v době návrhu. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Revival atributu [index] (s příponou pro sestavení modelu). Pro EF Core: 2, 3.

[Úložiště GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Poskytuje obálku kolem EF Coreho poskytovatele databáze v paměti. Díky tomu bude fungovat podobně jako u relačního poskytovatele. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implementace dočasné podpory. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>EfCoreTemporalTable

Pomocí představených metod rozšíření můžete snadno provádět dočasné dotazy na oblíbené databázi: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)``AsTemporalContained(startDate, endDate)`. Pro EF Core: 3.

[Úložiště GitHub](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a>EFCore.TimeTraveler

Povolí plně funkční Entity Framework Core dotazy pro SQL Server dočasnou [historii](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) pomocí kódu EF Core, entit a mapování, které jste už definovali.  Procházením kódu v `using (TemporalQuery.AsOf(targetDateTime)) {...}`se procházejte časem. Pro EF Core: 3.

[Úložiště GitHub](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore.TemporalTables

Knihovna rozšíření pro Entity Framework Core, která umožňuje vývojářům, kteří používají SQL Server, snadno používat dočasné tabulky. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Vysoce výkonná mezipaměť dotazů druhé úrovně. Pro EF Core: 2.

[Úložiště GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework plus

Rozšiřuje vaše DbContext o funkce, jako například: zahrnutí filtru, auditování, ukládání do mezipaměti, budoucí dotaz, dávkové odstranění, dávkové aktualizace a další. Pro EF Core: 2, 3.

[Web](https://entityframework-plus.net/)
[úložiště GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Rozšíření Entity Framework

Rozšiřuje vaše DbContext o vysoce výkonné hromadné operace: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge a další. Pro EF Core: 2, 3.

[Web](https://entityframework-extensions.net/)
