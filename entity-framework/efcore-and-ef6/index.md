---
title: Porovnání Entity Framework 6 a Entity Framework Core-EF
description: Poskytuje pokyny k výběru mezi Entity Framework 6 a Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 9fe4905de5bd81fce083d620724b7fad4c6dd11b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182056"
---
# <a name="compare-ef-core--ef6"></a>Porovnání EF Core a EF6

Entity Framework je objektově-relační Mapovač (O/RM) pro .NET. Tento článek porovnává dvě verze: Entity Framework 6 a Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) je vyzkoušená technologie pro přístup k datům. Byla poprvé vydána v 2008 jako součást .NET Framework 3,5 SP1 a Visual Studio 2008 SP1. Počínaje verzí 4,1 se dodává jako balíček NuGet [EntityFramework](https://www.nuget.org/packages/EntityFramework/) . EF6 běží na .NET Framework 4. x a .NET Core od 3,0. a vyšší.

EF6 bude i nadále podporován produkt a bude dál zobrazovat opravy chyb a menší vylepšení.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) je kompletní přepis EF6, který byl poprvé vydán v 2016. Dodává se do balíčků NuGet, hlavní z nich je [Microsoft. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). EF Core je produkt pro různé platformy, který běží na .NET Core.

EF Core byla navržena tak, aby poskytovala vývojářské prostředí podobně jako EF6. Většina rozhraní API nejvyšší úrovně zůstává stejná, takže EF Core se budou dobře rozumět vývojářům, kteří používali EF6.

## <a name="feature-comparison"></a>Porovnání funkcí

