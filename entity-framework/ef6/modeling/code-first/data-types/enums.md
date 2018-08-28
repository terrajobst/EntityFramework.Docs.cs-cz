---
title: Podpora výčtu - Code First – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 529370ef7aba3975a36cbfdf5c439ee87b563c7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997231"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="d563d-102">Podpora výčtu – kód nejprve</span><span class="sxs-lookup"><span data-stu-id="d563d-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="d563d-103">**EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="d563d-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="d563d-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="d563d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="d563d-105">Tato videa a podrobný návod ukazuje, jak používat typy výčtu s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="d563d-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="d563d-106">Také ukazuje, jak používat výčty v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="d563d-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="d563d-107">Tento názorný postup použije Code First k vytvoření nové databáze, ale můžete také použít [Code First pro mapování na existující databázi](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="d563d-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="d563d-108">Podpora výčtu byla zavedena v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="d563d-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="d563d-109">Pokud chcete použít nové funkce jako výčty prostorové datové typy a funkce vracející tabulku, je potřeba cílit rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="d563d-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="d563d-110">Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d563d-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="d563d-111">V rozhraní Entity Framework výčet může mít následující základní typy: **bajtů**, **Int16**, **Int32**, **Int64** , nebo **SByte**.</span><span class="sxs-lookup"><span data-stu-id="d563d-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="d563d-112">Podívejte se na video</span><span class="sxs-lookup"><span data-stu-id="d563d-112">Watch the video</span></span>
<span data-ttu-id="d563d-113">Toto video ukazuje, jak používat typy výčtu s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="d563d-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="d563d-114">Také ukazuje, jak používat výčty v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="d563d-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="d563d-115">**Přednášející:**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="d563d-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="d563d-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="d563d-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d563d-117">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="d563d-117">Pre-Requisites</span></span>

<span data-ttu-id="d563d-118">Musíte mít Visual Studio 2012, Ultimate, Premium, Professional nebo Web Express edition nainstalované k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="d563d-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="d563d-119">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="d563d-119">Set up the Project</span></span>

1.  <span data-ttu-id="d563d-120">Otevřít Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d563d-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="d563d-121">Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**</span><span class="sxs-lookup"><span data-stu-id="d563d-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="d563d-122">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony</span><span class="sxs-lookup"><span data-stu-id="d563d-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="d563d-123">Zadejte **EnumCodeFirst** jako název projektu a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="d563d-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="d563d-124">Definovat nový Model využívající Code First</span><span class="sxs-lookup"><span data-stu-id="d563d-124">Define a New Model using Code First</span></span>

<span data-ttu-id="d563d-125">Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény).</span><span class="sxs-lookup"><span data-stu-id="d563d-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="d563d-126">Následující kód definuje třídu oddělení.</span><span class="sxs-lookup"><span data-stu-id="d563d-126">The code below defines the Department class.</span></span>

<span data-ttu-id="d563d-127">Kód také definuje výčet DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="d563d-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="d563d-128">Ve výchozím nastavení, je výčet **int** typu.</span><span class="sxs-lookup"><span data-stu-id="d563d-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="d563d-129">Vlastnost Name u třídy oddělení je DepartmentNames typu.</span><span class="sxs-lookup"><span data-stu-id="d563d-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="d563d-130">Otevřete soubor Program.cs a vložte následující definice tříd.</span><span class="sxs-lookup"><span data-stu-id="d563d-130">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="d563d-131">Definování uvolněn objekt DbContext odvozený typ</span><span class="sxs-lookup"><span data-stu-id="d563d-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="d563d-132">Kromě definování entit, budete muset definovat třídu, která je odvozena od položky DbContext a zveřejňuje DbSet&lt;TEntity&gt; vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d563d-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="d563d-133">DbSet&lt;TEntity&gt; vlastnosti umožňují kontextu vědět, jaké typy, které chcete zahrnout do modelu.</span><span class="sxs-lookup"><span data-stu-id="d563d-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="d563d-134">Instance typu DbContext odvozené spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, změňte sledování a zachovává data do databáze.</span><span class="sxs-lookup"><span data-stu-id="d563d-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="d563d-135">Kontext databáze a DbSet typy jsou definovány v sestavení objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="d563d-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="d563d-136">Přidáme odkaz na tuto knihovnu DLL pomocí balíčku NuGet objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="d563d-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="d563d-137">V Průzkumníku řešení klikněte pravým tlačítkem na název projektu.</span><span class="sxs-lookup"><span data-stu-id="d563d-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="d563d-138">Vyberte **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="d563d-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="d563d-139">V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku.</span><span class="sxs-lookup"><span data-stu-id="d563d-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="d563d-140">Klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="d563d-140">Click **Install**</span></span>

