---
title: Porovnat EF6 a EF Core
description: Pokyny, jak si vybrat mezi EF6 a EF Core.
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: e28c7d0299e5089f56fb0795d00917cfc30f5cf1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419647"
---
# <a name="compare-ef-core--ef6"></a>Porovnání EF Core a EF6

## <a name="ef-core"></a>EF Core

Entity Framework Core ([EF Core](../core/index.md)) je moderní mapovač objektové databáze pro rozhraní .NET. Podporuje linq dotazy, sledování změn, aktualizace a migrace schématu.

EF Core funguje s SQL Server/SQL Azure, SQLite, Azure Cosmos DB, MySQL, PostgreSQL a mnoha dalšími databázemi prostřednictvím [modelu pluginů poskytovatele databáze](../core/providers/index.md).

## <a name="ef6"></a>EF6

Entity Framework 6 ([EF6](../ef6/index.md)) je objekt-relační mapovač určený pro rozhraní .NET Framework, ale s podporou .NET Core. EF6 je stabilní, podporovaný produkt, ale již není aktivně vyvíjen.

## <a name="feature-comparison"></a>Porovnání funkcí

EF Core nabízí nové funkce, které nebudou implementovány v EF6. Však ne všechny funkce EF6 jsou aktuálně implementovány v EF Core.

Následující tabulky porovnávají funkce dostupné v EF Core a EF6. Toto je porovnání na vysoké úrovni a neuvádí všechny funkce nebo vysvětlit rozdíly mezi stejnou funkci v různých verzích EF.

Sloupec EF Core označuje verzi produktu, ve kterém se funkce poprvé objevila.

### <a name="creating-a-model"></a>Vytvoření modelu

