---
title: Protokol změn vliv na poskytovatele – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 44b200223153fca44cb2cfa3e78b3bedc7b4a552
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821332"
---
# <a name="provider-impacting-changes"></a>Změny s dopadem na poskytovatele

Tato stránka obsahuje odkazy na EF Core úložiště, které můžou vyžadovat autoři ostatní poskytovatelé databází reagovat žádostmi o přijetí změn. Záměrem je poskytnout výchozí bod pro autory existující poskytovatelé databází výrobců při aktualizaci jejich zprostředkovatele na novou verzi.

Začínáme tento protokol se změnami z 2.1 2.2. Před 2.1 jsme použili [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) a [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) popisky na naše problémy a žádosti o přijetí změn.

### <a name="21-----22"></a>2.1---> 2.2

#### <a name="test-only-changes"></a>Změny jen pro test

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 – Povolte přizpůsobitelné dají SQL v testech
  * Otestovat změny, které umožňují nepřísném plovoucího bodu porovnání v BuiltInDataTypesTestBase
  * Test změny, které umožňují testů dotaz znovu použít se dají jiný SQL
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Přidáte DbFunction testy do relační specifikace testů
  * Tak, aby se dají spustit tyto testy pro všechny poskytovatele databáze
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Vyčištění testovacího asynchronní
  * Odebrat `Wait` nepotřebné asynchronní volání a přejmenovat některé testovací metody
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Sjednocení protokolování testovací infrastrukturu
  * Přidání `CreateListLoggerFactory` a odebrat některé předchozí infrastruktury protokolování, který bude vyžadovat zprostředkovatelů pomocí tyto testy reagovat
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Spuštění více testů dotazu synchronně i asynchronně
  * Názvy testů a které budou zohledňovat došlo ke změně, což bude vyžadovat zprostředkovatelů pomocí tyto testy reagovat
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Přejmenování navigaci v modelu ComplexNavigations
  * Možná bude nutné zprostředkovatelů tyto testy pomocí react
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Vrátíte kontext do fondu namísto disposing v funkční testy
  * Tato změna zahrnuje některé refaktoring testů, může být nutné poskytovatelé react


#### <a name="test-and-product-code-changes"></a>Změny kódu testu a produktu

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Konsolidovat RelationalTypeMapping.Clone metody
  * Změny v 2.1, aby RelationalTypeMapping povolené pro zjednodušení v odvozených třídách. Nevěříme to byla rozbíjející poskytovatelů, ale zprostředkovatelé můžete využít výhod této změny v jejich odvozený typ mapování třídy.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Označených nebo pojmenovaných dotazů
  * Přidá infrastruktury pro označování dotazů LINQ a mají tyto visačky se zobrazují jako komentáře v SQL. To může vyžadovat poskytovatelé reagovat v generování SQL.
* https://github.com/aspnet/EntityFrameworkCore/pull/13115 -Podpora prostorových dat prostřednictvím chny Zarážky
  * Umožňuje mapování typů a členů překladatele k registraci mimo zprostředkovatele
    * Poskytovatelé musí volat základní. FindMapping() v jejich provádění ITypeMappingSource, aby to fungovalo
  * Postupovat podle tohoto vzoru přidává prostorových na svého poskytovatele, který je konzistentní napříč poskytovatelů.

