---
title: Entity Framework glosář - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 298913891fb372bf57d7504c5a54f1dc83ea1a80
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490680"
---
# <a name="entity-framework-glossary"></a>Entity Framework – glosář
## <a name="code-first"></a>Kód nejprve
Vytvoření modelu Entity Framework pomocí kódu. Můžete cílit na modelu a stávající nebo novou databázi.

## <a name="context"></a>Kontext
Třída, která představuje relaci s databází, umožňuje dotazování a uložit data. Kontext je odvozena z třídy DbContext nebo objektu ObjectContext.

## <a name="convention-code-first"></a>Konvence (Code First)
Pravidlo, které používá Entity Framework k odvození tvar model z vaší třídy.

## <a name="database-first"></a>První databáze
Vytvoření modelu Entity Framework, pomocí EF designeru, zaměřuje na existující databázi.

## <a name="eager-loading"></a>Předběžné načítání
Vzor načítání souvisejících dat, kde dotaz pro jeden typ entity se také načtou související entity jako součást dotazu.

## <a name="ef-designer"></a>EF designeru
Vizuálního návrháře v sadě Visual Studio, která umožňuje vytvoření modelu Entity Framework pomocí polí a čáry.

## <a name="entity"></a>Entity
Třída nebo objekt, který představuje data aplikací, jako jsou zákazníci, výrobky a objednávky.

## <a name="entity-data-model"></a>Entity Data Model
Model, který popisuje entit a vztahů mezi nimi. EF používá k popisu konceptuální model, proti kterému EDM vývojářské programy. EDM je založena na modelu relace Entity zavedené zotavení po havárii. Petr Svoboda. EDM byla původně vyvinuta primárním cílem stávají common data model napříč sada pro vývojáře a server technologií od Microsoftu. EDM slouží také jako součást protokolu OData.

## <a name="explicit-loading"></a>explicitní načtení
Vzor načítání souvisejících dat, kde jsou načteny související objekty voláním rozhraní API.

## <a name="fluent-api"></a>Rozhraní Fluent API
Rozhraní API, které lze použít ke konfiguraci modelu Code First.

## <a name="foreign-key-association"></a>Přidružení cizího klíče
Přidružení mezi entitami, kde je vlastnost, která představuje klíč, cizí součástí třídy entity závislé. Vlastnost ID kategorie obsahuje například produktu.

## <a name="identifying-relationship"></a>Určení vztahu
Vztah, kde primární klíč entity, objektu zabezpečení je součástí primárního klíče entity závislé. V tento druh vztahu závislé entity nemůže existovat bez instančního objektu entity.

## <a name="independent-association"></a>Nezávislé přidružení
Přidružení mezi entitami tam, kde není žádná vlastnost představující cizí klíč ve třídě závislé entity. Například třída produkt obsahuje vztah ke kategorii, ale žádná vlastnost ID kategorie. Entity Framework sleduje stav přidružení bez ohledu na jejich stav entit na koncích dva přidružení.

## <a name="lazy-loading"></a>Opožděné načtení
Vzor načítání souvisejících dat, kde načteny související objekty jsou automaticky při přístupu k vlastnosti navigace.

## <a name="model-first"></a>První model
Vytvoření modelu Entity Framework, pomocí EF designeru, který potom slouží k vytvoření nové databáze.

## <a name="navigation-property"></a>Navigační vlastnost
Vlastnosti entity, která odkazuje na jinou entitou. Například produkt obsahuje vlastnost navigace kategorie a kategorie obsahuje produkty navigační vlastnost.

## <a name="poco"></a>OBJEKTŮ POCO
Zkratka pro objekt CLR prostý staré. Jednoduché uživatelské třídu, která nemá žádné závislosti pomocí libovolné architektury. V souvislosti s EF třídu entity, která není odvozena od EntityObject, implementuje všechna rozhraní, nebo představuje libovolné atributy definované v EF. Takové entity třídy, které jsou oddělené od rozhraní trvalost se také označují jako "trvalost ignorant".  

## <a name="relationship-inverse"></a>Inverzní relace
V opačném směru relace, například produktu. Kategorie a kategorie. Produkt.

## <a name="self-tracking-entity"></a>Vlastní sledování entity
Entity vytvořené ze šablony generování kódu, který pomáhá s N-Vrstvý vývoj.

## <a name="table-per-concrete-type-tpc"></a>Tabulky na konkrétní typ (TPC)
Metoda mapování dědičnosti, kde každý neabstraktního typu v hierarchii je mapována do samostatné tabulky v databázi.

## <a name="table-per-hierarchy-tph"></a>Za hierarchii tabulky (TPH)
Metoda mapování dědičnosti, kde jsou všechny typy v hierarchii namapovány na stejnou tabulku v databázi. Diskriminátor sloupců, které se používá k určení, jaký typ každý řádek je přidružený.

## <a name="table-per-type-tpt"></a>Za typ tabulky (TPT)
Metoda mapování dědičnosti, kde běžné vlastnosti všech typů v hierarchii jsou namapovány na stejnou tabulku v databázi, ale vlastnosti jedinečné pro jednotlivé typy jsou mapovány do samostatné tabulky.

## <a name="type-discovery"></a>Typ zjišťování
Proces identifikace typů, které by měla být součástí modelu Entity Framework.
