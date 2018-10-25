---
title: Protokolování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 65501b5ac03ae544c51b7fc1a07fa9eea849f1e3
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022142"
---
# <a name="logging"></a>Protokolování

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) na Githubu.

## <a name="aspnet-core-applications"></a>Aplikace ASP.NET Core

EF Core mechanismy protokolování ASP.NET Core integruje automaticky pokaždé, když `AddDbContext` nebo `AddDbContextPool` se používá. Proto při použití technologie ASP.NET Core, protokolování by měla být nastavená způsobem popsaným v [dokumentace k ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Další aplikace

EF Core protokolování aktuálně vyžaduje implementaci třídy ILoggerFactory, která sama o sobě nakonfigurovanou ILoggerProvider jeden nebo více. Běžné poskytovatele se dodávají v následujících balíčků:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): jednoduchý konzolový protokolovače.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): podporuje Azure App Services "Diagnostické protokoly" a "Protokolování datového proudu" funkce.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): protokoly a monitorování ladicí program pomocí System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): protokoly do protokolu událostí Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): podporuje EventSource/naslouchacího procesu událostí.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): protokoly pro naslouchací proces trasování pomocí System.Diagnostics.TraceSource.TraceEvent().

Po instalaci příslušné balíčky, aplikace by měl vytvořit instanci typu singleton nebo globální LoggerFactory. Například použití protokolovací nástroj konzoly:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Potom by měly být zaregistrovány tuto instanci typu singleton nebo globální s EF Core na `DbContextOptionsBuilder`. Příklad:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Je velmi důležité, že aplikace nevytvářejte novou implementaci třídy ILoggerFactory instanci pro všechny instance kontextu. To způsobí nevrácení paměti a slabým výkonem.

## <a name="filtering-what-is-logged"></a>Co je protokolováno filtrování

Nejjednodušší způsob, jak filtrovat, co se do protokolu zapíše se při registraci ILoggerProvider jeho konfiguraci. Příklad:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

V tomto příkladu je filtrovaná vrátí pouze zprávy v protokolu:
 * v kategorii 'Microsoft.EntityFrameworkCore.Database.Command.
 * na úrovni "Informační"

Pro jádro EF Core protokolovač kategorie jsou definovány v `DbLoggerCategory` třídy usnadňují vyhledání kategorie, ale tyto přeložit na jednoduché řetězce.

Další informace o základní infrastruktuře protokolování najdete v [dokumentace k ASP.NET Core protokolování](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
