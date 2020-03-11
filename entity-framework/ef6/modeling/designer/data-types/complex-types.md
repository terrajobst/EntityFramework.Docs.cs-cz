---
title: Komplexní typy – Návrhář EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418649"
---
# <a name="complex-types---ef-designer"></a>Komplexní typy – Návrhář EF
Toto téma ukazuje, jak namapovat komplexní typy pomocí Entity Framework Designer (EF Designer) a jak se dotazovat na entity, které obsahují vlastnosti komplexního typu.

Následující obrázek ukazuje hlavní okna, která se používají při práci s návrhářem EF.

![Návrhář EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> Při sestavování koncepčního modelu se může zobrazit upozornění týkající se nemapovaných entit a přidružení v Seznam chyb. Tato upozornění můžete ignorovat, protože po výběru databáze z modelu budou tyto chyby pryč.

## <a name="what-is-a-complex-type"></a>Co je komplexní typ

Komplexní typy jsou jiné než skalární vlastnosti typů entit, které umožňují organizovat skalární vlastnosti v rámci entit. Podobně jako entity, komplexní typy sestávají z skalárních vlastností nebo jiných vlastností komplexního typu.

Při práci s objekty, které reprezentují komplexní typy, mějte na paměti následující:

-   Komplexní typy nemají klíče, a proto nemohou existovat nezávisle. Komplexní typy mohou existovat pouze jako vlastnosti typů entit nebo jiných komplexních typů.
-   Komplexní typy se nemůžou účastnit přidružení a nemůžou obsahovat navigační vlastnosti.
-   Vlastnosti komplexního typu nemohou mít **hodnotu null**. K hodnotě **InvalidOperationException **dojde, pokud je volána metoda **DbContext. SaveChanges** a byl zjištěn složitý objekt s hodnotou null. Skalární vlastnosti komplexních objektů mohou mít **hodnotu null**.
-   Komplexní typy nemůžou dědit z jiných komplexních typů.
-   Komplexní typ musíte definovat jako **třídu**. 
-   EF detekuje změny členů v objektu komplexního typu, když je volána metoda **DbContext. DetectChanges** . Entity Framework volá automaticky **DetectChanges** při volání následujících členů: **negenerickými. Find**, **negenerickými. Local**, **negenerickými. Remove**, **negenerickými. Add**, **negenerickými. Attach**, **DbContext. SaveChanges**, **DbContext. GetValidationErrors**, **DbContext. entry**, **DbChangeTracker. Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Refaktorování vlastností entity do nového komplexního typu

Pokud již máte entitu ve koncepčním modelu, můžete chtít Refaktorovat některé vlastnosti do vlastnosti komplexního typu.

Na návrhové ploše vyberte jednu nebo více vlastností (kromě vlastností navigace) entity, klikněte pravým tlačítkem myši a vyberte **refaktoring –&gt; přesunout na nový komplexní typ**.

![Refaktorovat](~/ef6/media/refactor.png)

Do **prohlížeče modelu**se přidá nový komplexní typ s vybranými vlastnostmi. Komplexnímu typu je udělen výchozí název.

Komplexní vlastnost nově vytvořeného typu nahradí vybrané vlastnosti. Všechna mapování vlastností jsou zachována.

![Refaktorovat 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Vytvořit nový komplexní typ

Můžete také vytvořit nový komplexní typ, který neobsahuje vlastnosti existující entity.

Klikněte pravým tlačítkem na složku **komplexních typů** v prohlížeči modelů, přejděte na **komplexní typ AddNew...** . Případně můžete vybrat složku **komplexních typů** a stisknout klávesu **INSERT** na klávesnici.

![Přidat nový komplexní typ](~/ef6/media/addnewcomplextype.png)

Do složky s výchozím názvem se přidá nový komplexní typ. Nyní můžete do typu přidat vlastnosti.

## <a name="add-properties-to-a-complex-type"></a>Přidání vlastností do komplexního typu

Vlastnosti komplexního typu mohou být skalární typy nebo existující komplexní typy. Vlastnosti komplexního typu ale nemůžou mít cyklické odkazy. Například komplexní typ **OnsiteCourseDetails** nemůže mít vlastnost komplexního typu **OnsiteCourseDetails**.

Vlastnost můžete do komplexního typu přidat libovolným způsobem uvedeným níže.

-   V prohlížeči modelů klikněte pravým tlačítkem myši na komplexní typ, přejděte na **Přidat**a pak na **skalární vlastnost** nebo **komplexní vlastnost**a pak vyberte požadovaný typ vlastnosti. Případně můžete vybrat složitý typ a pak na klávesnici stisknout klávesu **Insert** .  

    ![Přidat vlastnosti do komplexního typu](~/ef6/media/addpropertiestocomplextype.png)

    Do komplexního typu se přidá nová vlastnost s výchozím názvem.

- Ani

-   Klikněte pravým tlačítkem na plochu **Návrháře EF** a vyberte **Kopírovat**, klikněte pravým tlačítkem na komplexní typ v **prohlížeči modelů** a vyberte **Vložit**.

## <a name="rename-a-complex-type"></a>Přejmenování komplexního typu

Při přejmenování komplexního typu jsou všechny odkazy na typ aktualizovány v celém projektu.

-   Pomalu dvakrát klikněte na komplexní typ v **prohlížeči modelů**.
    Název bude vybrán a v režimu úprav.

- Ani

-   V **prohlížeči modelů** klikněte pravým tlačítkem na komplexní typ a vyberte **Přejmenovat**.

- Ani

-   V prohlížeči modelů vyberte složitý typ a stiskněte klávesu F2.

- Ani

-   V **prohlížeči modelů** klikněte pravým tlačítkem na komplexní typ a vyberte **vlastnosti**. Upravte název v okně **vlastnosti** .

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Přidání existujícího komplexního typu k entitě a mapování vlastností na sloupce tabulky

1.  Klikněte pravým tlačítkem na entitu, přejděte na **Přidat nový**a vyberte **složitá vlastnost**.
    Do entity se přidá vlastnost komplexního typu s výchozím názvem. K vlastnosti je přiřazen výchozí typ (zvoleno z existujících komplexních typů).
2.  Přiřaďte požadovaný typ k vlastnosti v okně **vlastnosti** .
    Po přidání vlastnosti komplexního typu do entity je nutné namapovat její vlastnosti na sloupce tabulky.
3.  Pravým tlačítkem myši klikněte na typ entity na návrhové ploše nebo v **prohlížeči modelů** a vyberte **mapování tabulek**.
    Mapování tabulek se zobrazí v okně **Podrobnosti mapování** .
4.  Rozbalte uzel **mapy pro &lt;název tabulky&gt;**  Node.
    Zobrazí se uzel  **mapování sloupců** .
5.  Rozbalte uzel **mapování sloupců** .
    Zobrazí se seznam všech sloupců v tabulce. Výchozí vlastnosti (pokud existují), na které jsou sloupce namapovány pod položkou **hodnota nebo vlastnost** záhlaví.
6.  Vyberte sloupec, který chcete namapovat, a potom klikněte pravým tlačítkem na odpovídající **hodnotu nebo vlastnost** pole.
    Zobrazí se rozevírací seznam všech skalárních vlastností.
7.  Vyberte odpovídající vlastnost.

    ![Mapování komplexního typu](~/ef6/media/mapcomplextype.png)

8.  Opakujte kroky 6 a 7 pro každý sloupec tabulky.

>[!NOTE]
> Chcete-li odstranit mapování sloupce, vyberte sloupec, který chcete namapovat, a poté klikněte na pole **hodnota nebo vlastnost** . Pak v rozevíracím seznamu vyberte **Odstranit** .

## <a name="map-a-function-import-to-a-complex-type"></a>Mapování importu funkce na komplexní typ

Importy funkcí jsou založeny na uložených procedurách. Chcete-li namapovat import funkce na komplexní typ, sloupce vrácené odpovídajícím uloženým postupem musí odpovídat vlastností komplexního typu v Number a musí mít typy úložiště, které jsou kompatibilní s typy vlastností.

-   Dvakrát klikněte na importovanou funkci, kterou chcete namapovat na komplexní typ.

    ![Importy funkcí](~/ef6/media/functionimports.png)

-   Zadejte nastavení nového importu funkce následujícím způsobem:
    -   Zadejte uloženou proceduru, pro kterou vytváříte import funkce v poli **název uložené procedury** . Toto pole obsahuje rozevírací seznam, který zobrazuje všechny uložené procedury v modelu úložiště.
    -   Zadejte název importované funkce v poli  **importovat název funkce** .
    -   Jako návratový typ vyberte **složitý** a pak v rozevíracím seznamu zvolte vhodný typ, a pak zadejte konkrétní komplexní návratový typ.

        ![Upravit import funkce](~/ef6/media/editfunctionimport.png)

-   Klikněte na tlačítko **OK**.
    V koncepčním modelu se vytvoří položka importování funkce.

### <a name="customize-column-mapping-for-function-import"></a>Přizpůsobení mapování sloupce pro import funkce

-   V prohlížeči modelů klikněte pravým tlačítkem na import funkce a vyberte **mapování importu funkcí**.
    Zobrazí se okno **Podrobnosti mapování** a zobrazí se výchozí mapování pro import funkce. Šipky označují mapování mezi hodnotami sloupce a hodnotami vlastností. Ve výchozím nastavení se názvy sloupců považují za stejné jako názvy vlastností komplexního typu. Výchozí názvy sloupců se zobrazí v šedém textu.
-   V případě potřeby změňte názvy sloupců tak, aby odpovídaly názvům sloupců, které jsou vráceny uloženou procedurou, která odpovídá importu funkce.

## <a name="delete-a-complex-type"></a>Odstranění komplexního typu

Při odstranění komplexního typu se odstraní typ z konceptuálního modelu a mapování pro všechny instance typu budou odstraněna. Odkazy na tento typ však nejsou aktualizovány. Například Pokud entita má vlastnost komplexního typu typu ComplexType1 a ComplexType1 je v **prohlížeči modelů**odstraněna, není aktualizována odpovídající vlastnost entity. Model nebude ověřen, protože obsahuje entitu, která odkazuje na odstraněný komplexní typ. Odkazy na odstraněné komplexní typy můžete aktualizovat nebo odstranit pomocí Entity Designer.

-   V prohlížeči modelů klikněte pravým tlačítkem na komplexní typ a vyberte **Odstranit**.

- Ani

-   V prohlížeči modelů vyberte složitý typ a stiskněte klávesu DELETE na klávesnici.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Dotaz na entity obsahující vlastnosti komplexního typu

Následující kód ukazuje, jak spustit dotaz, který vrací kolekci objektů typu entity, které obsahují vlastnost komplexního typu.

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
