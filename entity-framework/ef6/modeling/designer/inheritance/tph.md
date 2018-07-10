---
title: Dědičnost návrháře TPH - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
caps.latest.revision: 3
ms.openlocfilehash: 0a017d3b97808cede3134119940b2e5839d0f282
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912718"
---
# <a name="designer-tph-inheritance"></a>Návrháře TPH dědičnosti
Tento podrobný návod ukazuje, jak implementovat dědičnosti na hierarchii tabulky (TPH) v konceptuálním modelu se Návrhář Entity Framework (EF designeru). Dědičnost TPH pomocí jedné tabulky databáze zachovat data pro všechny typy entit v hierarchii dědičnosti.

V tomto názorném postupu jsme mapování tabulky osoba na tři typy entit: osoba (základní typ), studenty (je odvozena od osoby) a kurzů vedených (je odvozena od osoby). Vytvoříme Koncepční model z databáze (Database First) a poté změňte model implementovat dědičnost TPH pomocí EF designeru.

Je možné namapovat na TPH dědičnosti pomocí modelu první, ale vy byste chtěli zapsat generování pracovního postupu vlastní databázi, což je komplexní. By pak přiřaďte tento pracovní postup **pracovní postup generování databáze** vlastnost v EF designeru. Jednodušší alternativu je použití Code First.

## <a name="other-inheritance-options"></a>Další možnosti dědičnosti

Za typ tabulky (TPT) je jiný druh dědičnosti, ve kterém samostatných tabulek v databázi se mapují na entity, které se účastní dědičnosti.  Informace o tom, jak mapovat na typ tabulky dědičnosti s EF designeru najdete v tématu [EF návrháře TPT dědičnosti](~/ef6/modeling/designer/inheritance/tpt.md).

Smíšené dědičnosti modely a tabulky na konkrétní typ dědičnosti (TPC) podporuje modul runtime rozhraní Entity Framework, ale nepodporuje EF designeru. Pokud chcete použít TPC nebo smíšené dědičnosti, máte dvě možnosti: použijte Code First, nebo ručně upravit soubor EDMX. Pokud budete chtít pracovat s souboru EDMX, v okně podrobností mapování zařadí do "nouzový režim" a nebude možné změnit mapování pomocí návrháře.

## <a name="prerequisites"></a>Požadavky

K dokončení toho návodu budete potřebovat:

