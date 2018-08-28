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
<a name="design-time-services"></a>Služby v době návrhu
====================
Některé služby používají nástroje se používají pouze v době návrhu. Tyto služby jsou spravovány samostatně z služby modulu runtime EF Core tak tomu, aby se nasazuje s vaší aplikací. Chcete-li přepsat jednu z těchto služeb (například služba pro vygenerování souborů migrace), přidejte implementaci `IDesignTimeServices` do projektu po spuštění.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
