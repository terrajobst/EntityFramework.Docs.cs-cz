---
title: Podpora výčtu – Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419106"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="35679-102">Podpora výčtu – Code First</span><span class="sxs-lookup"><span data-stu-id="35679-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="35679-103">**EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="35679-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="35679-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="35679-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="35679-105">Toto video a podrobný návod vám ukáže, jak používat typy výčtu s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="35679-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="35679-106">Také ukazuje, jak používat výčty v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="35679-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="35679-107">Tento návod použije Code First k vytvoření nové databáze, ale můžete také použít [Code First k namapování na existující databázi](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="35679-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="35679-108">Podpora výčtu byla představena v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="35679-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="35679-109">Chcete-li používat nové funkce, jako jsou například výčty, prostorové datové typy a funkce vracející tabulku, je nutné cílit .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="35679-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="35679-110">Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="35679-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="35679-111">V Entity Framework výčet může mít následující základní typy: **Byte**, **Int16**, **Int32**, **Int64** nebo **SByte**.</span><span class="sxs-lookup"><span data-stu-id="35679-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="35679-112">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="35679-112">Watch the video</span></span>
<span data-ttu-id="35679-113">Toto video ukazuje, jak používat typy výčtu s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="35679-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="35679-114">Také ukazuje, jak používat výčty v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="35679-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="35679-115">**Prezentující**: Helena Kornich</span><span class="sxs-lookup"><span data-stu-id="35679-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="35679-116">**Video**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="35679-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="35679-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="35679-117">Pre-Requisites</span></span>

<span data-ttu-id="35679-118">K dokončení tohoto Názorného postupu budete muset mít nainstalovanou verzi sady Visual Studio 2012, Ultimate, Premium, Professional nebo web Express.</span><span class="sxs-lookup"><span data-stu-id="35679-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="35679-119">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="35679-119">Set up the Project</span></span>

1.  <span data-ttu-id="35679-120">Otevřete Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="35679-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="35679-121">V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .</span><span class="sxs-lookup"><span data-stu-id="35679-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="35679-122">V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="35679-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="35679-123">Jako název projektu zadejte **EnumCodeFirst** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="35679-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="35679-124">Definování nového modelu pomocí Code First</span><span class="sxs-lookup"><span data-stu-id="35679-124">Define a New Model using Code First</span></span>

<span data-ttu-id="35679-125">Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model.</span><span class="sxs-lookup"><span data-stu-id="35679-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="35679-126">Následující kód definuje třídu oddělení.</span><span class="sxs-lookup"><span data-stu-id="35679-126">The code below defines the Department class.</span></span>

<span data-ttu-id="35679-127">Kód také definuje výčet DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="35679-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="35679-128">Ve výchozím nastavení je výčet typu **int** .</span><span class="sxs-lookup"><span data-stu-id="35679-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="35679-129">Vlastnost Name třídy oddělení je typu DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="35679-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="35679-130">Otevřete soubor Program.cs a vložte následující definice tříd.</span><span class="sxs-lookup"><span data-stu-id="35679-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="35679-131">Definice DbContext odvozeného typu</span><span class="sxs-lookup"><span data-stu-id="35679-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="35679-132">Kromě definování entit musíte definovat třídu, která je odvozena z DbContext a zpřístupňuje Negenerickými&lt;vlastnosti TEntity&gt;.</span><span class="sxs-lookup"><span data-stu-id="35679-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="35679-133">Vlastnosti Negenerickými&lt;TEntity&gt; umožňují kontextu zjistit, které typy chcete do modelu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="35679-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="35679-134">Instance DbContext odvozeného typu spravuje objekty entit za běhu, což zahrnuje vyplnění objektů daty z databáze, sledování změn a uchování dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="35679-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="35679-135">Typy DbContext a Negenerickými jsou definovány v sestavení EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="35679-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="35679-136">Do této knihovny DLL přidáme odkaz pomocí balíčku NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="35679-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="35679-137">V Průzkumník řešení klikněte pravým tlačítkem myši na název projektu.</span><span class="sxs-lookup"><span data-stu-id="35679-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="35679-138">Vyberte **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="35679-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="35679-139">V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="35679-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="35679-140">Klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="35679-140">Click **Install**</span></span>