- Nejnovější verzi sady Visual Studio.
- [Ukázkové databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

-   Otevřít Visual Studio 2012.
-   Vyberte **souboru -&gt; nové –&gt; projektu**
-   V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony.
-   Zadejte **TPHDBFirstSample** jako název.
-   Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

-   Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte **Add -&gt; nová položka**.
-   Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.
-   Zadejte **TPHModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.
-   V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další**.
-   Klikněte na tlačítko **nové připojení**.
    V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.
    Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.
-   V dialogovém okně Zvolte vaše databázové objekty tabulky uzlu, vyberte **osoba** tabulky.
-   Klikněte na tlačítko **Dokončit**.

V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí. Všechny objekty, které jste vybrali v dialogovém okně Zvolte vaše databázové objekty jsou přidány do modelu.

To znamená jak **osoba** vypadá tabulka v databázi.

![PersonTable](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementace tabulky za hierarchie dědičnosti

**Osoba** tabulka má **diskriminátoru** sloupec, který může mít jednu ze dvou hodnot: "Studenta" a "Kurzů vedených". V závislosti na hodnotě **osoba** tabulky se namapují na **Student** entity nebo **kurzů vedených** entity. **Osoba** tabulka má také dva sloupce **HireDate** a **EnrollmentDate**, která musí být **s možnou hodnotou Null** vzhledem k tomu, že uživatel nemůže být pro studenty a instruktorem ve stejnou dobu (alespoň není v tomto názorném postupu).

### <a name="add-new-entities"></a>Přidat nové entity

-   Přidáte novou entitu.
    Chcete-li to provést, klikněte pravým tlačítkem na prázdné místo návrhové plochy Entity Framework Designer a vyberte **Add -&gt;Entity**.
-   Typ **kurzů vedených** pro **název Entity** a vyberte **osoba** z rozevíracího seznamu pro **základní typ**.
-   Klikněte na tlačítko **OK**.
-   Přidejte další novou entitu. Typ **Student** pro **název Entity** a vyberte **osoba** z rozevíracího seznamu pro **základní typ**.

Na návrhovou plochu byly přidány dva nové typy entit. Šipka ukazuje z nové typy entit, které se **osoba** typu entity; to znamená, že **osoba** je základním typem pro nové typy entit.

-   Klikněte pravým tlačítkem myši **HireDate** vlastnost **osoba** entity. Vyberte **Vyjmout** (nebo pomocí klávesy Ctrl-X).
-   Klikněte pravým tlačítkem myši **kurzů vedených** entity a vyberte **vložit** (nebo pomocí klávesy Ctrl-V).
-   Klikněte pravým tlačítkem myši **HireDate** vlastnosti a vyberte **vlastnosti**.
-   V **vlastnosti** okno, nastaveno **Nullable** vlastnost **false**.
-   Klikněte pravým tlačítkem myši **EnrollmentDate** vlastnost **osoba** entity. Vyberte **Vyjmout** (nebo pomocí klávesy Ctrl-X).
-   Klikněte pravým tlačítkem myši **Student** entity a vyberte **vložit (nebo klíče pomocí Ctrl-V).**
-   Vyberte **EnrollmentDate** vlastnost a nastavte **Nullable** vlastnost **false**.
-   Vyberte **osoba** typu entity. V **vlastnosti** okno, nastavte jeho **abstraktní** vlastnost **true**.
-   Odstranit **diskriminátoru** vlastnost z **osoba**. V následující části je vysvětlen z důvodů, proč je nutné ji odstranit.

### <a name="map-the-entities"></a>Mapovat entity

-   Klikněte pravým tlačítkem myši **kurzů vedených** a vyberte **mapování tabulky.**
    V okně Podrobnosti mapování je vybraná entita instruktorem.
-   Klikněte na tlačítko **&lt;přidat tabulku nebo zobrazení&gt;** v **podrobnosti mapování** okna.
    **&lt;Přidat tabulku nebo zobrazení&gt;** pole stane rozevírací seznam tabulek nebo zobrazení pro které je možné mapovat vybrané entity.
-   Vyberte **osoba** z rozevíracího seznamu.
-   **Podrobnosti mapování** okno se aktualizuje s výchozí mapování sloupců a možnost pro přidání podmínky.
-   Klikněte na  **&lt;přidat podmínku&gt;**.
    **&lt;Přidat podmínku&gt;** pole bude rozevírací seznam sloupců, pro které můžete nastavit podmínky.
-   Vyberte **diskriminátoru** z rozevíracího seznamu.
-   V **operátor** sloupec **podrobnosti mapování** okně = z rozevíracího seznamu.
-   V **/vlastnost Value** sloupců, typ **kurzů vedených**. Konečný výsledek by měl vypadat nějak takto:

    ![MappingDetails2](~/ef6/media/mappingdetails2.png)

-   Opakujte tyto kroky pro **Student** typ entity, ale vytvořit podmínku, která je rovna **Student** hodnotu.  
    *Z důvodu jsme chtěli odebrat **diskriminátoru** je vlastnost, protože sloupec tabulky nelze mapovat více než jednou. V tomto sloupci se použije pro podmíněné mapování, proto jej nelze použít pro vlastnost mapování také. Jediným způsobem, který může sloužit pro obě, pokud používá podmínku **Is Null** nebo **Is Not Null** porovnání.*

Tabulky na hierarchii dědičnosti je nyní implementována.

![FinalTPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Použití modelu

Otevřít **Program.cs** souboru, kde **hlavní** je definována metoda. Vložte následující kód do **hlavní** funkce. Kód spustí tři dotazy. První dotaz vrací všechny **osoba** objekty. Druhý dotaz používá **OfType** metoda vrátí **kurzů vedených** objekty. Používá se třetí dotaz **OfType** metoda vrátí **Student** objekty.

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
