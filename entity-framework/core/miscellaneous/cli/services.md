---
title: Služby návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997528"
---
<a name="design-time-services"></a><span data-ttu-id="f9fe7-102">Služby v době návrhu</span><span class="sxs-lookup"><span data-stu-id="f9fe7-102">Design-time services</span></span>
====================
<span data-ttu-id="f9fe7-103">Některé služby používají nástroje se používají pouze v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="f9fe7-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="f9fe7-104">Tyto služby jsou spravovány samostatně z služby modulu runtime EF Core tak tomu, aby se nasazuje s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="f9fe7-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="f9fe7-105">Chcete-li přepsat jednu z těchto služeb (například služba pro vygenerování souborů migrace), přidejte implementaci `IDesignTimeServices` do projektu po spuštění.</span><span class="sxs-lookup"><span data-stu-id="f9fe7-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
