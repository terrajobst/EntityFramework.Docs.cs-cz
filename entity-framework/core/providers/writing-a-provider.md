---
title: "Zápis poskytovatele databáze - EF jádra"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 4bddf5858ab2c6b2fd22571a20edb3f7c85e2853
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="writing-a-database-provider"></a>Zápis poskytovatele databáze

Informace o vytváření zprostředkovatele databáze Entity Framework Core najdete v tématu [tak chcete napsat poskytovatele EF základní](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) podle [podle Arthur Vickerse](https://github.com/ajcvickers).

Základní EF základní kód je open source a obsahuje několik poskytovatelů databáze, které lze použít jako referenci. Zdrojový kód najdete na https://github.com/aspnet/EntityFrameworkCore.

## <a name="the-providers-beware-label"></a>Zprostředkovatele pozor popisek

Jakmile začnete pracovat na poskytovatele, sledovat [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) popisek na našem Githubu problémy a žádosti o přijetí změn. Tento popisek používáme k identifikaci změny, které můžou mít vliv poskytovatel zapisovače.

## <a name="suggested-naming-of-third-party-providers"></a>Navrhované pojmenování poskytovatelů třetích stran

Doporučujeme používat následující pojmenování pro balíčky NuGet. To je konzistentní s názvy zásilek tým EF jádra.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Příklad:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
