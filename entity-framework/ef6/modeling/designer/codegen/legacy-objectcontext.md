---
title: Vracení do objektu ObjectContext v Entity Framework Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418656"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Vracení do objektu ObjectContext v Entity Framework Designer
V předchozí verzi Entity Framework model vytvořený pomocí návrháře EF vygeneroval kontext, který je odvozen z tříd ObjectContext a entity, které jsou odvozeny z objektů EntityObject.

Počínaje EF 4.1 doporučujeme odkládací šablonu pro generování kódu, která generuje kontext odvozený od tříd entit DbContext a POCO.

V aplikaci Visual Studio 2012 získáte DbContext kód vygenerovaný ve výchozím nastavení pro všechny nové modely vytvořené pomocí návrháře EF. Stávající modely budou nadále generovat kód založený na ObjectContext, pokud se nerozhodnete přepínat na generátor kódu založeného na DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Návrat zpět na generování kódu ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. zakažte generování kódu DbContext.

Generování odvozených tříd DbContext a POCO je zpracováváno dvěma soubory. TT v projektu. Pokud rozbalíte soubor. edmx v Průzkumníkovi řešení, zobrazí se tyto soubory. Odstraňte oba tyto soubory z projektu.

![Obecné soubory kódu](~/ef6/media/codegenfiles.png)

Pokud používáte VB.NET, budete muset vybrat tlačítko **Zobrazit všechny soubory** a zobrazit vnořené soubory.

![Zobrazit všechny soubory](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. opětovné povolení generování kódu ObjectContext

V Návrháři EF otevřete model, klikněte pravým tlačítkem na prázdnou část návrhové plochy a vyberte **vlastnosti**.

V okno Vlastnosti změňte **strategii generování kódu** z **none** na **Default**.

![Obecná strategie kódu](~/ef6/media/codegenstrategy.png)
