---
title: Protokolování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 6a8499f9f0220087e76f2e0b3a75ce551c4ddb80
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197502"
---
# <a name="logging"></a><span data-ttu-id="aaa8c-102">protokolování</span><span class="sxs-lookup"><span data-stu-id="aaa8c-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="aaa8c-103">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="aaa8c-104">ASP.NET Core aplikací</span><span class="sxs-lookup"><span data-stu-id="aaa8c-104">ASP.NET Core applications</span></span>

<span data-ttu-id="aaa8c-105">EF Core se automaticky integruje s mechanismy protokolování ASP.NET Core při `AddDbContext` každém `AddDbContextPool` použití nebo.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="aaa8c-106">Při použití ASP.NET Core by proto mělo být protokolování nakonfigurované, jak je popsáno v [dokumentaci k ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="aaa8c-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="aaa8c-107">Další aplikace</span><span class="sxs-lookup"><span data-stu-id="aaa8c-107">Other applications</span></span>

<span data-ttu-id="aaa8c-108">EF Core protokolování vyžaduje ILoggerFactory, který je sám nakonfigurovaný s jedním nebo více zprostředkovateli protokolování.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="aaa8c-109">Mezi běžné poskytovatele se dodává následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="aaa8c-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="aaa8c-110">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Jednoduchý protokolovací nástroj konzoly.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="aaa8c-111">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Podporuje funkce Azure App Services ' diagnostické protokoly ' a ' log Stream '.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="aaa8c-112">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Protokoluje monitorování ladicího programu pomocí System. Diagnostics. Debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="aaa8c-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="aaa8c-113">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Protokoluje protokol událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="aaa8c-114">[Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Podporuje EventSource/naslouchacího procesu událostí.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="aaa8c-115">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Protokoluje naslouchací proces trasování pomocí `System.Diagnostics.TraceSource.TraceEvent()`.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="aaa8c-116">Po instalaci příslušných balíčků by aplikace měla vytvořit singleton nebo globální instanci třídy LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="aaa8c-117">Například pomocí protokolovacího nástroje konzoly:</span><span class="sxs-lookup"><span data-stu-id="aaa8c-117">For example, using the console logger:</span></span>

# <a name="version-30tabv3"></a>[<span data-ttu-id="aaa8c-118">Verze 3,0</span><span class="sxs-lookup"><span data-stu-id="aaa8c-118">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[<span data-ttu-id="aaa8c-119">Verze 2. x</span><span class="sxs-lookup"><span data-stu-id="aaa8c-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="aaa8c-120">Následující ukázka kódu používá `ConsoleLoggerProvider` konstruktor, který byl zastaralý ve verzi 2,2 a nahrazen v 3,0.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="aaa8c-121">Při použití 2,2 je bezpečné ignorovat a potlačit upozornění.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="aaa8c-122">Tato instance typu Singleton/Global by měla být registrována s EF Core `DbContextOptionsBuilder`v.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="aaa8c-123">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aaa8c-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="aaa8c-124">Je velmi důležité, aby aplikace nevytvářely novou instanci ILoggerFactory pro každou instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="aaa8c-125">Výsledkem bude nevracení paměti a špatný výkon.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="aaa8c-126">Filtrování, co je protokolováno</span><span class="sxs-lookup"><span data-stu-id="aaa8c-126">Filtering what is logged</span></span>

<span data-ttu-id="aaa8c-127">Aplikace může řídit, co se protokoluje, konfigurací filtru v ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="aaa8c-128">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aaa8c-128">For example:</span></span>

# <a name="version-30tabv3"></a>[<span data-ttu-id="aaa8c-129">Verze 3,0</span><span class="sxs-lookup"><span data-stu-id="aaa8c-129">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[<span data-ttu-id="aaa8c-130">Verze 2. x</span><span class="sxs-lookup"><span data-stu-id="aaa8c-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="aaa8c-131">Následující ukázka kódu používá `ConsoleLoggerProvider` konstruktor, který byl zastaralý ve verzi 2,2 a nahrazen v 3,0.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="aaa8c-132">Při použití 2,2 je bezpečné ignorovat a potlačit upozornění.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

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

<span data-ttu-id="aaa8c-133">V tomto příkladu je protokol filtrován tak, aby vracel pouze zprávy:</span><span class="sxs-lookup"><span data-stu-id="aaa8c-133">In this example, the log is filtered to only return messages:</span></span>
 * <span data-ttu-id="aaa8c-134">v kategorii Microsoft. EntityFrameworkCore. Database. Command</span><span class="sxs-lookup"><span data-stu-id="aaa8c-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="aaa8c-135">na úrovni Information</span><span class="sxs-lookup"><span data-stu-id="aaa8c-135">at the 'Information' level</span></span>

<span data-ttu-id="aaa8c-136">Pro EF Core jsou ve `DbLoggerCategory` třídě definovány kategorie protokolovacích nástrojů, které usnadňují hledání kategorií, ale tyto jsou vyřešeny na jednoduché řetězce.</span><span class="sxs-lookup"><span data-stu-id="aaa8c-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="aaa8c-137">Další podrobnosti o základní infrastruktuře protokolování najdete v [dokumentaci k protokolování ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="aaa8c-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
