---
title: Vydání sady Visual Studio – EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416938"
---
# <a name="visual-studio-releases"></a>Verze sady Visual Studio

Doporučujeme vždy použít nejnovější verzi sady Visual Studio, protože obsahuje nejnovější nástroje pro .NET, NuGet a Entity Framework.
V různých ukázkách a návodech v Entity Framework dokumentaci se předpokládá, že používáte nejnovější verzi sady Visual Studio.

Je však možné použít starší verze sady Visual Studio s různými verzemi Entity Framework, pokud vezmete v úvahu některé rozdíly:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15,7 a novější

- Tato verze sady Visual Studio zahrnuje nejnovější verze Entity Framework nástrojů a modul runtime EF 6,2 a nevyžaduje další kroky instalace.
Další podrobnosti o těchto verzích najdete v tématu [co je nového](~/ef6/what-is-new/index.md) .
- Přidání Entity Framework do nových projektů pomocí nástrojů EF automaticky přidá balíček NuGet pro EF 6,2.
Můžete ručně nainstalovat nebo upgradovat na jakýkoli dostupný balíček NuGet pro EF online.
- Ve výchozím nastavení je instance SQL Server dostupná v této verzi sady Visual Studio instancí LocalDB s názvem MSSQLLocalDB.
Oddíl serveru připojovacího řetězce, který byste měli použít, je "(LocalDB)\\MSSQLLocalDB".
Nezapomeňte použít doslovné řetězec s předponou `@` nebo dvojitým zpětným lomítkem "\\\\" při zadávání připojovacího řetězce v C# kódu.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 do sady Visual Studio 2017 15,6

- Tyto verze sady Visual Studio zahrnují nástroje Entity Framework a modul runtime 6.1.3.
Další podrobnosti o těchto verzích najdete v [předchozích verzích](~/ef6/what-is-new/past-releases.md#ef-613) .
- Přidání Entity Framework do nových projektů pomocí nástrojů EF automaticky přidá balíček NuGet pro EF 6.1.3.
Můžete ručně nainstalovat nebo upgradovat na jakýkoli dostupný balíček NuGet pro EF online.
- Ve výchozím nastavení je instance SQL Server dostupná v této verzi sady Visual Studio instancí LocalDB s názvem MSSQLLocalDB.
Oddíl serveru připojovacího řetězce, který byste měli použít, je "(LocalDB)\\MSSQLLocalDB".
Nezapomeňte použít doslovné řetězec s předponou `@` nebo dvojitým zpětným lomítkem "\\\\" při zadávání připojovacího řetězce v C# kódu.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Tato verze sady Visual Studio zahrnuje a starší verze Entity Framework nástrojů a modulu runtime.
Doporučuje se upgradovat na Entity Framework Tools 6.1.3 pomocí [instalačního programu](https://www.microsoft.com/download/details.aspx?id=40762) , který je k dispozici na webu Microsoft Download Center.
Další podrobnosti o těchto verzích najdete v [předchozích verzích](~/ef6/what-is-new/past-releases.md#ef-613) .
- Přidáním Entity Framework do nových projektů pomocí upgradovaných nástrojů EF se automaticky přidá balíček NuGet pro EF 6.1.3.
Můžete ručně nainstalovat nebo upgradovat na jakýkoli dostupný balíček NuGet pro EF online.
- Ve výchozím nastavení je instance SQL Server dostupná v této verzi sady Visual Studio instancí LocalDB s názvem MSSQLLocalDB.
Oddíl serveru připojovacího řetězce, který byste měli použít, je "(LocalDB)\\MSSQLLocalDB".
Nezapomeňte použít doslovné řetězec s předponou `@` nebo dvojitým zpětným lomítkem "\\\\" při zadávání připojovacího řetězce v C# kódu.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Tato verze sady Visual Studio zahrnuje a starší verze Entity Framework nástrojů a modulu runtime.
Doporučuje se upgradovat na Entity Framework Tools 6.1.3 pomocí [instalačního programu](https://www.microsoft.com/download/details.aspx?id=40762) , který je k dispozici na webu Microsoft Download Center.
Další podrobnosti o těchto verzích najdete v [předchozích verzích](~/ef6/what-is-new/past-releases.md#ef-613) .
- Přidáním Entity Framework do nových projektů pomocí upgradovaných nástrojů EF se automaticky přidá balíček NuGet pro EF 6.1.3.
Můžete ručně nainstalovat nebo upgradovat na jakýkoli dostupný balíček NuGet pro EF online.
- Ve výchozím nastavení je instance SQL Server dostupná v této verzi sady Visual Studio instancí LocalDB s názvem v 11.0.
Oddíl serveru připojovacího řetězce, který byste měli použít, je "(LocalDB)\\v 11.0".
Nezapomeňte použít doslovné řetězec s předponou `@` nebo dvojitým zpětným lomítkem "\\\\" při zadávání připojovacího řetězce v C# kódu.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Verze Entity Framework Tools dostupná v této verzi sady Visual Studio není kompatibilní s modulem runtime Entity Framework 6 a nelze ji upgradovat.
- Ve výchozím nastavení budou nástroje Entity Framework do vašich projektů přidávat Entity Framework 4,0.
Aby bylo možné vytvářet aplikace pomocí libovolných novějších verzí EF, bude nejprve nutné nainstalovat [rozšíření Správce balíčků NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Ve výchozím nastavení je veškeré generování kódu ve verzi nástrojů EF založené na objektů EntityObject a Entity Framework 4.
Doporučujeme, abyste přepnuli generování kódu, aby byl založen na DbContext a Entity Framework 5, instalací šablon pro generování kódu DbContext pro [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) nebo [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Po instalaci rozšíření Správce balíčků NuGet můžete ručně nainstalovat nebo upgradovat na jakýkoli dostupný balíček NuGet pro EF online a použít EF6 s Code First, který nevyžaduje návrháře.
- Ve výchozím nastavení je instance SQL Server dostupná v této verzi sady Visual Studio SQL Server Express s názvem SQLEXPRESS.
Oddíl serveru připojovacího řetězce, který byste měli použít, je ".\\SQLEXPRESS ".
Nezapomeňte použít doslovné řetězec s předponou `@` nebo dvojitým zpětným lomítkem "\\\\" při zadávání připojovacího řetězce v C# kódu.
