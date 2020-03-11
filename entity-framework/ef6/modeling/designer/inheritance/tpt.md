---
title: Dědičnost TPT v Návrháři – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418208"
---
# <a name="designer-tpt-inheritance"></a>Dědičnost TPT v Návrháři
V tomto podrobném návodu se dozvíte, jak implementovat dědičnost tabulek na typ (TPT) v modelu pomocí Entity Framework Designer (EF Designer). Dědičnost typu tabulka na typ používá samostatnou tabulku v databázi pro zachování dat pro nezděděné vlastnosti a klíčové vlastnosti pro každý typ v hierarchii dědičnosti.

V tomto návodu namapujeme **kurz** (základní typ), **OnlineCourse** (odvozený od kurzu) a **OnsiteCourse** (z **kurzu**) na tabulky se stejnými názvy. Vytvoříme model z databáze a pak upravíte model pro implementaci TPT dědičnosti.

Můžete také začít s Model First a potom databázi vygenerovat z modelu. Návrhář EF používá ve výchozím nastavení strategii TPT, takže jakákoli dědičnost v modelu bude namapována na samostatné tabulky.

## <a name="other-inheritance-options"></a>Další možnosti dědičnosti

Tabulka na hierarchii (TPH) je jiný typ dědičnosti, ve kterém se používá jedna databázová tabulka k údržbě dat pro všechny typy entit v hierarchii dědičnosti.  Informace o tom, jak namapovat dědění tabulky na hierarchii s Entity Designer, naleznete v tématu [dědičnosti TPH v nástroji EF Designer](~/ef6/modeling/designer/inheritance/tph.md). 

Všimněte si, že modul runtime Entity Framework podporuje dědičnost typu tabulky podle konkrétního typu (TPC) a smíšené modely dědičnosti, ale Návrhář EF je nepodporuje. Pokud chcete použít TPC nebo smíšenou dědičnost, máte dvě možnosti: použijte Code First nebo ručně upravte soubor EDMX. Pokud se rozhodnete pracovat se souborem EDMX, bude okno Podrobnosti mapování přepnuto do bezpečného režimu a nebudete moci použít návrháře ke změně mapování.

## <a name="prerequisites"></a>Předpoklady

K dokončení toho návodu budete potřebovat:

- Poslední verze sady Visual Studio.
- [Ukázková databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

-   Otevřete Visual Studio 2012.
-   Vybrat **soubor-&gt; projekt New-&gt;**
-   V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .
-   Jako název zadejte **TPTDBFirstSample** .
-   Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

-   V Průzkumník řešení klikněte pravým tlačítkem na projekt a vyberte **Přidat-&gt; nová položka**.
-   V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
-   Jako název souboru zadejte **TPTModel. edmx** a pak klikněte na **Přidat**.
-   V dialogovém okně Vybrat obsah modelu vyberte možnost ** generovat z databáze**a poté klikněte na tlačítko **Další**.
-   Klikněte na **nové připojení**.
    V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.
    Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.
-   V dialogovém okně zvolte objekty databáze v uzlu tabulky vyberte tabulky **oddělení**, **kurz, OnlineCourse a OnsiteCourse** .
-   Klikněte na tlačítko **Dokončit**.

Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu. Do modelu jsou přidány všechny objekty, které jste vybrali v dialogovém okně zvolit objekty databáze.

## <a name="implement-table-per-type-inheritance"></a>Implementace dědičnosti tabulek na typ

-   Na návrhové ploše klikněte pravým tlačítkem myši na typ entity **OnlineCourse** a vyberte možnost **vlastnosti**.
-   V okně **vlastnosti** nastavte vlastnost základní typ na **kurz**.
-   Klikněte pravým tlačítkem myši na typ entity **OnsiteCourse** a vyberte možnost **vlastnosti**.
-   V okně **vlastnosti** nastavte vlastnost základní typ na **kurz**.
-   Klikněte pravým tlačítkem na přidružení (spojnici) mezi typy entit **OnlineCourse** a **Course** .
    Vyberte možnost **Odstranit z modelu**.
-   Klikněte pravým tlačítkem na přidružení mezi typy entit **OnsiteCourse** a **Course** .
    Vyberte možnost **Odstranit z modelu**.

Nyní odstraníme vlastnost **CourseID** z **OnlineCourse** a **OnsiteCourse** , protože tyto třídy dědí **CourseID** od typu základ **kurzu** .

-   Klikněte pravým tlačítkem na vlastnost **CourseID** typu entity **OnlineCourse** a pak vyberte **Odstranit z modelu**.
-   Klikněte pravým tlačítkem na vlastnost **CourseID** typu entity **OnsiteCourse** a pak vyberte **Odstranit z modelu** .
-   Dědičnost typu tabulka-na typ je nyní implementována.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Použití modelu

Otevřete soubor **program.cs** , kde je definována metoda **Main** . Do funkce **Main** vložte následující kód. Kód spouští tři dotazy. První dotaz vrátí zpět všechny **kurzy** týkající se určeného oddělení. Druhý dotaz používá metodu **OfType** k vrácení **OnlineCourses** souvisejících se zadaným oddělením. Třetí dotaz vrátí **OnsiteCourses**.

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
