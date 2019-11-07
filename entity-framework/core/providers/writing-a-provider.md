---
title: Zápis poskytovatele databáze – EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d52a8581772cc5405e94966fa7ebdff4128c252
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654779"
---
# <a name="writing-a-database-provider"></a>Vytvoření poskytovatele databáze

Informace o tom, jak napsat poskytovatele databáze Entity Framework Core, najdete v tématu, chcete [-li vytvořit poskytovatele EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) pomocí [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Tyto příspěvky nebyly od EF Core 1,1 aktualizovány a došlo k významným změnám od doby, kdy [problém 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) sleduje aktualizace této dokumentace.

Základ kódu EF Core je open source a obsahuje několik poskytovatelů databáze, které je možné použít jako referenci. Zdrojový kód můžete najít na <https://github.com/aspnet/EntityFrameworkCore>. Může být také užitečné si prohlédnout kód pro běžně používané poskytovatele třetích stran, například [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)a [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). Konkrétně jsou tyto projekty instalačním programem, aby rozšířily a spouštěly funkční testy, které publikujeme na NuGet. Tento druh instalace se důrazně doporučuje.

## <a name="keeping-up-to-date-with-provider-changes"></a>Udržování aktuálnosti se změnami od poskytovatele

Od verze 2,1 jsme vytvořili [protokol změn](provider-log.md) , které by mohly potřebovat odpovídající změny v kódu poskytovatele. Slouží k tomu, aby vám pomohla při aktualizaci existujícího poskytovatele pro práci s novou verzí EF Core.

Před 2,1 jsme použili [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) a [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) popisky na naše problémy GitHubu a žádosti o přijetí změn pro podobný účel. Continiueme, že tyto zapnutouy využijeme k tomu, abychom zjistili, které pracovní položky v dané vydané verzi můžou také vyžadovat, aby se práce prováděla v poskytovatelích. `providers-beware` popisek obvykle znamená, že implementace pracovní položky může přerušit poskytovatele, zatímco `providers-fyi` popisek obvykle znamená, že zprostředkovatelé nebudou přerušeny, ale může být nutné změnit kód, například pro povolení nových funkcí.

## <a name="suggested-naming-of-third-party-providers"></a>Navrhované pojmenování poskytovatelů třetích stran

Doporučujeme použít následující pojmenování balíčků NuGet. To je konzistentní s názvy balíčků, které doručí tým EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Příklad:

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
