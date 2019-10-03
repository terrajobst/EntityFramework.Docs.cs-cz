---
title: CUD uložené procedury návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813551"
---
# <a name="designer-cud-stored-procedures"></a>CUD uložené procedury návrháře

V tomto podrobném návodu se dozvíte, jak namapovat operace CREATE @ no__t-0insert, Update a Delete (CUD) typu entity na uložené procedury pomocí Entity Framework Designer (EF Designer).  Ve výchozím nastavení Entity Framework automaticky generuje příkazy SQL pro operace CUD, ale můžete také namapovat uložené procedury na tyto operace.  

Všimněte si, že Code First nepodporuje mapování na uložené procedury nebo funkce. Uložené procedury nebo funkce však můžete volat pomocí metody System. data. entity. Negenerickými. SqlQuery. Příklad:

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Předpoklady při mapování operací CUD na uložené procedury

Při mapování operací CUD na uložené procedury platí následující požadavky:

- Pokud mapujete jednu z operací CUD na uloženou proceduru, namapujte na ni všechny. Pokud nemapujete všechny tři, nemapované operace selžou, pokud se spustí, a vyvolá **UpdateException** will.
- Všechny parametry uložené procedury je nutné namapovat na vlastnosti entity.
- Pokud server vygeneruje hodnotu primárního klíče pro vložený řádek, musíte tuto hodnotu namapovat zpátky na klíčovou vlastnost entity. V následujícím příkladu vrátí procedura **InsertPerson** stored nově vytvořený primární klíč jako součást sady výsledků uložených procedur. Primární klíč je namapován na klíč entity (**PersonID**) pomocí **vazeb výsledku &lt;Add @ no__t-3** feature v Návrháři EF.
- Volání uložených procedur jsou namapována 1:1 s entitami v koncepčním modelu. Například Pokud implementujete hierarchii dědičnosti v koncepčním modelu a poté namapujete uložené procedury CUD pro **nadřazený objekt** (základní) a **podřízené** (odvozené) entity, uložení **podřízených** změn bude volat pouze **podřízený**element. s uložené procedury nebudou aktivovat volání uložených procedur **nadřazené**procedury.

## <a name="prerequisites"></a>Požadavky

K dokončení toho návodu budete potřebovat:

- Poslední verze sady Visual Studio.
- [Ukázková databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

- Otevřete Visual Studio 2012.
- Vybrat **soubor-&gt; nový-&gt; projekt**
- V levém podokně klikněte na položku **Visual C @ no__t-1**a potom vyberte šablonu **konzoly** .
- Do pole název zadejte **CUDSProcsSample** AS.
- Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

- Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení a vyberte možnost **Přidat-&gt; nová položka**.
- V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
- Jako název souboru zadejte **CUDSProcs. edmx** a pak klikněte na **Přidat**.
- V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.
- Klikněte na **nové připojení**. V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.
    Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.
- V dialogovém okně zvolte objekty databáze pod **tabulkami** node vyberte tabulku **osoba** .
- V uzlu **uložené procedury a funkce** vyberte také následující uložené procedury: **DeletePerson**, **InsertPerson**a **UpdatePerson**.
- Počínaje sadou Visual Studio 2012 Návrhář EF podporuje hromadný import uložených procedur. Ve výchozím nastavení je zaškrtnuto políčko **Importovat vybrané uložené procedury a funkce do modelu entity** . Vzhledem k tomu, že v tomto příkladu máme uložené procedury, které vkládají, aktualizují a odstraňují typy entit, nechcete je naimportovat a zruší zaškrtnutí tohoto políčka.

    ![Importovat S – procs](~/ef6/media/importsprocs.jpg)

- Klikněte na tlačítko **Dokončit**.
    Zobrazí se Návrhář EF, který poskytuje návrhovou plochu pro úpravu vašeho modelu.

## <a name="map-the-person-entity-to-stored-procedures"></a>Mapování entity Person na uložené procedury

- Klikněte pravým tlačítkem na typ **osoba**@no__t – 1entity a vyberte **mapování uložených procedur**.
- Mapování uložených procedur se zobrazí v **podrobnostech mapování** window.
- Klikněte na **@no__t – 1Select Vložit funkci @ no__t-2**.
    Pole se zobrazí v rozevíracím seznamu uložených procedur v modelu úložiště, které lze namapovat na typy entit v koncepčním modelu.
    V rozevíracím seznamu vyberte **InsertPerson** from.
- Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity. Všimněte si, že šipky označují směr mapování: Hodnoty vlastností jsou dodány parametrům uložené procedury.
- Klikněte na **@no__t – vazba výsledku 1Add @ no__t-2**.
- Zadejte **NewPersonID**, název parametru vráceného procedurou **InsertPerson** stored. Nezapomeňte zadat počáteční ani koncové mezery.
- Stiskněte klávesu **ENTER**.
- Ve výchozím nastavení je **NewPersonID** Is mapována na klíč entity **PersonID**. Všimněte si, že šipka indikuje směr mapování: Do vlastnosti je zadána hodnota sloupce výsledek.

    ![Mapování podrobností](~/ef6/media/mappingdetails.png)

- Klikněte na **@no__t – 1Select Update – funkce @ no__t-2** And vyberte **UpdatePerson** from výsledného rozevíracího seznamu.
- Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity.
- Klikněte na **&lt;Select odstranit funkci @ no__t-2** And vyberte **DeletePerson** from výsledného rozevíracího seznamu.
- Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity.

Operace vložení, aktualizace a odstranění **osoby**typu  entity jsou nyní namapovány na uložené procedury.

Pokud chcete povolit kontrolu souběžnosti při aktualizaci nebo odstranění entity s uloženými procedurami, použijte jednu z následujících možností:

- Použijte **výstupní** parametr pro vrácení počtu ovlivněných řádků z uložené procedury a zaškrtnutí **parametru ovlivněné řádky** checkbox vedle názvu parametru. Pokud je vrácená hodnota nula, když je operace volána, vyvolá se  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will.
- Zaškrtněte políčko **použít původní hodnotu** vedle vlastnosti, kterou chcete použít pro kontrolu souběžnosti. Při pokusu o aktualizaci se při zápisu dat zpět do databáze použije hodnota vlastnosti, která byla původně načtena z databáze. Pokud hodnota neodpovídá hodnotě v databázi, vyvolá se **OptimisticConcurrencyException** will.

## <a name="use-the-model"></a>Použití modelu

Otevřete soubor **program.cs** , kde je definována metoda **Main** . Do funkce Main přidejte následující kód.

Kód vytvoří nový objekt **Person** a pak objekt aktualizuje a nakonec objekt odstraní.

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

- Zkompilujte a spusťte aplikaci. Program vytvoří následující výstup *

> [!NOTE]
> PersonID se automaticky generuje serverem, takže se pravděpodobně zobrazí jiné číslo *

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Pokud pracujete s konečnou verzí sady Visual Studio, můžete použít IntelliTrace s ladicím programem k zobrazení příkazů SQL, které se spustí.

![IntelliTrace](~/ef6/media/intellitrace.png)