EF Core nabízí nové funkce, které se neimplementují v EF6 (například [alternativní klíče](xref:core/modeling/alternate-keys), [dávkové aktualizace](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements)a [vyhodnocování smíšeného klienta/databáze v dotazech LINQ](xref:core/querying/client-eval). Ale vzhledem k tomu, že se jedná o nový základ kódu, chybí také některé funkce, které EF6 má.

Následující tabulky porovnávají funkce, které jsou k dispozici v EF Core a EF6. Jedná se o porovnání na vysoké úrovni a neuvádí všechny funkce nebo vysvětluje rozdíly mezi stejnou funkcí v různých verzích EF.

Sloupec EF Core označuje verzi produktu, ve které se funkce poprvé objevila.

### <a name="creating-a-model"></a>Vytvoření modelu

| **Funkce**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Základní mapování třídy                                   | Ano      | 1.0                                   |
| Konstruktory s parametry                          |          | 2.1                                   |
| Převody hodnot vlastností                            |          | 2.1                                   |
| Mapované typy bez klíčů                             |          | 2.1                                   |
| Konvence                                           | Ano      | 1.0                                   |
| Vlastní konvence                                    | Ano      | 1,0 (částečný)                         |
| Datové poznámky                                      | Ano      | 1.0                                   |
| Rozhraní Fluent API                                            | Ano      | 1.0                                   |
| Dědičnost Tabulka na hierarchii (TPH)                | Ano      | 1.0                                   |
| Dědičnost Tabulka na typ (TPT)                     | Ano      |                                       |
| Dědičnost Tabulka na konkrétní třídu (TPC)           | Ano      |                                       |
| Vlastnosti stavu stínu                               |          | 1.0                                   |
| Alternativní klíče                                        |          | 1.0                                   |
| M:n bez spojení entity                      | Ano      |                                       |
| Generování klíče: Databáze                              | Ano      | 1.0                                   |
| Generování klíče: Klient                                |          | 1.0                                   |
| Komplexní/vlastněné typy                                   | Ano      | 2.0                                   |
| Prostorová data                                          | Ano      | 2.2                                   |
| Grafická vizualizace modelu                      | Ano      |                                       |
| Editor grafického modelu                                | Ano      |                                       |
| Formát modelu: Kód                                    | Ano      | 1.0                                   |
| Formát modelu: EDMX (XML)                              | Ano      |                                       |
| Vytvořit model z databáze: Příkazový řádek              | Ano      | 1.0                                   |
| Vytvořit model z databáze: Průvodce VS                 | Ano      |                                       |
| Aktualizovat model z databáze                            | Částečně  |                                       |
| Filtry globálních dotazů                                  |          | 2.0                                   |
| Rozdělení tabulky                                       | Ano      | 2.0                                   |
| Rozdělování entit                                      | Ano      |                                       |
| Mapování skalární funkce databáze                      | Slabé     | 2.0                                   |
| Mapování polí                                         |          | 1.1                                   |
| Typy odkazů s možnou hodnotou null (C# 8,0)                     |          | 3.0                                   |

### <a name="querying-data"></a>Dotazy na data

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ – dotazy                                          | Ano      | 1,0 (probíhá zpracování složitých dotazů) |
| Čitelné vygenerované SQL                                | Slabé     | 1.0                                   |
| Překlad GroupBy                                   | Ano      | 2.1                                   |
| Načítají se související data: Eager                           | Ano      | 1.0                                   |
| Načítají se související data: Eager načítání pro odvozené typy |          | 2.1                                   |
| Načítají se související data: Psán                            | Ano      | 2.1                                   |
| Načítají se související data: Explicitně                        | Ano      | 1.1                                   |
| Nezpracované dotazy SQL: Typy entit                         | Ano      | 1.0                                   |
| Nezpracované dotazy SQL: Typy entit bez klíčů                 | Ano      | 2.1                                   |
| Nezpracované dotazy SQL: Vytváření pomocí LINQ                  |          | 1.0                                   |
| Explicitně kompilované dotazy                           | Slabé     | 2.0                                   |
| Dotazovací jazyk založený na textu (Entity SQL)                | Ano      |                                       |
| await foreach (C# 8,0)                                |          | 3.0                                   |

### <a name="saving-data"></a>Ukládání dat

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Sledování změn: Snímek                             | Ano      | 1.0                                   |
| Sledování změn: Oznámení                         | Ano      | 1.0                                   |
| Sledování změn: Proxy servery                              | Ano      |                                       |
| Přístup ke sledovanému stavu                               | Ano      | 1.0                                   |
| Optimistická souběžnost                                | Ano      | 1.0                                   |
| Transakce                                          | Ano      | 1.0                                   |
| Dávkování příkazů                                |          | 1.0                                   |
| Mapování uložených procedur                              | Ano      |                                       |
| Odpojená rozhraní API nízké úrovně grafu                     | Slabé     | 1.0                                   |
| Odpojený graf od konce do konce                         |          | 1,0 (částečný)                         |

### <a name="other-features"></a>Další funkce

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrace                                            | Ano      | 1.0                                   |
| Rozhraní API pro vytváření a odstraňování databází                       | Ano      | 1.0                                   |
| Počáteční data                                             | Ano      | 2.1                                   |
| Odolnost připojení                                 | Ano      | 1.1                                   |
| Zavěšení životního cyklu (události, zachycení)                | Ano      |                                       |
| Jednoduché protokolování (Database. log)                         | Ano      |                                       |
| Sdružování DbContext                                     |          | 2.0                                   |

### <a name="database-providers"></a>Poskytovatelé databází

| **Funkce**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Ano      | 1.0                                   |
| MySQL                                                 | Ano      | 1.0                                   |
| PostgreSQL                                            | Ano      | 1.0                                   |
| Oracle                                                | Ano      | 1.0                                   |
| SQLite                                                | Ano      | 1.0                                   |
| SQL Server Compact                                    | Ano      | 1,0 <sup>(1)</sup>                    |
| DB2                                                   | Ano      | 1.0                                   |
| Firebird                                              | Ano      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2,0 <sup>(1)</sup>                    |
| Databáze Cosmos                                             |          | 3.0                                   |
| V paměti (pro testování)                               |          | 1.0                                   |

<sup>1</sup> poskytovatelé SQL Server Compact a jet fungují jenom v .NET Framework (ne v .NET Core).

### <a name="net-implementations"></a>Implementace rozhraní .NET

| **Funkce**                                           | **EF6**            | **EF Core**                           |
|:------------------------------------------------------|:-------------------|:--------------------------------------|
| .NET Framework                                        | Ano                | 1,0 (odebrané v 3,0)                  |
| .NET Core                                             | Ano (přidáno v 6,3) | 1.0                                   |
| Mono & Xamarin                                        |                    | 1,0 (probíhající)                     |
| UWP                                                   |                    | 1,0 (probíhající)                     |

## <a name="guidance-for-new-applications"></a>Doporučení pro nové aplikace

Pokud jsou splněné obě následující podmínky, zvažte použití EF Core pro novou aplikaci:
* Aplikace potřebuje funkce .NET Core. Další informace najdete v tématu [Výběr mezi .NET Core a .NET Framework pro serverové aplikace](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core podporuje všechny funkce, které aplikace vyžaduje. Pokud chybí požadovaná funkce, podívejte se na plán [EF Core](xref:core/what-is-new/index) , kde zjistíte, jestli v budoucnu existují plány pro jejich podporu. 

Zvažte použití EF6, pokud jsou splněny obě následující podmínky:
* Aplikace se spustí v systému Windows a .NET Framework 4,0 nebo novějším.
* EF6 podporuje všechny funkce, které aplikace vyžaduje.

## <a name="guidance-for-existing-ef6-applications"></a>Doporučení pro existující aplikace s EF6

Vzhledem k zásadním změnám v EF Core Nedoporučujeme přesunovat aplikaci EF6, aby EF Core, pokud neexistuje přesvědčivý důvod k provedení změny. Pokud chcete přejít na EF Core, abyste mohli používat nové funkce, ujistěte se, že víte o jeho omezeních. Další informace najdete v tématu [přenos z EF6 na EF Core](porting/index.md). **Přesunutí z EF6 na EF Core je víc než při upgradu na port.**

## <a name="next-steps"></a>Další kroky

Další informace najdete v dokumentaci:
* <xref:core/index>
* <xref:ef6/index>
