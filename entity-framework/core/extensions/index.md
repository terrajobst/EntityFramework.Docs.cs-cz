---
title: Rozšíření & nástrojů – EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 86befa151adc8278ff8c76bdef023ca26a12508b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824625"
---
# <a name="ef-core-tools--extensions"></a>Rozšíření & nástrojů pro EF Core

Tyto nástroje a rozšíření poskytují další funkce pro Entity Framework Core 2,0 a novější.

> [!IMPORTANT]  
> Rozšíření jsou sestavená z nejrůznějších zdrojů, která nejsou udržována jako součást projektu Entity Framework Core. Při zvažování rozšíření jiného výrobce nezapomeňte vyhodnotit jeho kvalitu, licencování, kompatibilitu, podporu atd., abyste zajistili, že splňuje vaše požadavky.

## <a name="tools"></a>Nástroje

### <a name="llblgen-pro"></a>LLBLGen pro

LLBLGen pro je řešení modelování entit s podporou Entity Framework a Entity Framework Core. Umožňuje snadno definovat model entity a namapovat ho do vaší databáze, použít nejprve databázi nebo model, abyste mohli začít psát dotazy hned.

[Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Vývojář entit Devart

Vývojář entit je výkonný Návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik přístup k datům a LINQ to SQL. Podporuje navrhování EF Corech modelů vizuálně, použití prvního nebo databáze prvního přístupového C# modelu nebo generování kódu Visual Basic.

[Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core nástroje Power Tools

EF Core Power Tools je rozšíření sady Visual Studio 2017, které zpřístupňuje různé úlohy EF Core v době návrhu v jednoduchém uživatelském rozhraní. Zahrnuje zpětnou přípravu tříd DbContext a entit z existujících databází a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), správu migrací databáze a vizualizace modelů.

[Wiki GitHubu](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework vizuální Editor

Entity Framework Visual Editor je rozšíření sady Visual Studio, které přidává návrháře ORM pro vizuální návrh z EF 6 a EF Core třídy. Kód se generuje pomocí šablon T4, takže ho můžete přizpůsobit podle potřeby. Podporuje dědičnost, jednosměrná a obousměrná přidružení, výčty a možnost barevně kódovat vaše třídy a přidat textové bloky, které vysvětlují potenciálně učit složité části návrhu.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory je modul pro generování uživatelského rozhraní pro .NET Core, který může automatizovat generování tříd DbContext, entit, mapování konfigurace a tříd úložiště z databáze SQL Server.

[Úložiště GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Generátor Entity Framework Core LoreSoft

Generátor Entity Framework Core (EFG) je .NET Core CLI nástroj, který může generovat EF Core modely z existující databáze, podobně jako `dotnet ef dbcontext scaffold`, ale také podporuje bezpečné [obnovení](https://efg.loresoft.com/en/latest/regeneration/) kódu prostřednictvím nahrazení oblastí nebo analýzou souborů mapování. Tento nástroj podporuje generování modelů zobrazení, ověřování a kódu mapovače objektů.

[Dokumentace](https://efg.loresoft.com/en/latest/)
[kurzu](https://www.loresoft.com/Generate-ASP-NET-Web-API)

## <a name="extensions"></a>Rozšíření

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Knihovna modulů plug-in, která umožňuje automatické zaznamenávání změn dat provedených EF Core do tabulky historie.

[Úložiště GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Port .NET Core/.NET Standard System. Linq. Dynamic, který zahrnuje asynchronní podporu s EF Core.
System. Linq. Dynamic pochází jako ukázka společnosti Microsoft, která ukazuje, jak dynamicky vytvářet dotazy LINQ z řetězcových výrazů namísto kódu.

[Úložiště GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Rozšíření, které umožňuje ukládat výsledky EF Core dotazů do mezipaměti druhé úrovně, aby následné spouštění stejných dotazů se mohli vyhnout přístupu k databázi a načíst data přímo z mezipaměti.

[Úložiště GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Tato knihovna umožňuje načíst hodnoty primárního klíče (včetně složených klíčů) z libovolné entity jako slovníku.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Tato knihovna umožňuje silného typu přístupu k původním hodnotám vlastností entit.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

GECO (konzola generátoru) je jednoduchý generátor kódu založený na projektu konzoly, který běží na .NET Core a používá C# interpolované řetězce pro generování kódu. GECO zahrnuje generátor reverzních modelů pro EF Core s podporou pro pluralitování, jednotné navýšení a upravitelnou šablonu. Poskytuje také generátor počátečních dat, spouštěč skriptů a čistič databáze.

[Úložiště GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit. Microsoft. EntityFrameworkCore je verze knihovny LINQKit kompatibilní s EF Core. LINQKit je bezplatná sada rozšíření pro LINQ to SQL a Entity Framework Power Users. Umožňuje pokročilé funkce, jako je dynamické sestavování výrazů predikátů, a používání proměnných výrazu v poddotazech.  

[Úložiště GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq rozšiřuje zprostředkovatele LINQ, jako je například Entity Framework, aby umožnila opakované použití funkcí, přepisování dotazů a sestavování dynamických dotazů pomocí přeložitelných predikátů a selektorů.

[Úložiště GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Modul plug-in pro Microsoft. EntityFrameworkCore pro podporu úložiště, pracovních schémat a více databází s podporou distribuované transakce.

[Úložiště GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Rozšíření EF Core pro hromadné operace (vložení, aktualizace, odstranění).

[Úložiště GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Přidá do EF Core dobu trvání návrhu.

[Úložiště GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/pomelo. EntityFrameworkCore. Extensions. ToSql

Jednoduchá rozšiřující metoda, která získá příkaz SQL EF Core by generovala pro daný dotaz LINQ v jednoduchých scénářích. Metoda ToSql je omezená na jednoduché scénáře, protože EF Core může generovat více než jeden příkaz SQL pro jeden dotaz LINQ a různé příkazy SQL závisející na hodnotách parametrů.

[Úložiště GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Revival atributu [index] pro EF Core (s příponou pro sestavení modelu).

[Úložiště GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Poskytuje obálku kolem EF Coreho poskytovatele databáze v paměti. Díky tomu bude fungovat podobně jako u relačního poskytovatele.

[Úložiště GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implementace dočasné podpory pro EF Core.

[Úložiště GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Vysoce výkonná mezipaměť dotazů druhé úrovně pro EF Core.

[Úložiště GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework plus

Rozšiřuje vaše DbContext o funkce, jako například: zahrnutí filtru, auditování, ukládání do mezipaměti, budoucí dotaz, dávkové odstranění, dávkové aktualizace a další.

[Web](https://entityframework-plus.net/)
[úložiště GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Rozšíření Entity Framework

Rozšiřuje vaše DbContext o vysoce výkonné hromadné operace: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge a další.

[Web](https://entityframework-extensions.net/)