<span data-ttu-id="d563d-141">Všimněte si, že kromě sestavení EntityFramework System.ComponentModel.DataAnnotations a System.Data.Entity sestavení jsou přidány odkazy na také.</span><span class="sxs-lookup"><span data-stu-id="d563d-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="d563d-142">V horní části souboru Program.cs přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="d563d-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="d563d-143">V souboru Program.cs přidejte definici kontextu.</span><span class="sxs-lookup"><span data-stu-id="d563d-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="d563d-144">Zachovat a načtení dat</span><span class="sxs-lookup"><span data-stu-id="d563d-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="d563d-145">Otevřete soubor Program.cs, ve kterém je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="d563d-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="d563d-146">Přidejte následující kód do funkce Main.</span><span class="sxs-lookup"><span data-stu-id="d563d-146">Add the following code into the Main function.</span></span> <span data-ttu-id="d563d-147">Kód přidá nový objekt oddělení do kontextu.</span><span class="sxs-lookup"><span data-stu-id="d563d-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="d563d-148">Pak uloží data.</span><span class="sxs-lookup"><span data-stu-id="d563d-148">It then saves the data.</span></span> <span data-ttu-id="d563d-149">Kód také spustí dotaz LINQ, který vrátí oddělení, kde název je DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="d563d-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="d563d-150">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d563d-150">Compile and run the application.</span></span> <span data-ttu-id="d563d-151">Program vygeneruje následující výstup:</span><span class="sxs-lookup"><span data-stu-id="d563d-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="d563d-152">Zobrazí se vygenerovaný databáze</span><span class="sxs-lookup"><span data-stu-id="d563d-152">View the Generated Database</span></span>

<span data-ttu-id="d563d-153">Když spustíte aplikaci poprvé, Entity Framework vytvoří databázi za vás.</span><span class="sxs-lookup"><span data-stu-id="d563d-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="d563d-154">Když máme nainstalované Visual Studio 2012, vytvoří se databáze na instanci LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d563d-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="d563d-155">Ve výchozím nastavení, Entity Framework pojmenuje databázi za plně kvalifikovaný název odvozené kontextu (v tomto příkladu, který je **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="d563d-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="d563d-156">Následující časy, které se použije existující databázi.</span><span class="sxs-lookup"><span data-stu-id="d563d-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="d563d-157">Všimněte si, že pokud provedete změny modelu po vytvoření databáze, by měl použít migrace Code First aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="d563d-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="d563d-158">Zobrazit [Code First pro novou databázi](~/ef6/modeling/code-first/workflows/new-database.md) pro příklad použití migrace.</span><span class="sxs-lookup"><span data-stu-id="d563d-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="d563d-159">Chcete-li zobrazit databáze a dat, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d563d-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="d563d-160">V hlavní nabídce sady Visual Studio 2012 vyberte **zobrazení**  - &gt; **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="d563d-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="d563d-161">Pokud LocalDB se nenachází v seznamu serverů, klikněte pravým tlačítkem myši na **systému SQL Server** a vyberte **přidat SQL Server** použijte výchozí **ověřování Windows** pro připojení k Instanci LocalDB</span><span class="sxs-lookup"><span data-stu-id="d563d-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="d563d-162">Rozbalte uzel LocalDB</span><span class="sxs-lookup"><span data-stu-id="d563d-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="d563d-163">Rozbalit **databází** složky a zobrazí novou databázi procházením **oddělení** tabulky Poznámka Code First nevytvoří tabulku, která se mapuje na typ výčtu</span><span class="sxs-lookup"><span data-stu-id="d563d-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="d563d-164">K zobrazení dat, klikněte pravým tlačítkem na tabulku a vyberte **zobrazení dat**</span><span class="sxs-lookup"><span data-stu-id="d563d-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="d563d-165">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d563d-165">Summary</span></span>

<span data-ttu-id="d563d-166">V tomto názorném postupu jsme se podívali na tom, jak používat typy výčtu s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="d563d-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
