---
title: "Testování pomocí SQLite - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a>Testování pomocí SQLite

SQLite má režim v paměti, která umožňuje zápis testů na relační databázi, bez nutnosti skutečné databázových operací pomocí SQLite.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) na Githubu

## <a name="example-testing-scenario"></a>Příklad scénáře testování

Vezměte v úvahu následující služby, který umožňuje aplikaci provádět některé operace související s blogy. Interně používá `DbContext` která se připojuje k databázi systému SQL Server. Je užitečné odkládacího souboru tímto kontextem za účelem připojení k databázi v paměti SQLite tak, aby jsme můžete zapsat efektivní testy pro tuto službu bez nutnosti změnit kód, nebo můžete provést spoustu práce vytvoření testu dvojité kontextu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Příprava váš kontext

### <a name="avoid-configuring-two-database-providers"></a>Vyhnout konfiguraci dva poskytovatelé databáze

Ve vašich testech budete externě nakonfigurovat kontext pro použití poskytovatele InMemory. Pokud konfigurujete poskytovatele databáze přepsáním `OnConfiguring` v kontextu, budete muset přidat podmíněného kód, který Ujistěte se, pokud nebylo bylo nakonfigurováno pouze konfiguraci poskytovatele za databáze.

> [!TIP]  
> Pokud používáte ASP.NET Core, neměli vzhledem k tomu, že poskytovatel databáze nastaven mimo kontext (v souboru Startup.cs) potřebovat tento kód.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Přidejte konstruktor pro testování

Nejjednodušší způsob, jak povolit testování proti do jiné databáze je cílem upravit váš kontext vystavit konstruktor, který přijímá `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`informuje kontextu veškeré jeho nastavení, jako je například databázi, ke které připojit k. Toto je stejný objekt, který je sestavena spuštění metody OnConfiguring ve vašem kontextu.

## <a name="writing-tests"></a>Zápis testů

Klíč k testování s tímto poskytovatelem přístup je schopnost říct kontext, který má používat SQLite a řízení rozsahu databázi v paměti. Obor databáze je řízena otvírání a zavírání připojení. Databáze je vymezen na dobu trvání připojení je otevřeno. Obvykle budete chtít vyčištění databáze pro každou metodu test.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
