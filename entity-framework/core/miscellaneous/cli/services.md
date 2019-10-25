---
title: Služby v době návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811948"
---
# <a name="design-time-services"></a><span data-ttu-id="7dd8b-102">Služby v době návrhu</span><span class="sxs-lookup"><span data-stu-id="7dd8b-102">Design-time services</span></span>

<span data-ttu-id="7dd8b-103">Některé služby používané nástroji jsou používány pouze v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="7dd8b-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="7dd8b-104">Tyto služby se spravují odděleně od EF Core běhových služeb, aby se zabránilo jejich nasazení ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7dd8b-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="7dd8b-105">Chcete-li potlačit jednu z těchto služeb (například služba pro generování migračních souborů), přidejte implementaci `IDesignTimeServices` do projektu po spuštění.</span><span class="sxs-lookup"><span data-stu-id="7dd8b-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
