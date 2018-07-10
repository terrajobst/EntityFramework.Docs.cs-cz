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
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="f4e11-102">Návrhář dotazu uložené procedury</span><span class="sxs-lookup"><span data-stu-id="f4e11-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="f4e11-103">Tento podrobný návod ukazují, jak použít Návrhář Entity Framework (EF designeru) k importu uložené procedury do modelu a poté zavolejte importované uložené procedury k načtení výsledků.</span><span class="sxs-lookup"><span data-stu-id="f4e11-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="f4e11-104">Všimněte si, že Code First nepodporuje mapování uložené procedury nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="f4e11-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="f4e11-105">Pomocí metody System.Data.Entity.DbSet.SqlQuery však můžete volat uložené procedury nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="f4e11-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="f4e11-106">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f4e11-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="f4e11-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f4e11-107">Prerequisites</span></span>

<span data-ttu-id="f4e11-108">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="f4e11-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="f4e11-109">Nejnovější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4e11-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="f4e11-110">[Ukázkové databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="f4e11-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="f4e11-111">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="f4e11-111">Set up the Project</span></span>

-   <span data-ttu-id="f4e11-112">Otevřít Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f4e11-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="f4e11-113">Vyberte **souboru -&gt; nové –&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="f4e11-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="f4e11-114">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony.</span><span class="sxs-lookup"><span data-stu-id="f4e11-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="f4e11-115">Zadejte **EFwithSProcsSample** jako název.</span><span class="sxs-lookup"><span data-stu-id="f4e11-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="f4e11-116">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="f4e11-117">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="f4e11-117">Create a Model</span></span>

-   <span data-ttu-id="f4e11-118">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **Add -&gt; nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="f4e11-119">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.</span><span class="sxs-lookup"><span data-stu-id="f4e11-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="f4e11-120">Zadejte **EFwithSProcsModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="f4e11-121">V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="f4e11-122">Klikněte na tlačítko **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="f4e11-123">V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="f4e11-124">Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.</span><span class="sxs-lookup"><span data-stu-id="f4e11-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="f4e11-125">V dialogovém okně Zvolte vaše databázové objekty, zkontrolujte, **tabulky** zaškrtávací políčko, chcete-li vybrat všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="f4e11-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="f4e11-126">Také vybrat následující uložené procedury v části **uložené procedury a funkce** uzlu: **GetStudentGrades** a **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Importovat](~/ef6/media/import.jpg)

    <span data-ttu-id="f4e11-128">*Od verze Visual Studio 2012 EF designeru podporuje hromadný import uložených procedur. **Importovat vybrané uložených procedur a funkcí do modelu theentity** je ve výchozím nastavení zaškrtnuto.*</span><span class="sxs-lookup"><span data-stu-id="f4e11-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="f4e11-129">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-129">Click **Finish**.</span></span>

<span data-ttu-id="f4e11-130">Ve výchozím nastavení bude tvar výsledek každého importované uloženou proceduru nebo funkci, která vrátí více než jeden sloupec automaticky stane nový komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="f4e11-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="f4e11-131">V tomto příkladu chceme mapování výsledky **GetStudentGrades** funkce **StudentGrade** entity a výsledky **GetDepartmentName** k **žádný** (**žádný** je výchozí hodnota).</span><span class="sxs-lookup"><span data-stu-id="f4e11-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="f4e11-132">Pro importované funkce vrátit typ entity sloupců vrácený odpovídající uložené procedury musí přesně odpovídat Skalární vlastnosti typu vrácenou entitu.</span><span class="sxs-lookup"><span data-stu-id="f4e11-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="f4e11-133">Importovaná funkce může vrátit také kolekce jednoduché typy, komplexních typů nebo žádnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f4e11-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="f4e11-134">Klikněte pravým tlačítkem na návrhové ploše a vyberte **prohlížeč modelu**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="f4e11-135">V **prohlížeč modelu**vyberte **importované funkce**a potom dvakrát klikněte **GetStudentGrades** funkce.</span><span class="sxs-lookup"><span data-stu-id="f4e11-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="f4e11-136">V dialogovém okně upravit importované funkce vyberte **entity** a zvolte **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="f4e11-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="f4e11-137">***Importované funkce je sestavitelné** zaškrtávacího políčka v horní části **importované funkce** dialogové okno vám umožní mapovat složení funkcí. Pokud zaškrtnete toto políčko, zobrazí se pouze sestavitelný funkce (funkce vracející tabulku) v **uložená procedura nebo název funkce** rozevíracího seznamu. Pokud toto políčko nezaškrtávejte, v seznamu zobrazí pouze bez možnosti složení funkcí.*</span><span class="sxs-lookup"><span data-stu-id="f4e11-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="f4e11-138">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="f4e11-138">Use the Model</span></span>

<span data-ttu-id="f4e11-139">Otevřít **Program.cs** souboru, kde **hlavní** je definována metoda.</span><span class="sxs-lookup"><span data-stu-id="f4e11-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="f4e11-140">Přidejte následující kód do funkce Main.</span><span class="sxs-lookup"><span data-stu-id="f4e11-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="f4e11-141">Kód volá dvě uložené procedury: **GetStudentGrades** (vrátí **StudentGrades** pro zadaný rozbočovač *StudentId*) a **GetDepartmentName** (vrátí název oddělení ve výstupní parametr).</span><span class="sxs-lookup"><span data-stu-id="f4e11-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

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

<span data-ttu-id="f4e11-142">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4e11-142">Compile and run the application.</span></span> <span data-ttu-id="f4e11-143">Program vygeneruje následující výstup:</span><span class="sxs-lookup"><span data-stu-id="f4e11-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="f4e11-144">Výstupní parametry</span><span class="sxs-lookup"><span data-stu-id="f4e11-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="f4e11-145">Pokud se používají výstupních parametrů, jejich hodnoty nebudou dostupné, dokud nebude mít zcela načíst výsledky.</span><span class="sxs-lookup"><span data-stu-id="f4e11-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="f4e11-146">Je to z důvodu základní chování DbDataReader naleznete v tématu [načítání dat pomocí čtečky dat](http://go.microsoft.com/fwlink/?LinkID=398589) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f4e11-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
