---
title: Protokolování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985571"
---
# <a name="logging"></a><span data-ttu-id="d228d-102">Protokolování</span><span class="sxs-lookup"><span data-stu-id="d228d-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="d228d-103">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d228d-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="d228d-104">Aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d228d-104">ASP.NET Core applications</span></span>

<span data-ttu-id="d228d-105">EF Core mechanismy protokolování ASP.NET Core integruje automaticky pokaždé, když `AddDbContext` nebo `AddDbContextPool` se používá.</span><span class="sxs-lookup"><span data-stu-id="d228d-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="d228d-106">Proto při použití technologie ASP.NET Core, protokolování by měla být nastavená způsobem popsaným v [dokumentace k ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="d228d-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="d228d-107">Další aplikace</span><span class="sxs-lookup"><span data-stu-id="d228d-107">Other applications</span></span>

<span data-ttu-id="d228d-108">EF Core protokolování aktuálně vyžaduje implementaci třídy ILoggerFactory, která sama o sobě nakonfigurovanou ILoggerProvider jeden nebo více.</span><span class="sxs-lookup"><span data-stu-id="d228d-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="d228d-109">Běžné poskytovatele se dodávají v následujících balíčků:</span><span class="sxs-lookup"><span data-stu-id="d228d-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="d228d-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Jednoduchý konzolový protokolovače.</span><span class="sxs-lookup"><span data-stu-id="d228d-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="d228d-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Podporuje Azure App Services "Diagnostické protokoly" a 'Protokolování datového proudu' funkce.</span><span class="sxs-lookup"><span data-stu-id="d228d-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="d228d-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Protokoluje události do ladicího programu Sledování pomocí System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="d228d-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="d228d-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Protokoluje události do protokolu událostí Windows.</span><span class="sxs-lookup"><span data-stu-id="d228d-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="d228d-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Podporuje EventSource/naslouchacího procesu událostí.</span><span class="sxs-lookup"><span data-stu-id="d228d-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="d228d-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Pro naslouchací proces trasování pomocí System.Diagnostics.TraceSource.TraceEvent() ukládá do protokolu.</span><span class="sxs-lookup"><span data-stu-id="d228d-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

> [!NOTE]
> <span data-ttu-id="d228d-116">Následující kód používá ukázkový `ConsoleLoggerProvider` konstruktor, který bylo vyřazeno ve verzi 2.2.</span><span class="sxs-lookup"><span data-stu-id="d228d-116">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="d228d-117">Správné nahrazení pro zastaralý protokolování rozhraní API bude k dispozici ve verzi 3.0.</span><span class="sxs-lookup"><span data-stu-id="d228d-117">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="d228d-118">Do té doby se můžete bezpečně ignorovat a tato upozornění už nezobrazovala.</span><span class="sxs-lookup"><span data-stu-id="d228d-118">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="d228d-119">Po instalaci příslušné balíčky, aplikace by měl vytvořit instanci typu singleton nebo globální LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="d228d-119">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="d228d-120">Například použití protokolovací nástroj konzoly:</span><span class="sxs-lookup"><span data-stu-id="d228d-120">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="d228d-121">Potom by měly být zaregistrovány tuto instanci typu singleton nebo globální s EF Core na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d228d-121">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="d228d-122">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d228d-122">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="d228d-123">Je velmi důležité, že aplikace nevytvářejte novou implementaci třídy ILoggerFactory instanci pro všechny instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="d228d-123">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="d228d-124">To způsobí nevrácení paměti a slabým výkonem.</span><span class="sxs-lookup"><span data-stu-id="d228d-124">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="d228d-125">Co je protokolováno filtrování</span><span class="sxs-lookup"><span data-stu-id="d228d-125">Filtering what is logged</span></span>

> [!NOTE]
> <span data-ttu-id="d228d-126">Následující kód používá ukázkový `ConsoleLoggerProvider` konstruktor, který bylo vyřazeno ve verzi 2.2.</span><span class="sxs-lookup"><span data-stu-id="d228d-126">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="d228d-127">Správné nahrazení pro zastaralý protokolování rozhraní API bude k dispozici ve verzi 3.0.</span><span class="sxs-lookup"><span data-stu-id="d228d-127">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="d228d-128">Do té doby se můžete bezpečně ignorovat a tato upozornění už nezobrazovala.</span><span class="sxs-lookup"><span data-stu-id="d228d-128">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="d228d-129">Nejjednodušší způsob, jak filtrovat, co se do protokolu zapíše se při registraci ILoggerProvider jeho konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="d228d-129">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="d228d-130">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d228d-130">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="d228d-131">V tomto příkladu je filtrovaná vrátí pouze zprávy v protokolu:</span><span class="sxs-lookup"><span data-stu-id="d228d-131">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="d228d-132">v kategorii 'Microsoft.EntityFrameworkCore.Database.Command.</span><span class="sxs-lookup"><span data-stu-id="d228d-132">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="d228d-133">na úrovni "Informační"</span><span class="sxs-lookup"><span data-stu-id="d228d-133">at the 'Information' level</span></span>

<span data-ttu-id="d228d-134">Pro jádro EF Core protokolovač kategorie jsou definovány v `DbLoggerCategory` třídy usnadňují vyhledání kategorie, ale tyto přeložit na jednoduché řetězce.</span><span class="sxs-lookup"><span data-stu-id="d228d-134">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="d228d-135">Další informace o základní infrastruktuře protokolování najdete v [dokumentace k ASP.NET Core protokolování](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="d228d-135">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
