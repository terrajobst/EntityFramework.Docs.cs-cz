---
title: Nástroje a rozšíření – EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 1edc7a20f54b2d26f899c93e98dfaf6d62c29f86
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490724"
---
# <a name="ef-core-tools--extensions"></a>EF Core nástroje a rozšíření

Nástroje a rozšíření poskytují další funkce pro Entity Framework Core.

> [!IMPORTANT]  
> Rozšíření jsou vytvořené pomocí široké škály zdrojů a není zachována jako součást projektu Entity Framework Core. Při zvažování rozšíření třetích stran, ujistěte se, k vyhodnocení kvality, licencování, kompatibility, podpora, atd. a ujistěte se, že splňují vaše požadavky.

## <a name="tools"></a>Nástroje

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro je entita modelování řešení s podporou pro Entity Framework a Entity Framework Core. To vám umožní snadno definovat entity model a mapování na databázi, nejprve pomocí databáze nebo model nejprve, takže můžete začít používat hned psaní dotazů.

[Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity pro vývojáře

Pro vývojáře entity je výkonný Návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access a LINQ to SQL. Můžete použít Model-First a Database-First přístupy k návrhu modelu ORM a generování kódu C# nebo Visual Basic .NET pro něj. Zavádí nové přístupy pro navrhování modelů ORM, zvyšuje produktivitu a usnadňuje vývoj aplikací databáze.

[Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Visual Studio 2017 + rozšíření. Můžete provést zpětnou analýzu třídy DbContext a POCO z existující databáze nebo databázový projekt SQL Server a vizualizovat a kontrolovat vaše DbContext různými způsoby.

[Wiki úložiště GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a>Editor sady Visual Entity Framework

Rozšíření sady Visual Studio 2017, které přidá návrhář ORM pro vizuální návrh Entity Framework 6, Core 2.0 a 2.1 základní třídy. Kód je generována pomocí šablony T4, takže lze kompletně upravit podle potřeby. Dědičnost, jednosměrnou a obousměrnou přidružení jsou všechny podporované, jak jsou, výčty a schopnost barevné označení tříd a přidejte textové bloky vysvětlit potenciálně složité součástí návrhu.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

## <a name="extensions"></a>Rozšíření

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Modul plug-in pro Microsoft.EntityFrameworkCore podporují nahrávání dat automaticky změní historie.

[Úložiště GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Dynamické rozšíření Linq pro Microsoft.EntityFrameworkCore, který přidává podpory asynchronních operací

 [Úložiště GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Pokus o zaznamenat některé dobré nebo osvědčené postupy v rozhraní API, které podporuje testování – včetně malé architektura pro N + 1 dotazy vyhledávání.

[Úložiště GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Druhé úrovně, ukládání do mezipaměti knihovny. Druhé úrovně mezipaměti se mezipaměť dotazů. Výsledky příkazů EF se uloží do mezipaměti, takže stejné příkazy EF se data načítají z mezipaměti namísto jejich spuštění na databázi znovu.

[Úložiště GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Načte a uloží celý odpojené entity grafy (entita s jejich podřízené entity a seznamy). Inspirovat [GraphDiff](https://github.com/refactorthis/GraphDiff/). Cílem je také přidat simplificate některé moduly plug-in některých opakujících se úloh, jako je auditování a stránkování.

[Úložiště GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Načtěte primární klíč (včetně složených klíčů) z jakékoli entity jako slovník.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Reaktivní rozšíření obálky pro aktivní pozorovatelné objekty Entity Framework entit.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Přidáte aktivační události pro entity s vložit, aktualizovat a odstraňovat události. Existují tři události pro každou: před, po a nebude úspěšná.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Zadaný přístup k původní hodnota vlastnosti vaší entity. Jednoduché a komplexní vlastnosti jsou podporovány, navigaci nebo kolekce nejsou.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco poskytuje generátor Reverse modelu s podporou Pluralizace/Singularization a upravovat šablony založené na jazyce C# 6.0 interpolovaných řetězců a běží na.Net Core. Také poskytuje generátor skript počáteční hodnoty se skripty SQL sloučení a runner skriptu.

[Úložiště Github](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore je bezplatná sada rozšíření pro LINQ na SQL a EntityFrameworkCore zkušeným uživatelům. S podporou Include(...) a IDbAsync.

[Úložiště GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore poskytuje užitečná rozšíření pro používání zprostředkovatelů LINQ jako je například rozhraní Entity Framework, které podporují pouze menší podmnožinu funkcí .NET opětovné použití funkce, přepisování dotazy, díky kterým i, hodnoty Null a sestavování dynamických dotazů pomocí Přeložitelné predikáty a selektory.

[Úložiště GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Modul plug-in pro Microsoft.EntityFrameworkCore zajistit podporu pro úložiště a jednotky pracovních vzorů a více databáze s distribuovanou transakci nepodporuje.

[Úložiště GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Opožděné načtení pro jádro EF Core 1.1

[Úložiště GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

EntityFrameworkCore rozšíření pro hromadné operace (Insert, Update, Delete).

[Úložiště GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Přidá pluralizace návrhu na EF Core.

[Úložiště GitHub](https://github.com/bricelam/EFCore.Pluralizer)
