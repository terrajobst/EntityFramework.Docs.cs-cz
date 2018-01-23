---
title: "Testování s InMemory - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="testing-with-inmemory"></a>Testování s InMemory

Zprostředkovatel InMemory je užitečné, když chcete testovat komponent pomocí něco, co blíží připojení k databázi skutečné bez režie skutečné databázových operací.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) na Githubu.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory není relační databáze

EF základní databáze zprostředkovatelé nemusí být relačních databází. InMemory je navržený jako databázi obecné účely pro testování a není navržen tak, aby napodoboval relační databáze.

Některé příklady tohoto:
* InMemory vám umožní uložit data, která by způsobila porušení omezení referenční integrity v relační databázi.

* Pokud používáte DefaultValueSql(string) pro vlastnost v modelu, to je rozhraní API, relační databáze a nebude mít žádný vliv, při spuštění proti InMemory.

> [!TIP]  
> Pro mnoho testovací účely nebude podstatné tyto rozdíly. Ale pokud chcete k testování proti něco, co se chová podobně jako hodnota true, relační databáze, pak zvažte použití [SQLite v paměti režimu](sqlite.md).

## <a name="example-testing-scenario"></a>Příklad scénáře testování

Vezměte v úvahu následující služby, který umožňuje aplikaci provádět některé operace související s blogy. Interně používá `DbContext` která se připojuje k databázi systému SQL Server. Je užitečné odkládacího souboru tímto kontextem za účelem připojení k databázi InMemory, aby jsme můžete zapsat efektivní testy pro tuto službu bez nutnosti změnit kód, nebo můžete provést spoustu práce vytvoření testu dvojité kontextu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Příprava váš kontext

### <a name="avoid-configuring-two-database-providers"></a>Vyhnout konfiguraci dva poskytovatelé databáze

Ve vašich testech budete externě nakonfigurovat kontext pro použití poskytovatele InMemory. Pokud konfigurujete poskytovatele databáze přepsáním `OnConfiguring` v kontextu, budete muset přidat podmíněného kód, který Ujistěte se, pokud nebylo bylo nakonfigurováno pouze konfiguraci poskytovatele za databáze.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Pokud používáte ASP.NET Core, neměli vzhledem k tomu, že poskytovatel databáze je již nakonfigurována mimo kontext (v souboru Startup.cs) potřebovat tento kód.

### <a name="add-a-constructor-for-testing"></a>Přidejte konstruktor pro testování

Nejjednodušší způsob, jak povolit testování proti do jiné databáze je cílem upravit váš kontext vystavit konstruktor, který přijímá `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`informuje kontextu veškeré jeho nastavení, jako je například databázi, ke které připojit k. Toto je stejný objekt, který je sestavena spuštění metody OnConfiguring ve vašem kontextu.

## <a name="writing-tests"></a>Zápis testů

Klíč k testování s tímto poskytovatelem přístup je schopnost říct kontext, který má použít poskytovatele InMemory a řízení rozsahu databázi v paměti. Obvykle budete chtít vyčištění databáze pro každou metodu test.

Tady je příklad testovací třídu, která používá databázi InMemory. Každá metoda testovací Určuje jedinečný název databáze, což znamená, že každá z metod má svou vlastní databázi InMemory.

>[!TIP]
> Chcete-li použít `.UseInMemoryDatabase()` metoda rozšíření, odkaz na balíček NuGet `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
