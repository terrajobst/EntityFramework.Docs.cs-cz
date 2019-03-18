---
title: Protokol změn vliv na poskytovatele – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 70fe2d934901f5366c96904b08f49a35f6590b47
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131397"
---
# <a name="provider-impacting-changes"></a>Změny s dopadem na poskytovatele

Tato stránka obsahuje odkazy na EF Core úložiště, které můžou vyžadovat autoři ostatní poskytovatelé databází reagovat žádostmi o přijetí změn. Záměrem je poskytnout výchozí bod pro autory existující poskytovatelé databází výrobců při aktualizaci jejich zprostředkovatele na novou verzi.

Začínáme tento protokol se změnami z 2.1 2.2. Před 2.1 jsme použili [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) a [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) popisky na naše problémy a žádosti o přijetí změn.

## <a name="22-----30"></a>2.2 ---> 3.0

* https://github.com/aspnet/EntityFrameworkCore/pull/14022
  * Odebraný zastaralá rozhraní API a přetížení s sbaleném volitelný parametr
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* https://github.com/aspnet/EntityFrameworkCore/pull/14589
  * Odebraný zastaralá rozhraní API
* https://github.com/aspnet/EntityFrameworkCore/pull/15044
  * Podtřídy třídy CharTypeMapping byla pravděpodobně přerušena z důvodu změny chování potřeba opravit několik chyb ve základní implementaci.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Změny jen pro test

* [https://github.com/aspnet/EntityFrameworkCore/pull/12057](https://github.com/aspnet/EntityFrameworkCore/pull/12057) – Povolte přizpůsobitelné dají SQL v testech
  * Otestovat změny, které umožňují nepřísném plovoucího bodu porovnání v BuiltInDataTypesTestBase
  * Test změny, které umožňují testů dotaz znovu použít se dají jiný SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/12072](https://github.com/aspnet/EntityFrameworkCore/pull/12072) -Přidáte DbFunction testy do relační specifikace testů
  * Tak, aby se dají spustit tyto testy pro všechny poskytovatele databáze
* [https://github.com/aspnet/EntityFrameworkCore/pull/12362](https://github.com/aspnet/EntityFrameworkCore/pull/12362) -Vyčištění testovacího asynchronní
  * Odebrat `Wait` nepotřebné asynchronní volání a přejmenovat některé testovací metody
* [https://github.com/aspnet/EntityFrameworkCore/pull/12666](https://github.com/aspnet/EntityFrameworkCore/pull/12666) -Sjednocení protokolování testovací infrastrukturu
  * Přidání `CreateListLoggerFactory` a odebrat některé předchozí infrastruktury protokolování, který bude vyžadovat zprostředkovatelů pomocí tyto testy reagovat
* [https://github.com/aspnet/EntityFrameworkCore/pull/12500](https://github.com/aspnet/EntityFrameworkCore/pull/12500) -Spuštění více testů dotazu synchronně i asynchronně
  * Názvy testů a které budou zohledňovat došlo ke změně, což bude vyžadovat zprostředkovatelů pomocí tyto testy reagovat
* [https://github.com/aspnet/EntityFrameworkCore/pull/12766](https://github.com/aspnet/EntityFrameworkCore/pull/12766) -Přejmenování navigaci v modelu ComplexNavigations
  * Možná bude nutné zprostředkovatelů tyto testy pomocí react
* [https://github.com/aspnet/EntityFrameworkCore/pull/12141](https://github.com/aspnet/EntityFrameworkCore/pull/12141) -Vrátíte kontext do fondu namísto disposing v funkční testy
  * Tato změna zahrnuje některé refaktoring testů, může být nutné poskytovatelé react


### <a name="test-and-product-code-changes"></a>Změny kódu testu a produktu

* [https://github.com/aspnet/EntityFrameworkCore/pull/12109](https://github.com/aspnet/EntityFrameworkCore/pull/12109) -Konsolidovat RelationalTypeMapping.Clone metody
  * Změny v 2.1, aby RelationalTypeMapping povolené pro zjednodušení v odvozených třídách. Nevěříme to byla rozbíjející poskytovatelů, ale zprostředkovatelé můžete využít výhod této změny v jejich odvozený typ mapování třídy.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12069](https://github.com/aspnet/EntityFrameworkCore/pull/12069) -Označených nebo pojmenovaných dotazů
  * Přidá infrastruktury pro označování dotazů LINQ a mají tyto visačky se zobrazují jako komentáře v SQL. To může vyžadovat poskytovatelé reagovat v generování SQL.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13115](https://github.com/aspnet/EntityFrameworkCore/pull/13115) -Podpora prostorových dat prostřednictvím chny Zarážky
  * Umožňuje mapování typů a členů překladatele k registraci mimo zprostředkovatele
    * Poskytovatelé musí volat základní. FindMapping() v jejich provádění ITypeMappingSource, aby to fungovalo
  * Postupovat podle tohoto vzoru přidává prostorových na svého poskytovatele, který je konzistentní napříč poskytovatelů.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13199](https://github.com/aspnet/EntityFrameworkCore/pull/13199) -Přidat vylepšené ladění pro vytvoření poskytovatele služby
  * Umožňuje DbContextOptionsExtensions implementovat nové rozhraní, která pomáhá uživatelům pochopit, proč se opětovně sestaven vnitřní chybě služby zprostředkovatele
* [https://github.com/aspnet/EntityFrameworkCore/pull/13289](https://github.com/aspnet/EntityFrameworkCore/pull/13289) -Přidá CanConnect rozhraní API pro použití kontroly stavu
  * Tato žádost o přijetí změn přidá konceptu `CanConnect` který budou používat ASP.NET Core stavu kontroly k určení, jestli je databáze k dispozici. Ve výchozím nastavení, relační implementace jen volá `Exist`, ale zprostředkovatelé můžete implementovat něco jiného v případě potřeby. Nerelační poskytovatelé muset implementovat nové rozhraní API v pořadí pro kontrolu stavu, který má být použitelná.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13306](https://github.com/aspnet/EntityFrameworkCore/pull/13306) -Aktualizace základní RelationalTypeMapping nenastavovat DbParameter velikost
  * Zastavte, protože to může způsobit zkrácení nastavení velikosti ve výchozím nastavení. Poskytovatelé pravděpodobně nutné přidat své vlastní logiky, pokud velikost musí být nastavena.
* https://github.com/aspnet/EntityFrameworkCore/pull/13372 -RevEng: Vždy určit typ sloupce pro desítkové sloupce
  * Vždy Konfigurujte typ sloupce pro desítkové sloupce automaticky generovaný kód, místo konfigurace podle konvence.
  * Poskytovatelé, neměli byste potřebovat všechny změny na své straně.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13469](https://github.com/aspnet/EntityFrameworkCore/pull/13469) -Přidá CaseExpression pro generování výrazy SQL CASE
* [https://github.com/aspnet/EntityFrameworkCore/pull/13648](https://github.com/aspnet/EntityFrameworkCore/pull/13648) -Přidává možnost zadat mapování typů SqlFunctionExpression ke zlepšení odvozování typů úložiště argumenty a výsledky.
