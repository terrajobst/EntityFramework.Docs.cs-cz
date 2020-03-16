---
title: Glosář Entity Framework – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402199"
---
# <a name="entity-framework-glossary"></a>Glosář Entity Framework
## <a name="code-first"></a>Code First
Vytvoření modelu Entity Framework pomocí kódu. Model může cílit na existující databázi nebo novou databázi.

## <a name="context"></a>Kontext
Třída, která představuje relaci s databází a umožňuje dotazování a ukládání dat. Kontext je odvozen z třídy DbContext nebo ObjectContext.

## <a name="convention-code-first"></a>Konvence (Code First)
Pravidlo, které Entity Framework používá k odvození tvaru vašeho modelu z vašich tříd.

## <a name="database-first"></a>Database First
Vytvoření modelu Entity Framework pomocí návrháře EF, který cílí na stávající databázi.

## <a name="eager-loading"></a>Eager načítání
Vzor načítání souvisejících dat, kde dotaz na jeden typ entity také načte související entity jako součást dotazu.

## <a name="ef-designer"></a>Návrhář EF
Vizuální Návrhář v aplikaci Visual Studio, který umožňuje vytvořit Entity Framework model pomocí polí a čar.

## <a name="entity"></a>Entita
Třída nebo objekt reprezentující data aplikace, jako jsou například zákazníci, produkty a objednávky.

## <a name="entity-data-model"></a>Entity Data Model
Model, který popisuje entity a vztahy mezi nimi. EF používá model EDM k popisu koncepčního modelu, proti kterému vývojářské programy. Model EDM sestaví na modelu vztahů mezi entitami, který zavádí Dr. Petra Chen. Model EDM byl původně vyvinut s primárním cílem, který se stane běžným datovým modelem v rámci sady technologií pro vývojáře a server od Microsoftu. Model EDM se používá také jako součást protokolu OData.

## <a name="explicit-loading"></a>explicitní načítání
Vzor načítání souvisejících dat v případě, že související objekty jsou načteny voláním rozhraní API.

## <a name="fluent-api"></a>Rozhraní Fluent API
Rozhraní API, které lze použít ke konfiguraci Code Firstho modelu.

## <a name="foreign-key-association"></a>přidružení cizího klíče
Přidružení mezi entitami, kde vlastnost reprezentující cizí klíč je součástí třídy závislé entity. Například produkt obsahuje vlastnost KódKategorie.

## <a name="identifying-relationship"></a>Identifikace vztahu
Vztah, ve kterém je primární klíč hlavní entity součástí primárního klíče závislé entity. V tomto typu relace nemůže závislá entita existovat bez hlavní entity.

## <a name="independent-association"></a>nezávislé přidružení
Přidružení mezi entitami, kde neexistuje žádná vlastnost reprezentující cizí klíč ve třídě závislé entity. Například třída produktu obsahuje relaci ke kategorii, ale žádnou vlastnost KódKategorie. Entity Framework sleduje stav přidružení nezávisle na stavu entit na obou koncích přidružení.

## <a name="lazy-loading"></a>opožděné načítání
Vzor načítání souvisejících dat, kde jsou související objekty automaticky načteny, když je k dispozici navigační vlastnost.

## <a name="model-first"></a>Model First
Vytvoření modelu Entity Framework pomocí návrháře EF, který je pak použit k vytvoření nové databáze.

## <a name="navigation-property"></a>Navigační vlastnost
Vlastnost entity, která odkazuje na jinou entitu. Například produkt obsahuje navigační vlastnost kategorie a kategorie obsahuje navigační vlastnost Products.

## <a name="poco"></a>POCO
Zkratka pro objekt CLR v prostém starém formátu. Jednoduchá třída uživatele, která nemá žádné závislosti s žádným rozhraním. V kontextu EF Třída entity, která není odvozena od objektů EntityObject, implementuje jakákoli rozhraní nebo přenáší žádné atributy definované v EF. Takové třídy entit, které jsou odděleny od rozhraní trvalosti, jsou také označovány jako "trvalé ignorování".  

## <a name="relationship-inverse"></a>Inverzní relace
Opačný konec relace, například produkt. Kategorie a kategorie. Produktu.

## <a name="self-tracking-entity"></a>entita pro sledování sebe
Entita vytvořená z šablony pro generování kódu, která pomáhá s vývojem na N-vrstvách.

## <a name="table-per-concrete-type-tpc"></a>Pro konkrétní typ tabulky (TPC)
Metoda mapování dědičnosti, kde jsou jednotlivé neabstraktní typy v hierarchii namapovány na samostatnou tabulku v databázi.

## <a name="table-per-hierarchy-tph"></a>Tabulka na hierarchii (TPH)
Metoda mapování dědičnosti, kde jsou všechny typy v hierarchii namapovány na stejnou tabulku v databázi. Ke zjištění, ke kterému typu jsou přidruženy jednotlivé řádky, se používají sloupce diskriminátorů.

## <a name="table-per-type-tpt"></a>Tabulka podle typu (TPT)
Metoda mapování dědičnosti, kde jsou společné vlastnosti všech typů v hierarchii namapovány na stejnou tabulku v databázi, ale vlastnosti jedinečné pro každý typ jsou namapovány na samostatnou tabulku.

## <a name="type-discovery"></a>Zjišťování typů
Proces určení typů, které by měly být součástí modelu Entity Framework.
