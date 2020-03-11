---
title: Klávesové zkratky Entity Framework Designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: c75eafcca0863faa1ad64202e98b61832827377c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418516"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Klávesové zkratky Entity Framework Designer
Tato stránka poskytuje seznam shorcuts klávesnice, které jsou k dispozici na různých obrazovkách Entity Framework Tools pro sadu Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Průvodce model EDM (Entity Data Model) ADO.NET

### <a name="step-one-choose-model-contents"></a>Krok 1: volba obsahu modelu

![Průvodce 1](~/ef6/media/wizardone.png)

| Zástupce  | Akce                                                     | Poznámky:                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **ALT + n** | Přejít na další obrazovku                                        | Není k dispozici pro všechny výběry obsahu modelu. |
| **ALT + f** | Průvodce dokončením                                              | Není k dispozici pro všechny výběry obsahu modelu. |
| **ALT + w** | Přepněte fokus na "co by měl model obsahovat?" oknì. |                                                     |

### <a name="step-two-choose-your-connection"></a>Krok 2: volba připojení

![Průvodce 2](~/ef6/media/wizardtwo.png)

| Zástupce  | Akce                                                     | Poznámky:                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **ALT + n** | Přejít na další obrazovku                                        |                                                         |
| **ALT + p** | Přejít na předchozí obrazovku                                    |                                                         |
| **ALT + w** | Přepněte fokus na "co by měl model obsahovat?" oknì. |                                                         |
| **ALT + c** | Otevřete okno Vlastnosti připojení.                    | Umožňuje definici nového připojení k databázi. |
| **ALT + e** | Vyloučit citlivá data z připojovacího řetězce          |                                                         |
| **ALT + i** | Zahrnout citlivá data do připojovacího řetězce            |                                                         |
| **ALT + s** | Přepnutí možnosti Uložit nastavení připojení v App. config |                                                         |

### <a name="step-three-choose-your-version"></a>Krok 3: volba vaší verze

![Průvodce třemi](~/ef6/media/wizardthree.png)

| Zástupce  | Akce                                             | Poznámky:                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **ALT + n** | Přejít na další obrazovku                                |                                                                                       |
| **ALT + p** | Přejít na předchozí obrazovku                            |                                                                                       |
| **ALT + w** | Přepnout fokus na Entity Framework výběru verze | Umožňuje zadat jinou verzi Entity Framework pro použití v projektu. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Krok 4: volba databázových objektů a nastavení

![Průvodce čtyři](~/ef6/media/wizardfour.png)

| Zástupce  | Akce                                                                                    | Poznámky:                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Průvodce dokončením                                                                             |                                                                     |
| **ALT + p** | Přejít na předchozí obrazovku                                                                   |                                                                     |
| **ALT + w** | Přepnout fokus na podokno pro výběr databázového objektu                                            | Umožňuje určit, aby byly objekty databáze zpětně analyzovány.    |
| **ALT + s** | Přepne možnost "názvy objektů generovaných doplnit jednotné nebo množné číslo".                       |                                                                     |
| **Alt + k** | Přepnutí příkazu "zahrnout sloupce cizího klíče do modelu"                              | Není k dispozici pro všechny výběry obsahu modelu.                 |
| **ALT + i** | Přepnutí možnosti "Import vybraných uložených procedur a funkcí do modelu entity" | Není k dispozici pro všechny výběry obsahu modelu.                 |
| **ALT + m** | Přepne fokus na textové pole "obor názvů modelu".                                        | Není k dispozici pro všechny výběry obsahu modelu.                 |
| **Space** | Přepnout výběr u elementu                                                               | Pokud element obsahuje podřízené položky, budou také přepnuty všechny podřízené prvky. |
| **Zbývá**  | Sbalit podřízený strom                                                                       |                                                                     |
| **Kliknutím** | Rozbalit podřízenou stromovou strukturu                                                                         |                                                                     |
| **Následn**    | Přejít na předchozí prvek ve stromové struktuře                                                      |                                                                     |
| **Dolů**  | Přejít na další prvek ve stromové struktuře                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Plocha návrháře EF

![Plocha návrháře](~/ef6/media/designersurface.png)

