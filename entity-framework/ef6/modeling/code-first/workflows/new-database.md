---
title: Kód nejprve do nové databáze - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
caps.latest.revision: 3
ms.openlocfilehash: 75057fc73b082f4c745171ed77cddc358229a130
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912121"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="e2ad6-102">Kód nejprve do nové databáze</span><span class="sxs-lookup"><span data-stu-id="e2ad6-102">Code First to a New Database</span></span>
<span data-ttu-id="e2ad6-103">Tato videa a podrobný návod poskytuje úvod k vývoji Code First cílení na novou databázi.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="e2ad6-104">Tento scénář obsahuje cílení na databázi, která neexistuje a vytvoří Code First nebo prázdnou databázi, který Code First přidá nové tabulky příliš.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too.</span></span> <span data-ttu-id="e2ad6-105">Kód nejprve umožňuje definovat model pomocí jazyka C\# nebo VB.Net třídy.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="e2ad6-106">Další konfigurace můžete volitelně provést pomocí atributů ve třídách a vlastnostech nebo s použitím rozhraní API fluent.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="e2ad6-107">Podívejte se na video</span><span class="sxs-lookup"><span data-stu-id="e2ad6-107">Watch the video</span></span>
<span data-ttu-id="e2ad6-108">Toto video obsahuje úvod k vývoji Code First cílení na novou databázi.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="e2ad6-109">Tento scénář obsahuje cílení na databázi, která neexistuje a vytvoří Code First nebo prázdnou databázi, který Code First přidá nové tabulky příliš.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too.</span></span> <span data-ttu-id="e2ad6-110">Kód nejprve umožňuje definovat model pomocí tříd jazyka C# nebo VB.Net.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="e2ad6-111">Další konfigurace můžete volitelně provést pomocí atributů ve třídách a vlastnostech nebo s použitím rozhraní API fluent.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="e2ad6-112">**Přednášející:**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="e2ad6-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="e2ad6-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="e2ad6-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

# <a name="pre-requisites"></a><span data-ttu-id="e2ad6-114">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="e2ad6-114">Pre-Requisites</span></span>

<span data-ttu-id="e2ad6-115">Budete muset mít alespoň Visual Studio 2010 nebo Visual Studio 2012 nainstalovat k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="e2ad6-116">Pokud používáte Visual Studio 2010, musíte také mít [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) nainstalované.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="e2ad6-117">1. Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="e2ad6-117">1. Create the Application</span></span>

<span data-ttu-id="e2ad6-118">Pro zjednodušení budeme vytvářet základní Konzolová aplikace, které používá Code First pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="e2ad6-119">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2ad6-119">Open Visual Studio</span></span>
-   <span data-ttu-id="e2ad6-120">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="e2ad6-121">Vyberte **Windows** v levé nabídce a **konzolové aplikace**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="e2ad6-122">Zadejte **CodeFirstNewDatabaseSample** jako název</span><span class="sxs-lookup"><span data-stu-id="e2ad6-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="e2ad6-123">Vyberte **OK**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="e2ad6-124">2. Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="e2ad6-124">2. Create the Model</span></span>

<span data-ttu-id="e2ad6-125">Umožňuje definovat velmi jednoduchý model s použitím třídy.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="e2ad6-126">Jsme právě definujete je v souboru Program.cs, ale v reálné aplikaci tříd si by rozdělit na samostatné soubory a potenciálně samostatného projektu.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="e2ad6-127">Pod Program definice třídy v souboru Program.cs přidejte následující dvě třídy.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="e2ad6-128">Můžete si všimnout, že provádíme dvě navigační vlastnosti (Blog.Posts a Post.Blog) virtuální.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="e2ad6-129">To umožňuje funkci opožděné načtení Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="e2ad6-130">Opožděné načtení znamená, že obsah těchto vlastností se automaticky načtou z databáze při pokusu o přístup k nim.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="e2ad6-131">3. Vytvoření kontextu</span><span class="sxs-lookup"><span data-stu-id="e2ad6-131">3. Create a Context</span></span>

