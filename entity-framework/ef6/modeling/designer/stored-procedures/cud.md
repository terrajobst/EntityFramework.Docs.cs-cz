---
title: Návrháře CUD uložených procedur komponentami TableAdapter-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 35a00aa817c8643352c517c233977efd49e3baac
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489554"
---
# <a name="designer-cud-stored-procedures"></a>Návrháře CUD uložené procedury
Tento podrobný návod ukazují, jak vytvořit mapování\\vložení, aktualizace a odstranění (vytvoření) operace typu entity na uložené procedury pomocí návrháře Entity Framework (EF designeru).  Ve výchozím nastavení Entity Framework automaticky generuje příkazů SQL pro operace vytvoření, ale můžete také namapovat uložených procedur na těchto operací.  

Všimněte si, že Code First nepodporuje mapování uložené procedury nebo funkce. Pomocí metody System.Data.Entity.DbSet.SqlQuery však můžete volat uložené procedury nebo funkce. Příklad:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Důležité informace o mapování CUD operace na uložené procedury

Při mapování CUD operace na uložené procedury, platí následující aspekty: 

-   Při jedné z operací CUD mapování uložené procedury, namapujte všechny z nich. Pokud není mapování všech třech umístěních, nenamapované operace se nezdaří, pokud jsou provedeny a **UpdateException** bude vyvolána výjimka.
-   Každý parametr uložené procedury musí být namapovaný na vlastnosti entity.
-   Pokud server vygeneruje hodnotu primárního klíče pro vložený řádek, je nutné mapovat tato hodnota zpět na vlastnost klíče entity. V následujícím příkladu **InsertPerson** uloženou proceduru vrátí nově vytvořený primární klíč v rámci uložené procedury sada výsledků dotazu. Primární klíč je namapována na klíč entity (**PersonID**) pomocí **&lt;přidat výsledných vazeb&gt;** funkce EF designeru.
-   Volání uložené procedury jsou namapované 1:1 s entitami v konceptuálním modelu. Například pokud se rozhodnete implementovat hierarchie dědičnosti v konceptuálním modelu a pak mapování vytvoření uložené procedury pro **nadřazené** (základní) a **podřízené** (odvozené) entity, uložení **Podřízené** změny bude volat pouze **podřízené**uživatele uložených procedur, nebude spustí **nadřazené**uživatele uložené procedury volání.

## <a name="prerequisites"></a>Požadavky

K dokončení toho návodu budete potřebovat:

- Nejnovější verzi sady Visual Studio.
- [Ukázkové databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

-   Otevřít Visual Studio 2012.
-   Vyberte **souboru -&gt; nové –&gt; projektu**
-   V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony.
-   Zadejte **CUDSProcsSample** jako název.
-   Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

-   Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte **Add -&gt; nová položka**.
-   Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.
-   Zadejte **CUDSProcs.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.
-   V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další**.
-   Klikněte na tlačítko **nové připojení**. V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.
    Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.
-   V okně Zvolte vaše databázové objekty dialogovém okně **tabulky** uzlu, vyberte **osoba** tabulky.
-   Také vybrat následující uložené procedury v části **uložené procedury a funkce** uzlu: **DeletePerson**, **InsertPerson**, a **UpdatePerson** . 
-   Od verze Visual Studio 2012 EF designeru podporuje hromadný import uložených procedur. **Importovat vybrané uložených procedur a funkcí do entity model** je ve výchozím nastavení zaškrtnuto. Protože v tomto příkladu budeme mít uložené procedury, které vložit, aktualizovat a odstranit typy entit, nechcete, aby importovat jsme se zrušit zaškrtnutí tohoto políčka. 

    ![Importovat S Procs](~/ef6/media/importsprocs.jpg)

-   Klikněte na tlačítko **Dokončit**.
    EF designeru, která poskytuje návrhové ploše pro úpravy váš model, se zobrazí.

## <a name="map-the-person-entity-to-stored-procedures"></a>Mapa entita osoba, která má uložené procedury

-   Klikněte pravým tlačítkem myši **osoba** typu entity a vyberte **mapování uložené procedury**.
-   Mapování uložené procedury joinkind **podrobnosti mapování** okna.
-   Klikněte na tlačítko  **&lt;vyberte Vložit funkci&gt;**.
    Pole stane rozevíracího seznamu uložených procedur v modelu úložiště, který je možné mapovat na typy entit v konceptuálním modelu.
    Vyberte **InsertPerson** z rozevíracího seznamu.
-   Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastností entity. Všimněte si, že šipky označují směr mapování: vlastnost hodnot pro parametry uložené procedury.
-   Klikněte na tlačítko  **&lt;přidat vazbu výsledek&gt;**.
-   Typ **NewPersonID**, název parametru vrácené **InsertPerson** uložené procedury. Zajistěte, aby zadejte počáteční ani koncové mezery.
-   Stisknutím klávesy **zadejte**.
-   Ve výchozím nastavení **NewPersonID** je namapována na klíč entity **PersonID**. Všimněte si, že šipka označuje směr mapování: pro vlastnost není zadána hodnota ve sloupci výsledků.

    ![Podrobnosti mapování](~/ef6/media/mappingdetails.png)

-   Klikněte na tlačítko **&lt;vyberte funkce aktualizace&gt;** a vyberte **UpdatePerson** v rozevíracím seznamu rozevíracího seznamu.
-   Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastností entity.
-   Klikněte na tlačítko **&lt;vyberte funkce Odstranit&gt;** a vyberte **DeletePerson** v rozevíracím seznamu rozevíracího seznamu.
-   Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastností entity.

Vložení, aktualizace a odstranění operace **osoba** typ entity jsou teď namapovaných na uložené procedury.

Pokud chcete povolit souběžnost kontroly při aktualizaci nebo odstranění entity s uloženými procedurami, použijte jednu z následujících možností:

-   Použití **výstup** parametr a vrátit tak počet ovlivněné řádky z uložené procedury a kontrolu **parametr ovlivněných řádků** zaškrtávací políčko vedle názvu parametru. Pokud vrácená hodnota je nula, při volání operace [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) bude vyvolána výjimka.
-   Zkontrolujte **použít původní hodnota** zaškrtávací políčko vedle vlastnosti, která chcete použít pro kontrolu souběžnosti. Při pokusu o aktualizaci, použije se hodnota vlastnosti, která byla původně načtena z databáze při zápis dat zpět do databáze. Pokud hodnota neodpovídá hodnotě v databázi, **OptimisticConcurrencyException** bude vyvolána výjimka.

## <a name="use-the-model"></a>Použití modelu

Otevřít **Program.cs** souboru, kde **hlavní** je definována metoda. Přidejte následující kód do funkce Main.

Kód vytvoří novou **osoba** pak aktualizuje objekt objektu a nakonec odstraní objekt.         

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

-   Kompilace a spuštění aplikace. Program vygeneruje následující výstup *
    >[!NOTE]
> PersonID je automaticky vytvořen serveru, pravděpodobně se zobrazí jiné číslo *

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Při práci s verzí sady Visual Studio Ultimate, vám pomůže nástroj Intellitrace v ladicím programu naleznete v tématu příkazy SQL, které se provádějí.

![Intellitrace](~/ef6/media/intellitrace.png)
