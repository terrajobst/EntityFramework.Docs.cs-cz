---
title: Návrhář dědičnosti TPH – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418425"
---
# <a name="designer-tph-inheritance"></a>Dědičnost typu TPH návrháře
V tomto podrobném návodu se dozvíte, jak implementovat dědičnost typu Table-per-Hierarchy (TPH) ve koncepčním modelu pomocí Entity Framework Designer (EF Designer). Dědičnost TPH používá jednu databázovou tabulku k uchování dat pro všechny typy entit v hierarchii dědičnosti.

V tomto návodu namapujeme tabulku Person na tři typy entit: osoba (základní typ), student (odvozený od osoby) a instruktor (odvozený od osoby). Vytvoříme koncepční model z databáze (Database First) a pak upravíte model pro implementaci dědičnosti TPH pomocí návrháře EF.

Je možné namapovat na dědičnost TPH pomocí Model First, ale museli byste napsat vlastní pracovní postup generování databáze, který je složitý. Tento pracovní postup byste pak přiřadili vlastnosti **pracovního postupu generování databáze** v Návrháři EF. Jednodušší alternativou je použití Code First.

## <a name="other-inheritance-options"></a>Další možnosti dědičnosti

Tabulka na typ (TPT) je jiný typ dědičnosti, ve kterém jsou oddělené tabulky v databázi mapovány na entity, které se účastní dědičnosti.  Informace o tom, jak namapovat dědičnost tabulek na typ pomocí návrháře EF, najdete v tématu [DĚDIČNOST TPT návrháře EF](~/ef6/modeling/designer/inheritance/tpt.md).

Dědičnost typů (TPC) podle konkrétního typu () a smíšené modely dědičnosti jsou podporovány modulem runtime Entity Framework, ale Návrhář EF je nepodporuje. Pokud chcete použít TPC nebo smíšenou dědičnost, máte dvě možnosti: použijte Code First nebo ručně upravte soubor EDMX. Pokud se rozhodnete pracovat se souborem EDMX, bude okno Podrobnosti mapování přepnuto do bezpečného režimu a nebudete moci použít návrháře ke změně mapování.

## <a name="prerequisites"></a>Předpoklady

K dokončení toho návodu budete potřebovat:

