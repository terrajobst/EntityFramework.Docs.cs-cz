---
title: Výběr verze modulu runtime Entity Framework pro modely návrháře EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418145"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Výběr verze modulu runtime Entity Framework pro modely návrháře EF
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Počínaje EF6 se do návrháře EF přidala následující obrazovka, která umožňuje vybrat verzi modulu runtime, pro kterou chcete při vytváření modelu cílit. Obrazovka se zobrazí, pokud v projektu ještě není nainstalovaná nejnovější verze Entity Framework. Pokud je už nejnovější verze nainstalovaná, bude se používat jenom ve výchozím nastavení.

![Obnovovací](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Cílení na EF6. x

Můžete zvolit EF6 z obrazovky zvolit vaši verzi a přidat do projektu modul runtime EF6. Po přidání EF6 se tato obrazovka v aktuálním projektu přestane zobrazovat.

EF6 se zakáže, pokud už máte nainstalovanou starší verzi EF (vzhledem k tomu, že nemůžete cílit na více verzí modulu runtime ze stejného projektu). Pokud zde není povolená možnost EF6, proveďte upgrade projektu na EF6 pomocí těchto kroků:

1.  V Průzkumník řešení klikněte pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet...**
2.  Vybrat **aktualizace**
3.  Vyberte **EntityFramework** (Ujistěte se, že je bude aktualizovat na požadovanou verzi).
4.  Kliknout na **aktualizovat**

 

## <a name="targeting-ef5x"></a>Cílení na EF5. x

Můžete zvolit EF5 z obrazovky zvolit vaši verzi a přidat do projektu modul runtime EF5. Po přidání EF5 se stále zobrazuje obrazovka s možností EF6 zakázaná.

Pokud máte již nainstalovanou verzi EF4. x modulu runtime, uvidíte, že je verze EF uvedená na obrazovce, nikoli EF5. V této situaci můžete upgradovat na EF5 pomocí následujících kroků:

1.  Výběr **nástrojů –&gt; správce balíčků knihovny –&gt; konzolu Správce balíčků**
2.  Spustit **instalaci-Package EntityFramework-Version 5.0.0**

 

## <a name="targeting-ef4x"></a>Cílení na EF4. x

Do projektu můžete nainstalovat modul runtime EF4. x pomocí následujících kroků:

1.  Výběr **nástrojů –&gt; správce balíčků knihovny –&gt; konzolu Správce balíčků**
2.  Spustit **instalaci-Package EntityFramework-Version 4.3.0**
