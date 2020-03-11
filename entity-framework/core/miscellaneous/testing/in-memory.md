---
title: Testování s pamětí EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416507"
---
# <a name="testing-with-inmemory"></a>Testování s InMemory

Zprostředkovatel inMemory je užitečný v případě, že chcete testovat komponenty pomocí objektu, který se blíží k reálné databázi, bez režie pro skutečné databázové operace.

> [!TIP]  
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) tohoto článku můžete zobrazit na GitHubu.

## <a name="inmemory-is-not-a-relational-database"></a>Nepaměť není relační databáze.

Poskytovatelé databáze EF Core nemusí být relační databáze. InMemory je navržen jako databáze pro obecné účely testování a není navržena pro napodobení relační databázi.

Mezi příklady patří:

* InMemory vám umožní ukládat data, která by v relační databázi porušila omezení referenční integrity.
* Pokud pro vlastnost v modelu používáte DefaultValueSql (String), jedná se o rozhraní API relační databáze a nebude mít žádný vliv, pokud je spuštěný proti paměti.
* [Souběžnost přes verzi časového razítka nebo řádku](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` nebo `IsRowVersion`) není podporována. Pokud je aktualizace provedena pomocí starého tokenu souběžnosti, nebude vyvolána žádná [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) .

> [!TIP]  
> Pro mnoho testovacích účelů tyto rozdíly nezáleží. Pokud však chcete testovat proti nějakému objektu, který se chová podobně jako skutečná relační databáze, zvažte použití [režimu v paměti SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Ukázkový scénář testování

Vezměte v úvahu následující službu, která umožňuje, aby kód aplikace prováděl některé operace týkající se blogů. Interně používá `DbContext`, které se připojují k databázi SQL Server. Je vhodné tento kontext prohodit, aby se připojil k databázi inMemory, aby bylo možné zapsat efektivní testy pro tuto službu, aniž byste museli upravovat kód, nebo udělat spoustu práce pro vytvoření dvojitě funkčního testu v kontextu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Připravte svůj kontext

### <a name="avoid-configuring-two-database-providers"></a>Nekonfigurují se dva poskytovatelé databáze.

V testech provedete externě konfiguraci kontextu pro použití poskytovatele inMemory. Pokud konfigurujete poskytovatele databáze přepsáním `OnConfiguring` v kontextu, budete muset přidat nějaký podmíněný kód, abyste měli jistotu, že jste poskytovatele databáze nakonfigurovali jenom v případě, že ještě není nakonfigurovaný.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Pokud používáte ASP.NET Core, neměli byste tento kód potřebovat, protože váš poskytovatel databáze je už nakonfigurovaný mimo kontext (v Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Přidání konstruktoru pro testování

Nejjednodušší způsob, jak povolit testování v jiné databázi, je upravit svůj kontext, aby vystavoval konstruktor, který přijímá `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informuje kontext o všech nastaveních, například o tom, ke které databázi se chcete připojit. Jedná se o stejný objekt, který je sestaven spuštěním metody při konfiguraci v kontextu.

## <a name="writing-tests"></a>Zápis testů

Klíčem k testování pomocí tohoto poskytovatele je schopnost sdělit kontextu použití poskytovatele inMemory a řídit obor databáze v paměti. Obvykle požadujete pro každou testovací metodu čistou databázi.

Zde je příklad třídy testu, která používá databázi inMemory. Každá metoda testu určuje jedinečný název databáze, což znamená, že každá metoda má svou vlastní databázi inMemory.

>[!TIP]
> Chcete-li použít metodu rozšíření `.UseInMemoryDatabase()`, odkázat na balíček NuGet [Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
