---
title: CUD uložené procedury návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418320"
---
# <a name="designer-cud-stored-procedures"></a>CUD uložené procedury návrháře

Tento podrobný návod ukazuje, jak namapovat operace vytvoření\\vložení, aktualizace a odstranění (CUD) typu entity do uložených procedur pomocí Entity Framework Designer (EF Designer).  Ve výchozím nastavení Entity Framework automaticky generuje příkazy SQL pro operace CUD, ale můžete také namapovat uložené procedury na tyto operace.  

Všimněte si, že Code First nepodporuje mapování na uložené procedury nebo funkce. Uložené procedury nebo funkce však můžete volat pomocí metody System. data. entity. Negenerickými. SqlQuery. Příklad:

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Předpoklady při mapování operací CUD na uložené procedury

Při mapování operací CUD na uložené procedury platí následující požadavky:

- Pokud mapujete jednu z operací CUD na uloženou proceduru, namapujte na ni všechny. Pokud nemapujete všechny tři, nemapované operace selžou při spuštění a vyvolá se **UpdateException** .
- Všechny parametry uložené procedury je nutné namapovat na vlastnosti entity.
- Pokud server vygeneruje hodnotu primárního klíče pro vložený řádek, musíte tuto hodnotu namapovat zpátky na klíčovou vlastnost entity. V následujícím příkladu vrátí **InsertPerson** uloženou proceduru nově vytvořený primární klíč jako součást sady výsledků uložených procedur. Primární klíč je namapován na klíč entity (**PersonID**) pomocí **&lt;funkce přidat vazby výsledku&gt;**  funkci návrháře EF.
- Volání uložených procedur jsou namapována 1:1 s entitami v koncepčním modelu. Například Pokud implementujete hierarchii dědičnosti v koncepčním modelu a poté namapujete uložené procedury CUD pro **nadřazený objekt** (základní) a **podřízené** (odvozené) entity, uložením **podřízených** změn budou volána pouze uložené procedury pro **nadřazený** **objekt.**

## <a name="prerequisites"></a>Předpoklady

K dokončení toho návodu budete potřebovat:

- Poslední verze sady Visual Studio.
- [Ukázková databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

- Otevřete Visual Studio 2012.
- Vybrat **soubor-&gt; projekt New-&gt;**
- V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .
- Jako název zadejte **CUDSProcsSample** .
- Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

- Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení a vyberte možnost **Přidat-&gt; novou položku**.
- V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
- Jako název souboru zadejte **CUDSProcs. edmx** a pak klikněte na **Přidat**.
- V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.
- Klikněte na **nové připojení**. V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.
    Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.
- V dialogovém okně zvolte objekty databáze pod uzlem  **tabulky** vyberte tabulku **Person** .
- V uzlu **uložené procedury a funkce** vyberte také následující uložené procedury: **DeletePerson**, **InsertPerson**a **UpdatePerson**.
- Počínaje sadou Visual Studio 2012 Návrhář EF podporuje hromadný import uložených procedur. Ve výchozím nastavení je zaškrtnuto políčko **Importovat vybrané uložené procedury a funkce do modelu entity** . Vzhledem k tomu, že v tomto příkladu máme uložené procedury, které vkládají, aktualizují a odstraňují typy entit, nechcete je naimportovat a zruší zaškrtnutí tohoto políčka.

    ![Importovat S – procs](~/ef6/media/importsprocs.jpg)

- Klikněte na tlačítko **Dokončit**.
    Zobrazí se Návrhář EF, který poskytuje návrhovou plochu pro úpravu vašeho modelu.

## <a name="map-the-person-entity-to-stored-procedures"></a>Mapování entity Person na uložené procedury

- Klikněte pravým tlačítkem na typ entity  **osoba** a vyberte možnost **mapování uložených procedur**.
- Mapování uložených procedur se zobrazí v okně **Podrobnosti mapování** .
- Klikněte **&lt;vyberte vložit&gt;funkce **.
    Pole se zobrazí v rozevíracím seznamu uložených procedur v modelu úložiště, které lze namapovat na typy entit v koncepčním modelu.
    V rozevíracím seznamu vyberte **InsertPerson** .
- Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity. Všimněte si, že šipky označují směr mapování: hodnoty vlastností jsou dodány parametrům uložené procedury.
- Klikněte **&lt;přidat&gt;vazby výsledků **.
- Zadejte **NewPersonID**, název parametru vráceného uloženou procedurou **InsertPerson** . Nezapomeňte zadat počáteční ani koncové mezery.
- Stiskněte klávesu **ENTER**.
- Ve výchozím nastavení je **NewPersonID** namapována na klíč entity **PersonID**. Všimněte si, že šipka indikuje směr mapování: hodnota sloupce výsledek je dodána vlastnosti.

    ![Mapování podrobností](~/ef6/media/mappingdetails.png)

- Klikněte na **&lt;vybrat funkci aktualizovat&gt;**  a z výsledného rozevíracího seznamu vyberte **UpdatePerson** .
- Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity.
- Klikněte **&lt;vyberte odstranit funkci&gt;**  a z výsledného rozevíracího seznamu vyberte **DeletePerson** .
- Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity.

Operace vložení, aktualizace a odstranění **osoby** typu entity jsou nyní namapovány na uložené procedury.

Pokud chcete povolit kontrolu souběžnosti při aktualizaci nebo odstranění entity s uloženými procedurami, použijte jednu z následujících možností:

- Pomocí parametru **Output** vrátí počet ovlivněných řádků z uložené procedury a zaškrtněte **parametr ovlivněných řádků** zaškrtávací políčko vedle názvu parametru. Pokud je vrácena hodnota nula při volání operace, bude vyvolána  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) .
- Zaškrtněte políčko **použít původní hodnotu** vedle vlastnosti, kterou chcete použít pro kontrolu souběžnosti. Při pokusu o aktualizaci se při zápisu dat zpět do databáze použije hodnota vlastnosti, která byla původně načtena z databáze. Pokud hodnota neodpovídá hodnotě v databázi, bude vyvolána **OptimisticConcurrencyException** .

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
