---
title: Porovnat EF6 a EF Core
description: Pokyny k výběru mezi EF6 a EF Core.
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: e28c7d0299e5089f56fb0795d00917cfc30f5cf1
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888145"
---
# <a name="compare-ef-core--ef6"></a>Porovnání EF Core a EF6

## <a name="ef-core"></a>EF Core

Entity Framework Core ([EF Core](../core/index.md)) je moderní objekt – Mapovač databáze pro .NET. Podporuje dotazy LINQ, sledování změn, aktualizace a migrace schématu.

EF Core pracuje s SQL Server/SQL Azure, SQLite, Azure Cosmos DB, MySQL, PostgreSQL a mnoha dalšími databázemi prostřednictvím [modelu modulů plug-in zprostředkovatele databáze](../core/providers/index.md).

## <a name="ef6"></a>EF6

Entity Framework 6 ([EF6](../ef6/index.md)) je objektově-relační Mapovač navržený pro .NET Framework, ale s podporou pro .NET Core. EF6 je stabilní, podporovaný produkt, ale už se aktivně nevyvíjí.

## <a name="feature-comparison"></a>Porovnání funkcí

EF Core nabízí nové funkce, které se v EF6 neimplementují. Ne všechny funkce EF6 se ale v EF Core v tuto chvíli implementují.

Následující tabulky porovnávají funkce, které jsou k dispozici v EF Core a EF6. Jedná se o porovnání na vysoké úrovni, které neobsahuje seznam všech funkcí nebo vysvětluje rozdíly mezi stejnou funkcí v různých verzích EF.

Sloupec EF Core označuje verzi produktu, ve které se funkce poprvé objevila.

### <a name="creating-a-model"></a>Vytvoření modelu

