---
title: "Protokolování - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 807560e563eddfb72d4286353b1403a0d2e2a441
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/08/2017
---
# <a name="logging"></a>protokolování

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) na Githubu.

## <a name="aspnet-core-applications"></a>Aplikace ASP.NET Core

Základní EF automaticky integruje s mechanims protokolování jádra ASP.NET vždy, když `AddDbContext` nebo `AddDbContextPool` se používá. Proto při použití ASP.NET Core, protokolování by měl být nakonfigurovaný podle popisu v [ASP.NET Core dokumentaci](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Ostatní aplikace

Základní EF protokolování aktuálně vyžaduje implementaci třídy ILoggerFactory, který je sám nakonfigurované jeden nebo více ILoggerProvider. V následujících balíčcích jsou sice běžné zprostředkovatele:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): jednoduché konzolové protokolovače.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): podporuje Azure App Services 'Diagnostické protokoly' a 'protokolu datového proudu, funkce.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): ladicí program monitorování pomocí System.Diagnostics.Debug.WriteLine() v protokolech.
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): protokoluje události do protokolu událostí systému Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener podporuje.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): protokoly pomocí System.Diagnostics.TraceSource.TraceEvent() naslouchací proces trasování.

Po instalaci příslušné balíčky, aplikace by měl vytvořit singleton, globální instanci LoggerFactory. Například pomocí konzoly protokolovacího nástroje:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Tuto instanci typu singleton, globální by pak zaregistrována EF základní na `DbContextOptionsBuilder`. Příklad:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Je velmi důležité, aby aplikace nevytvářejte novou instanci implementaci třídy ILoggerFactory pro každou instanci kontextu. Díky tomu bude mít za následek nízký výkon a nevrácená paměť systému.

## <a name="filtering-what-is-logged"></a>Filtrování, co je protokolováno

Nejjednodušší způsob, jak filtrovat, co je protokolováno je při registraci ILoggerProvider jeho konfiguraci. Příklad:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

V tomto příkladu je filtrovaná vrátit pouze zprávy v protokolu:
 * v kategorii "Microsoft.EntityFrameworkCore.Database.Command.
 * na úrovni "Informace"

Pro základní EF protokolovacího nástroje kategorie jsou definovány v `DbLoggerCategory` třída snadno najít kategorií, ale ty odkazující na jednoduchý řetězce.

Další informace o základní protokolování infrastruktury najdete v [ASP.NET Core protokolování dokumentaci](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).