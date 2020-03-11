---
title: Protokolování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: e8adc39ec01ff75112b03446a488df6199cc7041
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416580"
---
# <a name="logging"></a>Protokolování

> [!TIP]  
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) tohoto článku můžete zobrazit na GitHubu.

## <a name="aspnet-core-applications"></a>ASP.NET Core aplikací

EF Core se automaticky integruje s mechanismy protokolování ASP.NET Core vždy, když se používá `AddDbContext` nebo `AddDbContextPool`. Při použití ASP.NET Core by proto mělo být protokolování nakonfigurované, jak je popsáno v [dokumentaci k ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Další aplikace

EF Core protokolování vyžaduje ILoggerFactory, který je sám nakonfigurovaný s jedním nebo více zprostředkovateli protokolování. Mezi běžné poskytovatele se dodává následující balíčky:

* [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): jednoduchý protokolovací nástroj konzoly.
* [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): podporuje funkce Azure App Services diagnostické protokoly a log Stream.
* [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): protokoly do monitorování ladicího programu pomocí System. Diagnostics. Debug. WriteLine ().
* [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): protokoly do protokolu událostí systému Windows.
* [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): podporuje EventSource/naslouchacího procesu událostí.
* [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): protokoluje naslouchací proces trasování pomocí `System.Diagnostics.TraceSource.TraceEvent()`.

Po instalaci příslušných balíčků by aplikace měla vytvořit singleton nebo globální instanci třídy LoggerFactory. Například pomocí protokolovacího nástroje konzoly:

### <a name="version-3x"></a>[Verze 3. x](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[Verze 2. x](#tab/v2)

> [!NOTE]
> Následující ukázka kódu používá konstruktor `ConsoleLoggerProvider`, který byl zastaralý ve verzi 2,2 a nahrazen v 3,0. Při použití 2,2 je bezpečné ignorovat a potlačit upozornění.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

Tato instance typu Singleton/Global by pak měla být zaregistrovaná v EF Core `DbContextOptionsBuilder`. Příklad:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Je velmi důležité, aby aplikace nevytvářely novou instanci ILoggerFactory pro každou instanci kontextu. Výsledkem bude nevracení paměti a špatný výkon.

## <a name="filtering-what-is-logged"></a>Filtrování, co je protokolováno

Aplikace může řídit, co se protokoluje, konfigurací filtru v ILoggerProvider. Příklad:

### <a name="version-3x"></a>[Verze 3. x](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[Verze 2. x](#tab/v2)

> [!NOTE]
> Následující ukázka kódu používá konstruktor `ConsoleLoggerProvider`, který byl zastaralý ve verzi 2,2 a nahrazen v 3,0. Při použití 2,2 je bezpečné ignorovat a potlačit upozornění.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

V tomto příkladu je protokol filtrován tak, aby vracel pouze zprávy:

* v kategorii Microsoft. EntityFrameworkCore. Database. Command
* na úrovni Information

Pro EF Core jsou kategorie protokolovacích nástrojů definovány ve `DbLoggerCategory` třídě, aby bylo možné snadno najít kategorie, ale tyto jsou vyřešeny na jednoduché řetězce.

Další podrobnosti o základní infrastruktuře protokolování najdete v [dokumentaci k protokolování ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
