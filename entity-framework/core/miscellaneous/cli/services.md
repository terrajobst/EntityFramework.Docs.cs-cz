---
title: "Návrhu služby - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a><span data-ttu-id="85895-102">Návrhu služby</span><span class="sxs-lookup"><span data-stu-id="85895-102">Design-time services</span></span>
====================
<span data-ttu-id="85895-103">Některé služby používané nástroje se používají jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="85895-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="85895-104">Tyto služby jsou spravovány samostatně z EF jádra služby modulu runtime zabránit jejich nasazuje s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="85895-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="85895-105">Pokud chcete přepsat, jednu z těchto služeb (například služba pro vygenerování souborů migrace), přidejte implementace `IDesignTimeServices` do projektu po spuštění.</span><span class="sxs-lookup"><span data-stu-id="85895-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
