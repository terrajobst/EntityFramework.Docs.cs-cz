---
title: Vytvoření modelu – EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: bd9843a93121f53518a307c9d2d43b68ae03369c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182467"
---
# <a name="creating-a-model"></a>Vytvoření modelu

Model EF ukládá podrobnosti o tom, jak se třídy a vlastnosti aplikace mapují na tabulky a sloupce databáze. Existují dva hlavní způsoby, jak vytvořit model EF:

- **Použití Code First**: vývojář zapisuje kód pro určení modelu. EF generuje modely a mapování za běhu na základě tříd entit a další konfiguraci modelu poskytovanou vývojářem.

- **Pomocí NÁVRHÁŘE EF**: vývojář kreslí pole a řádky pro určení modelu pomocí návrháře EF. Výsledný model je uložen jako XML v souboru s příponou EDMX. Objekty domény aplikace se obvykle generují automaticky z koncepčního modelu.

## <a name="ef-workflows"></a>Workflowy EF

Oba tyto přístupy lze použít k zacílení existující databáze nebo vytvoření nové databáze, což vede ke 4 různým pracovním postupům.
Zjistěte, který z nich je pro vás nejvhodnější:  

|                                           | Přeji si pouze napsat kód...                                                                                                                   | Chci použít návrháře...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Vytvářím novou databázi**          | [Použijte **Code First** k definování modelu v kódu a následnému vygenerování databáze.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Použijte **model First** k definování modelu pomocí polí a řádků a následnému vygenerování databáze.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Potřebuji získat přístup k existující databázi** | [Pomocí **Code First** můžete vytvořit model založený na kódu, který se mapuje na existující databázi.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Pomocí **Database First** můžete vytvořit model polí a čar, který se mapuje na existující databázi.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Podívejte se na video: Jaký pracovní postup EF mám použít?

Toto krátké video vysvětluje rozdíly a najdete tu, která je pro vás nejvhodnější.

**Prezentující**: [Rowan Miller](https://romiller.com/)

![které pracovní postup miniatury](../media/whichworkflow-thumb.png) [wmv](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Pokud se po sledování videa stále nemusíte rozhodovat, jestli chcete použít návrháře EF nebo Code First, přečtěte si obojí.

## <a name="a-look-under-the-hood"></a>Pohled pod digestoř

Bez ohledu na to, zda používáte Code First nebo Návrhář EF, má model EF vždycky několik součástí:

- Objekty domény aplikace nebo samotné typy entit. To se často označuje jako vrstva objektu.

- Koncepční model skládající se z typů entit a vztahů specifických pro doménu, popsaných pomocí [model EDM (Entity Data Model)](~/ef6/resources/glossary.md#entity-data-model). Tato vrstva je často označována znakem "C", pro _koncepční_.

- Model úložiště reprezentující tabulky, sloupce a relace, jak jsou definovány v databázi. Tato vrstva se často označuje pomocí pozdějšího "S" pro _úložiště_.  

- Mapování mezi koncepčním modelem a schématem databáze. Toto mapování se často označuje jako mapování "C-S".

Modul pro mapování EF využívá mapování "C-S" na transformace operací s entitami – například vytváření, čtení, aktualizace a odstraňování ekvivalentních operací s tabulkami v databázi.

Mapování mezi koncepčním modelem a objekty aplikace je často označováno jako mapování "O-C". V porovnání s mapováním "C-S" je mapování "O-C" implicitní a 1:1: entity, vlastnosti a vztahy definované v koncepčním modelu jsou vyžadovány pro porovnání tvarů a typů objektů rozhraní .NET. Z EF4 a mimo ni může být vrstva objektů složena z jednoduchých objektů s vlastnostmi bez jakýchkoli závislostí na EF. Ty jsou obvykle označovány jako objekty CLR v prostém Old (POCO) a mapování typů a vlastností je provedeno na bázi konvencí pro porovnání názvů. Dříve existovala v EF 3,5 specifická omezení pro vrstvu objektů, například entity, které se musí odvozovat od třídy objektů EntityObject a musí přenášet atributy EF pro implementaci mapování "O-C".
