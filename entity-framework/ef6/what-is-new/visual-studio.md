---
title: Verze sady Visual Studio – EF6
author: divega
ms.date: 2018-07-05
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
caps.latest.revision: 3
ms.openlocfilehash: 7bd08a46b1d6acc5a565952e834f01546a5262c8
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912031"
---
# <a name="visual-studio-releases"></a>Verze sady Visual Studio

Doporučujeme vždy používat nejnovější verzi sady Visual Studio, protože obsahuje nejnovější nástroje pro .NET NuGet a Entity Framework.
Ve skutečnosti různé ukázky a návody v dokumentaci rozhraní Entity Framework se předpokládá, že používáte nejnovější verzi sady Visual Studio.

Je to možné je však používat starší verze sady Visual Studio s různými verzemi nástroje Entity Framework tak dlouho, dokud je provést do účtu určité rozdíly:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 a novější

- Tato verze sady Visual Studio obsahuje nejnovější verzi nástrojů Entity Framework a modulu runtime EF 6.2 a nevyžaduje další kroky.
Zobrazit [novinky](~/ef6/what-is-new/index.md) podrobné informace o těchto verzích.
- Přidání Entity Framework pro nové projekty pomocí nástrojů EF automaticky přidá balíček EF 6.2 NuGet.
Ručně můžete nainstalovat nebo upgradovat na libovolný balíček EF NuGet k dispozici online.
- Ve výchozím nastavení je instance systému SQL Server k dispozici s touto verzí sady Visual Studio na instanci LocalDB volá MSSQLLocalDB.
Část serveru byste měli použít připojovací řetězec je "(localdb)\\MSSQLLocalDB".
Nezapomeňte použít doslovný řetězec s předponou `@` nebo dvojité zpětných lomítek "\\\\" při zadávání připojovacího řetězce v kódu jazyka C#.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 do sady Visual Studio 2017 15.6

- Tato verze sady Visual Studio zahrnují nástroje Entity Framework a modulu runtime 6.1.3.
Zobrazit [posledních verzí](~/ef6/what-is-new/past-releases.md#ef-613) podrobné informace o těchto verzích.
- Přidání Entity Framework pro nové projekty pomocí nástrojů EF automaticky přidá EF 6.1.3 balíček NuGet.
Ručně můžete nainstalovat nebo upgradovat na libovolný balíček EF NuGet k dispozici online.
- Ve výchozím nastavení je instance systému SQL Server k dispozici s touto verzí sady Visual Studio na instanci LocalDB volá MSSQLLocalDB.
Část serveru byste měli použít připojovací řetězec je "(localdb)\\MSSQLLocalDB".
Nezapomeňte použít doslovný řetězec s předponou `@` nebo dvojité zpětných lomítek "\\\\" při zadávání připojovacího řetězce v kódu jazyka C#.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Tato verze sady Visual Studio obsahuje a starší verzi nástroje Entity Framework a modulu runtime.
Doporučuje se upgradovat na Entity Framework Tools 6.1.3, pomocí [instalační program](https://www.microsoft.com/en-us/download/details.aspx?id=40762) k dispozici na webu Microsoft Download Center.
Zobrazit [posledních verzí](~/ef6/what-is-new/past-releases.md#ef-613) podrobné informace o těchto verzích.
- Přidání Entity Framework pro nové projekty pomocí nástrojů, upgradovaný EF automaticky přidá EF 6.1.3 balíček NuGet.
Ručně můžete nainstalovat nebo upgradovat na libovolný balíček EF NuGet k dispozici online.
- Ve výchozím nastavení je instance systému SQL Server k dispozici s touto verzí sady Visual Studio na instanci LocalDB volá MSSQLLocalDB.
Část serveru byste měli použít připojovací řetězec je "(localdb)\\MSSQLLocalDB".
Nezapomeňte použít doslovný řetězec s předponou `@` nebo dvojité zpětných lomítek "\\\\" při zadávání připojovacího řetězce v kódu jazyka C#.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Tato verze sady Visual Studio obsahuje a starší verzi nástroje Entity Framework a modulu runtime.
Doporučuje se upgradovat na Entity Framework Tools 6.1.3, pomocí [instalační program](https://www.microsoft.com/en-us/download/details.aspx?id=40762) k dispozici na webu Microsoft Download Center.
Zobrazit [posledních verzí](~/ef6/what-is-new/past-releases.md#ef-613) podrobné informace o těchto verzích.
- Přidání Entity Framework pro nové projekty pomocí nástrojů, upgradovaný EF automaticky přidá EF 6.1.3 balíček NuGet.
Ručně můžete nainstalovat nebo upgradovat na libovolný balíček EF NuGet k dispozici online.
- Ve výchozím nastavení je instance systému SQL Server k dispozici s touto verzí sady Visual Studio v11.0 volat instanci LocalDB.
Část serveru byste měli použít připojovací řetězec je "(localdb)\\v11.0".
Nezapomeňte použít doslovný řetězec s předponou `@` nebo dvojité zpětných lomítek "\\\\" při zadávání připojovacího řetězce v kódu jazyka C#.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Verze Entity Framework Tools k dispozici s touto verzí sady Visual Studio není kompatibilní s modulem runtime Entity Framework 6 a proto nejde upgradovat.
- Ve výchozím nastavení přidá rozhraní Entity Framework nástroje Entity Framework 4.0 do vašich projektů.
Pokud chcete vytvářet aplikace pomocí jakékoli novější verze EF, musíte nejdřív nainstalovat [rozšíření Správce balíčků NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Ve výchozím nastavení vychází všechny generování kódu ve verzi nástroje EF EntityObject a Entity Framework 4.
Doporučujeme vám, že přepnete generování kódu byl založený na kontext databáze a Entity Framework 5 nainstalováním DbContext šablony generování kódu pro [jazyka C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) nebo [jazyka Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Po instalaci rozšíření Správce balíčků NuGet, můžete ručně nainstalovat nebo upgradovat na libovolný balíček EF NuGet k dispozici online a pomocí EF6 Code First, která nevyžaduje návrháře.
- Ve výchozím nastavení je instance systému SQL Server k dispozici s touto verzí sady Visual Studio, SQL Server Express s názvem SQLEXPRESS.
Část serveru byste měli použít připojovací řetězec je ". \\SQLEXPRESS ".
Nezapomeňte použít doslovný řetězec s předponou `@` nebo dvojité zpětných lomítek "\\\\" při zadávání připojovacího řetězce v kódu jazyka C#.
