---
title: Funkce vracející tabulku (TVF) – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182526"
---
# <a name="table-valued-functions-tvfs"></a>Funkce vracející tabulku (TVF)
> [!NOTE]
> **EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Video a podrobný návod vám ukáže, jak pomocí Entity Framework Designer namapovat funkce vracející tabulku (TVF). Také ukazuje, jak volat TVF z dotazu LINQ.

TVF se v tuto chvíli podporují jenom v pracovním postupu Database First.

Podpora TVF byla představena ve verzi Entity Framework 5. Všimněte si, že pokud chcete použít nové funkce, jako jsou funkce vracející tabulku, výčty a prostorové typy, musíte cílit .NET Framework 4,5. Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.

TVF jsou velmi podobné uloženým procedurám s jedním klíčovým rozdílem: výsledek TVF je sestavitelný. To znamená, že výsledky z TVF lze použít v dotazu LINQ, zatímco výsledky uložené procedury nemohou.

## <a name="watch-the-video"></a>Přehrát video

**Prezentující**: Helena Kornich

[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Předpoklady

K dokončení tohoto návodu budete potřebovat:

- Nainstalujte [školní databázi](~/ef6/resources/school-database.md).

- Máte novější verzi sady Visual Studio

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřít Visual Studio
2.  V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .
3.  V levém podokně klikněte na položku **Visual C @ no__t-1**a potom vyberte šablonu **konzoly** .
4.  Jako název projektu zadejte **TVF** a klikněte na **OK** .

## <a name="add-a-tvf-to-the-database"></a>Přidání TVF do databáze

-   Vyberte **zobrazení-&gt; Průzkumník objektů systému SQL Server**
-   Pokud LocalDB není v seznamu serverů: Klikněte pravým tlačítkem na **SQL Server** a vyberte **Přidat SQL Server** pro připojení k serveru LocalDB použijte výchozí **ověřování systému Windows** .
-   Rozbalte uzel LocalDB
-   V uzlu databáze klikněte pravým tlačítkem myši na uzel školní databáze a vyberte **Nový dotaz...**
-   V editoru T-SQL vložte následující definici TVF

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

-   Klikněte pravým tlačítkem myši v editoru T-SQL a vyberte **Spustit** .
-   Funkce GetStudentGradesForCourse se přidá do školní databáze.

 

## <a name="create-a-model"></a>Vytvoření modelu

1.  Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka** .
2.  V nabídce vlevo vyberte **data** a v podokně **šablony** vyberte **ADO.NET model EDM (Entity Data Model)** .
3.  Jako název souboru zadejte **TVFModel. edmx** a pak klikněte na **Přidat** .
4.  V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další** .
5.  Klikněte na **nové připojení** ENTER **(LocalDB) \\mssqllocaldb** do textového pole název serveru zadejte **School** for název databáze klikněte na **OK** .
6.  V dialogovém okně zvolte objekty databáze pod **tabulkami** Node vyberte **osoba**, **StudentGrade**a **kurz** tables
7.  Vyberte funkci **GetStudentGradesForCourse** , která se nachází pod **uloženými procedurami a funkcemi** node poznámkami, která začíná sadou Visual Studio 2012, Entity Designer umožňuje Batch importovat uložené procedury a funkce.
8.  Klikněte na **Dokončit** .
9.  Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu. Do modelu jsou přidány všechny objekty, které jste vybrali v poli **zvolit objekty databáze** dialog.
10. Ve výchozím nastavení se tvar výsledku každé importované uložené procedury nebo funkce automaticky změní na nový komplexní typ v modelu entity. Ale chceme namapovat výsledky funkce GetStudentGradesForCourse na entitu StudentGrade: Klikněte pravým tlačítkem myši na návrhovou plochu a vyberte možnost **prohlížeč modelů** v prohlížeči modelů, vyberte **Import funkcí**a pak dvakrát klikněte na funkci **GetStudentGradesForCourse** v dialogovém okně Upravit import funkce. Vyberte **entity** . @no__t – 4and zvolit **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Zachovat a načíst data

Otevřete soubor, kde je definována metoda Main. Do funkce Main přidejte následující kód.

Následující kód ukazuje, jak vytvořit dotaz, který používá funkci vracející tabulku. Dotaz projektuje výsledky do anonymního typu, který obsahuje související název kurzu a související studenty se stupněm větším nebo rovnou 3,5.

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

Zkompilujte a spusťte aplikaci. Program vytvoří následující výstup:

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na to, jak pomocí Entity Framework Designer namapovat funkce vracející tabulku (TVF). Také ukazuje, jak volat TVF z dotazu LINQ.