| **Funkce**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Základní mapování třídy                                   | Ano      | 1.0                                   |
| Konstruktory s parametry                          |          | 2.1                                   |
| Převody hodnot vlastností                            |          | 2.1                                   |
| Mapované typy bez klíčů                             |          | 2.1                                   |
| Konvence                                           | Ano      | 1.0                                   |
| Vlastní konvence                                    | Ano      | 1,0 (částečně; [#214](https://github.com/dotnet/efcore/issues/214)) |
| Datové poznámky                                      | Ano      | 1.0                                   |
| Rozhraní Fluent API                                            | Ano      | 1.0                                   |
| Dědičnost: tabulka na hierarchii (TPH)                | Ano      | 1.0                                   |
| Dědičnost: tabulka na typ (TPT)                     | Ano      | Plánováno na 5,0 ([#2266](https://github.com/dotnet/efcore/issues/2266)) |
| Dědičnost: tabulka na konkrétní třídu (TPC)           | Ano      | Roztáhnout na 5,0 ([#3170](https://github.com/dotnet/efcore/issues/3170)) <sup>(1)</sup> |
| Vlastnosti stavu stínu                               |          | 1.0                                   |
| Alternativní klíče                                        |          | 1.0                                   |
| Navigace m:n                              | Ano      | Plánováno na 5,0 ([#19003](https://github.com/dotnet/efcore/issues/19003)) |
| M:n bez spojení entity                      | Ano      | V backlogu ([#1368](https://github.com/dotnet/efcore/issues/1368)) |
| Generování klíče: databáze                              | Ano      | 1.0                                   |
| Generování klíče: klient                                |          | 1.0                                   |
| Komplexní/vlastněné typy                                   | Ano      | 2.0                                   |
| Prostorová data                                          | Ano      | 2.2                                   |
| Formát modelu: kód                                    | Ano      | 1.0                                   |
| Vytvořit model z databáze: příkazový řádek              | Ano      | 1.0                                   |
| Aktualizovat model z databáze                            | Částečné  | V backlogu ([#831](https://github.com/dotnet/efcore/issues/831)) |
| Filtry globálních dotazů                                  |          | 2.0                                   |
| Rozdělení tabulky                                       | Ano      | 2.0                                   |
| Rozdělování entit                                      | Ano      | Roztáhnout na 5,0 ([#620](https://github.com/dotnet/efcore/issues/620)) <sup>(1)</sup> |
| Mapování skalární funkce databáze                      | Slabé     | 2.0                                   |
| Mapování polí                                         |          | 1.1                                   |
| Typy odkazů s možnou hodnotou null (C# 8,0)                     |          | 3,0                                   |
| Grafická vizualizace modelu                      | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |
| Editor grafického modelu                                | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |
| Formát modelu: EDMX (XML)                              | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |
| Vytvořit model z databáze: Průvodce VS                 | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |

### <a name="querying-data"></a>Dotazy na data

| **Funkce**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ – dotazy                                          | Ano      | 1.0                                   |
| Čitelné vygenerované SQL                                | Slabé     | 1.0                                   |
| Překlad GroupBy                                   | Ano      | 2.1                                   |
| Načítají se související data: Eager                           | Ano      | 1.0                                   |
| Načítání souvisejících dat: Eager načítání pro odvozené typy |          | 2.1                                   |
| Načítají se související data: opožděné                            | Ano      | 2.1                                   |
| Načítají se související data: explicitní                        | Ano      | 1.1                                   |
| Nezpracované dotazy SQL: typy entit                         | Ano      | 1.0                                   |
| Nezpracované dotazy SQL: typy entit bez klíčů                 | Ano      | 2.1                                   |
| Nezpracované dotazy SQL: sestavení pomocí LINQ                  |          | 1.0                                   |
| Explicitně kompilované dotazy                           | Slabé     | 2.0                                   |
| await foreach (C# 8,0)                                |          | 3,0                                   |
| Dotazovací jazyk založený na textu (Entity SQL)                | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |

### <a name="saving-data"></a>Ukládání dat

| **Funkce**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Sledování změn: snímek                             | Ano      | 1.0                                   |
| Sledování změn: oznámení                         | Ano      | 1.0                                   |
| Sledování změn: proxy                              | Ano      | Sloučeno pro 5,0 ([#10949](https://github.com/dotnet/efcore/issues/10949)) |
| Přístup ke sledovanému stavu                               | Ano      | 1.0                                   |
| Optimistická souběžnost                                | Ano      | 1.0                                   |
| Transakce                                          | Ano      | 1.0                                   |
| Dávkování příkazů                                |          | 1.0                                   |
| Mapování uložených procedur                              | Ano      | V backlogu ([#245](https://github.com/dotnet/efcore/issues/245)) |
| Odpojená rozhraní API nízké úrovně grafu                     | Slabé     | 1.0                                   |
| Odpojený graf od konce do konce                         |          | 1,0 (částečně; [#5536](https://github.com/dotnet/efcore/issues/5536)) |

### <a name="other-features"></a>Další funkce

| **Funkce**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrace                                            | Ano      | 1.0                                   |
| Rozhraní API pro vytváření a odstraňování databází                       | Ano      | 1.0                                   |
| Počáteční data                                             | Ano      | 2.1                                   |
| Odolnost připojení                                 | Ano      | 1.1                                   |
| Zachycovače                                          | Ano      | 3,0                                   |
| Události                                                | Ano      | 3,0 (částečně; [#626](https://github.com/dotnet/efcore/issues/626)) |
| Jednoduché protokolování (Database. log)                         | Ano      | Sloučeno pro 5,0 ([#1199](https://github.com/dotnet/efcore/issues/1199)) |
| Sdružování DbContext                                     |          | 2.0                                   |

### <a name="database-providers-sup3sup"></a>Poskytovatelé databáze <sup>(3)</sup>

| **Funkce**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Server SQL                                            | Ano      | 1.0                                   |
| MySQL                                                 | Ano      | 1.0                                   |
| PostgreSQL                                            | Ano      | 1.0                                   |
| Oracle                                                | Ano      | 1.0                                   |
| SQLite                                                | Ano      | 1.0                                   |
| SQL Server Compact                                    | Ano      | 1,0 <sup>(4)</sup>                    |
| DB2                                                   | Ano      | 1.0                                   |
| Firebird                                              | Ano      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2,0 <sup>(4)</sup>                    |
| Azure Cosmos DB                                       |          | 3,0                                   |
| V paměti (pro testování)                               |          | 1.0                                   |

<sup>1</sup> cíle Stretch se pro danou verzi pravděpodobně nedosáhnou. Pokud se ale vše dostanou, pokusíme se je načíst do.

<sup>2</sup> některé funkce EF6 nebudou implementovány v EF Core. Tyto funkce závisí buď na EF6's podkladové model EDM (Entity Data Model) (EDM), nebo jsou komplexní funkce s poměrně nízkou návratností investic. Vždy jsme provedli zpětnou vazbu, ale zatímco EF Core v EF6 povoluje mnoho věcí, není to pro EF Core vhodné pro podporu všech funkcí EF6.

<sup>3</sup> EF Core poskytovatelé databáze, které implementují třetí strany, můžou při aktualizaci na nové hlavní verze EF Core zpozdit. Další informace najdete v tématu [poskytovatelé databáze](../core/providers/index.md) .

<sup>4</sup> poskytovatelé SQL Server Compact a jet fungují jenom v .NET Framework (ne v .NET Core).

### <a name="supported-platforms"></a>Podporované platformy

EF Core 3,1 běží na .NET Core a .NET Framework pomocí .NET Standard 2,0. EF Core 5,0 se ale na .NET Framework nespustí. Další podrobnosti najdete v tématu [platformy](../core/platforms/index.md) .

EF 6.4 běží na .NET Core a .NET Framework, a to prostřednictvím cílení na více platforem.

## <a name="guidance-for-new-applications"></a>Doporučení pro nové aplikace

U všech nových aplikací použijte EF Core pro .NET Core, pokud aplikace nepotřebuje něco, co je [podporováno pouze v .NET Framework](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

## <a name="guidance-for-existing-ef6-applications"></a>Doporučení pro existující aplikace s EF6

EF Core není náhradou za EF6. Přesunutí z EF6 na EF Core bude nejspíš vyžadovat změny aplikace.

Při přesunu aplikace EF6 do .NET Core:
* Používejte EF6, pokud je kód pro přístup k datům stabilní a není pravděpodobně možné vyvíjet nebo potřebovat nové funkce.
* Port, který se má EF Core při vývoji kódu pro přístup k datům nebo pokud aplikace potřebuje nové funkce, které jsou k dispozici pouze v EF Core.
* Pro výkon se také často provádí přenos do EF Core. Ale ne všechny scénáře jsou rychlejší, proto udělejte jako první profilaci.

Další informace najdete v [článku o přechodu z EF6 na EF Core](porting/index.md).
