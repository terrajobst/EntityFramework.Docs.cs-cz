---
title: Komplexní typy – EF designeru - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
caps.latest.revision: 3
ms.openlocfilehash: 6d0744921085afdafa35d137d276c54738cdd465
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912694"
---
# <a name="complex-types---ef-designer"></a>Komplexní typy – EF designeru
Toto téma ukazuje, jak namapovat komplexní typy se Návrhář Entity Framework (EF designeru) a zadávat dotazy na entity, které obsahují vlastností komplexního typu.

Následující obrázek znázorňuje hlavní windows, které se používají při práci s EF designeru.

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> Při vytváření konceptuální model, upozornění na nenamapované entit a přidružení může zobrazit v seznamu chyb. Tato upozornění můžete ignorovat, protože až zvolíte, aby se vygenerovala databáze z modelu, chyby zmizí.

## <a name="what-is-a-complex-type"></a>Co je komplexní typ

Komplexní typy jsou vlastnosti neskalární typy entit, které umožňují Skalární vlastnosti, které chcete být uspořádány v rámci entit. Stejně jako entity komplexní typy skládat z Skalární vlastnosti nebo dalších vlastností komplexního typu.

Při práci s objekty, které představují komplexní typy, mějte na paměti toto:

-   Komplexní typy nemají klíče a proto nemůže existovat nezávisle na sobě. Komplexní typy může existovat pouze jako typy entit nebo jiné komplexní typy vlastností.
-   Komplexní typy nemůže být součástí přidružení a nesmí obsahovat navigační vlastnosti.
-   Komplexní typ vlastnosti nemohou být **null**. ** InvalidOperationException ** dochází při **DbContext.SaveChanges** se nazývá a komplexní objekt s hodnotou null. Může být Skalární vlastnosti komplexních objektů **null**.
-   Komplexní typy nemůžou dědit z jiné komplexní typy.
-   Je nutné definovat jako komplexní typ **třídy**. 
-   EF zjistí změny členů na komplexní typ objektu při **DbContext.DetectChanges** je volána. Zavolá rozhraní Entity Framework **metoda DetectChanges** automaticky kdy jsou volány následující členy: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Refaktorovat vlastnosti Entity novou komplexní typ

Pokud už máte v konceptuálním modelu entity můžete chtít Refaktorovat některých vlastností do vlastností komplexního typu.

Na návrhové ploše, vyberte jednu nebo více vlastností (s výjimkou navigačních vlastností) entity, klikněte pravým tlačítkem a vyberte **Refaktorovat -&gt; přesunout na nový komplexní typ**.

![Refaktorovat](~/ef6/media/refactor.png)

Nový komplexní typ s vybranou vlastností se přidá do **prohlížeč modelu**. Komplexní typ je přiřazen výchozí název.

Komplexní vlastnosti nově vytvořeného typu nahradí vybraných vlastností. Jsou zachovány všechny mapování vlastností.

