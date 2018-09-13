---
title: Entity Framework Designer klávesové zkratky – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: c75eafcca0863faa1ad64202e98b61832827377c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490243"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Entity Framework Designer klávesové zkratky
Tato stránka obsahuje seznam zástupců klávesnice aplikace, které jsou dostupné na různých obrazovkách Entity Framework Tools for Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Průvodce datovým modelem ADO.NET Entity

### <a name="step-one-choose-model-contents"></a>Jeden krok: Výběr obsahu modelu

![Průvodce jeden](~/ef6/media/wizardone.png)

| Zástupce  | Akce                                                     | Poznámky                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **ALT + n** | Přesunout na další obrazovce                                        | Není k dispozici pro celý výběr obsahu modelu. |
| **ALT + f** | Dokončení Průvodce                                              | Není k dispozici pro celý výběr obsahu modelu. |
| **ALT + w** | Přepnout fokus "co by měl model obsahující?" podokno. |                                                     |

### <a name="step-two-choose-your-connection"></a>Krok 2: Zvolte připojení

![Průvodce dvě](~/ef6/media/wizardtwo.png)

| Zástupce  | Akce                                                     | Poznámky                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **ALT + n** | Přesunout na další obrazovce                                        |                                                         |
| **ALT + p** | Přesunout na předchozí obrazovku                                    |                                                         |
| **ALT + w** | Přepnout fokus "co by měl model obsahující?" podokno. |                                                         |
| **ALT + c** | Otevřete okno "Vlastnosti připojení"                    | Umožňuje definice nového připojení k databázi. |
| **ALT + e** | Vyloučit citlivá data z připojovacího řetězce          |                                                         |
| **ALT + i** | Zahrnutí důvěrných osobních údajů v připojovacím řetězci            |                                                         |
| **ALT + s** | Přepněte možnost "Uložit nastavení připojení v souboru App.Config" |                                                         |

### <a name="step-three-choose-your-version"></a>Krok 3: Zvolte vaši verzi

![Průvodce tři](~/ef6/media/wizardthree.png)

| Zástupce  | Akce                                             | Poznámky                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **ALT + n** | Přesunout na další obrazovce                                |                                                                                       |
| **ALT + p** | Přesunout na předchozí obrazovku                            |                                                                                       |
| **ALT + w** | Přepnout fokus na výběr verze rozhraní Entity Framework | Umožňuje zadání jinou verzi rozhraní Entity Framework pro použití v projektu. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Krok 4: Zvolte vaše databázové objekty a nastavení

![Průvodce čtyři](~/ef6/media/wizardfour.png)

| Zástupce  | Akce                                                                                    | Poznámky                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Dokončení Průvodce                                                                             |                                                                     |
| **ALT + p** | Přesunout na předchozí obrazovku                                                                   |                                                                     |
| **ALT + w** | Přepnout fokus na podokno výběru objektu databáze                                            | Umožňuje zadání databázových objektů pro zpětnou analýzou.    |
| **ALT + s** | Přepnout "Pluralize jednotné nebo množné číslo generované názvy objektu" možnost                       |                                                                     |
| **ALT + k** | Přepněte možnost "Zahrnout do modelu sloupce cizích klíčů"                              | Není k dispozici pro celý výběr obsahu modelu.                 |
| **ALT + i** | Přepněte možnost "Import vybraných uložených procedur a funkcí do entity model" | Není k dispozici pro celý výběr obsahu modelu.                 |
| **ALT + m** | Přepne fokus do pole text "Model Namespace"                                        | Není k dispozici pro celý výběr obsahu modelu.                 |
| **místo** | Přepnout výběr na prvek                                                               | Pokud element obsahuje podřízené položky, budou všechny podřízené prvky také přepínat |
| **doleva**  | Sbalit podřízeného stromu                                                                       |                                                                     |
| **doprava** | Rozbalte strom podřízené                                                                         |                                                                     |
| **Nahoru**    | Přejít na předchozí prvek ve stromu                                                      |                                                                     |
| **Dolů**  | Přejděte na další prvek ve stromu                                                          |                                                                     |

## <a name="ef-designer-surface"></a>EF návrhové ploše

![Návrhové ploše](~/ef6/media/designersurface.png)

