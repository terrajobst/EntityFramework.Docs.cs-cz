---
title: Testování s InMemory – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: f814c8955e155688bb5e8d34b9c9f6d24dcc6601
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900259"
---
# <a name="testing-with-inmemory"></a>Testování s InMemory

Zprostředkovatel InMemory je užitečné, když chcete testovat komponenty pomocí nějakého nástroje, které se blíží připojení k databázi skutečné bez režie skutečné databázových operací.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) na Githubu.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory není relační databáze

Poskytovatelé databází EF Core nemají být relačních databází. InMemory byla navržena jako univerzální databázi pro účely testování a není navržen tak, aby napodoboval relační databáze.

Příklady zahrnují:

* InMemory vám umožní uložit data, která by způsobila porušení omezení referenční integrity v relační databázi.
* Pokud používáte DefaultValueSql(string) pro vlastnost v modelu, to je relační databáze rozhraní API a nebude mít žádný vliv, pokud provádějí InMemory.
* [Souběžnost prostřednictvím verze časové razítko/řádku](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` nebo `IsRowVersion`) se nepodporuje. Ne [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) bude vyvolána, pokud se provádí aktualizace pomocí staré tokenem souběžnosti.

> [!TIP]  
> Pro mnoho testovací účely nebude důležité tyto rozdíly. Nicméně, pokud chcete testování proti objektu, který se chová podobně jako true relační databáze, pak zvažte použití [režimu in-memory SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Ukázkový scénář testování

Vezměte v úvahu následující služba, která umožňuje provádět některé operace související s blogy kódu aplikace. Interně používá `DbContext` , která se připojuje k databázi SQL serveru. Bylo by užitečné přepínat tímto kontextem za účelem připojení k databázi InMemory tak, aby jsme zápisu efektivních testů pro tuto službu bez nutnosti upravovat kód, nebo dělat spoustu práce pro vytvoření testu double kontextu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Připravte váš kontext

### <a name="avoid-configuring-two-database-providers"></a>Nechcete konfigurovat dva poskytovatelé databází

Ve vašich testech se chystáte externě nakonfigurovat místní na použití poskytovatele InMemory. Pokud konfigurujete poskytovatele databáze tak, že přepíšete `OnConfiguring` v kontextu, pak budete muset přidat některé podmíněný kód tak, aby, pokud ještě nebyl nakonfigurován pouze konfigurace poskytovatele databáze.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Pokud používáte ASP.NET Core, by neměla od svého poskytovatele databáze je již nakonfigurován mimo kontext (v souboru Startup.cs) musí tento kód.

### <a name="add-a-constructor-for-testing"></a>Přidejte konstruktor pro účely testování

Nejjednodušší způsob, jak povolit testování na jinou databázi je upravit kontext k vystavení konstruktor, který přijímá `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` říká kontextu veškeré jeho nastavení, jako je například pro připojení k databázi. Toto je stejný objekt, který je sestavený používané metody OnConfiguring váš kontext.

## <a name="writing-tests"></a>Zápis testů

Klíčem k testování s tímto poskytovatelem je možnost předat kontext, který má používat poskytovatele InMemory a řízení rozsahu databázi v paměti. Obvykle chcete vyčistit databázi pro každou metodu testu.

Tady je příklad testovací třídy, která používá databázi InMemory. Každá testovací metoda Určuje jedinečný název databáze, což znamená, že každá z metod má svou vlastní databázi InMemory.

>[!TIP]
> Použít `.UseInMemoryDatabase()` metody rozšíření, odkaz na balíček NuGet `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
