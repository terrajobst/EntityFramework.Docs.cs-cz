---
title: Návrhář dotazu uložených procedur komponentami TableAdapter-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
caps.latest.revision: 3
ms.openlocfilehash: a08c1afc02266b35372a49fca1e829963e4785b2
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912748"
---
# <a name="designer-query-stored-procedures"></a>Návrhář dotazu uložené procedury
Tento podrobný návod ukazují, jak použít Návrhář Entity Framework (EF designeru) k importu uložené procedury do modelu a poté zavolejte importované uložené procedury k načtení výsledků. 

Všimněte si, že Code First nepodporuje mapování uložené procedury nebo funkce. Pomocí metody System.Data.Entity.DbSet.SqlQuery však můžete volat uložené procedury nebo funkce. Příklad:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Požadavky

K dokončení toho návodu budete potřebovat:

- Nejnovější verzi sady Visual Studio.
- [Ukázkové databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

-   Otevřít Visual Studio 2012.
-   Vyberte **souboru -&gt; nové –&gt; projektu**
-   V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony.
-   Zadejte **EFwithSProcsSample** jako název.
-   Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

-   Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **Add -&gt; nová položka**.
-   Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.
-   Zadejte **EFwithSProcsModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.
-   V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další**.
-   Klikněte na tlačítko **nové připojení**.  
    V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.  
    Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.
-   V dialogovém okně Zvolte vaše databázové objekty, zkontrolujte, **tabulky** zaškrtávací políčko, chcete-li vybrat všechny tabulky.  
    Také vybrat následující uložené procedury v části **uložené procedury a funkce** uzlu: **GetStudentGrades** a **GetDepartmentName**. 

    ![Importovat](~/ef6/media/import.jpg)

    *Od verze Visual Studio 2012 EF designeru podporuje hromadný import uložených procedur. **Importovat vybrané uložených procedur a funkcí do modelu theentity** je ve výchozím nastavení zaškrtnuto.*
-   Klikněte na tlačítko **Dokončit**.

Ve výchozím nastavení bude tvar výsledek každého importované uloženou proceduru nebo funkci, která vrátí více než jeden sloupec automaticky stane nový komplexní typ. V tomto příkladu chceme mapování výsledky **GetStudentGrades** funkce **StudentGrade** entity a výsledky **GetDepartmentName** k **žádný** (**žádný** je výchozí hodnota).

Pro importované funkce vrátit typ entity sloupců vrácený odpovídající uložené procedury musí přesně odpovídat Skalární vlastnosti typu vrácenou entitu. Importovaná funkce může vrátit také kolekce jednoduché typy, komplexních typů nebo žádnou hodnotu.

-   Klikněte pravým tlačítkem na návrhové ploše a vyberte **prohlížeč modelu**.
-   V **prohlížeč modelu**vyberte **importované funkce**a potom dvakrát klikněte **GetStudentGrades** funkce.
-   V dialogovém okně upravit importované funkce vyberte **entity** a zvolte **StudentGrade**.  
    ***Importované funkce je sestavitelné** zaškrtávacího políčka v horní části **importované funkce** dialogové okno vám umožní mapovat složení funkcí. Pokud zaškrtnete toto políčko, zobrazí se pouze sestavitelný funkce (funkce vracející tabulku) v **uložená procedura nebo název funkce** rozevíracího seznamu. Pokud toto políčko nezaškrtávejte, v seznamu zobrazí pouze bez možnosti složení funkcí.*

## <a name="use-the-model"></a>Použití modelu

Otevřít **Program.cs** souboru, kde **hlavní** je definována metoda. Přidejte následující kód do funkce Main.

Kód volá dvě uložené procedury: **GetStudentGrades** (vrátí **StudentGrades** pro zadaný rozbočovač *StudentId*) a **GetDepartmentName** (vrátí název oddělení ve výstupní parametr).  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

Kompilace a spuštění aplikace. Program vygeneruje následující výstup:

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Výstupní parametry
-----------------

Pokud se používají výstupních parametrů, jejich hodnoty nebudou dostupné, dokud nebude mít zcela načíst výsledky. Je to z důvodu základní chování DbDataReader naleznete v tématu [načítání dat pomocí čtečky dat](http://go.microsoft.com/fwlink/?LinkID=398589) další podrobnosti.