| Zástupce                                                                                | Akce                      | Poznámky                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Zadejte/prostor**                                                                         | Přepnout výběr            | Přepne výběr u objektu s fokus.                                                                                                                                                                                         |
| **ESC**                                                                                 | Zrušit výběr            | Zruší aktuální výběr.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Vybrat vše                  | Vybere všechny obrazce na návrhové ploše.                                                                                                                                                                                       |
| **Šipka nahoru**                                                                            | Posunout nahoru                     | Přesune vybrané entity do jedné mřížce přírůstku. <br/> Pokud v seznamu přesune do předchozí dílčího pole na stejné úrovni.                                                                                                                            |
| **Šipka dolů**                                                                          | Přesunout dolů                   | Přesune vybrané entity dolů přírůstek jedné mřížce. <br/> Pokud v seznamu přesune na další dílčího pole na stejné úrovni.                                                                                                                              |
| **Šipka doleva**                                                                          | Doleva                   | Přesune vybrané entity levé jedné mřížce přírůstku. <br/> Pokud v seznamu přesune do předchozí dílčího pole na stejné úrovni.                                                                                                                          |
| **Šipka doprava**                                                                         | Doprava                  | Přesune vybrané přírůstek mřížky doprava o jednu entitu. <br/> Pokud v seznamu přesune na další dílčího pole na stejné úrovni.                                                                                                                             |
| **Shift + šipka vlevo**                                                                  | Velikost tvaru vlevo             | Šířka vybranou entitu snižuje v přírůstcích po jedné mřížce.                                                                                                                                                                     |
| **Shift + šipka vpravo**                                                                 | Velikost tvaru vpravo            | Zvětšuje šířku vybranou entitu v přírůstcích po jedné mřížce.                                                                                                                                                                   |
| **Domovská stránka**                                                                                | První Peer                  | Přesune fokus a výběr na první objekt na návrhové ploše na stejné úrovni druhé strany.                                                                                                                                         |
| **ukončení**                                                                                 | Poslední Peer                   | Přesune fokus a výběr na poslední objekt na návrhové ploše na stejné úrovni druhé strany.                                                                                                                                          |
| **Ctrl + Home**                                                                         | První Peer (Přepne fokus)          | Stejně jako první sdílené, ale aktivuje místo přesunete fokus a výběr.                                                                                                                                                          |
| **CTRL + End**                                                                          | Poslední Peer (Přepne fokus)           | Stejné jako poslední vytvořit partnerský vztah, ale přesune fokus místo přesunete fokus a výběr.                                                                                                                                                           |
| **Karta**                                                                                 | Další Peer                   | Přesune fokus a výběr další objekt na návrhové ploše na stejné úrovni druhé strany.                                                                                                                                          |
| **Shift + Tab**                                                                           | Předchozí Peer               | Přesune fokus a výběr na předchozí objekt na návrhové ploše na stejné úrovni druhé strany.                                                                                                                                      |
| **Ctrl + Alt + Tab**                                                                        | Další partnerské (Přepne fokus)           | Stejně jako další vytvoření partnerského vztahu, ale přesune fokus místo přesunete fokus a výběr.                                                                                                                                                           |
| **Alt + Ctrl + Shift + Tab**                                                                  | Předchozí Peer (Přepne fokus)       | Stejné jako předchozí sdílené, ale aktivuje místo přesunete fokus a výběr.                                                                                                                                                       |
| **&lt;**                                                                                | Ascend                      | Přejde na další objekt na návrhové plochy jeden úroveň výše v hierarchii. Pokud nejsou žádné prvky nad tohoto obrazce v hierarchii (to znamená, že objekt je umístit přímo na návrhové ploše), je vybrán diagramu. |
| **&gt;**                                                                                | Sestup                     | Přejde na další obsaženého objektu na návrhové plochy jednu úroveň pod tohoto objektu v hierarchii. Pokud neexistují žádné obsaženého objektu, jde no-op.                                                                              |
| **CTRL + &lt;**                                                                         | Ascend (Přepne fokus)              | Stejné jako ascend příkazu, ale aktivuje bez výběru.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Sestup (Přepne fokus)             | Stejné jako sestup příkazu, ale aktivuje bez výběru.                                                                                                                                                                         |
| **Shift + End**                                                                         | Použijte k připojení         | Z entity přesune na entitu, která tuto entitu je připojena k.                                                                                                                                                               |
| **Del**                                                                                 | Odstranit                      | Odstranění objektu nebo konektor z diagramu.                                                                                                                                                                                     |
| **Plug-in**                                                                                 | Insert                      | Přidá novou vlastnost s entitou, když se vybere hlavičku oddílu "Skalární vlastnosti" nebo samotné vlastnosti.                                                                                                           |
| **PG nahoru**                                                                               | Diagram posunout nahoru           | Posune návrhové ploše, v přírůstcích po rovna 75 % výšky aktuálně viditelné návrhové plochy.                                                                                                                    |
| **PG dolů**                                                                             | Diagram posunout dolů         | Posouvání návrhové plochy.                                                                                                                                                                                                    |
| **SHIFT + PAGE dolů**                                                                     | Diagram posunout doprava        | Posune návrhovou plochu na pravé straně.                                                                                                                                                                                            |
| **SHIFT + Page Up**                                                                       | Diagram posunout doleva         | Posune návrhovou plochu na levé straně.                                                                                                                                                                                             |
| **F2**                                                                                  | Vstoupit do režimu úprav             | Standardní klávesové zkratky pro přechod do režimu úprav pro ovládací prvek text.                                                                                                                                                               |
| **SHIFT + F10**                                                                         | Zobrazení místní nabídky.       | Standardní klávesové zkratky pro zobrazení vybrané položky místní nabídky.                                                                                                                                                          |
| **Ctrl + Shift + myš levé tlačítko myši**  <br/> **Ctrl + Shift + MouseWheel vpřed**  | Sémantické přiblížení v            | Zvětší zobrazení oblasti zobrazení diagramu pod ukazatelem myši.                                                                                                                                                                 |
| **Ctrl + Shift + myš, klikněte pravým tlačítkem na** <br/> **Ctrl + Shift + MouseWheel zpětně** | Sémantické přiblížení navýšení kapacity           | Změní měřítko z oblasti zobrazení diagramu pod ukazatelem myši. Znovu centra diagramu, když jste oddálit příliš daleko pro aktuální diagram center.                                                                          |
| **Ctrl + Shift + '+'** <br/> **CTRL + MouseWheel vpřed**                        | Přiblížit                     | Přiblíží středu zobrazení diagramu.                                                                                                                                                                                         |
| **Ctrl + Shift + "-"** <br/> **CTRL + MouseWheel zpětně**                       | Horizonální oddálení                    | Oddálí od kliknutí na oblast zobrazení diagramu. Znovu centra diagramu, když jste oddálit příliš daleko pro aktuální diagram center.                                                                                            |
| **Ctrl + Shift + vykreslení obdélníku s levým tlačítkem myši**                  | Zvětšení oblasti                   | Přiblíží, umístěné uprostřed oblasti, kterou jste vybrali. Při podržení klávesy Shift + ovládacího prvku uvidíte, že se ukazatel změní na lupy, které vám umožní definovat oblasti pro zvětšení.                        |
| **Kontext nabídka Key + jsem.**                                                              | Otevřete okno Podrobnosti o mapování | Otevře se okno Podrobnosti o mapování mapování pro vybranou entitu upravit                                                                                                                                                               |

