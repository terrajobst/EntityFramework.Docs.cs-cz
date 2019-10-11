---
title: Získat Entity Framework – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181742"
---
# <a name="get-entity-framework"></a>Jak získat Entity Framework
Entity Framework se skládá z nástrojů EF pro Visual Studio a modulu runtime EF.

## <a name="ef-tools-for-visual-studio"></a>Nástroje EF pro Visual Studio

Entity Framework Tools pro Visual Studio obsahuje návrháře EF a Průvodce modelem EF a jsou nutné pro databázi nejprve a modelovat první pracovní postupy. Nástroje EF jsou zahrnuté ve všech nejnovějších verzích sady Visual Studio. Pokud provedete vlastní instalaci sady Visual Studio, bude nutné zajistit, aby se položka "Entity Framework 6 Tools" vybrala buď tak, že vyberete úlohu, která ji obsahuje, nebo ji vyberete jako jednotlivou součást.

Pro některé starší verze sady Visual Studio jsou aktualizované nástroje EF k dispozici jako soubor ke stažení. Návod, jak získat nejnovější verzi nástrojů EF dostupných pro vaši verzi sady Visual Studio, najdete v tématu [verze sady Visual Studio](~/ef6/what-is-new/visual-studio.md) .

## <a name="ef-runtime"></a>Modul runtime EF

Nejnovější verze Entity Framework je k dispozici jako [balíček NuGet EntityFramework](https://nuget.org/packages/EntityFramework/). Pokud nejste obeznámeni se správcem balíčků NuGet, doporučujeme, abyste si přečetli [Přehled NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Instalace balíčku NuGet pro EF

Balíček EntityFramework můžete nainstalovat tak, že kliknete pravým tlačítkem na složku **odkazy** v projektu a vyberete **Spravovat balíčky NuGet...**

![Správa balíčků NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Instalace z konzoly Správce balíčků

Alternativně můžete nainstalovat EntityFramework spuštěním následujícího příkazu v [konzole správce balíčků](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Instalace konkrétní verze EF

Od EF 4,1 a vyšší jsou nové verze modulu runtime EF vydány jako [balíček NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/). Kteroukoli z těchto verzí lze přidat do projektu založeného na .NET Framework spuštěním následujícího příkazu v [konzole správce balíčků](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)sady Visual Studio:

``` powershell
Install-Package EntityFramework -Version <number>
```

Všimněte si, že `<number>` představuje konkrétní verzi EF, která se má nainstalovat. Například 6.2.0 je verze čísla pro EF 6,2.   

Moduly runtime EF před 4,1 byly součástí .NET Framework a nelze je nainstalovat samostatně.

### <a name="installing-the-latest-preview"></a>Instalace nejnovější verze Preview

Výše uvedené metody vám poskytnou nejnovější plně podporovanou verzi Entity Framework. K dispozici jsou často předprodejní verze Entity Framework, které bychom rádi si vyzkoušeli a poskytli nám zpětnou vazbu.

Pokud chcete nainstalovat nejnovější verzi Preview nástroje EntityFramework, můžete v okně Spravovat balíčky NuGet vybrat možnost **Zahrnout předprodejní verze** . Pokud nejsou k dispozici žádné předprodejní verze, bude automaticky získána nejnovější plně podporovaná verze Entity Framework.

![Zahrnout předběžné verze](~/ef6/media/includeprerelease.png)

Případně můžete spustit následující příkaz v [konzole správce balíčků](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
