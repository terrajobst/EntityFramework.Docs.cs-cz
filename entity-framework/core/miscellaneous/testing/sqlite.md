---
title: Testování s SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996865"
---
# <a name="testing-with-sqlite"></a>Testování s SQLite

SQLite má režim v paměti, která umožňuje používat k psaní testů pro relační databázi, bez režie skutečné databázových operací SQLite.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) na Githubu

## <a name="example-testing-scenario"></a>Ukázkový scénář testování

Vezměte v úvahu následující služba, která umožňuje provádět některé operace související s blogy kódu aplikace. Interně používá `DbContext` , která se připojuje k databázi SQL serveru. Bylo by užitečné přepínat tímto kontextem za účelem připojení k databázi SQLite v paměti tak, aby jsme zápisu efektivních testů pro tuto službu bez nutnosti upravovat kód, nebo dělat spoustu práce pro vytvoření testu double kontextu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Připravte váš kontext

### <a name="avoid-configuring-two-database-providers"></a>Nechcete konfigurovat dva poskytovatelé databází

Ve vašich testech se chystáte externě nakonfigurovat místní na použití poskytovatele InMemory. Pokud konfigurujete poskytovatele databáze tak, že přepíšete `OnConfiguring` v kontextu, pak budete muset přidat některé podmíněný kód tak, aby, pokud ještě nebyl nakonfigurován pouze konfigurace poskytovatele databáze.

> [!TIP]  
> Pokud používáte ASP.NET Core, by neměla od svého poskytovatele databáze je nakonfigurovaný mimo kontext (v souboru Startup.cs) musí tento kód.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Přidejte konstruktor pro účely testování

Nejjednodušší způsob, jak povolit testování na jinou databázi je upravit kontext k vystavení konstruktor, který přijímá `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` říká kontextu veškeré jeho nastavení, jako je například pro připojení k databázi. Toto je stejný objekt, který je sestavený používané metody OnConfiguring váš kontext.

## <a name="writing-tests"></a>Zápis testů

Klíčem k testování s tímto poskytovatelem je možnost předat kontext, který má použít SQLite a řízení rozsahu databázi v paměti. Obor databáze je řízena otevírání a zavírání připojení. Databáze je vymezen na dobu, po kterou je připojení otevřeno. Obvykle chcete vyčistit databázi pro každou metodu testu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
