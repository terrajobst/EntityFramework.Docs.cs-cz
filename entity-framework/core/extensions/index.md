---
title: Nástroje a rozšíření – EF jádra
author: ErikEJ
ms.author: divega
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/extensions/index
ms.openlocfilehash: 6c8cb3e0d8552f274118e4020b7e2e8009af7e11
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769436"
---
# <a name="ef-core-tools--extensions"></a>EF základní nástroje a rozšíření

Nástroje a rozšíření poskytují další funkce pro Entity Framework Core.

> [!IMPORTANT]  
> Rozšíření jsou vytvořené různých zdrojů a není zachována jako součást projektu Entity Framework Core. Až budete zvažovat rozšíření třetích stran, ujistěte se, že jste vyhodnocení kvality, licencování, kompatibility, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.

## <a name="tools"></a>Nástroje

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro je entita modelování řešení s podporou rozhraní Entity Framework a Entity Framework Core. Vám umožní snadno definovat modelu entity a mapy ke své databázi, nejprve pomocí databáze nebo model nejprve, takže můžete začít používat hned zápis dotazů.

[website](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity vývojáře

Entity vývojáře je výkonný návrháře ORM ADO.NET Entity Framework, NHibernate, LinqConnect, přístup k datům webu Telerik a technologie LINQ to SQL. Můžete použít první Model a první databáze blíží k návrhu modelu ORM a generování kódu jazyka C# nebo Visual Basic .NET pro ni. Zavádí nové přístupy k navrhování modely ORM, zvyšuje produktivitu a usnadňuje vývoj aplikací databáze.

[website](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF základní výkonné nástroje

Visual Studio 2017 + rozšíření. Můžete zpětnou třídy DbContext a objektů POCO z existující databáze nebo databáze systému SQL Server projektu a vizualizovat a zkontrolovat vaše DbContext různými způsoby.

[Wiki Githubu](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a>Rozšíření

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Modul plug-in pro Microsoft.EntityFrameworkCore pro podporu automaticky historie změn dat záznam.

[Úložiště GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Dynamické Linq rozšíření pro Microsoft.EntityFrameworkCore, který přidává podporu asynchronní

 [Úložiště GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Pokus o zaznamenat některé funkční nebo osvědčené postupy v rozhraní API, která podporuje testování – včetně malé rozhraní vyhledávání N + 1 dotazy.

[Úložiště GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Knihovna druhé úrovně ukládání do mezipaměti. Druhé úrovně mezipaměti je mezipaměť a dotazu. Výsledky EF příkazy se uloží do mezipaměti, aby stejné příkazy EF načte data z mezipaměti namísto je znovu prováděna v databázi.

[Úložiště GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Načítá a ukládá grafy celý odpojit entity (entita s jejich podřízených entit a seznamy). INSPIROVANÉ [GraphDiff](https://github.com/refactorthis/GraphDiff/). Cílem je také přidat simplificate některé moduly plug-in některé opakované úkoly, jako je auditování a stránkování.

[Úložiště GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Primární klíč (včetně složené klíče) ze všechny entity načíst jako slovník.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Přepnutí do reaktivního rozšíření obálek pro aktivní pozorovatelné objekty rozhraní Entity Framework entit.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Přidejte aktivační události na váš entity s vložit, aktualizovat a odstraňovat události. Existují tři události pro každou: před, po a při selhání.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Získáte typové přístup k původní hodnota vlastnosti vaší entity. Jsou podporovány jednoduché a komplexní vlastností, navigační nebo kolekce nejsou.

[Úložiště GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco poskytuje generátor Reverse modelu s podporou Pluralizační/Singularization a upravovat šablony podle jazyka C# 6.0 interpolované řetězce a spuštěná na .net Core. Poskytuje také generátor skriptů počáteční hodnoty s skripty SQL sloučení a runner skriptu.

[Úložiště Github](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore je bezplatná sada rozšíření pro výrazy LINQ SQL a EntityFrameworkCore skupiny power users. S podporou Include(...) a IDbAsync.

[Úložiště GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore poskytuje rozšíření užitečná pro používání LINQ zprostředkovatelů například Entity Framework podporují pouze menší podmnožinu funkcí rozhraní .NET, opětovné použití funkce přepisování dotazy, i přitom bezpečných hodnotu null a sestavování dynamických dotazů pomocí nepřeložitelná predikáty a selektorů.

[Úložiště GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Modul plug-in pro Microsoft.EntityFrameworkCore pro podporu více databáze, úložiště a jednotky pracovních vzorů s distribuované transakce podporován.

[Úložiště GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Opožděného načítání pro základní EF 1.1

[Úložiště GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

EntityFrameworkCore rozšíření pro hromadné operace (příkaz Insert, Update, Delete).

[Úložiště GitHub](https://github.com/borisdj/EFCore.BulkExtensions)
