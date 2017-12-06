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
<a name="design-time-services"></a>Návrhu služby
====================
Některé služby používané nástroje se používají jenom v době návrhu. Tyto služby jsou spravovány samostatně z EF jádra služby modulu runtime zabránit jejich nasazuje s vaší aplikací. Pokud chcete přepsat, jednu z těchto služeb (například služba pro vygenerování souborů migrace), přidejte implementace `IDesignTimeServices` do projektu po spuštění.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
