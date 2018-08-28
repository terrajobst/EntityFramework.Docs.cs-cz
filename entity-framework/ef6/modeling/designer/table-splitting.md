---
title: Tabulka návrháře rozdělení - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 87b6e1bd0374f77dfffab342c659cf4e16c8a337
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994500"
---
# <a name="designer-table-splitting"></a>Tabulka návrháře rozdělení
Tento návod ukazuje, jak mapovat více typů entit na jedné tabulky tak, že upravíte model se Návrhář Entity Framework (EF designeru).

Jeden z důvodů, proč můžete chtít použít tabulku rozdělení je zpoždění načítání některé vlastnosti, při použití opožděné načtení pro načtení objektů. Vlastnosti, které mohou obsahovat velmi velké množství dat do samostatné entity a načíst jenom ji v případě potřeby můžete oddělit.

Následující obrázek znázorňuje hlavní windows, které se používají při práci s EF designeru.

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Požadavky

K dokončení toho návodu budete potřebovat:

- Nejnovější verzi sady Visual Studio.
- [Ukázkové databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

Tento návod používá Visual Studio 2012.

-   Otevřít Visual Studio 2012.
-   Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**.
-   V levém podokně klikněte na tlačítko Visual C\#a pak vyberte šablonu Konzolová aplikace.
-   Zadejte **TableSplittingSample** jako název projektu a klikněte na tlačítko **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Vytvořit Model založený na databáze školy

-   Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**.
-   Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.
-   Zadejte **TableSplittingModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.
-   V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další.**
-   Klikněte na tlačítko nové připojení. V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.
    Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.
-   V dialogovém okně Zvolte vaše databázové objekty Rozbalit **tabulky** uzlu a kontrolu **osoba** tabulky. Tím se přidá do zadané tabulky **School** modelu.
-   Klikněte na tlačítko **Dokončit**.

V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí. Všechny objekty, které jste vybrali v **zvolte vaše databázové objekty** dialogové okno se přidají do modelu.

## <a name="map-two-entities-to-a-single-table"></a>Dvě entity, které mapují na jedné tabulky

V této části se rozdělí **osoba** entity do dvou entit a jejich namapování na jednu tabulku.

> [!NOTE]
> **Osoba** entita neobsahuje žádné vlastnosti, které mohou obsahovat velké množství dat; používá se jenom jako příklad.

-   Klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, přejděte na **přidat nový**a klikněte na tlačítko **Entity**.
    **Novou entitu** zobrazí se dialogové okno.
-   Typ **HireInfo** pro **název Entity** a **PersonID** pro **vlastnost Key** název.
-   Klikněte na tlačítko **OK**.
-   Nový typ entity se vytvoří a zobrazí na návrhové ploše.
-   Vyberte **HireDate** vlastnost **osoba** typu entity a stiskněte klávesu **Ctrl + X** klíče.
-   Vyberte **HireInfo** entity a stiskněte klávesu **Ctrl + V** klíče.
-   Vytvoření přidružení mezi **osoba** a **HireInfo**. Chcete-li to provést, klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, přejděte na **přidat nový**a klikněte na tlačítko **přidružení**.
-   **Přidat přidružení** zobrazí se dialogové okno. **PersonHireInfo** ve výchozím nastavení je zadaný název.
-   Zadejte násobnost **1(One)** na obou stranách relace.
-   Stisknutím klávesy **OK**.

Vyžaduje další krok **podrobnosti mapování** okna. Pokud toto okno nelze zobrazit, klikněte pravým tlačítkem na návrhové ploše a vyberte **podrobnosti mapování**.

-   Vyberte **HireInfo** typu entity a klikněte na tlačítko **&lt;přidat tabulku nebo zobrazení&gt;** v **podrobnosti mapování** okna.
-   Vyberte **osoba** z **&lt;přidat tabulku nebo zobrazení&gt;** pole rozevíracího seznamu. Seznam obsahuje tabulky nebo zobrazení pro které je možné mapovat vybrané entity.
    Příslušné vlastnosti by měly být namapované ve výchozím nastavení.

    ![Mapování](~/ef6/media/mapping.png)

-   Vyberte **PersonHireInfo** přidružení na návrhové ploše.
-   Klikněte pravým tlačítkem na přidružení na návrhové ploše a vyberte **vlastnosti**.
-   V **vlastnosti** okna, vyberte **referenční omezení** vlastnosti a klikněte na tlačítko se třemi tečkami.
-   Vyberte **osoba** z **hlavní** rozevíracího seznamu.
-   Stisknutím klávesy **OK**.

 

## <a name="use-the-model"></a>Použití modelu

-   Vložte následující kód do metody Main.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Kompilace a spuštění aplikace.

Následující příkazy T-SQL, které byly spuštěny před **School** databáze jako výsledek spuštění této aplikace. 

-   Následující **vložit** se spustil v důsledku spuštění kontextu. SaveChanges() a kombinuje data z **osoba** a **HireInfo** entity

    ![Insert](~/ef6/media/insert.png)

-   Následující **vyberte** se spustil v důsledku spuštění kontextu. People.FirstOrDefault() a vybere pouze sloupce mapovat na **osoby**

    ![Select1](~/ef6/media/select1.png)

-   Následující **vyberte** byl proveden v důsledku přístup k existingPerson.Instructor vlastnost navigace a vybere pouze sloupce, které jsou namapované na **HireInfo**

    ![Select2](~/ef6/media/select2.png)