<span data-ttu-id="e2ad6-132">Nyní je možné definovat odvozené kontext, který reprezentuje relaci s databází, což nám pro dotazování a uložit data.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="e2ad6-133">Kontext, který je odvozen od System.Data.Entity.DbContext a zveřejňuje typu DbSet definujeme&lt;TEntity&gt; pro každou třídu v náš model.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="e2ad6-134">Nyní zapínáme využíval typy z Entity Framework, takže je potřeba přidat balíček NuGet objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="e2ad6-135">**Project –&gt; spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="e2ad6-136">Poznámka: Pokud nemáte k dispozici **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="e2ad6-137">možnost byste měli nainstalovat [nejnovější verze Nugetu](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="e2ad6-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="e2ad6-138">Vyberte **Online** kartu</span><span class="sxs-lookup"><span data-stu-id="e2ad6-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="e2ad6-139">Vyberte **EntityFramework** balíčku</span><span class="sxs-lookup"><span data-stu-id="e2ad6-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="e2ad6-140">Klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-140">Click **Install**</span></span>

<span data-ttu-id="e2ad6-141">Přidat sadu pomocí příkazu pro System.Data.Entity v horní části souboru program.cs.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="e2ad6-142">Pod příspěvkem třída v souboru Program.cs přidejte následující odvozené kontextu.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="e2ad6-143">Tady je úplný seznam všech co Program.cs teď může obsahovat.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="e2ad6-144">To je veškerý kód, který musíme začít ukládání a načítání dat.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="e2ad6-145">Samozřejmě je poměrně děje na pozadí a provedeme podívat, jestli ve chvíli, než se ale první Pojďme pozorování v akci.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="e2ad6-146">4. Čtení a zápis dat</span><span class="sxs-lookup"><span data-stu-id="e2ad6-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="e2ad6-147">V souboru Program.cs implementujte metodu Main, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="e2ad6-148">Tento kód vytvoří novou instanci třídy náš kontext a použije ho k vložení nového blogu.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="e2ad6-149">Pak používá dotaz LINQ k načtení všech blogy z databáze abecedně podle názvu.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="e2ad6-150">Teď můžete aplikaci spustit a otestování.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="e2ad6-151">Kde jsou moje Data?</span><span class="sxs-lookup"><span data-stu-id="e2ad6-151">Where’s My Data?</span></span>

<span data-ttu-id="e2ad6-152">Podle konvence DbContext vytvořila databázi za vás.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="e2ad6-153">Pokud místní instance SQL Express je k dispozici (nainstalované ve výchozím nastavení se sadou Visual Studio 2010) pak Code First vytvořil databázi na instanci</span><span class="sxs-lookup"><span data-stu-id="e2ad6-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="e2ad6-154">Pokud systém SQL Express není k dispozici, pak Code First, pokusí se použít [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (nainstalované ve výchozím nastavení pomocí sady Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="e2ad6-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="e2ad6-155">Plně kvalifikovaný název odvozené kontextu, v našem, který se má stejný název databáze **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="e2ad6-156">Jedná se o pouze výchozích konvencí a existují různé způsoby, jak změnit databázi, která používá Code First, další informace najdete v **jak DbContext zjistí Model a připojení k databázi** tématu.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="e2ad6-157">Můžete připojit k této databázi pomocí Průzkumníku serveru v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2ad6-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="e2ad6-158">**Zobrazení –&gt; Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="e2ad6-159">Klikněte pravým tlačítkem na **datová připojení** a vyberte **přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="e2ad6-160">Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat jako zdroj dat Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="e2ad6-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="e2ad6-162">Připojte se k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali</span><span class="sxs-lookup"><span data-stu-id="e2ad6-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="e2ad6-163">Nyní jsme mohli prohlédnout schématu, které Code First vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-163">We can now inspect the schema that Code First created.</span></span>

![SchemaInitial](~/ef6/media/schemainitial.png)

<span data-ttu-id="e2ad6-165">DbContext dobře fungoval jaké třídy, které zahrnují zobrazením vlastnosti DbSet, které jsme definovali v modelu.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="e2ad6-166">Potom použije výchozí sadu Code First konvence k určení názvy tabulek a sloupců, určit datové typy, najít primární klíče, atd.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="e2ad6-167">Později v tomto názorném postupu podíváme na potlačení Tato konvence.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="e2ad6-168">5. Práce se změnami modelu</span><span class="sxs-lookup"><span data-stu-id="e2ad6-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="e2ad6-169">Nyní je čas na něm provést nějaké změny náš model, když jsme provedení těchto změn je také potřeba aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="e2ad6-170">K tomu budeme používat funkci migrace Code First nebo migrace pro krátké.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="e2ad6-171">Migrace umožňuje nám umožnily seřazené sady kroků, které popisují, jak (a) naše schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="e2ad6-172">Každý z těchto kroků, označované jako migrace, obsahuje část kódu, jsou zde popsány změny, které má být použita.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="e2ad6-173">Prvním krokem je povolení migrace Code First pro naše BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="e2ad6-174">**Nástroje –&gt; Správce balíčků knihoven -&gt; Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="e2ad6-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="e2ad6-175">Spustit **povolení migrace** příkazu v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="e2ad6-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="e2ad6-176">Nová složka migrace je přidaný do projektu, který obsahuje dvě položky:</span><span class="sxs-lookup"><span data-stu-id="e2ad6-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="e2ad6-177">**Configuration.cs** – tento soubor obsahuje nastavení, které budou používat migrace pro migraci BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="e2ad6-178">Nemusíme přitom něco změnit, ale tady je, kde můžete určit obor názvů změní počáteční hodnoty data, zaregistrujte poskytovatele pro ostatní databáze, migrace se generují v atd.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="e2ad6-179">**&lt;časové razítko&gt;\_InitialCreate.cs** – Toto je první migraci, představuje změny, které již byly implementovány do databáze se nebudou prázdnou databázi, který obsahuje tabulky blogů a příspěvky .</span><span class="sxs-lookup"><span data-stu-id="e2ad6-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="e2ad6-180">I když umožňujeme Code First automaticky vytvoří tyto tabulky pro USA, teď, když jsme výslovného souhlasu pro migrace se převedl do migrace.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="e2ad6-181">Kód nejprve má také zaznamenána v místní databázi, tato migrace již byl použit.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="e2ad6-182">Časové razítko souboru se používá pro účely řazení.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="e2ad6-183">Teď můžeme změnit náš model, přidejte do třídy blogu vlastnost adresa Url:</span><span class="sxs-lookup"><span data-stu-id="e2ad6-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="e2ad6-184">Spustit **přidat migrace AddUrl** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="e2ad6-185">Příkaz Přidat migrace kontroluje změny od poslední migrace a vygeneruje uživatelské rozhraní nového migrace se všechny změny, které se nacházejí.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="e2ad6-186">Můžeme poskytnout migrace názvu; v tomto případě voláme "AddUrl" migrace.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="e2ad6-187">Automaticky generovaný kód říká, že potřebujeme přidat adresu Url sloupci, který může obsahovat data řetězce, na vlastníka. Blogy tabulky.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="e2ad6-188">V případě potřeby může upravíme automaticky generovaný kód, ale není to nutné v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="e2ad6-189">Spustit **aktualizace databáze** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="e2ad6-190">Tento příkaz použije všechny probíhající migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="e2ad6-191">Již byla použita naší InitialCreate migrace, migrace se pouze použít naše nová AddUrl migrace.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="e2ad6-192">Tip: Můžete použít **– Verbose** přepnout při volání metody Update-databázi pro zobrazení, která je spouštěna na databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="e2ad6-193">Blogy tabulky v databázi je nyní přidán nový sloupec adresy Url:</span><span class="sxs-lookup"><span data-stu-id="e2ad6-193">The new Url column is now added to the Blogs table in the database:</span></span>

![SchemaWithUrl](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="e2ad6-195">6. Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="e2ad6-195">6. Data Annotations</span></span>

<span data-ttu-id="e2ad6-196">Zatím jste pouze umožňujeme EF zjišťování modelu s použitím jeho výchozí konvence, ale že se chystáte nastat situace, kdy našich tříd není konvencím a musíme být schopni provést další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="e2ad6-197">Existují dvě možnosti pro tento účel; Podíváme se na datových poznámek v této části a pak rozhraní fluent API v další části.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="e2ad6-198">Přidejme do našeho modelu třídu uživatelů</span><span class="sxs-lookup"><span data-stu-id="e2ad6-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="e2ad6-199">Musíme také přidat sadu náš odvozené kontext</span><span class="sxs-lookup"><span data-stu-id="e2ad6-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="e2ad6-200">Pokud jsme se pokusili přidejte migraci dostali bychom o tom, že chyba "*EntityType 'Uživatele' nemá definovaný žádný klíč. Definujte klíč pro tento element EntityType."*</span><span class="sxs-lookup"><span data-stu-id="e2ad6-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="e2ad6-201">protože EF nemá žádnou možnost zjistit, že uživatelské jméno by měly být primární klíč pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="e2ad6-202">Vytvoříme nyní použít anotacemi dat, takže potřebujeme přidat, a s použitím příkazu v horní části souboru program.cs</span><span class="sxs-lookup"><span data-stu-id="e2ad6-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="e2ad6-203">Nyní opatřit poznámkami vlastnost Username k určení, že se jedná o primární klíč</span><span class="sxs-lookup"><span data-stu-id="e2ad6-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="e2ad6-204">Použití **přidat migrace AddUser** příkaz k migraci na použití těchto změn v databázi</span><span class="sxs-lookup"><span data-stu-id="e2ad6-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="e2ad6-205">Spustit **aktualizace databáze** příkaz pro použití nového migrace databáze</span><span class="sxs-lookup"><span data-stu-id="e2ad6-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="e2ad6-206">Nová tabulka se teď přidá do databáze:</span><span class="sxs-lookup"><span data-stu-id="e2ad6-206">The new table is now added to the database:</span></span>

![SchemaWithUsers](~/ef6/media/schemawithusers.png)

<span data-ttu-id="e2ad6-208">Úplný seznam podporovaných EF poznámky je:</span><span class="sxs-lookup"><span data-stu-id="e2ad6-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="e2ad6-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="e2ad6-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="e2ad6-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="e2ad6-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="e2ad6-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="e2ad6-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="e2ad6-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="e2ad6-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="e2ad6-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="e2ad6-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="e2ad6-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="e2ad6-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="e2ad6-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="e2ad6-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="e2ad6-222">7. Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="e2ad6-222">7. Fluent API</span></span>

<span data-ttu-id="e2ad6-223">V předchozí části jsme se podívali na pomocí datové poznámky doplňují nebo potlačit, co bylo zjištěno konvencí.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="e2ad6-224">Druhý způsob konfigurace modelu je přes Code First rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="e2ad6-225">Většinu konfiguraci modelu lze provést pomocí jednoduchých datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="e2ad6-226">Rozhraní fluent API je rozšířený způsob určení konfigurace modelu, která zahrnuje všechno, co se anotacemi dat způsobů, jak kromě toho některé pokročilejší konfigurace s anotacemi dat nebylo možné.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="e2ad6-227">Datové poznámky a rozhraní fluent API můžete použít společně.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="e2ad6-228">Pro přístup k rozhraní API fluent přepsat metodu OnModelCreating v DbContext.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="e2ad6-229">Řekněme, že jsme chtěli přejmenovat sloupec, který User.DisplayName je uložen v zobrazíte\_název.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="e2ad6-230">Potlačí metodu OnModelCreating na BloggingContext následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="e2ad6-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="e2ad6-231">Použití **přidat migrace ChangeDisplayName** příkaz k migraci na použití těchto změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="e2ad6-232">Spustit **aktualizace databáze** příkaz pro použití novou migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="e2ad6-233">Sloupec DisplayName přejmenovává na zobrazení\_název:</span><span class="sxs-lookup"><span data-stu-id="e2ad6-233">The DisplayName column is now renamed to display\_name:</span></span>

![SchemaWithDisplayNameRenamed](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="e2ad6-235">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e2ad6-235">Summary</span></span>

<span data-ttu-id="e2ad6-236">V tomto názorném postupu jsme se podívali na vývoji Code First pomocí nové databáze.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="e2ad6-237">Jsme definována model s použitím tříd pak používá k vytvoření databáze a ukládání a načítání dat tohoto modelu.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="e2ad6-238">Po vytvoření databáze můžeme použít ke změně schématu jako náš model se migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="e2ad6-239">Také jsme viděli, jak nakonfigurovat model s použitím datových poznámek a rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="e2ad6-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
