---
title: Code First k nové databázi – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182571"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="0c5f3-102">Code First k nové databázi</span><span class="sxs-lookup"><span data-stu-id="0c5f3-102">Code First to a New Database</span></span>
<span data-ttu-id="0c5f3-103">Toto video a podrobný návod vám poskytnou Úvod do Code Firstho vývoje, který cílí na novou databázi.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="0c5f3-104">Tento scénář zahrnuje cílení na databázi, která neexistuje, Code First vytvoří nebo prázdnou databázi, do které Code First přidat nové tabulky.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="0c5f3-105">Code First umožňuje definovat model pomocí tříd C\# nebo VB.Net.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="0c5f3-106">Další konfiguraci můžete volitelně provést pomocí atributů u tříd a vlastností nebo pomocí rozhraní API Fluent.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="0c5f3-107">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="0c5f3-107">Watch the video</span></span>
<span data-ttu-id="0c5f3-108">Toto video poskytuje Úvod do Code First vývoje cílící na novou databázi.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="0c5f3-109">Tento scénář zahrnuje cílení na databázi, která neexistuje, Code First vytvoří nebo prázdnou databázi, do které Code First přidat nové tabulky.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="0c5f3-110">Code First umožňuje definovat model pomocí C# tříd nebo VB.NET.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="0c5f3-111">Další konfiguraci můžete volitelně provést pomocí atributů u tříd a vlastností nebo pomocí rozhraní API Fluent.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="0c5f3-112">**Prezentující**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="0c5f3-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="0c5f3-113">**Video**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="0c5f3-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="0c5f3-114">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="0c5f3-114">Pre-Requisites</span></span>