| Zástupce                                                                                | Akce                      | Poznámky:                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Mezera/ENTER**                                                                         | Přepnout výběr            | Přepíná výběr objektu s fokusem.                                                                                                                                                                                         |
| **Kláves**                                                                                 | Zrušit výběr            | Zruší aktuální výběr.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Vybrat vše                  | Vybere všechny tvary na návrhové ploše.                                                                                                                                                                                       |
| **Šipka nahoru**                                                                            | Přesunout nahoru                     | Přesune vybranou entitu o jeden krok nahoru. <br/> Pokud se v seznamu přesune na předchozí dílčí pole na stejné úrovni.                                                                                                                            |
| **Šipka dolů**                                                                          | Přesunout dolů                   | Přesune vybranou entitu o krok o jednu mřížku dolů. <br/> V seznamu se přesune na další dílčí pole na stejné úrovni.                                                                                                                              |
| **Šipka vlevo**                                                                          | Přesunout doleva                   | Přesune vybranou entitu o krok o jednu mřížku vlevo. <br/> Pokud se v seznamu přesune na předchozí dílčí pole na stejné úrovni.                                                                                                                          |
| **Šipka doprava**                                                                         | Přesunout doprava                  | Přesune vybranou entitu o krok vpravo o jednu mřížku. <br/> V seznamu se přesune na další dílčí pole na stejné úrovni.                                                                                                                             |
| **Shift + šipka doleva**                                                                  | Velikost obrazce vlevo             | Zmenší šířku vybrané entity o jeden přírůstek mřížky.                                                                                                                                                                     |
| **Shift + šipka doprava**                                                                 | Velikost obrazce vpravo            | Zvětší šířku vybrané entity o jeden přírůstek mřížky.                                                                                                                                                                   |
| **Domovské**                                                                                | První partnerský uzel                  | Přesune fokus a výběr na první objekt na návrhové ploše na stejné úrovni partnera.                                                                                                                                         |
| **Ukončení**                                                                                 | Poslední partnerský uzel                   | Přesune fokus a výběr na poslední objekt na návrhové ploše na stejné úrovni partnera.                                                                                                                                          |
| **Ctrl + Home**                                                                         | První partner (fokus)          | Stejné jako první partnerský uzel, ale přesouvá fokus místo přesunutí fokusu a výběru.                                                                                                                                                          |
| **CTRL + END**                                                                          | Poslední partner (fokus)           | Stejné jako poslední partner, ale přesune fokus místo přesunutí fokusu a výběru.                                                                                                                                                           |
| **Rážky**                                                                                 | Další partnerský uzel                   | Přesune fokus a výběr na další objekt na návrhové ploše na stejné úrovni partnera.                                                                                                                                          |
| **SHIFT + TAB**                                                                           | Předchozí partnerský uzel               | Přesune fokus a výběr na předchozí objekt na návrhové ploše na stejné úrovni partnera.                                                                                                                                      |
| **ALT + CTRL + TAB**                                                                        | Další partner (fokus)           | Stejné jako další partner, ale přesune fokus místo přesunutí fokusu a výběru.                                                                                                                                                           |
| **ALT + CTRL + SHIFT + TAB**                                                                  | Předchozí partner (fokus)       | Stejné jako předchozí partner, ale přesune fokus místo přesunutí fokusu a výběru.                                                                                                                                                       |
| **&lt;**                                                                                | Hrál                      | Přesune se na další objekt na návrhové ploše o jednu úroveň výš v hierarchii. Pokud nad tímto obrazcem v hierarchii neexistují žádné tvary (to znamená, že objekt je umístěn přímo na návrhové ploše), je vybrán diagram. |
| **&gt;**                                                                                | Tahem                     | Přesune se na další zahrnutý objekt na návrhové ploše jedna úroveň pod touto hierarchií v hierarchii. Pokud neexistuje žádný z obsažených objektů, jedná se o no-op.                                                                              |
| **CTRL + &lt;**                                                                         | Ascend (fokus)              | Totéž jako příkaz Ascend, ale přesune fokus bez výběru.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Spodní dotahem (fokus)             | Totéž jako příkaz v dolním tvaru, ale přesune fokus bez výběru.                                                                                                                                                                         |
| **Shift + End**                                                                         | Sledovat připojení         | Z entity se přesune k entitě, ke které je tato entita připojená.                                                                                                                                                               |
| **Del**                                                                                 | Odstranit                      | Odstraní objekt nebo spojnici z diagramu.                                                                                                                                                                                     |
| **INSERT**                                                                                 | Vložit                      | Přidá novou vlastnost k entitě, pokud je vybrána buď záhlaví "skalární vlastnosti" nebo samotná vlastnost.                                                                                                           |
| **Str nahoru**                                                                               | Posunout diagram nahoru           | Posune návrhovou plochu nahoru, v přírůstcích se rovná 75% výšky aktuálně viditelné návrhové plochy.                                                                                                                    |
| **PG dolů**                                                                             | Posunout diagram dolů         | Posune plochu návrhu dolů.                                                                                                                                                                                                    |
| **Shift + pg dolů**                                                                     | Posunout diagram doprava        | Posune návrhovou plochu doprava.                                                                                                                                                                                            |
| **Shift + pg nahoru**                                                                       | Posunout diagram doleva         | Posune návrhovou plochu doleva.                                                                                                                                                                                             |
| **F2**                                                                                  | Přejít do režimu úprav             | Standardní klávesová zkratka pro vstup do režimu úprav ovládacího prvku text                                                                                                                                                               |
| **SHIFT + F10**                                                                         | Zobrazit místní nabídku       | Standardní klávesová zkratka pro zobrazení místní nabídky vybrané položky                                                                                                                                                          |
| **CTRL + SHIFT + kliknutí levým tlačítkem myši**  <br/> **CTRL + SHIFT + MouseWheel – přední**  | Sémantické přiblížení            | Přiblíží se k oblasti zobrazení diagramu pod ukazatelem myši.                                                                                                                                                                 |
| **CTRL + SHIFT + kliknutí pravým tlačítkem myši** <br/> **CTRL + SHIFT + MouseWheel dozadu** | Sémantické zmenšení           | Přiblíží se k oblasti zobrazení diagramu pod ukazatelem myši. Diagram se znovu vycentruje, když se pro aktuální centrum diagramů zmenší příliš daleko.                                                                          |
| **Ctrl + Shift + ' + '** <br/> **CTRL + MouseWheel před**                        | Přiblížit                     | Přiblíží se ke středu zobrazení diagramu.                                                                                                                                                                                         |
| **Ctrl + Shift + '-'** <br/> **CTRL + MouseWheel dozadu**                       | Oddálit                    | Přiblíží se z oblasti kliknutí v zobrazení diagramu. Diagram se znovu vycentruje, když se pro aktuální centrum diagramů zmenší příliš daleko.                                                                                            |
| **CTRL + SHIFT + nakreslit obdélník pomocí levého tlačítka myši dolů**                  | Oblast přiblížení                   | Přiblíží se ke středu oblasti, kterou jste vybrali. Když podržíte klávesu CTRL + SHIFT, uvidíte, že se kurzor změní na lupu, což vám umožní definovat oblast pro přiblížení.                        |
| **Klíč místní nabídky + 'M**                                                              | Otevřít okno podrobností mapování | Otevře okno Podrobnosti mapování pro úpravu mapování pro vybranou entitu.                                                                                                                                                               |

