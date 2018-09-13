---
title: Výběr Entity Framework verze modulu Runtime EF návrháře modelů - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488488"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Výběr Entity Framework verze modulu Runtime EF návrháře modelů
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Počínaje EF6 na tomto obrázku byl přidán do EF designeru a umožňuje tak vybrat verzi modulu runtime, kterou chcete cílit při vytváření modelu. Na obrazovce se objeví, když v projektu již není nainstalována nejnovější verze Entity Framework. Pokud už je nainstalovaná nejnovější verze budou používat jenom ve výchozím nastavení.

![Obrazovka](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Cílení EF6.x

EF6 můžete vybrat z obrazovky zvolte si verzi přidat modul runtime EF6 do projektu. Po přidání EF6, už nebudou zobrazovat tuto obrazovku v aktuálním projektu.

EF6 bude zakázáno, pokud už máte starší verzi EF nainstalovaný (protože je nelze cílit na více verzí modulu runtime ve stejném projektu). Pokud tady není povolená možnost EF6, upgradujte váš projekt EF6 pomocí těchto kroků:

1.  Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat balíčky NuGet...**
2.  Vyberte **aktualizace**
3.  Vyberte **EntityFramework** (ujistěte se, že se bude aktualizovat na požadovanou verzi)
4.  Klikněte na tlačítko **aktualizace**

 

## <a name="targeting-ef5x"></a>Cílení EF5.x

EF5 můžete vybrat z obrazovky zvolte si verzi modulu runtime EF5 přidejte do projektu. Po přidání EF5, pořád uvidíte na obrazovce s EF6 možnost zakázána.

Pokud máte EF4.x verzi modulu runtime nainstalovány uvidíte, že tato verze EF uvedené v EF5 obrazovky spíše než. V takovém případě můžete upgradovat na EF5 pomocí následujících kroků:

1.  Vyberte **nástroje –&gt; Správce balíčků knihoven -&gt; Konzola správce balíčků**
2.  Spustit **Install-Package EntityFramework – verze 5.0.0**

 

## <a name="targeting-ef4x"></a>Cílení EF4.x

Modul runtime EF4.x můžete nainstalovat do vašeho projektu pomocí následujících kroků:

1.  Vyberte **nástroje –&gt; Správce balíčků knihoven -&gt; Konzola správce balíčků**
2.  Spustit **EntityFramework Install-Package-verze 4.3.0**
