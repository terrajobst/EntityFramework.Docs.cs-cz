---
title: Dotazy na uložené procedury v Návrháři – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182473"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="3bf32-102">Uložené procedury dotazu návrháře</span><span class="sxs-lookup"><span data-stu-id="3bf32-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="3bf32-103">Tento podrobný návod ukazuje, jak použít Entity Framework Designer (EF Designer) k importu uložených procedur do modelu a následnému volání importovaných uložených procedur pro načtení výsledků.</span><span class="sxs-lookup"><span data-stu-id="3bf32-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="3bf32-104">Všimněte si, že Code First nepodporuje mapování na uložené procedury nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="3bf32-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="3bf32-105">Uložené procedury nebo funkce však můžete volat pomocí metody System. data. entity. Negenerickými. SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="3bf32-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="3bf32-106">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3bf32-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="3bf32-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3bf32-107">Prerequisites</span></span>

<span data-ttu-id="3bf32-108">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="3bf32-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="3bf32-109">Poslední verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3bf32-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="3bf32-110">[Ukázková databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="3bf32-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3bf32-111">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="3bf32-111">Set up the Project</span></span>

-   <span data-ttu-id="3bf32-112">Otevřete Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3bf32-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="3bf32-113">Vybrat **soubor-&gt; projekt New-&gt;**</span><span class="sxs-lookup"><span data-stu-id="3bf32-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="3bf32-114">V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="3bf32-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="3bf32-115">Jako název zadejte **EFwithSProcsSample** .</span><span class="sxs-lookup"><span data-stu-id="3bf32-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="3bf32-116">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="3bf32-117">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="3bf32-117">Create a Model</span></span>

-   <span data-ttu-id="3bf32-118">Klikněte pravým tlačítkem na projekt v Průzkumník řešení a vyberte **Přidat-&gt; novou položku**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="3bf32-119">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="3bf32-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="3bf32-120">Jako název souboru zadejte **EFwithSProcsModel. edmx** a pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="3bf32-121">V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="3bf32-122">Klikněte na **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="3bf32-123">V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="3bf32-124">Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="3bf32-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="3bf32-125">V dialogovém okně zvolte objekty databáze zaškrtněte políčko **tabulky** pro výběr všech tabulek.</span><span class="sxs-lookup"><span data-stu-id="3bf32-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="3bf32-126">V uzlu **uložené procedury a funkce** vyberte také následující uložené procedury: **GetStudentGrades** a **getdepartment**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Import](~/ef6/media/import.jpg)

    <span data-ttu-id="3bf32-128">*Počínaje sadou Visual Studio 2012 Návrhář EF podporuje hromadný import uložených procedur. Ve výchozím nastavení je zaškrtnuté políčko **Importovat vybrané uložené procedury a funkce do modelu theentity** .*</span><span class="sxs-lookup"><span data-stu-id="3bf32-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="3bf32-129">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-129">Click **Finish**.</span></span>

<span data-ttu-id="3bf32-130">Ve výchozím nastavení se obrazec výsledek u každé importované uložené procedury nebo funkce, která vrací více než jeden sloupec, automaticky změní na nový komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="3bf32-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="3bf32-131">V tomto příkladu chceme namapovat výsledky **GetStudentGrades** funkce na entitu **StudentGrade** a výsledky třídy **getdepartment** na **none** (**žádná** je výchozí hodnota).</span><span class="sxs-lookup"><span data-stu-id="3bf32-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="3bf32-132">Pro import funkce, který vrátí typ entity, se sloupce vrácené odpovídající uloženou procedurou musí přesně shodovat s skalárními vlastnostmi vráceného typu entity.</span><span class="sxs-lookup"><span data-stu-id="3bf32-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="3bf32-133">Import funkce může také vracet kolekce jednoduchých typů, komplexních typů nebo žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="3bf32-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="3bf32-134">Klikněte pravým tlačítkem myši na návrhovou plochu a vyberte možnost **prohlížeč modelů**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="3bf32-135">V **prohlížeči modelů**vyberte **importy funkcí**a potom poklikejte na funkci **GetStudentGrades** .</span><span class="sxs-lookup"><span data-stu-id="3bf32-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="3bf32-136">V dialogovém okně Upravit import funkce vyberte **entity** a zvolte **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="3bf32-137">*V horní **části dialogového okna importy funkce se** zobrazí zaškrtávací políčko **importovat funkci** , které vám umožní mapovat na sestavitelované funkce. Pokud toto políčko zaškrtnete, zobrazí se v rozevíracím seznamu **název uložené procedury nebo funkce** pouze funkce s možností složení (funkce vracející tabulku). Pokud toto políčko nezaškrtnete, v seznamu se zobrazí pouze funkce bez možnosti složení.*</span><span class="sxs-lookup"><span data-stu-id="3bf32-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="3bf32-138">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="3bf32-138">Use the Model</span></span>

<span data-ttu-id="3bf32-139">Otevřete soubor **program.cs** , kde je definována metoda **Main** .</span><span class="sxs-lookup"><span data-stu-id="3bf32-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="3bf32-140">Do funkce Main přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="3bf32-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="3bf32-141">Kód volá dva uložené procedury: **GetStudentGrades** (vrátí **StudentGrades** pro zadané *StudentID*) a **getdepartment** (vrátí název oddělení v výstupním parametru).</span><span class="sxs-lookup"><span data-stu-id="3bf32-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

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

<span data-ttu-id="3bf32-142">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3bf32-142">Compile and run the application.</span></span> <span data-ttu-id="3bf32-143">Program vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="3bf32-143">The program produces the following output:</span></span>

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="3bf32-144">Výstupní parametry</span><span class="sxs-lookup"><span data-stu-id="3bf32-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="3bf32-145">Pokud se používají výstupní parametry, jejich hodnoty nebudou k dispozici, dokud nebudou výsledky zcela načteny.</span><span class="sxs-lookup"><span data-stu-id="3bf32-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="3bf32-146">Důvodem je základní chování DbDataReader. Další informace najdete v tématu [načtení dat pomocí objektu DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) .</span><span class="sxs-lookup"><span data-stu-id="3bf32-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
