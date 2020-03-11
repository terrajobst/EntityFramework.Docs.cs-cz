---
title: Testování pomocí EF Core SQLite
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417300"
---
# <a name="testing-with-sqlite"></a>Testování s SQLite

SQLite má režim v paměti, který umožňuje použít SQLite k zápisu testů na relační databázi bez režie pro skutečné databázové operace.

> [!TIP]  
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) najdete v tomto článku na GitHubu.

## <a name="example-testing-scenario"></a>Ukázkový scénář testování

Vezměte v úvahu následující službu, která umožňuje, aby kód aplikace prováděl některé operace týkající se blogů. Interně používá `DbContext`, které se připojují k databázi SQL Server. Pro připojení k databázi SQLite v paměti by bylo vhodné tento kontext prohodit, aby bylo možné zapsat efektivní testy pro tuto službu, aniž by bylo nutné kód upravovat, nebo udělat spoustu práce, aby bylo možné vytvořit test Double v kontextu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Připravte svůj kontext

### <a name="avoid-configuring-two-database-providers"></a>Nekonfigurují se dva poskytovatelé databáze.

V testech provedete externě konfiguraci kontextu pro použití poskytovatele inMemory. Pokud konfigurujete poskytovatele databáze přepsáním `OnConfiguring` v kontextu, budete muset přidat nějaký podmíněný kód, abyste měli jistotu, že jste poskytovatele databáze nakonfigurovali jenom v případě, že ještě není nakonfigurovaný.

> [!TIP]  
> Pokud používáte ASP.NET Core, neměli byste tento kód potřebovat, protože váš poskytovatel databáze je nakonfigurovaný mimo kontext (v Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Přidání konstruktoru pro testování

Nejjednodušší způsob, jak povolit testování v jiné databázi, je upravit svůj kontext, aby vystavoval konstruktor, který přijímá `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informuje kontext o všech nastaveních, například o tom, ke které databázi se chcete připojit. Jedná se o stejný objekt, který je sestaven spuštěním metody při konfiguraci v kontextu.

## <a name="writing-tests"></a>Zápis testů

Klíč k testování pomocí tohoto poskytovatele je schopnost sdělit kontextu použití SQLite a řídit obor databáze v paměti. Rozsah databáze je řízen otevřením a zavřením připojení. Databáze je vymezena na dobu, po kterou je připojení otevřeno. Obvykle požadujete pro každou testovací metodu čistou databázi.

>[!TIP]
> Pokud chcete použít `SqliteConnection()` a metodu rozšíření `.UseSqlite()`, odkazujte na balíček NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
