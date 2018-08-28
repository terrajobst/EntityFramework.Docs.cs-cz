---
title: Porovnání EF Core a EF6 jednotlivých funkcí
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: ed6ce51e7560e19e0d572f3d81cef7cbb310beec
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995332"
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>Porovnání EF Core a EF6 jednotlivých funkcí

Následující tabulka obsahuje porovnání funkcí, které jsou k dispozici v EF Core a EF6. Jeho účelem je poskytnout základní porovnání a není seznamu všechny funkce nebo se pokuste o uvádí podrobné informace o možných rozdíly mezi jak stejné funkce.

EF Core sloupec obsahuje číslo verze produktu, ve kterém nejprve funkci objevily.

| **Vytvoření modelu**                                  | **EF 6** | **EF Core**                           |
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
| Prostorová data                                          | Ano      |                                       |
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
|                                                       |          |                                       |
| **Dotazy na data**                                     | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Ukládání dat**                                       | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Další funkce**                                    | **EF6**  | **EF Core**                           |
| Migrace                                            | Ano      | 1.0                                   |
| Databáze vytváření/odstraňování rozhraní API                       | Ano      | 1.0                                   |
| Počáteční hodnota dat                                             | Ano      | 2.1                                   |
| Odolnost připojení                                 | Ano      | 1.1                                   |
| Životní cyklus zavěšení (události, zachycení)                | Ano      |                                       |
| Jednoduché protokolování (Database.Log)                         | Ano      |                                       |
| Sdružování DbContext                                     |          | 2.0                                   |
|                                                       |          |                                       |
| **Poskytovatelé databází**                                | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Platformy**                                         | **EF6**  | **EF Core**                           |
| Rozhraní .NET framework (konzola, WinForms, WPF, technologii ASP.NET)      | Ano      | 1.0                                   |
| .NET core (konzola, ASP.NET Core)                     |          | 1.0                                   |
| Mono & Xamarin                                        |          | 1.0 (Probíhá)                     |
| UWP                                                   |          | 1.0 (Probíhá)                     |

<sup>1</sup> je aktuálně placené zprostředkovatele k dispozici. Bezplatné oficiálního zprostředkovatele pro Oracle se právě zpracovává.
<sup>2</sup> tohoto zprostředkovatele funguje pouze na rozhraní .NET Framework (ne na .NET Core).
