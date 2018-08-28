---
title: Vytvoření modelu - EF6
author: divega
ms.date: 2018-07-05
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: fda9caedfe1dd6c919bb1917bda007ad8ddd4539
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995574"
---
# <a name="creating-a-model"></a>Vytvoření modelu

Modelu EF jsou uloženy podrobnosti o použití tříd a vlastností mapování do databáze tabulky a sloupce. Vytvoření modelu EF dvěma způsoby:

- **Pomocí Code First**: vývojář píše kód k určení modelu. EF generuje modely a mapování na modul runtime podle tříd entit a další model konfigurace od vývojáře.

- **Pomocí EF designeru**: vývojáře kreslí polí a řádky určené k určení modelu pomocí EF designeru. Výsledný model se ukládá jako kód XML v souboru s příponou EDMX. Objekty domény aplikace obvykle automaticky generovány v konceptuálním modelu.

## <a name="ef-workflows"></a>EF pracovních postupů

Jak z těchto postupů je možné cílit na existující databázi nebo vytvořte novou databázi, což vede k 4 různé pracovní postupy.
Přečtěte si o tom, které jedna je pro vás nejvhodnější:  

|                                           | Chci napsat kód...                                                                                                                   | Chci používat návrháře...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Vytvářím nové databáze**          | [Použití **Code First** definovat model v kódu a pak vytvořte databázi.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Použití **první Model** definovat model pomocí polí a řádky a pak vytvořte databázi.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Potřebuji pro přístup k existující databázi** | [Použití **Code First** pro vytvoření kódu na základě modelu, který se mapuje na existující databázi.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Použití **Database First** k vytvoření polí a řádky modelu, který se mapuje na existující databázi.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Podívejte se na video: jaké EF pracovního postupu použít?

Toto krátké video vysvětluje rozdíly a tom, jak najít ten, který je pro vás nejvhodnější.

**Přednášející:**: [Rowan Miller](http://romiller.com/)

![WhichWorkflow_Thumb](../media/whichworkflow-thumb.png) [WMV](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Pokud po zhlédnutí tohoto videa, které se můžete stále není s klidem rozhodování o tom, zda chcete použít EF designeru nebo Code First, zjistěte, jak!

## <a name="a-look-under-the-hood"></a>Pohled pod kapotu

Bez ohledu na to, ať už používáte Code First nebo EF designeru modelu EF vždy má několik součástí:

- Objekty domény aplikace nebo entity typy sami. To se často označuje jako objektové vrstvě

- Konceptuální model, který se skládá z entity specifického pro doménu typů a vztahů, popisují pomocí [modelu Entity Data Model](~/ef6/resources/glossary.md#entity-data-model). Tato vrstva je často označována s písmenem "C" pro _koncepční_.

- Model úložiště představující tabulky, sloupce a relace, jak jsou definovány v databázi. Tato vrstva je často označována pomocí novější "S", pro _úložiště_.  

- Mapování mezi konceptuálního modelu a schématu databáze. Toto mapování se často označuje jako "C-S" mapování.

Modul mapování na EF využívá "C-S" mapování transformace operací s entitami – jako například vytvářet, číst, aktualizovat a odstraňovat - do ekvivalentními operacemi na tabulky v databázi.

Mapování mezi Koncepční model a objekty aplikaci se často označuje jako "Vstupně-C" mapování. Ve srovnání s mapování "C-S", "O-C" mapování je implicitní a 1: 1: entity, vlastnosti a vztahy definované v konceptuálním modelu musí odpovídat tvary a typy objektů .NET. Z EF4 a nad rámec objekty vrstvy se může skládat z jednoduchého objekty s vlastnostmi nezávisle na EF. Tyto prvky jsou obvykle označovány jako prostý staré CLR objektů POCO a mapování typů a vlastností se provádí základní na název odpovídající konvencí. Dříve v EF 3.5 existovaly zvláštní omezení pro objektové vrstvě, stejně jako entity s odvodit z třídy EntityObject a muset nosit EF atributy implementovat mapování "Vstupně-C".
