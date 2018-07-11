---
title: Získat Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
caps.latest.revision: 4
ms.openlocfilehash: 400bf1428e6754a88dbc1264c346bb66282725a0
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949000"
---
# <a name="get-entity-framework"></a>Získat Entity Framework
Entity Framework se skládá z EF nástrojů pro Visual Studio a modul Runtime EF.

## <a name="ef-tools-for-visual-studio"></a>EF Tools for Visual Studio

Entity Framework Tools for Visual Studio zahrnují EF designeru a v Průvodci modelů EF a jsou nejprve nutné pro databázi a model první pracovní postupy. EF nástroje jsou součástí všechny nejnovější verze sady Visual Studio. -Li provést vlastní instalaci sady Visual Studio, budete muset zajistit, aby položka "Entity Framework 6 Tools" je vybrána volba úlohu, která ji obsahuje nebo výběrem jednotlivých součástí.

U některých starších verzí sady Visual Studio aktualizované EF nástroje jsou k dispozici ke stažení. Zobrazit [verzí sady Visual Studio](~/ef6/what-is-new/visual-studio.md) pokyny o tom, jak získat nejnovější verzi nástrojů EF dostupných pro vaši verzi sady Visual Studio.

## <a name="ef-runtime"></a>Modul Runtime EF

Nejnovější verzi rozhraní Entity Framework je k dispozici jako [balíček EntityFramework NuGet](http://nuget.org/packages/EntityFramework/). Pokud nejste obeznámeni s Správce balíčků NuGet, doporučujeme vám přečíst si téma [NuGet přehled](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Instaluje se balíček NuGet EF

Můžete nainstalovat balíček EntityFramework kliknutím pravým tlačítkem na **odkazy** složky vašeho projektu a výběrem **spravovat balíčky NuGet...**

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Instalace z konzoly Správce balíčků

Alternativně můžete nainstalovat EntityFramework spuštěním následujícího příkazu v [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Instalace konkrétní verze EF

Z EF 4.1 a vyšší, byly vydány nové verze modulu EF runtime jako [balíčku NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/). Některé z těchto verzí můžete přidat do projektu na základě rozhraní .NET Framework, spuštěním následujícího příkazu v sadě Visual Studio [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Všimněte si, že `<number>` představuje konkrétní verze EF k instalaci. Například 6.2.0 je verze číslo pro EF 6.2.   

Moduly runtime EF před 4.1 byly součástí rozhraní .NET Framework a se nedá nainstalovat samostatně.

### <a name="installing-the-latest-preview"></a>Instalace nejnovější verze Preview

Výše uvedené metody získáte nejnovější plně podporované verze rozhraní Entity Framework. Jsou často předběžné verze dostupná, budeme rádi, kdybyste k vyzkoušení a sdělte nám svůj názor na rozhraní Entity Framework.

Chcete-li nainstalovat nejnovější verzi preview objektu EntityFramework, můžete vybrat **zahrnout předprodejní verze** v okně Správa balíčků NuGet. Pokud jsou k dispozici žádné předběžné verze se automaticky zobrazí nejnovější plně podporovanou verzi rozhraní Entity Framework.

![IncludePreRelease](~/ef6/media/includeprerelease.png)

Alternativně můžete spustit následující příkaz [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