| **Funkce**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Základní mapování tříd                                   | Ano      | 1.0                                   |
| Konstruktory s parametry                          |          | 2.1                                   |
| Převody hodnoty vlastnosti                            |          | 2.1                                   |
| Mapované typy bez klíčů                             |          | 2.1                                   |
| Zásady                                           | Ano      | 1.0                                   |
| Vlastní konvence                                    | Ano      | 1.0 (částečná; [#214](https://github.com/dotnet/efcore/issues/214)) |
| Datové poznámky                                      | Ano      | 1.0                                   |
| Fluent API                                            | Ano      | 1.0                                   |
| Dědičnost: Tabulka na hierarchii (TPH)                | Ano      | 1.0                                   |
| Dědičnost: Tabulka podle typu (TPT)                     | Ano      | Plánováno na 5,0 ([#2266](https://github.com/dotnet/efcore/issues/2266)) |
| Dědičnost: Tabulka na konkrétní třídu (TPC)           | Ano      | Roztažení pro 5,0 ([#3170](https://github.com/dotnet/efcore/issues/3170)) <sup>(1)</sup> |
| Vlastnosti stavu stínů                               |          | 1.0                                   |
| Alternativní klávesy                                        |          | 1.0                                   |
| Navigace n:N                              | Ano      | Plánováno na 5,0 ([#19003](https://github.com/dotnet/efcore/issues/19003)) |
| Entita N:N bez spojení                      | Ano      | Na nevyřízených položkách ([#1368](https://github.com/dotnet/efcore/issues/1368)) |
| Generování klíčů: Databáze                              | Ano      | 1.0                                   |
| Generování klíčů: Klient                                |          | 1.0                                   |
| Komplexní/vlastněné typy                                   | Ano      | 2.0                                   |
| Prostorová data                                          | Ano      | 2,2                                   |
| Formát modelu: Kód                                    | Ano      | 1.0                                   |
| Vytvoření modelu z databáze: Příkazový řádek              | Ano      | 1.0                                   |
| Aktualizovat model z databáze                            | Částečné  | Na nevyřízených položkách ([#831](https://github.com/dotnet/efcore/issues/831)) |
| Globální filtry dotazů                                  |          | 2.0                                   |
| Rozdělení tabulky                                       | Ano      | 2.0                                   |
| Rozdělení entity                                      | Ano      | Roztažení pro 5,0 ([#620](https://github.com/dotnet/efcore/issues/620)) <sup>(1)</sup> |
| Mapování skalárních funkcí databáze                      | Slabé     | 2.0                                   |
| Mapování pole                                         |          | 1.1                                   |
| Typy odkazů s možnou hodnotou Null (C# 8.0)                     |          | 3.0                                   |
| Grafická vizualizace modelu                      | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |
| Grafický editor modelů                                | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |
| Formát modelu: EDMX (XML)                              | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |
| Vytvořit model z databáze: Průvodce VS                 | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |

### <a name="querying-data"></a>Dotazování na data

| **Funkce**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ – dotazy                                          | Ano      | 1.0                                   |
| Čitelné generované sql                                | Slabé     | 1.0                                   |
| GroupBy překlad                                   | Ano      | 2.1                                   |
| Načítání souvisejících dat: Dychtivý                           | Ano      | 1.0                                   |
| Načítání souvisejících dat: Eager načítání odvozených typů |          | 2.1                                   |
| Načítání souvisejících dat: Opožděné                            | Ano      | 2.1                                   |
| Načítání souvisejících dat: Explicitní                        | Ano      | 1.1                                   |
| Nezpracované dotazy SQL: Typy entit                         | Ano      | 1.0                                   |
| Nezpracované dotazy SQL: Typy bezklíčových entit                 | Ano      | 2.1                                   |
| Nezpracované dotazy SQL: Skládání s LINQ                  |          | 1.0                                   |
| Explicitně zkompilované dotazy                           | Slabé     | 2.0                                   |
| await foreach (C# 8.0)                                |          | 3.0                                   |
| Textový dotazovací jazyk (Entity SQL)                | Ano      | Žádná podpora není plánována <sup>(2)</sup>     |

### <a name="saving-data"></a>Ukládání dat

| **Funkce**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Sledování změn: Snímek                             | Ano      | 1.0                                   |
| Sledování změn: Oznámení                         | Ano      | 1.0                                   |
| Sledování změn: Proxy servery                              | Ano      | Sloučeno pro 5.0 ([#10949](https://github.com/dotnet/efcore/issues/10949)) |
| Přístup ke sledovanému stavu                               | Ano      | 1.0                                   |
| Optimistická souběžnost                                | Ano      | 1.0                                   |
| Transakce                                          | Ano      | 1.0                                   |
| Dávkování výkazů                                |          | 1.0                                   |
| Mapování uložené procedury                              | Ano      | Na nevyřízených položkách ([#245](https://github.com/dotnet/efcore/issues/245)) |
| Odpojená nízkoúrovňová api grafu                     | Slabé     | 1.0                                   |
| Odpojený graf End-to-end                         |          | 1.0 (částečná; [#5536](https://github.com/dotnet/efcore/issues/5536)) |

### <a name="other-features"></a>Další funkce

| **Funkce**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrace                                            | Ano      | 1.0                                   |
| Vytváření a odstraňování api pro vytváření a odstraňování databáze                       | Ano      | 1.0                                   |
| Údaje o osivu                                             | Ano      | 2.1                                   |
| Odolnost připojení                                 | Ano      | 1.1                                   |
| Zachycovače                                          | Ano      | 3.0                                   |
| Události                                                | Ano      | 3.0 (částečné; [#626](https://github.com/dotnet/efcore/issues/626)) |
| Jednoduché protokolování (Database.Log)                         | Ano      | Sloučeno pro 5.0 ([#1199](https://github.com/dotnet/efcore/issues/1199)) |
| Sdružování kontextu DbContext                                     |          | 2.0                                   |

### <a name="database-providers-sup3sup"></a>Poskytovatelé databází <sup>(3)</sup>

| **Funkce**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Ano      | 1.0                                   |
| MySQL                                                 | Ano      | 1.0                                   |
| PostgreSQL                                            | Ano      | 1.0                                   |
| Oracle                                                | Ano      | 1.0                                   |
| SQLite                                                | Ano      | 1.0                                   |
| SQL Server Compact                                    | Ano      | 1,0 <sup>(4)</sup>                    |
| DB2                                                   | Ano      | 1.0                                   |
| Firebird                                              | Ano      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2,0 <sup>(4)</sup>                    |
| Azure Cosmos DB                                       |          | 3.0                                   |
| V paměti (pro testování)                               |          | 1.0                                   |

<sup>1</sup> Stretch cíle nejsou pravděpodobné, že bude dosaženo pro dané vydání. Pokud však věci půjdou dobře, pokusíme se je vytáhnout.

<sup>2</sup> Některé funkce EF6 nebudou implementovány v EF Core. Tyto funkce závisí buď na podkladovém modelu dat entit EF6 (EDM) a/nebo jsou složitými funkcemi s relativně nízkou návratností investic. Vždy vítáme zpětnou vazbu, ale zatímco EF Core umožňuje mnoho věcí, které nejsou možné v EF6, naopak není možné, aby EF Core podporoval všechny funkce EF6.

<sup>3</sup> Poskytovatelé základních databází EF implementovaní třetími stranami mohou mít zpoždění při aktualizaci na nové hlavní verze EF Core. Další informace naleznete v [tématu Zprostředkovatelé databáze.](../core/providers/index.md)

<sup>4</sup> Zprostředkovatelé SQL Server Compact a Jet pracují pouze v rozhraní .NET Framework (nikoli na jádru .NET).

### <a name="supported-platforms"></a>Podporované platformy

EF Core 3.1 běží na rozhraní .NET Core a .NET Framework pomocí rozhraní .NET Standard 2.0. EF Core 5.0 však nebude spuštěna v rozhraní .NET Framework. Další [podrobnosti najdete v tématu Platformy.](../core/platforms/index.md)

EF6.4 běží na rozhraní .NET Core a .NET Framework prostřednictvím vícenásobného cílení.

## <a name="guidance-for-new-applications"></a>Pokyny pro nové aplikace

Použití EF Core na .NET Core pro všechny nové aplikace, pokud aplikace potřebuje něco, co je [podporováno pouze na rozhraní .NET Framework](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

## <a name="guidance-for-existing-ef6-applications"></a>Pokyny pro stávající aplikace EF6

EF Core není náhradou za EF6. Přechod z EF6 na EF Core bude pravděpodobně vyžadovat změny vaší aplikace.

Při přesunu aplikace EF6 do jádra .NET:
* Pokračujte v používání EF6, pokud je přístupový kód dat stabilní a není pravděpodobné, že se bude vyvíjet nebo potřebovat nové funkce.
* Port na EF Core, pokud se vyvíjí kód přístupu k datům nebo pokud aplikace potřebuje nové funkce dostupné jenom v EF Core.
* Portování na EF Core se také často provádí pro výkon. Ne všechny scénáře jsou však rychlejší, takže některé profilování jako první.

Další informace naleznete [v tématu Porting from EF6 to EF Core.](porting/index.md)
