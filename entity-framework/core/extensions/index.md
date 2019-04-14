---
title: Nástroje a rozšíření – EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: d432ca36c166f7baf71253709bf58b1f5428a11a
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562504"
---
# <a name="ef-core-tools--extensions"></a>EF Core nástroje a rozšíření

Tyto nástroje a rozšíření poskytují další funkce pro Entity Framework Core 2.0 a novější.

> [!IMPORTANT]  
> Rozšíření jsou vytvořeny pomocí široké škály zdrojů a nejsou spravovány jako součást projektu Entity Framework Core. Při zvažování rozšíření třetích stran, nezapomeňte vyhodnotit její kvality, licencování, kompatibility, podpora, atd. a ujistěte se, že vyhovuje vašim požadavkům.

## <a name="tools"></a>Nástroje

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro je entita modelování řešení s podporou pro Entity Framework a Entity Framework Core. To vám umožní snadno definovat entity model a mapování na databázi, nejprve pomocí databáze nebo model nejprve, takže můžete začít používat hned psaní dotazů.

[Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity pro vývojáře

Pro vývojáře entity je výkonný Návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access a LINQ to SQL. Jeho vizuální návrh modely EF Core podporuje, nejprve pomocí modelu nebo databáze nejprve přístupů, a C# nebo generování kódu jazyka Visual Basic. 

[Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools je rozšíření sady Visual Studio 2017, která zveřejňuje různé úlohy návrhu EF Core v jednoduché uživatelské rozhraní. Zahrnuje zpětné analýze třídy DbContext a entitou ze stávajících databází a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), správu migracemi databází a vizualizace modelu.

[GitHub wiki](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Editor sady Visual Entity Framework

Editor sady Visual Entity Framework je rozšíření sady Visual Studio, který přidá návrhář ORM pro vizuální návrh EF 6 a EF Core tříd. Kód je generována pomocí šablony T4, takže je možné přizpůsobit podle potřeby. Podporuje dědičnosti, jednosměrný a přidružení obousměrný, výčty a schopnost barevné označení tříd a přidejte textové bloky vysvětlují potenciálně složité součástí návrhu.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory je modul pro generování uživatelského rozhraní pro .NET Core, který můžete automatizovat generování třídy DbContext, entity, mapování konfigurace a třídy úložiště z databáze SQL serveru.

[Úložiště GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft vaší Entity Framework Core generátor

Entity Framework Core generátor (efg) je nástroj příkazového řádku .NET Core, který může vytvořit modely EF Core z existující databáze, podobně jako `dotnet ef dbcontext scaffold`, ale také podporuje bezpečné kód [opětovné generování](https://efg.loresoft.com/en/latest/regeneration/) přes oblast nahrazení nebo pomocí analýzy mapování souborů. Tento nástroj podporuje generování Zobrazit modely, ověření a mapování kódu objektu. 

[Kurz](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[dokumentace](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Rozšíření

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Modul plug-in knihovny, která umožňuje automaticky záznam změny prováděné EF Core do tabulky historie.

[Úložiště GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

.NET Core / .NET Standard port z System.Linq.Dynamic, která zahrnuje podpory asynchronních operací s EF Core.
System.Linq.Dynamic pochází z ukázky Microsoft, která ukazuje, jak vytvářet dotazy LINQ dynamicky z řetězcové výrazy namísto kódu.

[Úložiště GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Rozšíření, která umožňuje ukládání do mezipaměti druhé úrovně, výsledky dotazů EF Core tak, aby další spuštění stejného dotazů můžete zabránit přístupu k databázi a načtou data přímo z mezipaměti.

[Úložiště GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Tato knihovna umožňuje načíst hodnoty primárního klíče (včetně složené klíče) v libovolné entitě jako slovník.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Tato knihovna umožňuje silného typu přístup k původní hodnoty vlastností entity. 

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (generátor konzoly) je generátor kódu jednoduchý konzolový projekt, který běží na .NET Core a používá podle C# interpolovaných řetězců pro generování kódu. Geco zahrnuje generátor pro jádro EF Core s podporou pluralizace singularization a upravovat šablony. Také poskytuje počáteční hodnoty generátor dat skriptu, skript runner a databázi čisticí.

[Úložiště GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore je EF Core – kompatibilní verzi knihovny LINQKit. LINQKit je bezplatná sada rozšíření pro LINQ na SQL a Entity Framework zkušeným uživatelům. To umožňuje pokročilé funkce, jako jsou dynamické vytváření predikátu výrazů a použití proměnných výrazu v poddotazy.  

[Úložiště GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq rozšiřuje zprostředkovatelé dotazů LINQ jako je například rozhraní Entity Framework umožňující opětovné použití funkce přepisování dotazy a sestavování dynamických dotazů pomocí Přeložitelné predikáty a selektory.

[Úložiště GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Modul plug-in pro Microsoft.EntityFrameworkCore zajistit podporu více databází, úložiště a jednotky pracovních vzorů s distribuovanou transakci nepodporuje.

[Úložiště GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

EF Core rozšíření pro hromadné operace (Insert, Update, Delete).

[Úložiště GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Přidá pluralizace návrhu na EF Core.

[Úložiště GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

Pro daný dotaz LINQ v jednoduché scénáře vygeneruje metodu jednoduché rozšíření, která získá EF Core příkaz jazyka SQL. Metoda ToSql je omezené na jednoduché scénáře, protože EF Core můžete generovat více než jeden příkaz SQL pro jednoho dotazu LINQ a různé příkazy SQL v závislosti na hodnoty parametrů.

[Úložiště GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Revival atribut [Index] pro jádro EF Core (s příponou pro vytváření modelů).

[Úložiště GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Poskytuje obálku kolem poskytovatele databáze EF Core v paměti. Umožňuje více fungují jako relační zprostředkovatele.

[Úložiště GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implementace dočasné podpory pro jádro EF Core.

[Úložiště GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Vysoce výkonné druhé úrovně dotazu mezipaměť jádra EF Core.

[Úložiště GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)