![Refactor2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Vytvořte nový komplexní typ

Můžete také vytvořit nový komplexní typ, který obsahuje vlastnosti existující entity.

Klikněte pravým tlačítkem na **komplexní typy** složky přejděte v prohlížeči modelů **komplexní typ funkce operací AddNew...** . Alternativně můžete vybrat **komplexní typy** složky a stiskněte klávesu **vložit** kláves na klávesnici.

![AddNewComplextype](~/ef6/media/addnewcomplextype.png)

Nový komplexní typ se přidá do složky s výchozím názvem. Nyní můžete přidat vlastnosti typu.

## <a name="add-properties-to-a-complex-type"></a>Přidání vlastností komplexního typu.

Vlastnosti komplexní typ může být Skalární typy nebo existující komplexní typy. Komplexní typ vlastnosti však nesmí obsahovat Kruhové odkazy. Například komplexní typ **OnsiteCourseDetails** nemůže mít vlastnost komplexního typu **OnsiteCourseDetails**.

Přidat vlastnost na komplexní typ v některém z níže uvedených způsobů.

-   Klikněte pravým tlačítkem na komplexní typ v modelu prohlížeč, přejděte na **přidat**, přejděte na **skalární vlastnost** nebo **komplexní vlastnost**, pak vyberte typ požadované vlastnosti. Alternativně můžete vybrat komplexní typ a potom stiskněte klávesu **vložit** kláves na klávesnici.  

    ![AddPropertiestoComplexType](~/ef6/media/addpropertiestocomplextype.png)

    Komplexní typ s výchozím názvem je přidána nová vlastnost.

- OR –

-   Klikněte pravým tlačítkem na vlastnost entity na **EF designeru** ploše a vyberte možnost **kopírování**, klikněte pravým tlačítkem na komplexní typ, **prohlížeč modelu** a vyberte  **Vložit**.

## <a name="rename-a-complex-type"></a>Přejmenovat komplexního typu.

Při přejmenování komplexní typ, se aktualizují všechny odkazy na typ v celém projektu.

-   Pomalu klikněte dvakrát na komplexní typ **prohlížeč modelu**.
    Název bude vybrán a v režimu úprav.

- OR –

-   Klikněte pravým tlačítkem na komplexní typ **prohlížeč modelu** a vyberte **přejmenovat**.

- OR –

-   Vyberte komplexní typ v prohlížeči modelů a stisknutím klávesy F2.

- OR –

-   Klikněte pravým tlačítkem na komplexní typ **prohlížeč modelu** a vyberte **vlastnosti**. Upravit název v **vlastnosti** okna.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Přidat existující komplexního typu Entity a namapujte jeho vlastnosti na sloupce tabulky

1.  Klikněte pravým tlačítkem na entitu, přejdete na **přidat nový**a vyberte **komplexní vlastnost**.
    Komplexní vlastnost s výchozím názvem se přidá k entitě. Výchozí typ (zvolit existující komplexní typy) je přiřazen k vlastnosti.
2.  Přiřaďte požadovaného typu vlastnosti v **vlastnosti** okna.
    Po přidání vlastnosti komplexní typ entity, je nutné mapovat na sloupce tabulky její vlastnosti.
3.  Klikněte pravým tlačítkem na typ entity na návrhové ploše nebo na **prohlížeč modelu** a vyberte **mapování tabulky**.
    Mapování tabulky se zobrazí v **podrobnosti mapování** okna.
4.  Rozbalte **mapuje &lt;název tabulky&gt;**  uzlu.
    A **mapování sloupců** uzel se objeví.
5.  Rozbalte **mapování sloupců** uzlu.
    Zobrazí se seznam všech sloupců v tabulce. Výchozí vlastnosti (pokud existuje) který mapovat sloupce, které jsou uvedeny v části **/vlastnost Value** záhlaví.
6.  Vyberte sloupec, který chcete propojit a klikněte pravým tlačítkem na příslušné **/vlastnost Value** pole.
    Zobrazí se rozevírací seznam Skalární vlastnosti.
7.  Vyberte příslušnou vlastnost.

    ![MapComplexType](~/ef6/media/mapcomplextype.png)

8.  Opakujte kroky 6 a 7 pro každý sloupec tabulky.

>[!NOTE]
> Pokud chcete odstranit mapování sloupce, vyberte sloupec, který chcete namapovat a potom klikněte na tlačítko **/vlastnost Value** pole. Vyberte **odstranit** z rozevíracího seznamu.

## <a name="map-a-function-import-to-a-complex-type"></a>Mapování importované funkce komplexního typu.

Funkce jsou založeny na uložené procedury. Mapování importované funkce na komplexní typ, sloupců vrácený odpovídající uložené procedury musí odpovídat vlastnosti komplexní typ, číslo a musí mít typy úložišť, které jsou kompatibilní s typy vlastností.

-   Dvakrát klikněte na importované funkce, která chcete namapovat komplexního typu.

    ![FunctionImports](~/ef6/media/functionimports.png)

-   Zadejte nastavení pro nové importované funkce takto:
    -   Zadejte uloženou proceduru, pro kterou vytváříte importované funkce v **název uložené procedury** pole. Toto pole je rozevírací seznam, který zobrazuje všechny uložené procedury v modelu úložiště.
    -   Zadejte název v importované funkce **importovat název funkce** pole.
    -   Vyberte **komplexní** jako návratový typ a pak zadejte konkrétní komplexní návratový typ zvolením příslušného typu z rozevíracího seznamu.

        ![EditFunctionImport](~/ef6/media/editfunctionimport.png)

-   Klikněte na tlačítko **OK**.
    Položka import funkce je vytvořena v konceptuálním modelu.

### <a name="customize-column-mapping-for-function-import"></a>Mapování importované funkce sloupec

-   Importovaná funkce v prohlížeči modelů klikněte pravým tlačítkem a vyberte **Import mapování funkcí**.
    **Podrobnosti mapování** okna se zobrazí a zobrazí výchozí mapování importované funkce. Šipky označují mapování mezi hodnot sloupců a hodnot vlastností. Ve výchozím nastavení názvy sloupců jsou předpokládá se, že stejné jako názvy vlastností komplexního typu. Výchozí názvy sloupců se zobrazí v šedé textu.
-   V případě potřeby změňte názvy sloupců tak, aby odpovídaly názvy sloupců, které jsou vráceny pomocí uložené procedury, která odpovídá na importované funkce.

## <a name="delete-a-complex-type"></a>Odstranit komplexního typu.

Když odstraníte komplexní typ, typ je odstraněn z Koncepční model a budou odstraněny mapování pro všechny instance daného typu. Odkazy na typ se ale neaktualizují. Například, pokud má entita komplexní vlastnost typu ComplexType1 a ComplexType1 se odstraní v **prohlížeč modelu**, odpovídající vlastnost entity není aktualizován. Model nebude ověřit, protože obsahuje entitu, která odkazuje na odstraněný komplexního typu. Můžete aktualizovat nebo odstranit odkazy na odstraněný komplexních typů pomocí návrháře entit.

-   Komplexní typ v prohlížeči modelů klikněte pravým tlačítkem a vyberte **odstranit**.

- OR –

-   Vyberte komplexní typ v prohlížeči modelů a na klávesnici stiskněte klávesu Delete.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Dotazy na entity obsahující vlastnosti tohoto komplexního typu

Následující kód ukazuje, jak spustit dotaz, který vrátí kolekci entit typu objektů, které obsahují komplexní typ vlastnosti.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
