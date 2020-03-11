---
title: Protokol změn ovlivňujících poskytovatele – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: b911a2da493e20c4e4ce6f1e25024bd0efd38b44
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417820"
---
# <a name="provider-impacting-changes"></a>Změny s dopadem na poskytovatele

Tato stránka obsahuje odkazy na žádosti o přijetí změn provedené v úložišti EF Core, které mohou vyžadovat, aby autoři jiných poskytovatelů databáze reagovali. Záměrem je poskytnout výchozí bod autorům stávajících poskytovatelů databází třetích stran při aktualizaci poskytovatele na novou verzi.

Tento protokol Začínáme změnami z 2,1 na 2,2. Před 2,1 jsme použili [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) a [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) popisky pro naše problémy a žádosti o přijetí změn.

## <a name="22-----30"></a>2.2 ---> 3.0

Počítejte s tím, že mnoho [změn na úrovni aplikace](../what-is-new/ef-core-3.0/breaking-changes.md) bude mít vliv i na poskytovatele.

* <https://github.com/aspnet/EntityFrameworkCore/pull/14022>
  * Odebrala se zastaralá rozhraní API a sbalená volitelná parametr přetížení.
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* <https://github.com/aspnet/EntityFrameworkCore/pull/14589>
  * Odebrána zastaralá rozhraní API
* <https://github.com/aspnet/EntityFrameworkCore/pull/15044>
  * Podtřídy CharTypeMapping byly pravděpodobně přerušeny kvůli změnám chování vyžadovaným pro opravu několika chyb v základní implementaci.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15090>
  * Přidali jsme základní třídu pro IDatabaseModelFactory a aktualizovali ji tak, aby používala objekt parametr k omezení budoucích přerušení.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15123>
  * Používané objekty parametrů v MigrationsSqlGenerator k omezení budoucích přerušení.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14972>
  * Explicitní konfigurace úrovní protokolu vyžaduje některé změny rozhraní API, které můžou poskytovatelé používat. Konkrétně platí, že pokud poskytovatelé používají infrastrukturu protokolování přímo, pak tato změna může toto použití poškodit. I poskytovatelé, kteří používají infrastrukturu (které budou veřejné), budou muset dál odvodit z `LoggingDefinitions` nebo `RelationalLoggingDefinitions`. Příklady najdete v tématu poskytovatelé SQL Server a v paměti.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15091>
  * Řetězce prostředků Core, relační a abstrakce jsou teď veřejné.
  * `CoreLoggerExtensions` a `RelationalLoggerExtensions` jsou teď veřejné. Poskytovatelé by měli používat tato rozhraní API při protokolování událostí, které jsou definovány na úrovni základní nebo relační. Nepoužívejte přímý přístup k prostředkům protokolování; Tyto jsou pořád interní.
  * `IRawSqlCommandBuilder` se změnila ze služby s jedním oborem na službu s oborem.
  * `IMigrationsSqlGenerator` se změnila ze služby s jedním oborem na službu s oborem.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14706>
  * Infrastruktura pro vytváření relačních příkazů byla veřejná, takže ji mohou bezpečně využívat poskytovatelé a jejich refaktoring mírně.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14733>
  * `ILazyLoader` se změnila z oboru na přechodovou službu.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14610>
  * `IUpdateSqlGenerator` se změnila z oboru na službu typu singleton.
  * Odebrali jsme taky `ISingletonUpdateSqlGenerator`
* <https://github.com/aspnet/EntityFrameworkCore/pull/15067>
  * Mnoho interního kódu, který používali poskytovatelé, je teď zveřejněné.
  * Neměl by již být necssary na odkaz `IndentedStringBuilder`, protože byl vytvořen z místa, ve kterém je vystavený.
  * Použití `NonCapturingLazyInitializer` by se mělo nahradit `LazyInitializer` z BCL
* <https://github.com/aspnet/EntityFrameworkCore/pull/14608>
  * Tato změna je plně popsaná v dokumentu o přerušujících změny aplikace. U zprostředkovatelů to může být ovlivněno, protože testování jádra EF může často vést k tomuto problému, takže se testovací infrastruktura změnila tak, aby to bylo méně pravděpodobná.
* <https://github.com/aspnet/EntityFrameworkCore/issues/13961>
  * `EntityMaterializerSource` bylo zjednodušené.
* <https://github.com/aspnet/EntityFrameworkCore/pull/14895>
  * Převod StartsWith se změnil způsobem, který poskytovatelé můžou chtít nebo potřebují reagovat.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15168>
  * Služby sady konvencí se změnily. Poskytovatelé by teď měli dědit buď z "ProviderConventionSet", nebo "RelationalConventionSet".
  * Vlastní nastavení lze přidat prostřednictvím `IConventionSetCustomizer` Services, ale je určeno pro použití jinými rozšířeními, nikoli poskytovateli.
  * Konvence používané v modulu runtime by se měly přeložit z `IConventionSetBuilder`.
