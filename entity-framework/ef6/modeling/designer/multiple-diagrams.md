---
title: Více diagramů na model – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e78b927a147143629ac5b73e23edf439ba6d0264
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418292"
---
# <a name="multiple-diagrams-per-model"></a>Více diagramů na model
> [!NOTE]
> **EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Toto video a stránka ukazuje, jak rozdělit model do více diagramů pomocí Entity Framework Designer (EF Designer). Tuto funkci můžete chtít použít, když je váš model moc velký, aby se mohl zobrazit nebo upravit.

V dřívějších verzích návrháře EF bylo možné mít pro každý soubor EDMX pouze jeden diagram. Počínaje sadou Visual Studio 2012 můžete pomocí návrháře EF rozdělit soubor EDMX do více diagramů.

## <a name="watch-the-video"></a>Přehrát video
Toto video ukazuje, jak rozdělit model do více diagramů pomocí Entity Framework Designer (EF Designer). Tuto funkci můžete chtít použít, když je váš model moc velký, aby se mohl zobrazit nebo upravit.

**Prezentující**: Helena Kornich

**Video**: [wmv](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Přehled návrháře EF

Když vytvoříte model pomocí Průvodce model EDM (Entity Data Model) návrháře EF, vytvoří se soubor. edmx a přidá se do vašeho řešení. Tento soubor definuje tvar entit a jejich mapování na databázi.

Návrhář EF se skládá z následujících součástí:

-   Vizuální návrhová plocha pro úpravu modelu. Můžete vytvářet, upravovat nebo odstraňovat entity a přidružení.
-    **Prohlížeč modelů** okno, které poskytuje stromové zobrazení modelu.  Entity a jejich přidružení jsou umístěné pod *\[\]* složky. Tabulky a omezení databáze jsou umístěny pod *\[\]* . Složka úložiště
-   Okno **podrobností mapování** pro zobrazení a úpravy mapování Můžete namapovat typy entit nebo přidružení k databázovým tabulkám, sloupcům a uloženým procedurám. 

Po dokončení průvodce model EDM (Entity Data Model) se automaticky otevře okno vizuální návrh plochy. Pokud není prohlížeč modelů viditelný, klikněte pravým tlačítkem myši na hlavní návrhovou plochu a vyberte možnost **prohlížeč modelů**.

Následující snímek obrazovky ukazuje soubor. edmx otevřený v Návrháři EF. Snímek obrazovky ukazuje vizuální návrhovou plochu (vlevo) a **prohlížeč modelů** okno (napravo).

![EF Designer 2](~/ef6/media/efdesigner2.png)

Chcete-li vrátit zpět operaci prováděnou v Návrháři EF, klikněte na tlačítko CTRL-Z.

## <a name="working-with-diagrams"></a>Práce s diagramy

Ve výchozím nastavení Návrhář EF vytvoří jeden diagram nazvaný Diagram1. Pokud máte diagram s velkým počtem entit a přidružení, budete ho chtít nejlépe rozdělit logicky. Počínaje sadou Visual Studio 2012 můžete zobrazit koncepční model ve více diagramech.   

Při přidávání nových diagramů se zobrazí ve složce diagramy v okně Prohlížeč modelů. Přejmenování diagramu: v okně prohlížeče modelů vyberte diagram, klikněte jednou na název a zadejte nový název.  Můžete také kliknout pravým tlačítkem myši na název diagramu a vybrat možnost **Přejmenovat**.

Název diagramu se zobrazí vedle názvu souboru. edmx v editoru sady Visual Studio. Například Model1. edmx\[Diagram1\].

![Schéma](~/ef6/media/diagramname.png)

Obsah diagramů (tvar a barva entit a přidružení) je uložený v souboru. edmx. diagram. Chcete-li zobrazit tento soubor, vyberte Průzkumník řešení a rozložte soubor. edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Soubor. edmx. diagram byste neměli upravovat ručně. obsah tohoto souboru je možná přepsaný návrhářem EF.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Rozdělení entit a přidružení do nového diagramu

V existujícím diagramu můžete vybrat entity (při výběru více entit podržte klávesu Shift). Klikněte pravým tlačítkem myši a vyberte **přesunout do nového diagramu**. Vytvoří se nový diagram a vybrané entity a jejich přidružení se přesunou do diagramu.

Případně můžete kliknout pravým tlačítkem na složku diagramy v prohlížeči modelů a vybrat **Přidat nový diagram.** Pak můžete přetahovat entity ze složky typy entit v prohlížeči modelu na návrhovou plochu.

Můžete také vyjmout nebo kopírovat entity (pomocí kláves CTRL-X nebo CTRL-C) z jednoho diagramu a vložit (pomocí klávesy CTRL-V) na druhé straně. Pokud diagram, do kterého chcete entitu vložit, již obsahuje entitu se stejným názvem, vytvoří se nová entita, která se přidá do modelu.  Příklad: diagram2 obsahuje entitu oddělení. Pak do diagram2 vložíte jiné oddělení. Entita Department1 je vytvořena a přidána do koncepčního modelu.   

Pokud chcete do diagramu zahrnout související entity, Rick klikněte na entitu a vyberte **zahrnout související**. Tato akce vytvoří kopii souvisejících entit a přidružení v zadaném diagramu.

## <a name="changing-the-color-of-entities"></a>Změna barvy entit

Kromě rozdělení modelu do více diagramů můžete také měnit barvy entit.

Chcete-li změnit barvu, vyberte entitu (nebo více entit) na návrhové ploše. Pak klikněte pravým tlačítkem myši a vyberte možnost **vlastnosti**. V okno Vlastnosti vyberte vlastnost **Barva výplně** . Zadejte barvu buď pomocí platného názvu barvy (například Red), nebo platného RGB (například 255, 128, 128). 

![Barva](~/ef6/media/color.png)

## <a name="summary"></a>Souhrn

V tomto tématu jsme se podívali na to, jak rozdělit model do více diagramů a také jak zadat jinou barvu pro entitu pomocí Entity Framework Designer. 
