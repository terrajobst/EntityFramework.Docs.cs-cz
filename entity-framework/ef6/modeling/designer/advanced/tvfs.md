---
title: Funkce vracející tabulku (Tvf) - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 6130e6bd550497509f3dcc0242077046d12c601a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489320"
---
# <a name="table-valued-functions-tvfs"></a>Funkce vracející tabulku (Tvf)
> [!NOTE]
> **EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Videa a podrobný návod ukazuje, jak mapovat funkce vracející tabulku (Tvf) pomocí Entity Framework Designer. Také ukazuje, jak volat z LINQ dotaz TVF.

Tvf jsou v tuto chvíli podporuje jenom v databázi prvního pracovního postupu.

Podpora TVF byla zavedena v rozhraní Entity Framework verze 5. Všimněte si, že chcete používat nové funkce, jako jsou funkce vracející tabulku, výčty a prostorové typy, které musí cílit na .NET Framework 4.5. Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.

Tvf jsou velmi podobné pro uložené procedury s jedním klíčovým rozdílem: výsledek TVF je složení. To znamená, že výsledky z TVF lze použít v dotazu LINQ, zatímco výsledky uložené procedury nelze.

## <a name="watch-the-video"></a>Podívejte se na video

**Přednášející:**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Předpoklady

K dokončení tohoto návodu, budete muset:

- Nainstalujte [databáze školy](~/ef6/resources/school-database.md).

- Nejnovější verze sady Visual Studio

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřít Visual Studio
2.  Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**
3.  V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony
4.  Zadejte **TVF** jako název projektu a klikněte na tlačítko **OK**

## <a name="add-a-tvf-to-the-database"></a>Přidat do databáze TVF

-   Vyberte **zobrazení –&gt; Průzkumník objektů systému SQL Server**
-   Pokud není v seznamu serverů LocalDB: klikněte pravým tlačítkem na **systému SQL Server** a vyberte **přidat SQL Server** použijte výchozí **ověřování Windows** pro připojení k serveru LocalDB
-   Rozbalte uzel LocalDB
-   V uzlu databáze, klikněte pravým tlačítkem na uzel databáze školy a vyberte **nový dotaz...**
-   V editoru jazyka T-SQL, vložte následující definice funkce TVF

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   Klikněte pravým tlačítkem myši na editor T-SQL a vyberte **spouštění**
-   Funkce GetStudentGradesForCourse se přidá do databáze školy

 

## <a name="create-a-model"></a>Vytvoření modelu

1.  Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**
2.  Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v **šablony** podokno
3.  Zadejte **TVFModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**
4.  V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **další**
5.  Klikněte na tlačítko **nové připojení** Enter **(localdb)\\mssqllocaldb** v textu názvu serveru pole zadejte **School** databázi pojmenujte kliknutím **OK**
6.  V okně Zvolte vaše databázové objekty dialogovém okně **tabulky** uzlu, vyberte **osoba**, **StudentGrade**, a **kurzu** tabulky
7.  Vyberte **GetStudentGradesForCourse** funkce umístěna ve složce **uložené procedury a funkce** uzel vědomí, že spuštění pomocí sady Visual Studio 2012, v návrháři entit umožňuje import služby batch Uložené procedury a funkce
8.  Klikněte na tlačítko **dokončit**
9.  V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí. Všechny objekty, které jste vybrali v **zvolte vaše databázové objekty** dialogové okno se přidají do modelu.
10. Ve výchozím nastavení bude tvar výsledku každý importovaný uloženou proceduru nebo funkci automaticky stane nový komplexní typ v modelu entity. Chceme, aby k mapování výsledky funkce GetStudentGradesForCourse StudentGrade entity, ale: klikněte pravým tlačítkem na návrhové ploše a vyberte **prohlížeč modelu** v modelu prohlížeče, vyberte **importované funkce**a potom dvakrát klikněte **GetStudentGradesForCourse** funkce v upravit funkce Import dialogu **entity** a zvolte **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Zachovat a načtení dat

Otevřete soubor, ve kterém je definována metoda Main. Přidejte následující kód do funkce Main.

Následující kód ukazuje, jak vytvořit dotaz, který používá funkci vracející tabulku. Výsledky dotazu projekty do anonymního typu, který obsahuje související název kurzu a související studentům na podnikové úrovni větší nebo rovna hodnotě 3.5.

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

Kompilace a spuštění aplikace. Program vygeneruje následující výstup:

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Souhrn

V tomto názorném postupu jsme se podívali na tom, jak mapovat funkce vracející tabulku (Tvf) pomocí Entity Framework Designer. To jsme si rovněž ukázali jak volat z LINQ dotaz TVF.
