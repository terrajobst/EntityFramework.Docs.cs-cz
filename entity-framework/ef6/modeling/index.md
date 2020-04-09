---
title: Vytvoření modelu - EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: bd9843a93121f53518a307c9d2d43b68ae03369c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419753"
---
# <a name="creating-a-model"></a>Vytvoření modelu

Model EF ukládá podrobnosti o tom, jak jsou třídy a vlastnosti aplikace mapovat na databázové tabulky a sloupce. Existují dva hlavní způsoby vytvoření modelu EF:

- **Použití code first**: Vývojář zapíše kód k určení modelu. EF generuje modely a mapování za běhu na základě tříd entit a další konfigurace modelu poskytované vývojářem.

- **Použití EF Designer**: Vývojář nakreslí pole a řádky k určení modelu pomocí EF Designer. Výsledný model je uložen jako XML v souboru s příponou EDMX. Objekty domény aplikace jsou obvykle generovány automaticky z koncepčního modelu.

## <a name="ef-workflows"></a>Pracovní postupy EF

Oba tyto přístupy lze použít k cílení existující databáze nebo vytvořit novou databázi, výsledkem je 4 různé pracovní postupy.
Zjistěte, který z nich je pro vás nejlepší:  

|                                           | Chci jen napsat kód...                                                                                                                   | Chci použít návrháře...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Vytvářím novou databázi**          | [Nejprve použijte **kód** k definování modelu v kódu a následném generování databáze.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Pomocí **modelu Nejprve** definujte model pomocí polí a řádků a pak vygenerujte databázi.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Potřebuji přístup k existující databázi** | [Pomocí **code first** vytvořte model založený na kódu, který se mapuje na existující databázi.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Pomocí **funkce Database First** můžete vytvořit pole a řádkový model, který se mapuje na existující databázi.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Podívejte se na video: Jaký pracovní postup EF mám použít?

Toto krátké video vysvětluje rozdíly, a jak najít ten, který je pro vás to pravé.

**Uvádí**: [Rowan Miller](https://romiller.com/)

![Který pracovní](../media/whichworkflow-thumb.png) postup palec [WMV](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) |  | [WMWWMW (ZIP)](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip) [MP4](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v)

Pokud se po shlédnutí videa stále necítíte pohodlně při rozhodování, zda chcete použít EF Designer nebo Code First, naučte se obojí!

## <a name="a-look-under-the-hood"></a>Pohled pod kapotu

Bez ohledu na to, zda používáte Code First nebo EF Designer, model EF má vždy několik součástí:

- Objekty domény aplikace nebo typy entit sami. To se často označuje jako vrstva objektu

- Koncepční model skládající se z typů a vztahů entit specifických pro doménu, popsaný pomocí [datového modelu entity](~/ef6/resources/glossary.md#entity-data-model). Tato vrstva je často označována písmenem "C", pro _konceptuální_.

- Model úložiště představující tabulky, sloupce a relace definované v databázi. Tato vrstva je často označována s novější "S", pro _skladování_.  

- Mapování mezi koncepčním modelem a schématem databáze. Toto mapování se často označuje jako mapování "C-S".

Modul mapování EF využívá mapování "C-S" k transformaci operací proti entitám , jako je vytváření, čtení, aktualizace a odstranění , na ekvivalentní operace s tabulkami v databázi.

Mapování mezi koncepčním modelem a objekty aplikace se často označuje jako mapování O-C. Ve srovnání s mapováním "C-S" je mapování "O-C" implicitní a 1:1: entity, vlastnosti a vztahy definované v koncepčním modelu jsou vyžadovány tak, aby odpovídaly tvarům a typům objektů .NET. Z EF4 a dále objekty vrstvy se mohou skládat z jednoduchých objektů s vlastnostmi bez závislostí na EF. Ty jsou obvykle označovány jako plain-old CLR objekty (POCO) a mapování typů a vlastností se provádí na základě konvence porovnávání názvů. Dříve v EF 3.5 existovala specifická omezení pro vrstvu objektu, jako jsou entity, které musí být odvozeny z entityObject třídy a museli provádět atributy EF k implementaci mapování "O-C".
