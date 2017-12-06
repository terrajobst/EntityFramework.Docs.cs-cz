---
title: "Základní EF & EF6 porovnání jednotlivých funkcí"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: 696ff2c8ec788c08880ecb3b07e10dc081b0323b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>Porovnání jednotlivých funkcí EF jádra a EF6

Následující tabulka porovnává funkce dostupné v EF jádra a EF6. Je určena k dát srovnání a není seznamu všechny funkce nebo se pokuste o uvádí podrobné informace o možných rozdíly mezi jak stejné funkce funguje.

Základní EF sloupec obsahuje číslo verze produktu, ve kterém se nejdřív objevil funkci.

| **Vytvoření modelu** |**EF 6** |**EF jádra** |
|-|-|-|
| Základní třída mapování                         | Ano | 1.0 |
| Konvence                                 | Ano | 1.0 |
| Vlastní konvence                          | Ano | 1.0 (částečné) |
| Datové poznámky                            | Ano | 1.0 |
| Rozhraní Fluent API                                  | Ano | 1.0 |
| Dědičnosti: Tabulky na hierarchii (TPH)      | Ano | 1.0 |
| Dědičnosti: Tabulky na typ (TPT)           | Ano |     |
| Dědičnosti: Tabulky na konkrétní třídy (TPC) | Ano |     |
| Vlastnosti stínové stavu                     |     | 1.0 |
| Alternativní klíče                              |     | 1.0 |
| M n bez připojení k entity            | Ano |     |
| Generování klíče: databáze                    | Ano | 1.0 |
| Generování klíče: klienta                      |     | 1.0 |
| Ve vlastnictví nebo komplexní typy                         | Ano | 2.0 |
| Prostorová data                                | Ano |     |
| Grafické vizualizace modelu            | Ano |     |
| Model grafického editoru                      | Ano |     |
| Model formátu: kódu                          | Ano | 1.0 |
| Model formátu: EDMX (XML)                    | Ano |     |
| Vytvoření modelu z databáze: příkazového řádku    | Ano | 1.0 |
| Vytvoření modelu z databáze: Průvodce VS       | Ano |     |
| Aktualizovat model z databáze                  | Částečné | |
| Filtry globální dotazu                        |     | 2.0 |
| Rozdělení tabulky                             | Ano | 2.0 |
| Rozdělení entity                            | Ano |     |
| Mapování skalární funkce databáze            | Slabé | 2.0 |
| Mapování polí                               |     | 1.1 |
| | | |
| **Dotazování na Data** |**EF6** |**EF jádra** |
| LINQ – dotazy                                | Ano | 1.0 (probíhající pro složité dotazy) |
| Parametr Readable generované SQL                      | Slabé | 1.0 |
| Vyhodnocení smíšený klient server              |     | 1.0 |
| Načítání dat v relaci: přes                 | Ano | 1.0 |
| Načítání dat v relaci: opožděné                  | Ano |     |
| Načítání dat v relaci: explicitní              | Ano | 1.1 |
| Nezpracovaná dotazů SQL: modelu typy                | Ano | 1.0 |
| Nezpracovaná dotazů SQL: nemodelová typy            | Ano |     |
| Nezpracovaná dotazů SQL: skládání s dotazy LINQ        |     | 1.0 |
| Explicitně kompilované dotazy                 | Slabé | 2.0 |
| | | |
| **Ukládání dat** |**EF6** |**EF jádra** |
| Sledování změn: snímku                   | Ano | 1.0 |
| Sledování změn: oznámení               | Ano | 1.0 |
| Přístup k sledovaných stavu                     | Ano | 1.0 |
| Optimistickou metodu souběžného zpracování                      | Ano | 1.0 |
| Transakce                                | Ano | 1.0 |
| Dávkování příkazů                      |     | 1.0 |
| Uložené procedury                            | Ano |     |
| Odpojení graf nízké úrovně rozhraní API           | Slabé | 1.0 |
| Odpojené grafu začátku do konce               |     | 1.0 (částečné) |
| | | |
| **Další funkce** |**EF6** |**EF jádra** |
| Migrace                                  | Ano | 1.0 |
| Vytvoření nebo odstranění databáze rozhraní API             | Ano | 1.0 |
| Počáteční hodnoty data                                   | Ano |     |
| Odolnost připojení                       | Ano | 1.1 |
| Životní cyklus háky (události, zachycením)      | Ano |     |
| Sdružování DbContext                           |     | 2.0 |
| | | |
| **Zprostředkovatelé databáze** |**EF6**|**EF jádra** |
| SQL Server                                  | Ano | 1.0 |
| MySQL                                       | Ano | 1.0 |
| PostgreSQL                                  | Ano | 1.0 |
| Oracle                                      | Ano | 1.0 (pouze placené<sup>(1)</sup>) |
| SQLite                                      | Ano | 1.0 |
| SQL Compact                                 | Ano | 1.0 <sup>(2)</sup> |
| DB2                                         | Ano |     |
| V paměti (pro testování)                      |     | 1.0 |
| | | |
| **Platformy** |**EF6** |**EF jádra** |
| Rozhraní .NET framework (konzole, WinForms, WPF, ASP.NET) | Ano | 1.0 |
| .NET core (konzole, ASP.NET Core)           |     | 1.0 |
| Mono & Xamarin                              |     | 1.0 (Probíhá) |
| UWP                                         |     | 1.0 (Probíhá) |

<sup>1</sup> volného oficiálního zprostředkovatele pro Oracle je pracuje.
<sup>2</sup> SQL Server Compact zprostředkovatel funguje pouze na rozhraní .NET Framework (ne jádro .NET).
