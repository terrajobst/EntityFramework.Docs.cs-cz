---
title: Dotazy na uložené procedury v Návrháři – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418390"
---
# <a name="designer-query-stored-procedures"></a>Uložené procedury dotazu návrháře
Tento podrobný návod ukazuje, jak použít Entity Framework Designer (EF Designer) k importu uložených procedur do modelu a následnému volání importovaných uložených procedur pro načtení výsledků. 

Všimněte si, že Code First nepodporuje mapování na uložené procedury nebo funkce. Uložené procedury nebo funkce však můžete volat pomocí metody System. data. entity. Negenerickými. SqlQuery. Příklad:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Předpoklady

K dokončení toho návodu budete potřebovat:

- Poslední verze sady Visual Studio.
- [Ukázková databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

-   Otevřete Visual Studio 2012.
-   Vybrat **soubor-&gt; projekt New-&gt;**
-   V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .
-   Jako název zadejte **EFwithSProcsSample** .
-   Vyberte **OK**.

## <a name="create-a-model"></a>Vytvoření modelu

-   Klikněte pravým tlačítkem na projekt v Průzkumník řešení a vyberte **Přidat-&gt; novou položku**.
-   V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
-   Jako název souboru zadejte **EFwithSProcsModel. edmx** a pak klikněte na **Přidat**.
-   V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.
-   Klikněte na **nové připojení**.  
    V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.  
    Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.
-   V dialogovém okně zvolte objekty databáze zaškrtněte políčko **tabulky** pro výběr všech tabulek.  
    V uzlu **uložené procedury a funkce** vyberte také následující uložené procedury: **GetStudentGrades** a **getdepartment**. 

    ![Import](~/ef6/media/import.jpg)

    *Počínaje sadou Visual Studio 2012 Návrhář EF podporuje hromadný import uložených procedur. Ve výchozím nastavení je zaškrtnuté políčko **Importovat vybrané uložené procedury a funkce do modelu theentity** .*
-   Klikněte na tlačítko **Dokončit**.

Ve výchozím nastavení se obrazec výsledek u každé importované uložené procedury nebo funkce, která vrací více než jeden sloupec, automaticky změní na nový komplexní typ. V tomto příkladu chceme namapovat výsledky **GetStudentGrades** funkce na entitu **StudentGrade** a výsledky třídy **getdepartment** na **none** (**žádná** je výchozí hodnota).

Pro import funkce, který vrátí typ entity, se sloupce vrácené odpovídající uloženou procedurou musí přesně shodovat s skalárními vlastnostmi vráceného typu entity. Import funkce může také vracet kolekce jednoduchých typů, komplexních typů nebo žádná hodnota.

-   Klikněte pravým tlačítkem myši na návrhovou plochu a vyberte možnost **prohlížeč modelů**.
-   V **prohlížeči modelů**vyberte **importy funkcí**a potom poklikejte na funkci **GetStudentGrades** .
-   V dialogovém okně Upravit import funkce vyberte **entity** a zvolte **StudentGrade**.  
    *V horní **části dialogového okna importy funkce se** zobrazí zaškrtávací políčko **importovat funkci** , které vám umožní mapovat na sestavitelované funkce. Pokud toto políčko zaškrtnete, zobrazí se v rozevíracím seznamu **název uložené procedury nebo funkce** pouze funkce s možností složení (funkce vracející tabulku). Pokud toto políčko nezaškrtnete, v seznamu se zobrazí pouze funkce bez možnosti složení.*

## <a name="use-the-model"></a>Použití modelu

Otevřete soubor **program.cs** , kde je definována metoda **Main** . Do funkce Main přidejte následující kód.

Kód volá dva uložené procedury: **GetStudentGrades** (vrátí **StudentGrades** pro zadané *StudentID*) a **getdepartment** (vrátí název oddělení v výstupním parametru).  

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

Zkompilujte a spusťte aplikaci. Program vytvoří následující výstup:

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Výstupní parametry
-----------------

Pokud se používají výstupní parametry, jejich hodnoty nebudou k dispozici, dokud nebudou výsledky zcela načteny. Důvodem je základní chování DbDataReader. Další informace najdete v tématu [načtení dat pomocí objektu DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) .
