---
title: Funkce vracející tabulku (Tvf) - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: f4b8c1bd442bac67a06584bd23c040381374f303
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993244"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="7484f-102">Funkce vracející tabulku (Tvf)</span><span class="sxs-lookup"><span data-stu-id="7484f-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="7484f-103">**EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="7484f-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="7484f-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="7484f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="7484f-105">Videa a podrobný návod ukazuje, jak mapovat funkce vracející tabulku (Tvf) pomocí Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7484f-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="7484f-106">Také ukazuje, jak volat z LINQ dotaz TVF.</span><span class="sxs-lookup"><span data-stu-id="7484f-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="7484f-107">Tvf jsou v tuto chvíli podporuje jenom v databázi prvního pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="7484f-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="7484f-108">Podpora TVF byla zavedena v rozhraní Entity Framework verze 5.</span><span class="sxs-lookup"><span data-stu-id="7484f-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="7484f-109">Všimněte si, že chcete používat nové funkce, jako jsou funkce vracející tabulku, výčty a prostorové typy, které musí cílit na .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="7484f-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="7484f-110">Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="7484f-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="7484f-111">Tvf jsou velmi podobné pro uložené procedury s jedním klíčovým rozdílem: výsledek TVF je složení.</span><span class="sxs-lookup"><span data-stu-id="7484f-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="7484f-112">To znamená, že výsledky z TVF lze použít v dotazu LINQ, zatímco výsledky uložené procedury nelze.</span><span class="sxs-lookup"><span data-stu-id="7484f-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="7484f-113">Podívejte se na video</span><span class="sxs-lookup"><span data-stu-id="7484f-113">Watch the video</span></span>

<span data-ttu-id="7484f-114">**Přednášející:**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="7484f-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="7484f-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="7484f-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="7484f-116">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="7484f-116">Pre-Requisites</span></span>

<span data-ttu-id="7484f-117">K dokončení tohoto návodu, budete muset:</span><span class="sxs-lookup"><span data-stu-id="7484f-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="7484f-118">Nainstalujte [databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="7484f-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="7484f-119">Nejnovější verze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7484f-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7484f-120">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="7484f-120">Set up the Project</span></span>

1.  <span data-ttu-id="7484f-121">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7484f-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="7484f-122">Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**</span><span class="sxs-lookup"><span data-stu-id="7484f-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="7484f-123">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony</span><span class="sxs-lookup"><span data-stu-id="7484f-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="7484f-124">Zadejte **TVF** jako název projektu a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="7484f-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="7484f-125">Přidat do databáze TVF</span><span class="sxs-lookup"><span data-stu-id="7484f-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="7484f-126">Vyberte **zobrazení –&gt; Průzkumník objektů systému SQL Server**</span><span class="sxs-lookup"><span data-stu-id="7484f-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="7484f-127">Pokud není v seznamu serverů LocalDB: klikněte pravým tlačítkem na **systému SQL Server** a vyberte **přidat SQL Server** použijte výchozí **ověřování Windows** pro připojení k serveru LocalDB</span><span class="sxs-lookup"><span data-stu-id="7484f-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="7484f-128">Rozbalte uzel LocalDB</span><span class="sxs-lookup"><span data-stu-id="7484f-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="7484f-129">V uzlu databáze, klikněte pravým tlačítkem na uzel databáze školy a vyberte **nový dotaz...**</span><span class="sxs-lookup"><span data-stu-id="7484f-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="7484f-130">V editoru jazyka T-SQL, vložte následující definice funkce TVF</span><span class="sxs-lookup"><span data-stu-id="7484f-130">In T-SQL Editor, paste the following TVF definition</span></span>

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

-   <span data-ttu-id="7484f-131">Klikněte pravým tlačítkem myši na editor T-SQL a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="7484f-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="7484f-132">Funkce GetStudentGradesForCourse se přidá do databáze školy</span><span class="sxs-lookup"><span data-stu-id="7484f-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="7484f-133">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="7484f-133">Create a Model</span></span>

1.  <span data-ttu-id="7484f-134">Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**</span><span class="sxs-lookup"><span data-stu-id="7484f-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="7484f-135">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v **šablony** podokno</span><span class="sxs-lookup"><span data-stu-id="7484f-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="7484f-136">Zadejte **TVFModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="7484f-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="7484f-137">V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="7484f-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="7484f-138">Klikněte na tlačítko **nové připojení** Enter **(localdb)\\mssqllocaldb** v textu názvu serveru pole zadejte **School** databázi pojmenujte kliknutím **OK**</span><span class="sxs-lookup"><span data-stu-id="7484f-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="7484f-139">V okně Zvolte vaše databázové objekty dialogovém okně **tabulky** uzlu, vyberte **osoba**, **StudentGrade**, a **kurzu** tabulky</span><span class="sxs-lookup"><span data-stu-id="7484f-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="7484f-140">Vyberte **GetStudentGradesForCourse** funkce umístěna ve složce **uložené procedury a funkce** uzel vědomí, že spuštění pomocí sady Visual Studio 2012, v návrháři entit umožňuje import služby batch Uložené procedury a funkce</span><span class="sxs-lookup"><span data-stu-id="7484f-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="7484f-141">Klikněte na tlačítko **dokončit**</span><span class="sxs-lookup"><span data-stu-id="7484f-141">Click **Finish**</span></span>
9.  <span data-ttu-id="7484f-142">V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="7484f-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="7484f-143">Všechny objekty, které jste vybrali v **zvolte vaše databázové objekty** dialogové okno se přidají do modelu.</span><span class="sxs-lookup"><span data-stu-id="7484f-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="7484f-144">Ve výchozím nastavení bude tvar výsledku každý importovaný uloženou proceduru nebo funkci automaticky stane nový komplexní typ v modelu entity.</span><span class="sxs-lookup"><span data-stu-id="7484f-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="7484f-145">Chceme, aby k mapování výsledky funkce GetStudentGradesForCourse StudentGrade entity, ale: klikněte pravým tlačítkem na návrhové ploše a vyberte **prohlížeč modelu** v modelu prohlížeče, vyberte **importované funkce**a potom dvakrát klikněte **GetStudentGradesForCourse** funkce v upravit funkce Import dialogu **entity** a zvolte **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="7484f-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="7484f-146">Zachovat a načtení dat</span><span class="sxs-lookup"><span data-stu-id="7484f-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="7484f-147">Otevřete soubor, ve kterém je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="7484f-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="7484f-148">Přidejte následující kód do funkce Main.</span><span class="sxs-lookup"><span data-stu-id="7484f-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="7484f-149">Následující kód ukazuje, jak vytvořit dotaz, který používá funkci vracející tabulku.</span><span class="sxs-lookup"><span data-stu-id="7484f-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="7484f-150">Výsledky dotazu projekty do anonymního typu, který obsahuje související název kurzu a související studentům na podnikové úrovni větší nebo rovna hodnotě 3.5.</span><span class="sxs-lookup"><span data-stu-id="7484f-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

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

<span data-ttu-id="7484f-151">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="7484f-151">Compile and run the application.</span></span> <span data-ttu-id="7484f-152">Program vygeneruje následující výstup:</span><span class="sxs-lookup"><span data-stu-id="7484f-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="7484f-153">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7484f-153">Summary</span></span>

<span data-ttu-id="7484f-154">V tomto názorném postupu jsme se podívali na tom, jak mapovat funkce vracející tabulku (Tvf) pomocí Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7484f-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="7484f-155">To jsme si rovněž ukázali jak volat z LINQ dotaz TVF.</span><span class="sxs-lookup"><span data-stu-id="7484f-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
