---
title: Funkce vracející tabulku (TVF) – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418695"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="8f00f-102">Funkce vracející tabulku (TVF)</span><span class="sxs-lookup"><span data-stu-id="8f00f-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="8f00f-103">**EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="8f00f-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="8f00f-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="8f00f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="8f00f-105">Video a podrobný návod vám ukáže, jak pomocí Entity Framework Designer namapovat funkce vracející tabulku (TVF).</span><span class="sxs-lookup"><span data-stu-id="8f00f-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="8f00f-106">Také ukazuje, jak volat TVF z dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="8f00f-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="8f00f-107">TVF se v tuto chvíli podporují jenom v pracovním postupu Database First.</span><span class="sxs-lookup"><span data-stu-id="8f00f-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="8f00f-108">Podpora TVF byla představena ve verzi Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="8f00f-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="8f00f-109">Všimněte si, že pokud chcete použít nové funkce, jako jsou funkce vracející tabulku, výčty a prostorové typy, musíte cílit .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="8f00f-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="8f00f-110">Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8f00f-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="8f00f-111">TVF jsou velmi podobné uloženým procedurám s jedním klíčovým rozdílem: výsledek TVF je sestavitelný.</span><span class="sxs-lookup"><span data-stu-id="8f00f-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="8f00f-112">To znamená, že výsledky z TVF lze použít v dotazu LINQ, zatímco výsledky uložené procedury nemohou.</span><span class="sxs-lookup"><span data-stu-id="8f00f-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="8f00f-113">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="8f00f-113">Watch the video</span></span>

<span data-ttu-id="8f00f-114">**Prezentující**: Helena Kornich</span><span class="sxs-lookup"><span data-stu-id="8f00f-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="8f00f-115">[Wmv](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="8f00f-115">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="8f00f-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8f00f-116">Pre-Requisites</span></span>

<span data-ttu-id="8f00f-117">K dokončení tohoto návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="8f00f-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="8f00f-118">Nainstalujte [školní databázi](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="8f00f-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="8f00f-119">Máte novější verzi sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8f00f-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="8f00f-120">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="8f00f-120">Set up the Project</span></span>

1.  <span data-ttu-id="8f00f-121">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f00f-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="8f00f-122">V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="8f00f-123">V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="8f00f-124">Jako název projektu zadejte **TVF** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="8f00f-125">Přidání TVF do databáze</span><span class="sxs-lookup"><span data-stu-id="8f00f-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="8f00f-126">Vybrat **zobrazení-&gt; Průzkumník objektů systému SQL Server**</span><span class="sxs-lookup"><span data-stu-id="8f00f-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="8f00f-127">Pokud LocalDB není v seznamu serverů: klikněte pravým tlačítkem na **SQL Server** a vyberte **Přidat SQL Server** pro připojení k serveru LocalDB použijte výchozí **ověřování systému Windows** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="8f00f-128">Rozbalte uzel LocalDB</span><span class="sxs-lookup"><span data-stu-id="8f00f-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="8f00f-129">V uzlu databáze klikněte pravým tlačítkem myši na uzel školní databáze a vyberte **Nový dotaz...**</span><span class="sxs-lookup"><span data-stu-id="8f00f-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="8f00f-130">V editoru T-SQL vložte následující definici TVF</span><span class="sxs-lookup"><span data-stu-id="8f00f-130">In T-SQL Editor, paste the following TVF definition</span></span>

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

-   <span data-ttu-id="8f00f-131">Klikněte pravým tlačítkem myši v editoru T-SQL a vyberte **Spustit** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="8f00f-132">Funkce GetStudentGradesForCourse se přidá do školní databáze.</span><span class="sxs-lookup"><span data-stu-id="8f00f-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="8f00f-133">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="8f00f-133">Create a Model</span></span>

1.  <span data-ttu-id="8f00f-134">Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="8f00f-135">V nabídce vlevo vyberte **data** a v podokně **šablony** vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="8f00f-136">Jako název souboru zadejte **TVFModel. edmx** a pak klikněte na **Přidat** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="8f00f-137">V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="8f00f-138">Klikněte na **nové připojení** ENTER **(LocalDB)\\mssqllocaldb** v textovém poli název serveru zadejte **School** pro název databáze klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="8f00f-139">V dialogovém okně zvolte objekty databáze pod uzlem  **tabulky** vyberte tabulky **Person**, **StudentGrade**a **Course** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="8f00f-140">Vyberte funkci **GetStudentGradesForCourse** nacházející se pod **uloženými procedurami a funkcemi** poznámkami k uzlu, která začíná sadou Visual Studio 2012, Entity Designer umožňuje Batch importovat uložené procedury a funkce.</span><span class="sxs-lookup"><span data-stu-id="8f00f-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="8f00f-141">Klikněte na **Dokončit** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-141">Click **Finish**</span></span>
9.  <span data-ttu-id="8f00f-142">Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="8f00f-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="8f00f-143">Do modelu jsou přidány všechny objekty, které jste vybrali v dialogovém okně **zvolit objekty databáze** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="8f00f-144">Ve výchozím nastavení se tvar výsledku každé importované uložené procedury nebo funkce automaticky změní na nový komplexní typ v modelu entity.</span><span class="sxs-lookup"><span data-stu-id="8f00f-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="8f00f-145">Ale chceme namapovat výsledky GetStudentGradesForCourse funkce na entitu StudentGrade: klikněte pravým tlačítkem myši na návrhovou plochu a vyberte možnost **prohlížeč modelů** v prohlížeči modelů, vyberte **Import funkcí**a potom poklikejte na funkci **GetStudentGradesForCourse** v dialogovém okně upravit import funkce, vyberte **entity** a zvolte **StudentGrade** .</span><span class="sxs-lookup"><span data-stu-id="8f00f-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="8f00f-146">Zachovat a načíst data</span><span class="sxs-lookup"><span data-stu-id="8f00f-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="8f00f-147">Otevřete soubor, kde je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="8f00f-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="8f00f-148">Do funkce Main přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="8f00f-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="8f00f-149">Následující kód ukazuje, jak vytvořit dotaz, který používá funkci vracející tabulku.</span><span class="sxs-lookup"><span data-stu-id="8f00f-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="8f00f-150">Dotaz projektuje výsledky do anonymního typu, který obsahuje související název kurzu a související studenty se stupněm větším nebo rovnou 3,5.</span><span class="sxs-lookup"><span data-stu-id="8f00f-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

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

<span data-ttu-id="8f00f-151">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8f00f-151">Compile and run the application.</span></span> <span data-ttu-id="8f00f-152">Program vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="8f00f-152">The program produces the following output:</span></span>

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="8f00f-153">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8f00f-153">Summary</span></span>

<span data-ttu-id="8f00f-154">V tomto návodu jsme se podívali na to, jak pomocí Entity Framework Designer namapovat funkce vracející tabulku (TVF).</span><span class="sxs-lookup"><span data-stu-id="8f00f-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="8f00f-155">Také ukazuje, jak volat TVF z dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="8f00f-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
