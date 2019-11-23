---
title: Rozdělení tabulky návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921780"
---
# <a name="designer-table-splitting"></a>Rozdělení tabulky návrháře
Tento návod ukazuje, jak namapovat více typů entit na jednu tabulku úpravou modelu pomocí Entity Framework Designer (EF Designer).

Jedním z důvodů, proč můžete chtít použít rozdělování tabulky, je zpoždění načítání některých vlastností při použití opožděného načítání pro načtení vašich objektů. Vlastnosti, které mohou obsahovat velmi velké množství dat, můžete oddělit do samostatné entity a v případě potřeby je načíst.

Následující obrázek ukazuje hlavní okna, která se používají při práci s návrhářem EF.

![Návrhář EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Požadavky

K dokončení toho návodu budete potřebovat:

- Poslední verze sady Visual Studio.
- [Ukázková databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

Tento návod používá Visual Studio 2012.

-   Otevřete Visual Studio 2012.
-   V nabídce **soubor** přejděte na příkaz **Nový**a klikněte na **projekt**.
-   V levém podokně klikněte na položku Visual C\#a pak vyberte šablonu Konzolová aplikace.
-   Jako název projektu zadejte **TableSplittingSample** a klikněte na **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Vytvoření modelu založeného na školní databázi

-   Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka**.
-   V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
-   Jako název souboru zadejte **TableSplittingModel. edmx** a pak klikněte na **Přidat**.
-   V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další.**
-   Klikněte na nové připojení. V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.
    Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.
-   V dialogovém okně zvolit objekty databáze rozložte **tabulky** uzel a ověřte tabulku **Person** . Tím se přidá zadaná tabulka do **školního** modelu.
-   Klikněte na tlačítko **Dokončit**.

Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu. Do modelu jsou přidány všechny objekty, které jste vybrali v dialogovém okně **zvolit objekty databáze** .

## <a name="map-two-entities-to-a-single-table"></a>Mapování dvou entit na jednu tabulku

V této části budete entitu **osoba** rozdělit do dvou entit a pak je namapovat na jednu tabulku.

> [!NOTE]
> Entita **osoby** neobsahuje žádné vlastnosti, které by mohly obsahovat velké množství dat. slouží pouze jako příklad.

-   Klikněte pravým tlačítkem myši na prázdnou oblast návrhové plochy, přejděte na **Přidat nový**a klikněte na **entita**.
    Zobrazí se dialogové okno **nová entity** .
-   Jako **název entity** zadejte **HireInfo** a pro název **Klíčové vlastnosti** **PersonID** .
-   Klikněte na tlačítko **OK**.
-   Vytvoří se nový typ entity, který se zobrazí na návrhové ploše.
-   Vyberte vlastnost **zaměstnánod**  **osoby** typu entity a stiskněte klávesy **CTRL + X** .
-   Vyberte entitu **HireInfo** a stiskněte klávesy **CTRL + V** .
-   Vytvořte přidružení mezi **osobami** a **HireInfo**. Provedete to tak, že kliknete pravým tlačítkem myši na prázdnou oblast návrhové plochy, najeďte na **Přidat nový**a kliknete na **asociace**.
-   Zobrazí se dialogové okno **přidat přidružení** . Ve výchozím nastavení je zadán název **PersonHireInfo** .
-   Zadejte násobnost **1 (jedna)** na obou koncích relace.
-   Stisknutím klávesy **OK**.

Další krok vyžaduje okno s **podrobnostmi mapování** . Pokud toto okno nevidíte, klikněte pravým tlačítkem myši na návrhovou plochu a vyberte **mapování podrobností**.

-   Vyberte typ entity **HireInfo** a v okně **Podrobnosti mapování** klikněte **&lt;přidat tabulku nebo zobrazení&gt;**  .
-   V rozevíracím seznamu **&lt;přidat tabulku nebo zobrazení&gt;**  pole vyberte **osoba** . Seznam obsahuje tabulky nebo zobrazení, do kterých lze mapovat vybranou entitu.
    Ve výchozím nastavení by měly být namapované odpovídající vlastnosti.

    ![Mapování](~/ef6/media/mapping.png)

-   Vyberte přidružení **PersonHireInfo** na návrhové ploše.
-   Na návrhové ploše klikněte pravým tlačítkem na přidružení a vyberte **vlastnosti**.
-   V okně **vlastnosti** vyberte vlastnost **referenčních omezení** a klikněte na tlačítko se třemi tečkami.
-   V rozevíracím seznamu **hlavní objekty** vyberte **osoba** .
-   Stisknutím klávesy **OK**.

 

## <a name="use-the-model"></a>Použití modelu

-   Do metody Main vložte následující kód.

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
-   Zkompilujte a spusťte aplikaci.

V důsledku spuštění této aplikace byly v databázi **School** provedeny následující příkazy jazyka T-SQL. 

-   Následující **vložení** bylo provedeno jako výsledek spuštěného kontextu. SaveChanges () a kombinuje data z entit **Person** a **HireInfo**

    ![Vložit](~/ef6/media/insert.png)

-   Následující **příkaz SELECT** byl proveden jako výsledek spuštěného kontextu. Lidi. FirstOrDefault () a vybere pouze sloupce namapované na **osobu**

    ![Vyberte 1](~/ef6/media/select1.png)

-   Následující **příkaz SELECT** byl proveden jako výsledek přístupu k navigační vlastnosti ExistingPerson. instruktor a vybere pouze sloupce namapované na **HireInfo** .

    ![Vybrat 2](~/ef6/media/select2.png)