<span data-ttu-id="35679-141">Všimněte si, že kromě sestavení EntityFramework jsou přidány také odkazy na System. ComponentModel. DataAnnotations a System. data. entity.</span><span class="sxs-lookup"><span data-stu-id="35679-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="35679-142">V horní části souboru Program.cs přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="35679-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="35679-143">Do Program.cs přidejte definici kontextu.</span><span class="sxs-lookup"><span data-stu-id="35679-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="35679-144">Zachovat a načíst data</span><span class="sxs-lookup"><span data-stu-id="35679-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="35679-145">Otevřete soubor Program.cs, kde je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="35679-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="35679-146">Do funkce Main přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="35679-146">Add the following code into the Main function.</span></span> <span data-ttu-id="35679-147">Kód přidá do kontextu nový objekt oddělení.</span><span class="sxs-lookup"><span data-stu-id="35679-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="35679-148">Pak data uloží.</span><span class="sxs-lookup"><span data-stu-id="35679-148">It then saves the data.</span></span> <span data-ttu-id="35679-149">Kód také spustí dotaz LINQ, který vrací oddělení, kde název je DepartmentNames. English.</span><span class="sxs-lookup"><span data-stu-id="35679-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="35679-150">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="35679-150">Compile and run the application.</span></span> <span data-ttu-id="35679-151">Program vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="35679-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="35679-152">Zobrazení vygenerované databáze</span><span class="sxs-lookup"><span data-stu-id="35679-152">View the Generated Database</span></span>

<span data-ttu-id="35679-153">Při prvním spuštění aplikace Entity Framework vytvoří pro vás databázi.</span><span class="sxs-lookup"><span data-stu-id="35679-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="35679-154">Vzhledem k tomu, že jsme nainstalovali Visual Studio 2012, databáze se vytvoří v instanci LocalDB.</span><span class="sxs-lookup"><span data-stu-id="35679-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="35679-155">Ve výchozím nastavení Entity Framework pojmenuje databázi za plně kvalifikovaným názvem odvozeného kontextu (v tomto příkladu, který je **EnumCodeFirst. EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="35679-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="35679-156">Následující časy budou použity při dalším použití existující databáze.</span><span class="sxs-lookup"><span data-stu-id="35679-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="35679-157">Všimněte si, že pokud provedete změny modelu po vytvoření databáze, měli byste použít Migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="35679-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="35679-158">Příklad použití migrace naleznete v tématu [Code First do nové databáze](~/ef6/modeling/code-first/workflows/new-database.md) .</span><span class="sxs-lookup"><span data-stu-id="35679-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="35679-159">Chcete-li zobrazit databázi a data, postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="35679-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="35679-160">V hlavní nabídce sady Visual Studio 2012 vyberte možnost **zobrazit** -&gt; **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="35679-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="35679-161">Pokud LocalDB není v seznamu serverů, klikněte pravým tlačítkem myši na **SQL Server** a vyberte **Přidat SQL Server** pro připojení k instanci LocalDB použít výchozí **ověřování systému Windows** .</span><span class="sxs-lookup"><span data-stu-id="35679-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="35679-162">Rozbalte uzel LocalDB</span><span class="sxs-lookup"><span data-stu-id="35679-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="35679-163">Rozložte složku **databáze** , aby se zobrazila nová databáze, a přejděte na poznámku k tabulce **oddělení** , která Code First nevytváří tabulku, která se mapuje na typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="35679-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="35679-164">Chcete-li zobrazit data, klikněte pravým tlačítkem myši na tabulku a vyberte možnost **Zobrazit data.**</span><span class="sxs-lookup"><span data-stu-id="35679-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="35679-165">Souhrn</span><span class="sxs-lookup"><span data-stu-id="35679-165">Summary</span></span>

<span data-ttu-id="35679-166">V tomto návodu jsme se podívali na to, jak používat typy výčtu s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="35679-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