## <a name="mapping-details-window"></a>Okno Podrobnosti o mapování

![Mapování podrobně popisuje klávesové zkratky](~/ef6/media/mappingdetailsshortcuts.png)

| Zástupce                  | Akce         | Poznámky                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Karta**                   | Přepněte kontext | Přepne mezi oblasti hlavního okna a v levém panelu nástrojů                                                                     |
| **Klávesy se šipkami**            | Navigace     | Přesunout nahoru a dolů řádky, nebo doprava a doleva ve sloupcích v oblasti hlavního okna. Přesunutí mezi tlačítky na panelu nástrojů na levé straně. |
| **Zadejte** <br/> **místo** | Vyberte         | Vybere tlačítko na panelu nástrojů na levé straně.                                                                                          |
| **ALT + Šipka dolů**      | Otevřít seznam      | Pokud je zaškrtnuto buňku, která má rozevíracího seznamu rozevírací seznam.                                                                     |
| **Zadejte**                 | Seznam Select    | Vybere element v rozevíracím seznamu.                                                                                               |
| **ESC**                   | Zavření seznamu     | Zavře rozevíracího seznamu.                                                                                                              |

## <a name="visual-studio-navigation"></a>Navigace v sadě Visual Studio

Entity Framework také poskytuje řadu akcí, které může mít vlastní klávesové zkratky namapována (ve výchozím nastavení jsou namapované žádné klávesové zkratky). Pokud chcete vytvořit tyto vlastní klávesové zkratky, klikněte na nabídku Nástroje a možnosti.  V rámci prostředí zvolte klávesnice.  Přejděte dolů v seznamu střední až můžete vybrat požadovaný příkaz, zadejte klávesovou zkratku v textovém poli "Klávesovou zkratku pro" a klikněte na přiřazení. Je to možné zkratky jsou určeny následujícím způsobem:

| Zástupce                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
