---
title: Vytvoření poskytovatele databáze – EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993663"
---
# <a name="writing-a-database-provider"></a>Vytvoření poskytovatele databáze

Informace o vytváření poskytovatele databáze Entity Framework Core najdete v tématu [tak, že chcete napsat poskytovatele EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) podle [podle Arthur Vickerse](https://github.com/ajcvickers).

> [!NOTE]
> Tyto příspěvky nebyly aktualizovány od EF Core 1.1 a od té doby byly provedeny významné změny [problém 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) se ke sledování aktualizací do této dokumentace.

Základ kódu EF Core je open source a obsahuje několik zprostředkovatelů databáze, které lze použít jako referenci. Můžete najít zdrojový kód v https://github.com/aspnet/EntityFrameworkCore. Může být také užitečné si prohlédněte si kód pro běžně používané poskytovatele třetí strany, jako například [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), a [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). Konkrétně se tyto projekty jsou nastavené rozšířit z a spuštění funkčních testů, které budeme publikovat na webu NuGet. Důrazně se doporučuje tento typ instalačního programu.

## <a name="keeping-up-to-date-with-provider-changes"></a>Udržovat přehled o změnách poskytovatele

Počínaje práce po vydání 2.1, vytvořili jsme [protokolu změn](provider-log.md) odpovídající změny v kódu poskytovatele, který může být nutné. To má pomoci při aktualizaci existujícího poskytovatele pro práci s novou verzi EF Core.

Před 2.1, které jsme použili [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) a [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) popisky na naše problémy s úložištěm GitHub a žádosti o přijetí změn pro k podobnému účelu. Budeme continiue používat tyto se zapnutou problémů poskytnout jako ukazatel toho, které pracovní položky v dané verzi může také vyžadovat práci udělat u zprostředkovatelů. A `providers-beware` popisek obvykle znamená, že provádění pracovní položky, mohou přestat fungovat poskytovatelů, zatímco `providers-fyi` popisek obvykle znamená, že poskytovatelů nebude přeruší se, že kód může potřebovat změnit i přesto, třeba, abyste umožnili nové funkce.

## <a name="suggested-naming-of-third-party-providers"></a>Navrhované názvy poskytovatelů třetích stran

Doporučujeme používat následující názvy balíčků NuGet. To je konzistentní s názvy balíčků od týmu EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Příklad:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