## <a name="mapping-details-window"></a>Okno podrobností mapování

![Zástupci podrobností mapování](~/ef6/media/mappingdetailsshortcuts.png)

| Zástupce                  | Akce         | Poznámky:                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Rážky**                   | Přepnout kontext | Přepíná mezi hlavní oblastí okna a panelem nástrojů vlevo.                                                                     |
| **Klávesy se šipkami**            | Navigace     | Přesunout nahoru a dolů řádky nebo doprava a doleva napříč sloupci v hlavní oblasti okna. Pohyb mezi tlačítky na panelu nástrojů na levé straně |
| **Napište** <br/> **Space** | Vyberte         | Vybere tlačítko na panelu nástrojů na levé straně.                                                                                          |
| **Alt + šipka dolů**      | Otevřít seznam      | Vyřazení seznamu, pokud je vybrána buňka s rozevíracím seznamem.                                                                     |
| **Napište**                 | Vybrat seznam    | Vybere prvek v rozevíracím seznamu.                                                                                               |
| **Kláves**                   | Zavřít seznam     | Zavře rozevírací seznam.                                                                                                              |

## <a name="visual-studio-navigation"></a>Navigace v aplikaci Visual Studio

Entity Framework také poskytuje řadu akcí, které mohou mít namapované vlastní klávesové zkratky (ve výchozím nastavení nejsou namapované žádné zkratky). Chcete-li vytvořit tyto vlastní klávesové zkratky, klikněte na nabídku Nástroje a pak na možnosti.  V části prostředí vyberte klávesnice.  Posuňte se dolů v seznamu uprostřed, dokud nebudete moct vybrat požadovaný příkaz, zadejte zástupce do textového pole "stiskněte klávesovou zkratku" a klikněte na přiřadit. Možné zkratky jsou následující:

| Zástupce                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Add. ComplexProperty. ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. AddEnumType**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Association**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ComplexProperty**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ComplexType**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. entity**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. FunctionImport**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. dědičnost**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. NavigationProperty**               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Close**                                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. sbalení**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. diagram. Sbalit vše**                     |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. diagram. ExpandAll**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. diagram. ExportasImage**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. diagram. LayoutDiagram**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Edit**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. EntityKey**                               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. expand**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Grid. ShowGrid**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Grid. SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Open**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. refaktoring. MovetoNewComplexType**           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. refaktoring. refaktoring. rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. rename**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ScalarPropertyFormat. DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. BaseType**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. entity**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. Property**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. podtyp**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. metodu SelectAll lze**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Validate**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 10**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 100**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 125**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 150**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 200**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 25**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 300**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 33**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 400**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 50**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 66**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 75**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. Custom**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomIn**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. Zooming**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomtoFit**                          |
| **Zobrazit. EntityDataModelBrowser**                                                                |
| **Zobrazit. EntityDataModelMappingDetails**                                                         |
