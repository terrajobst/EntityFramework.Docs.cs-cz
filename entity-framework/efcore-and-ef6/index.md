---
title: Porovnání Entity Framework 6 a Entity Framework Core
description: Poskytuje pokyny o tom, jak si vybrat mezi Entity Framework 6 a Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: d5fe9b388707f653fdeb2d6a5daa7215ced71c1d
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688716"
---
# <a name="compare-ef-core--ef6"></a>Porovnání EF Core a EF6

Entity Framework je objektově relační Mapovač (O/RM) pro .NET. Tento článek porovnává dvě verze: Entity Framework 6 a Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) je technologie vyzkoušená a otestovaná dat přístup. V rámci rozhraní .NET Framework 3.5 SP1 a Visual Studio 2008 SP1 2008 prvního vydání. Od verze 4.1 byla odeslaná jako [EntityFramework](https://www.nuget.org/packages/EntityFramework/) balíček NuGet. EF6 běží na rozhraní .NET Framework 4.x. to znamená, že je spuštěná jenom na Windows. 

EF6 i nadále podporované produkty a bude dál zobrazovat menší vylepšení a oprav chyb.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (jádro EF) je kompletní přepisování EF6, která byla poprvé zavedena ve 2016. To se dodává v balíčcích Nuget, hlavní právě jeden [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). EF Core je multiplatformní produkt, který můžete spustit na .NET Core nebo .NET Framework.

EF Core je navržená pro zajištění EF6 podobný prostředí pro vývojáře. Většina nejvyšší úrovně rozhraní API zůstanou stejné, takže pro vývojáře, kteří použili EF6 bude povědomý EF Core.

## <a name="feature-comparison"></a>Porovnání funkcí

EF Core nabízí nové funkce, které se nebude implementovat v EF6 (například [alternativní klíče](xref:core/modeling/alternate-keys), [dávkové aktualizace](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements), a [smíšené vyhodnocení klienta a databáze v dotazech LINQ](xref:core/querying/client-eval). Protože se jedná o nový kód základní, také chybí některé funkce, které má EF6 však.

Následující tabulce najdete porovnání funkcí dostupných v EF Core a EF6. Je základní porovnání a nebude seznamu všechny funkce nebo vysvětluje rozdíly mezi stejné funkce v různých verzích EF.

EF Core sloupec označuje verzi produktu, ve kterém nejprve funkci objevily.

### <a name="creating-a-model"></a>Vytvoření modelu

| **Funkce**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Základní třída mapování                                   | Ano      | 1.0                                   |
| Konstruktory s parametry                          |          | 2.1                                   |
| Převody hodnot vlastností                            |          | 2.1                                   |
| Mapovaných typech bez klíčů (typy dotazů)               |          | 2.1                                   |
| Konvence                                           | Ano      | 1.0                                   |
| Vlastní konvence                                    | Ano      | 1.0 (částečná podpora)                         |
| Datové poznámky                                      | Ano      | 1.0                                   |
| Rozhraní Fluent API                                            | Ano      | 1.0                                   |
| Dědičnosti: Tabulky na hierarchii (TPH)                | Ano      | 1.0                                   |
| Dědičnosti: Tabulky podle typu (TPT)                     | Ano      |                                       |
| Dědičnosti: Tabulky na konkrétní třídy (TPC)           | Ano      |                                       |
| Stínové vlastnosti stavu                               |          | 1.0                                   |
| Alternativní klíče                                        |          | 1.0                                   |
| Many-to-many bez připojení k entitě                      | Ano      |                                       |
| Generování klíčů: databáze                              | Ano      | 1.0                                   |
| Generování klíčů: klienta                                |          | 1.0                                   |
| Vlastní/komplexní typy                                   | Ano      | 2.0                                   |
| Prostorová data                                          | Ano      | 2.2                                   |
| Grafická vizualizace modelu                      | Ano      |                                       |
| Editor modelů grafické                                | Ano      |                                       |
| Vzor formátu: kód                                    | Ano      | 1.0                                   |
| Vzor formátu: EDMX (XML)                              | Ano      |                                       |
| Vytvoření modelu z databáze: Příkazový řádek              | Ano      | 1.0                                   |
| Vytvoření modelu z databáze: Průvodce VS                 | Ano      |                                       |
| Aktualizace modelu z databáze                            | Částečné  |                                       |
| Globální filtry dotazů                                  |          | 2.0                                   |
| Rozdělení tabulky                                       | Ano      | 2.0                                   |
| Rozdělení entity                                      | Ano      |                                       |
| Mapování skalární funkce databáze                      | Slabé     | 2.0                                   |
| Mapování polí                                         |          | 1.1                                   |

### <a name="querying-data"></a>Dotazy na data

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ – dotazy                                          | Ano      | 1.0 (Probíhá pro složité dotazy) |
| Parametr Readable generovaného SQL                                | Slabé     | 1.0                                   |
| Vyhodnocení smíšené klient/server                        |          | 1.0                                   |
| GroupBy překladu                                   | Ano      | 2.1                                   |
| Načítání souvisejících dat: nemůžou dočkat, až                           | Ano      | 1.0                                   |
| Načítání souvisejících dat: Eager načítání pro odvozené typy |          | 2.1                                   |
| Načítání souvisejících dat: opožděné                            | Ano      | 2.1                                   |
| Načítání souvisejících dat: explicitní                        | Ano      | 1.1                                   |
| Nezpracované dotazy SQL: typy entit                         | Ano      | 1.0                                   |
| Nezpracované dotazy SQL: typy nonentity (typy dotazů)       | Ano      | 2.1                                   |
| Nezpracované dotazy SQL: sestavování s jazykem LINQ                  |          | 1.0                                   |
| Explicitně kompilované dotazy                           | Slabé     | 2.0                                   |
| Jazyk založený na textu dotazu (Entity SQL)                | Ano      |                                       |

### <a name="saving-data"></a>Ukládání dat

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Sledování změn: snímku                             | Ano      | 1.0                                   |
| Sledování změn: oznámení                         | Ano      | 1.0                                   |
| Sledování změn: proxy servery                              | Ano      |                                       |
| Přístup k sledované stavu                               | Ano      | 1.0                                   |
| Optimistická souběžnost                                | Ano      | 1.0                                   |
| Transakce                                          | Ano      | 1.0                                   |
| Dávkové zpracování příkazů                                |          | 1.0                                   |
| Mapování uložené procedury                              | Ano      |                                       |
| Odpojení rozhraní graph API nízké úrovně                     | Slabé     | 1.0                                   |
| Odpojené grafu začátku do konce                         |          | 1.0 (částečná podpora)                         |

### <a name="other-features"></a>Další funkce

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrace                                            | Ano      | 1.0                                   |
| Databáze vytváření/odstraňování rozhraní API                       | Ano      | 1.0                                   |
| Počáteční hodnota dat                                             | Ano      | 2.1                                   |
| Odolnost připojení                                 | Ano      | 1.1                                   |
| Životní cyklus zavěšení (události, zachycení)                | Ano      |                                       |
| Jednoduché protokolování (Database.Log)                         | Ano      |                                       |
| Sdružování DbContext                                     |          | 2.0                                   |

### <a name="database-providers"></a>Poskytovatelé databází

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Ano      | 1.0                                   |
| MySQL                                                 | Ano      | 1.0                                   |
| PostgreSQL                                            | Ano      | 1.0                                   |
| Oracle                                                | Ano      | 1.0 <sup>(1)</sup>                    |
| SQLite                                                | Ano      | 1.0                                   |
| SQL Server Compact                                    | Ano      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | Ano      | 1.0                                   |
| Firebird                                              | Ano      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(2)</sup>                    |
| V paměti (pro účely testování)                               |          | 1.0                                   |

<sup>1</sup> není aktuálně placené zprostředkovatele k dispozici pro Oracle. Bezplatné oficiálního zprostředkovatele pro Oracle se právě zpracovává.

<sup>2</sup> zprostředkovatelů SQL Server Compact a Jet funguje jenom u rozhraní .NET Framework (ne na .NET Core).

### <a name="net-implementations"></a>Implementace .NET

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Rozhraní .NET framework (konzola, WinForms, WPF, technologii ASP.NET)      | Ano      | 1.0                                   |
| .NET core (konzola, ASP.NET Core)                     |          | 1.0                                   |
| Mono & Xamarin                                        |          | 1.0 (Probíhá)                     |
| UWP                                                   |          | 1.0 (Probíhá)                     |

## <a name="guidance-for-new-applications"></a>Doporučení pro nové aplikace

Zvažte použití EF Core pro novou aplikaci, pokud jsou splněny obě následující podmínky:
* Aplikace musí funkcí .NET Core. Další informace najdete v tématu [volba mezi .NET Core a .NET Framework pro serverové aplikace](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core podporuje všechny funkce, které aplikace vyžaduje. Pokud chybí požadované funkce, zkontrolujte, [EF Core – plán](xref:core/what-is-new/roadmap) a zjistěte, pokud jsou plány podpory v budoucnu. 

Zvažte použití EF6, pokud jsou splněny obě následující podmínky:
* Aplikace se bude spouštět na Windows a rozhraní .NET Framework 4.0 nebo novější.
* EF6 podporuje všechny funkce, které aplikace vyžaduje.

## <a name="guidance-for-existing-ef6-applications"></a>Doporučení pro existující aplikace s EF6

Z důvodu zásadní změny v EF Core nedoporučujeme přesun aplikace EF6 do EF Core, pokud neexistuje závažný důvod k provedení změny. Pokud chcete přesunout na EF Core používat nové funkce, ujistěte se, že budete vědět, jaká jsou její omezení. Další informace najdete v tématu [přenesení z EF6 do EF Core](porting/index.md). **Přesun z EF6 do EF Core je další port než upgrade.** 

## <a name="next-steps"></a>Další kroky

Další informace naleznete v dokumentaci:
* <xref:core/index>
* <xref:ef6/index>