* <https://github.com/aspnet/EntityFrameworkCore/pull/15288>
  * Osazení dat se refaktoruje na veřejné rozhraní API, aby nedocházelo k nutnosti používat interní typy. To by mělo mít vliv pouze na nerelační zprostředkovatele, protože osazení je zpracováváno základní relační třídou pro všechny relační zprostředkovatele.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Pouze změny testů

* <https://github.com/aspnet/EntityFrameworkCore/pull/12057> – povolí přizpůsobitelný delimeters SQL v testech
  * Testování změn, které umožňují nestriktní porovnání s plovoucí desetinnou čárkou v BuiltInDataTypesTestBase
  * Testování změn povolujících opakované použití testů dotazů s různými delimeters SQL
* <https://github.com/aspnet/EntityFrameworkCore/pull/12072> – Přidání testů DbFunction do testů relační specifikace
  * Tyto testy lze spustit u všech poskytovatelů databáze.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12362> – vyčištění asynchronního testu
  * Odebrání volání `Wait`, nepotřebného asynchronního a přejmenování některých testovacích metod
* <https://github.com/aspnet/EntityFrameworkCore/pull/12666> – infrastruktura testů pro sjednocení
  * Přidání `CreateListLoggerFactory` a odebrání některé předchozí infrastruktury protokolování, která bude vyžadovat, aby poskytovatelé pomocí těchto testů reagovali
* <https://github.com/aspnet/EntityFrameworkCore/pull/12500> – spuštění více testů dotazů synchronně a asynchronně
  * Názvy a faktoringy testů se změnily, což vyžaduje, aby poskytovatelé pomocí těchto testů reagovali.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12766> – přejmenování navigace v modelu ComplexNavigations
  * Poskytovatelé, kteří používají tyto testy, možná budou potřebovat reagovat.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12141> – vrátí kontext do fondu namísto funkce disposing ve funkčních testech.
  * Tato změna zahrnuje nějaký testovací refaktoring, který může vyžadovat, aby poskytovatelé reagovali.

### <a name="test-and-product-code-changes"></a>Testování a změny kódu produktu

* <https://github.com/aspnet/EntityFrameworkCore/pull/12109> – konsolidace metod RelationalTypeMapping. Clone
  * Změny v 2,1 pro RelationalTypeMapping povolené pro zjednodušení v odvozených třídách. Nevěříme, že došlo k přerušení poskytovatelům, ale poskytovatelé můžou tuto změnu využít v odvozených třídách mapování typu.
* <https://github.com/aspnet/EntityFrameworkCore/pull/12069> – tagované nebo pojmenované dotazy
  * Přidá infrastrukturu pro označování dotazů LINQ a tyto značky se zobrazí jako komentáře v SQL. To může vyžadovat, aby poskytovatelé reagovali při generování SQL.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13115> – podpora prostorových dat prostřednictvím NTS
  * Povoluje registraci typů mapování a překladatelů členů mimo poskytovatele.
    * Zprostředkovatelé musí volat základ. FindMapping () ve své implementaci ITypeMappingSource, aby fungoval
  * Podle tohoto vzoru přidejte prostorovou podporu pro vašeho poskytovatele, který je konzistentní napříč poskytovateli.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13199> – přidat vylepšené ladění pro vytváření poskytovatele služeb
  * Umožňuje DbContextOptionsExtensions implementovat nové rozhraní, které může lidem pomáhat pochopit, proč se poskytovatel interních služeb znovu sestavil.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13289> – přidá rozhraní API CanConnect pro použití kontrolami stavu.
  * Tato žádost o přijetí změn přidává koncept `CanConnect`, který budou používat ASP.NET Core kontroly stavu k určení, jestli je databáze k dispozici. Ve výchozím nastavení relační implementace volá pouze `Exist`, ale poskytovatelé můžou v případě potřeby implementovat něco jiného. Nerelační poskytovatelé budou muset implementovat nové rozhraní API, aby bylo možné použít kontrolu stavu.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13306> – aktualizovat základní RelationalTypeMapping, aby se nastavila velikost DbParameter
  * Zastaví velikost nastavení ve výchozím nastavení, protože může způsobit zkrácení. Poskytovatelé můžou potřebovat přidat vlastní logiku, pokud je potřeba nastavit velikost.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13372>-RevEng: vždy zadat typ sloupce pro desetinné sloupce
  * Vždy konfigurujte typ sloupce pro desetinné místo v kódu vygenerovaného pomocí konvence namísto konfigurace podle konvence.
  * Poskytovatelé by na svém konci neměli vyžadovat žádné změny.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13469> – přidá CaseExpression pro generování výrazů případu SQL.
* <https://github.com/aspnet/EntityFrameworkCore/pull/13648> – přidá možnost zadat mapování typů na SqlFunctionExpression ke zlepšení odvození typu úložiště argumentů a výsledků.
