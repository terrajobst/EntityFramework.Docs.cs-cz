---
title: Více diagramů za Model - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e78b927a147143629ac5b73e23edf439ba6d0264
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283612"
---
# <a name="multiple-diagrams-per-model"></a>Více diagramů podle modelu
> [!NOTE]
> **EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Toto video a stránce ukazuje, jak rozdělit do více diagramů pomocí návrháře Entity Framework (EF designeru) modelu. Můžete chtít tuto funkci používat, když se stane příliš velký, aby umožňuje zobrazit nebo upravit model.

V dřívějších verzích EF designeru může mít pouze jeden diagram na soubor EDMX. Od verze Visual Studio 2012, můžete použít EF designeru pro rozdělení souboru EDMX do více diagramů.

## <a name="watch-the-video"></a>Podívejte se na video
Toto video ukazuje, jak rozdělit do více diagramů pomocí návrháře Entity Framework (EF designeru) modelu. Můžete chtít tuto funkci používat, když se stane příliš velký, aby umožňuje zobrazit nebo upravit model.

**Přednášející:**: Julia Kornich

**Video**: [WMV](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Přehled EF designeru

Při vytváření modelu pomocí Průvodce datovým modelem Entity EF designeru soubor .edmx je vytvořen a přidán do vašeho řešení. Tento soubor definuje obrazec entity a jak jsou mapovány do databáze.

EF designeru se skládá z následujících součástí:

-   Vizuální návrhová plocha pro úpravy modelu. Můžete vytvořit, upravit nebo odstranit entit a přidružení.
-   A **prohlížeč modelu** okno, které poskytuje zobrazení stromu modelu.  Entity a jejich přidružení se nacházejí v části *\[%{ModelName/\]* složky. Databázové tabulky a omezení jsou umístěné v části  *\[%{ModelName/\]*. Složka Store.
-   A **podrobnosti mapování** okno pro zobrazení a úprava mapování. Typy entit a přidružení můžete namapovat na databázové tabulky, sloupce a uložené procedury. 

Po dokončení Průvodce datovým modelem Entity automaticky otevření okna návrhové plochy. Pokud prohlížeč modelu se nezobrazuje, klikněte pravým tlačítkem na hlavní návrhové ploše a vyberte **prohlížeč modelu**.

Následující snímek obrazovky ukazuje soubor .edmx otevřen v EF designeru. Snímek obrazovky ukazuje vizuální návrhová plocha (nalevo) a **prohlížeč modelu** okno (napravo).

![EF designeru 2](~/ef6/media/efdesigner2.png)

Chcete-li v EF designeru operaci vrátit zpět, klikněte na tlačítko Ctrl-Z.

## <a name="working-with-diagrams"></a>Práce s diagramy

Ve výchozím nastavení vytvoří EF designeru volá Diagram1 jeden diagram. Pokud máte diagramu s velkým množstvím entit a přidružení, bude nejvíc jako logicky rozdělit. Od verze Visual Studio 2012, můžete zobrazit koncepčního modelu ve více diagramů.   

Jak budete přidávat nové diagramy, se zobrazí ve složce diagramy v okně prohlížeče modelu. Chcete-li přejmenovat diagram: Vyberte diagramu v okně prohlížeče modelu, jednou klikněte na název a zadejte nový název.  Můžete také klikněte pravým tlačítkem na název diagramu a vybrat **přejmenovat**.

Název diagramu se zobrazí vedle názvu souboru EDMX, v editoru sady Visual Studio. Například Model1.edmx\[Diagram1\].

![DiagramName](~/ef6/media/diagramname.png)

Diagramy obsah (tvar a barva entit a přidružení) je uložen v. edmx.diagram souboru. Chcete-li zobrazit tento soubor, vyberte Průzkumníka řešení a rozbalit soubor .edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Byste neměli upravovat. edmx.diagram soubor ručně, obsah tohoto souboru možná přepsána EF designeru.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Rozdělení entit a přidružení do nového diagramu

Můžete vybrat entit na existující diagram (stiskněte a podržte klávesu Shift vyberte více entit). Klikněte pravým tlačítkem myši a vyberte **přesunout do nového diagramu**. Je vytvořen nový diagram a vybrané entity a jejich přidružení se přesunula do diagramu.

Alternativně můžete klikněte pravým tlačítkem na složku diagramy v prohlížeči modelů a vybrat **přidat nový Diagram.** Můžete pak přetáhněte a umístěte entity ze složky typy entit v modelu prohlížeče na návrhovou plochu.

Můžete také vyjmout nebo zkopírovat entity (pomocí klávesy Ctrl-X nebo Ctrl-C) z jednoho diagramu a vložit (pomocí klávesy Ctrl-V) na straně druhé. Pokud diagram, do které se chystáte vložit entita již obsahuje entity se stejným názvem, nová entita bude vytvořen a přidán do modelu.  Příklad: Diagram2 obsahuje entitu oddělení. Pak na Diagram2 vložte jiného oddělení. Department1 entity je vytvořen a přidán do koncepčního modelu.   

Zahrnout související entity v diagramu, rick klepněte na entitu a vyberte **zahrnout související**. To bude vytvořte kopii souvisejících entit a přidružení zadaného diagramu.

## <a name="changing-the-color-of-entities"></a>Změna barvy entity

Kromě modelu rozdělit do více diagramů, můžete také změnit barvy entity.

Chcete-li změnit barvu, vyberte entity (nebo více entit) na návrhové ploše. Potom klikněte pravým tlačítkem myši a vyberte **vlastnosti**. V okně Vlastnosti vyberte **Barva výplně** vlastnost. Zadejte barvu pomocí platná barva názvu (například Red) nebo platný RGB (například 255, 128, 128). 

![Barva](~/ef6/media/color.png)

## <a name="summary"></a>Souhrn

V tomto tématu jsme se podívali na tom, jak rozdělit do více diagramů modelu a také jak určit jinou barvu entity pomocí Entity Framework Designer. 
