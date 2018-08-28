---
title: Návrháře TPT dědičnosti - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996345"
---
# <a name="designer-tpt-inheritance"></a>Návrháře TPT dědičnosti
Tento podrobný návod ukazuje, jak implementovat za typ tabulky (TPT) dědičnosti ve vašem modelu pomocí návrháře Entity Framework (EF designeru). Dědičnost na typ tabulka používá do samostatné tabulky v databázi zachovat data pro-zděděné vlastnosti a vlastnosti klíče pro každý typ v hierarchii dědičnosti.

V tomto názorném postupu jsme se namapuje **kurzu** (základní typ), **OnlineCourse** (je odvozena z kurzu), a **OnsiteCourse** (je odvozena z **kurzu**) entity do tabulky se stejnými názvy. Vytvoříme model z databáze a poté změňte model implementovat TPT dědičnosti.

Můžete také začít s první Model a pak vygenerovat databáze z modelu. EF designeru ve výchozím nastavení používá TPT strategie a proto všechny dědění v modelu budou zmapována do samostatných tabulek.

## <a name="other-inheritance-options"></a>Další možnosti dědičnosti

Za hierarchii tabulky (TPH) je jiný druh dědičnosti v databázi, kterou jeden tabulky je používán pro zachování dat pro všechny typy entit v hierarchii dědičnosti.  Informace o tom, jak namapovat na hierarchii tabulky dědičnosti pomocí návrháře entit najdete v tématu [EF návrháře TPH dědičnosti](~/ef6/modeling/designer/inheritance/tph.md). 

Mějte na paměti, že tabulka na konkrétní zadejte dědičnosti (TPC) a smíšené dědičnosti modely jsou podporované modulem runtime Entity Frameworku, ale nejsou podporovány EF designeru. Pokud chcete použít TPC nebo smíšené dědičnosti, máte dvě možnosti: použijte Code First, nebo ručně upravit soubor EDMX. Pokud budete chtít pracovat s souboru EDMX, v okně podrobností mapování zařadí do "nouzový režim" a nebude možné změnit mapování pomocí návrháře.

## <a name="prerequisites"></a>Požadavky

K dokončení toho návodu budete potřebovat:

- Nejnovější verzi sady Visual Studio.
- [Ukázkové databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

-   Otevřít Visual Studio 2012.
-   Vyberte **souboru -&gt; nové –&gt; projektu**
-   V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony.
-   Zadejte **TPTDBFirstSample** jako název.
-   Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

-   Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **Add -&gt; nová položka**.
-   Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.
-   Zadejte **TPTModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.
-   V dialogovém okně Výběr obsahu modelu, vyberte ** Generovat z databáze ** a klepněte na tlačítko **Další**.
-   Klikněte na tlačítko **nové připojení**.
    V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.
    Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.
-   V dialogovém okně Zvolte vaše databázové objekty tabulky uzlu, vyberte **oddělení**, **kurzu OnlineCourse a OnsiteCourse** tabulky.
-   Klikněte na tlačítko **Dokončit**.

V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí. Všechny objekty, které jste vybrali v dialogovém okně Zvolte vaše databázové objekty jsou přidány do modelu.

## <a name="implement-table-per-type-inheritance"></a>Implementace dědičnosti na typ tabulky

-   Na návrhové ploše, klikněte pravým tlačítkem myši **OnlineCourse** typu entity a vyberte **vlastnosti**.
-   V **vlastnosti** okno, nastavte vlastnost typu Base na **kurzu**.
-   Klikněte pravým tlačítkem myši **OnsiteCourse** typu entity a vyberte **vlastnosti**.
-   V **vlastnosti** okno, nastavte vlastnost typu Base na **kurzu**.
-   Klikněte pravým tlačítkem na přidružení (řádek) mezi **OnlineCourse** a **kurzu** typy entit.
    Vyberte **odstranit z modelu**.
-   Klikněte pravým tlačítkem na přidružení mezi **OnsiteCourse** a **kurzu** typy entit.
    Vyberte **odstranit z modelu**.

Teď odstraníme **CourseID** vlastnost z **OnlineCourse** a **OnsiteCourse** vzhledem k tomu, že tyto třídy dědí **CourseID** z **kurzu** základní typ.

-   Klikněte pravým tlačítkem myši **CourseID** vlastnost **OnlineCourse** typu entity a pak vyberte **odstranit z modelu**.
-   Klikněte pravým tlačítkem myši **CourseID** vlastnost **OnsiteCourse** typu entity a pak vyberte **odstranit z modelu**
-   Každý typ tabulky dědičnosti je nyní implementována.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Použití modelu

Otevřít **Program.cs** souboru, kde **hlavní** je definována metoda. Vložte následující kód do **hlavní** funkce. Kód spustí tři dotazy. První dotaz vrací všechny **kurzy** související se zadaným oddělení. Druhý dotaz používá **OfType** metoda vrátí **OnlineCourses** související se zadaným oddělení. Vrátí třetí dotaz **OnsiteCourses**.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
