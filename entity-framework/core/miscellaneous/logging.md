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
# <a name="logging"></a><span data-ttu-id="f9570-102">protokolování</span><span class="sxs-lookup"><span data-stu-id="f9570-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="f9570-103">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="f9570-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="f9570-104">Aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9570-104">ASP.NET Core applications</span></span>

<span data-ttu-id="f9570-105">Základní EF automaticky integruje s mechanims protokolování jádra ASP.NET vždy, když `AddDbContext` nebo `AddDbContextPool` se používá.</span><span class="sxs-lookup"><span data-stu-id="f9570-105">EF Core integrates automatically with the logging mechanims of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="f9570-106">Proto při použití ASP.NET Core, protokolování by měl být nakonfigurovaný podle popisu v [ASP.NET Core dokumentaci](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="f9570-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="f9570-107">Ostatní aplikace</span><span class="sxs-lookup"><span data-stu-id="f9570-107">Other applications</span></span>

<span data-ttu-id="f9570-108">Základní EF protokolování aktuálně vyžaduje implementaci třídy ILoggerFactory, který je sám nakonfigurované jeden nebo více ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="f9570-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="f9570-109">V následujících balíčcích jsou sice běžné zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="f9570-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="f9570-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): jednoduché konzolové protokolovače.</span><span class="sxs-lookup"><span data-stu-id="f9570-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="f9570-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): podporuje Azure App Services 'Diagnostické protokoly' a 'protokolu datového proudu, funkce.</span><span class="sxs-lookup"><span data-stu-id="f9570-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="f9570-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): ladicí program monitorování pomocí System.Diagnostics.Debug.WriteLine() v protokolech.</span><span class="sxs-lookup"><span data-stu-id="f9570-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="f9570-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): protokoluje události do protokolu událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f9570-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="f9570-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener podporuje.</span><span class="sxs-lookup"><span data-stu-id="f9570-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="f9570-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): protokoly pomocí System.Diagnostics.TraceSource.TraceEvent() naslouchací proces trasování.</span><span class="sxs-lookup"><span data-stu-id="f9570-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="f9570-116">Po instalaci příslušné balíčky, aplikace by měl vytvořit singleton, globální instanci LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="f9570-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="f9570-117">Například pomocí konzoly protokolovacího nástroje:</span><span class="sxs-lookup"><span data-stu-id="f9570-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="f9570-118">Tuto instanci typu singleton, globální by pak zaregistrována EF základní na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f9570-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="f9570-119">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f9570-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="f9570-120">Je velmi důležité, aby aplikace nevytvářejte novou instanci implementaci třídy ILoggerFactory pro každou instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="f9570-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="f9570-121">Díky tomu bude mít za následek nízký výkon a nevrácená paměť systému.</span><span class="sxs-lookup"><span data-stu-id="f9570-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="f9570-122">Filtrování, co je protokolováno</span><span class="sxs-lookup"><span data-stu-id="f9570-122">Filtering what is logged</span></span>

<span data-ttu-id="f9570-123">Nejjednodušší způsob, jak filtrovat, co je protokolováno je při registraci ILoggerProvider jeho konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="f9570-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="f9570-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f9570-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="f9570-125">V tomto příkladu je filtrovaná vrátit pouze zprávy v protokolu:</span><span class="sxs-lookup"><span data-stu-id="f9570-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="f9570-126">v kategorii "Microsoft.EntityFrameworkCore.Database.Command.</span><span class="sxs-lookup"><span data-stu-id="f9570-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="f9570-127">na úrovni "Informace"</span><span class="sxs-lookup"><span data-stu-id="f9570-127">at the 'Information' level</span></span>

<span data-ttu-id="f9570-128">Pro základní EF protokolovacího nástroje kategorie jsou definovány v `DbLoggerCategory` třída snadno najít kategorií, ale ty odkazující na jednoduchý řetězce.</span><span class="sxs-lookup"><span data-stu-id="f9570-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="f9570-129">Další informace o základní protokolování infrastruktury najdete v [ASP.NET Core protokolování dokumentaci](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="f9570-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>