- Poslední verze sady Visual Studio.
- [Ukázková databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

-   Otevřete Visual Studio 2012.
-   Vybrat **soubor-&gt; projekt New-&gt;**
-   V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .
-   Jako název zadejte **TPHDBFirstSample** .
-   Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

-   Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení a vyberte možnost **Přidat-&gt; novou položku**.
-   V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
-   Jako název souboru zadejte **TPHModel. edmx** a pak klikněte na **Přidat**.
-   V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.
-   Klikněte na **nové připojení**.
    V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.
    Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.
-   V dialogovém okně zvolte objekty databáze v uzlu tabulky vyberte tabulku **osoba** .
-   Klikněte na tlačítko **Dokončit**.

Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu. Do modelu jsou přidány všechny objekty, které jste vybrali v dialogovém okně zvolit objekty databáze.

To znamená, jak tabulka **osoby** vypadá v databázi.

![Tabulka Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementace dědičnosti tabulek na hierarchii

Tabulka **Person** má sloupec **diskriminátor** , který může mít jednu ze dvou hodnot: "student" a "instruktor". V závislosti na hodnotě, kterou bude tabulka **Person** namapována na entitu **studenta** nebo na entitu **instruktora** . Tabulka **Person** má také dva sloupce, **ZaměstnánOd** a **EnrollmentDate**, které musí mít **hodnotu null** , protože osoba nemůže být studentem a instruktorem ve stejnou dobu (alespoň v tomto návodu).

### <a name="add-new-entities"></a>Přidat nové entity

-   Přidejte novou entitu.
    Provedete to tak, že kliknete pravým tlačítkem myši na prázdné místo na návrhové ploše Entity Framework Designer a vyberete **Přidat-&gt;entitu**.
-   Do pole **název entity** zadejte  **instruktora** a v rozevíracím seznamu pro **základní typ**vyberte **osoba** .
-   Klikněte na tlačítko **OK**.
-   Přidejte další novou entitu. Jako **název entity** zadejte  **student** a v rozevíracím seznamu pro **základní typ**vyberte **Person** .

Na návrhovou plochu se přidaly dva nové typy entit. Šipka směřuje od nových typů entit od typu **osoba** typ entity; To znamená, že  **osoba** je základním typem pro nové typy entit.

-   Klikněte pravým tlačítkem na vlastnost **zaměstnánod**  **osoby** entity. Vyberte **Vyjmout** (nebo použijte klávesu CTRL-X).
-   Klikněte pravým tlačítkem myši na **instruktora** entitu a vyberte **Vložit** (nebo použijte klávesu CTRL-V).
-   Klikněte pravým tlačítkem na vlastnost **HireDate** a vyberte možnost **vlastnosti**.
-   V okně **vlastnosti** nastavte vlastnost **Nullable** na **false**.
-   Klikněte pravým tlačítkem na vlastnost **EnrollmentDate**  **osoby** entity. Vyberte **Vyjmout** (nebo použijte klávesu CTRL-X).
-   Klikněte pravým tlačítkem na entitu **studenta** a vyberte **Vložit (nebo použijte klávesu CTRL-V).**
-   Vyberte vlastnost **EnrollmentDate** a nastavte vlastnost **Nullable** na **false**.
-   Vyberte typ entity  **osoba** . V okně **vlastnosti** nastavte jeho vlastnost **abstract** na **hodnotu true**.
-   Odstraňte vlastnost **diskriminátor** z **Person**. Důvod, proč by měl být odstraněn, je vysvětlen v následující části.

### <a name="map-the-entities"></a>Mapování entit

-   Klikněte pravým tlačítkem na **instruktor** a vyberte **mapování tabulky.**
    V okně podrobností mapování je vybrána entita Instructor.
-   V okna **podrobností mapování** klikněte na **&lt;přidat tabulku nebo zobrazení&gt;**  .
     **&lt;přidání tabulky nebo zobrazení&gt;** pole se zobrazí rozevírací seznam tabulek nebo zobrazení, do kterých lze vybranou entitu namapovat.
-   Z rozevíracího seznamu vyberte **osoba** .
-   Okno  **podrobností mapování** se aktualizuje s výchozími mapováními sloupců a možností pro přidání podmínky.
-   Klikněte na **&lt;přidat&gt;podmínky **.
     **&lt;přidat podmínku&gt;**  pole se zobrazí rozevírací seznam sloupců, pro které lze nastavit podmínky.
-   V rozevíracím seznamu vyberte **diskriminátor** .
-   Ve sloupci **operátor** v okně **Podrobnosti mapování** v rozevíracím seznamu vyberte =.
-   Do sloupce **hodnota nebo vlastnost** zadejte **instruktor**. Konečný výsledek by měl vypadat takto:

    ![Mapování podrobností](~/ef6/media/mappingdetails2.png)

-   Opakujte tento postup pro typ entity  **studenta** , ale nastavte podmínku rovnou hodnotě **student** .  
    *Důvodem, proč jsme chtěli odebrat vlastnost **diskriminátoru** , je to, že sloupec tabulky nejde namapovat více než jednou. Tento sloupec bude použit pro podmíněné mapování, takže jej nelze použít také pro mapování vlastností. Jediným způsobem, jak lze použít pro obě, pokud podmínka používá **hodnotu null** nebo není **null** porovnání.*

Dědičnost tabulky na hierarchii je nyní implementována.

![Konečný TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Použití modelu

Otevřete soubor **program.cs** , kde je definována metoda **Main** . Do funkce **Main** vložte následující kód. Kód spouští tři dotazy. První dotaz vrátí všechny objekty **Person** . Druhý dotaz používá metodu **OfType** k vrácení objektů **instruktora** . Třetí dotaz používá metodu **OfType** k vrácení objektů **studenta** .

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