<span data-ttu-id="0c5f3-115">Abyste mohli dokončit tento návod, budete muset mít nainstalovanou aspoň Visual Studio 2010 nebo Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="0c5f3-116">Pokud používáte Visual Studio 2010, budete také muset nainstalovat [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="0c5f3-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="0c5f3-117">1. Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="0c5f3-117">1. Create the Application</span></span>

<span data-ttu-id="0c5f3-118">Aby se zajistilo něco jednoduchého, vytvoříme základní konzolovou aplikaci, která používá Code First k provádění přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="0c5f3-119">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c5f3-119">Open Visual Studio</span></span>
-   <span data-ttu-id="0c5f3-120">**Soubor –&gt; projekt New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="0c5f3-121">V levé nabídce a v **konzolové aplikaci** vyberte **Windows** .</span><span class="sxs-lookup"><span data-stu-id="0c5f3-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="0c5f3-122">Jako název zadejte **CodeFirstNewDatabaseSample** .</span><span class="sxs-lookup"><span data-stu-id="0c5f3-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="0c5f3-123">Vybrat **OK**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="0c5f3-124">2. vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="0c5f3-124">2. Create the Model</span></span>

<span data-ttu-id="0c5f3-125">Pojďme definovat velmi jednoduchý model pomocí tříd.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="0c5f3-126">Jenom jsme definovali v souboru Program.cs, ale v reálné aplikaci byste své třídy rozdělili do samostatných souborů a potenciálně samostatného projektu.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="0c5f3-127">Pod definicí třídy programu v Program.cs přidejte následující dvě třídy.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

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

<span data-ttu-id="0c5f3-128">Všimněte si, že vytváříme dvě navigační vlastnosti (blog. post a post. blog) Virtual.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="0c5f3-129">To umožňuje funkci opožděného načítání Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="0c5f3-130">Opožděné načítání znamená, že obsah těchto vlastností bude automaticky načten z databáze při pokusu o přístup k nim.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="0c5f3-131">3. vytvoření kontextu</span><span class="sxs-lookup"><span data-stu-id="0c5f3-131">3. Create a Context</span></span>

<span data-ttu-id="0c5f3-132">Nyní je čas definovat odvozený kontext, který představuje relaci s databází, a můžeme nám umožnit dotazování a ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="0c5f3-133">Definujeme kontext, který je odvozen z typu System. data. entity. DbContext a zpřístupňuje typ Negenerickými&lt;TEntity&gt; pro každou třídu v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="0c5f3-134">Teď Začínáme používat typy z Entity Framework, takže musíme přidat balíček NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="0c5f3-135">**Projekt –&gt; spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="0c5f3-136">Poznámka: Pokud nemáte **balíčky pro správu NuGet...**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="0c5f3-137">možnost, měli byste nainstalovat [nejnovější verzi nugetu](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="0c5f3-137">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="0c5f3-138">Vyberte **online** kartu.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="0c5f3-139">Výběr balíčku **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="0c5f3-140">Klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="0c5f3-140">Click **Install**</span></span>

<span data-ttu-id="0c5f3-141">Do horní části Program.cs přidejte příkaz using pro System. data. entity.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="0c5f3-142">Pod třídou post v Program.cs přidejte následující odvozený kontext.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="0c5f3-143">Tady je úplný seznam toho, co by měl teď Program.cs obsahovat.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-143">Here is a complete listing of what Program.cs should now contain.</span></span>

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

<span data-ttu-id="0c5f3-144">To je celý kód, který potřebujeme k zahájení ukládání a načítání dat.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="0c5f3-145">Zjevně se trochu děje na pozadí a my se podíváme na to chvilku, ale nejdřív to uvidíme v akci.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="0c5f3-146">4. čtení & zápisu dat</span><span class="sxs-lookup"><span data-stu-id="0c5f3-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="0c5f3-147">Implementujte metodu Main v Program.cs, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="0c5f3-148">Tento kód vytvoří novou instanci našeho kontextu a pak ho použije k vložení nového blogu.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="0c5f3-149">Pak použije dotaz LINQ k načtení všech blogů z databáze seřazené abecedně podle názvu.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="0c5f3-150">Nyní můžete spustit aplikaci a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-150">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="0c5f3-151">Kde jsou moje data?</span><span class="sxs-lookup"><span data-stu-id="0c5f3-151">Where’s My Data?</span></span>

<span data-ttu-id="0c5f3-152">Podle konvence DbContext pro vás vytvořila databázi.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="0c5f3-153">Pokud je k dispozici místní instance SQL Express (ve výchozím nastavení nainstalovaná v rámci sady Visual Studio 2010), pak Code First vytvořila databázi v této instanci.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="0c5f3-154">Pokud SQL Express není k dispozici, Code First se pokusí a použije [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (ve výchozím nastavení nainstalované ve Visual Studiu 2012).</span><span class="sxs-lookup"><span data-stu-id="0c5f3-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="0c5f3-155">Databáze je pojmenována za plně kvalifikovaný název odvozeného kontextu, v našem případě **CodeFirstNewDatabaseSample. BloggingContext.**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="0c5f3-156">Jedná se pouze o výchozí konvence a existují různé způsoby, jak změnit databázi, kterou Code First používá. Další informace jsou k dispozici ve **způsobu, jakým DbContext zjistí téma připojení modelu a databáze** .</span><span class="sxs-lookup"><span data-stu-id="0c5f3-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="0c5f3-157">K této databázi se můžete připojit pomocí Průzkumník serveru v aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="0c5f3-158">**Zobrazení-&gt; Průzkumník serveru**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="0c5f3-159">Klikněte pravým tlačítkem na **datová připojení** a vyberte **Přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="0c5f3-160">Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat Microsoft SQL Server jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="0c5f3-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Výběr zdroje dat](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="0c5f3-162">Připojte se k LocalDB nebo SQL Express v závislosti na tom, který z nich máte nainstalovanou.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="0c5f3-163">Nyní můžeme zkontrolovat schéma, které Code First vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-163">We can now inspect the schema that Code First created.</span></span>

![Počáteční schéma](~/ef6/media/schemainitial.png)

<span data-ttu-id="0c5f3-165">DbContext rozpracovali třídy, které se mají zahrnout do modelu, a Prohlédněte si vlastnosti Negenerickými, které jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="0c5f3-166">Pak používá výchozí sadu Code First konvence k určení názvů tabulek a sloupců, určování datových typů, hledání primárních klíčů atd.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="0c5f3-167">Později v tomto návodu se podíváme na to, jak můžete tyto konvence přepsat.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="0c5f3-168">5. zvládnutí změn modelu</span><span class="sxs-lookup"><span data-stu-id="0c5f3-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="0c5f3-169">Teď je čas udělat v našem modelu nějaké změny. když tyto změny provedeme, musíme také aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="0c5f3-170">K tomu slouží funkce s názvem Migrace Code First nebo migrace pro krátké.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="0c5f3-171">Migrace nám umožňuje mít seřazenou sadu kroků, které popisují postup upgradu (a downgrade) našeho schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="0c5f3-172">Každý z těchto kroků, který se označuje jako migrace, obsahuje kód, který popisuje změny, které se mají použít.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="0c5f3-173">Prvním krokem je povolení Migrace Code First pro naše BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="0c5f3-174">**Nástroje – Správce balíčků knihovny&gt; –&gt; konzolu Správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="0c5f3-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="0c5f3-175">Spuštění příkazu **Povolit – migrace** v konzole správce balíčků</span><span class="sxs-lookup"><span data-stu-id="0c5f3-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="0c5f3-176">Do projektu se přidala nová složka migrace, která obsahuje dvě položky:</span><span class="sxs-lookup"><span data-stu-id="0c5f3-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="0c5f3-177">**Configuration.cs** – tento soubor obsahuje nastavení, která migrace použije pro migraci BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="0c5f3-178">Pro tento návod nemusíte nic měnit, ale tady je místo, kde můžete zadat počáteční data, registrovat poskytovatele pro další databáze, měnit obor názvů, v němž jsou tyto migrace generovány atd.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="0c5f3-179">**&lt;časové razítko&gt;\_InitialCreate.cs** – Toto je vaše první migrace, představuje změny, které již byly pro databázi aplikovány, aby se převzaly do prázdné databáze, která obsahuje tabulky Blogy a příspěvky.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="0c5f3-180">I když umožníme Code First automaticky vytvořit tyto tabulky pro nás, teď jsme se rozhodli, že jsme se rozhodli pro migrace, které jsme převedli na migraci.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="0c5f3-181">Code First také zaznamenal v naší místní databázi, že tato migrace již byla použita.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="0c5f3-182">Časové razítko v názvu souboru se používá pro účely řazení.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="0c5f3-183">Teď provedeme změnu našeho modelu a přidat do třídy blogu vlastnost URL:</span><span class="sxs-lookup"><span data-stu-id="0c5f3-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="0c5f3-184">Spusťte příkaz **Add-Migration AddUrl** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="0c5f3-185">Příkaz pro přidání migrace kontroluje od poslední migrace změny a vytvoří novou migraci s případnými změnami, které byly nalezeny.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="0c5f3-186">Migraci můžeme pojmenovat. v tomto případě zavoláme migraci "AddUrl".</span><span class="sxs-lookup"><span data-stu-id="0c5f3-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="0c5f3-187">Generovaný kód říká, že musíme přidat sloupec URL, který může uchovávat data řetězce, do dbo. Tabulka blogů</span><span class="sxs-lookup"><span data-stu-id="0c5f3-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="0c5f3-188">V případě potřeby můžeme upravit kód vygenerovaného kódu, ale v tomto případě to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

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

-   <span data-ttu-id="0c5f3-189">Spusťte příkaz **Update-Database** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="0c5f3-190">Tento příkaz použije všechny nedokončené migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="0c5f3-191">Naše migrace InitialCreate již byla použita, takže migrace budou pouze uplatňovat naši novou migraci AddUrl.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="0c5f3-192">Tip: při volání metody Update-Database můžete použít přepínač **– verbose** a zobrazit tak SQL, který se spouští proti databázi.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="0c5f3-193">Nový sloupec adresa URL je nyní přidán do tabulky Blogy v databázi:</span><span class="sxs-lookup"><span data-stu-id="0c5f3-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Schéma s adresou URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="0c5f3-195">6. datové poznámky</span><span class="sxs-lookup"><span data-stu-id="0c5f3-195">6. Data Annotations</span></span>

<span data-ttu-id="0c5f3-196">Zatím jsme zjistili, že se v EF zjistil model pomocí svých výchozích konvencí, ale v případě, že naše třídy nedodržují konvence, je potřeba, abyste mohli provést další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="0c5f3-197">Existují dvě možnosti. Podíváme se na poznámky k datům v této části a potom rozhraní Fluent API v další části.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="0c5f3-198">Pojďme do našeho modelu přidat třídu uživatelů</span><span class="sxs-lookup"><span data-stu-id="0c5f3-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="0c5f3-199">Musíme také přidat sadu do našeho odvozeného kontextu.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="0c5f3-200">Při pokusu o přidání migrace se zobrazí chyba*s informací, že uživatel EntityType nemá definován žádný klíč. Definujte klíč pro tento typ EntityType.*</span><span class="sxs-lookup"><span data-stu-id="0c5f3-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="0c5f3-201">vzhledem k tomu, že EF nemá žádný způsob, jak poznáte, že uživatelské jméno by mělo být primární klíč uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="0c5f3-202">Nyní budeme používat poznámky k datům, takže musíme přidat příkaz using na začátek Program.cs</span><span class="sxs-lookup"><span data-stu-id="0c5f3-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="0c5f3-203">Nyní zadejte poznámku na vlastnost username, abyste identifikovali, že se jedná o primární klíč.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="0c5f3-204">Použití příkazu **Add-Migration adduser** k vytvoření uživatelského rozhraní migrace pro použití těchto změn v databázi</span><span class="sxs-lookup"><span data-stu-id="0c5f3-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="0c5f3-205">Spuštění příkazu **Update-Database** pro použití nové migrace do databáze</span><span class="sxs-lookup"><span data-stu-id="0c5f3-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="0c5f3-206">Nová tabulka je nyní přidána do databáze:</span><span class="sxs-lookup"><span data-stu-id="0c5f3-206">The new table is now added to the database:</span></span>

![Schéma s uživateli](~/ef6/media/schemawithusers.png)

<span data-ttu-id="0c5f3-208">Úplný seznam poznámek podporovaných v EF jsou:</span><span class="sxs-lookup"><span data-stu-id="0c5f3-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="0c5f3-209">Attribute – atribut</span><span class="sxs-lookup"><span data-stu-id="0c5f3-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="0c5f3-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="0c5f3-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="0c5f3-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="0c5f3-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="0c5f3-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="0c5f3-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="0c5f3-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="0c5f3-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="0c5f3-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="0c5f3-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="0c5f3-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="0c5f3-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="0c5f3-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="0c5f3-222">7. Fluent API</span><span class="sxs-lookup"><span data-stu-id="0c5f3-222">7. Fluent API</span></span>

<span data-ttu-id="0c5f3-223">V předchozí části jsme se podívali na používání datových poznámek k doplnění nebo přepsání toho, co zjistila konvence.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="0c5f3-224">Druhým způsobem, jak model nakonfigurovat, je prostřednictvím rozhraní Code First Fluent API.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="0c5f3-225">Většinu konfigurací modelů můžete provádět pomocí jednoduchých datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="0c5f3-226">Rozhraní Fluent API je pokročilejším způsobem určení konfigurace modelu, která zahrnuje všechno, co poznámky k datům můžou dělat kromě pokročilejších konfigurací, které nejsou možné u datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="0c5f3-227">Datové poznámky a rozhraní API Fluent lze použít společně.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="0c5f3-228">Pokud chcete získat přístup ke službě Fluent API, přepište metodu OnModelCreating v DbContext.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="0c5f3-229">Řekněme, že jsme chtěli Přejmenovat sloupec, na který je uživatel. DisplayName uložený, aby se zobrazil název\_.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="0c5f3-230">Přepsat metodu OnModelCreating na BloggingContext pomocí následujícího kódu</span><span class="sxs-lookup"><span data-stu-id="0c5f3-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

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

-   <span data-ttu-id="0c5f3-231">Použijte příkaz **Add-Migration ChangeDisplayName** pro vytvoření uživatelského rozhraní migrace a použijte tyto změny v databázi.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="0c5f3-232">Spusťte příkaz **Update-Database** , který použije novou migraci do databáze.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="0c5f3-233">Sloupec DisplayName se teď přejmenoval na zobrazení\_název:</span><span class="sxs-lookup"><span data-stu-id="0c5f3-233">The DisplayName column is now renamed to display\_name:</span></span>

![Schéma se přejmenováním zobrazovaného názvu](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="0c5f3-235">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0c5f3-235">Summary</span></span>

<span data-ttu-id="0c5f3-236">V tomto návodu jsme se podívali na Code First vývoj s využitím nové databáze.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="0c5f3-237">Definovali jsme model pomocí tříd a pak jsme tento model použili k vytvoření databáze a uložení a načtení dat.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="0c5f3-238">Po vytvoření databáze jsme použili Migrace Code First ke změně schématu v podobě vývoje našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="0c5f3-239">Zjistili jsme také, jak nakonfigurovat model pomocí datových poznámek a rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="0c5f3-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
