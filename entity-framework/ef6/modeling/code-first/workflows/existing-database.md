---
title: Kód nejprve k existující databázi - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: f05420beb3dff2d632151fcbf48986b0d9cd18ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490607"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="a5806-102">Kód nejprve ke stávající databázi</span><span class="sxs-lookup"><span data-stu-id="a5806-102">Code First to an Existing Database</span></span>
<span data-ttu-id="a5806-103">Tato videa a podrobný návod poskytuje úvod k vývoji Code First cílení na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="a5806-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="a5806-104">Kód nejprve umožňuje definovat model pomocí jazyka C\# nebo VB.Net třídy.</span><span class="sxs-lookup"><span data-stu-id="a5806-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="a5806-105">Volitelně další konfigurace je možné provádět pomocí atributů ve třídách a vlastnostech nebo s použitím rozhraní API fluent.</span><span class="sxs-lookup"><span data-stu-id="a5806-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="a5806-106">Podívejte se na video</span><span class="sxs-lookup"><span data-stu-id="a5806-106">Watch the video</span></span>
<span data-ttu-id="a5806-107">Toto video je [nyní k dispozici na webu Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="a5806-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a5806-108">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="a5806-108">Pre-Requisites</span></span>

<span data-ttu-id="a5806-109">Budete muset mít **Visual Studio 2012** nebo **Visual Studio 2013** nainstalované k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="a5806-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="a5806-110">Budete také potřebovat verze **6.1** (nebo novější) z **Entity Framework Tools for Visual Studio** nainstalované.</span><span class="sxs-lookup"><span data-stu-id="a5806-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="a5806-111">Zobrazit [získat Entity Framework](~/ef6/fundamentals/install.md) informace o instalaci nejnovější verze Entity Framework Tools.</span><span class="sxs-lookup"><span data-stu-id="a5806-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="a5806-112">1. Vytvořit existující databáze</span><span class="sxs-lookup"><span data-stu-id="a5806-112">1. Create an Existing Database</span></span>

<span data-ttu-id="a5806-113">Obvykle Pokud cílíte na existující databázi, které se už vytvořili, ale v tomto návodu budeme potřebovat vytvořit databázi pro přístup k.</span><span class="sxs-lookup"><span data-stu-id="a5806-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="a5806-114">Pojďme tedy vygenerovala databáze.</span><span class="sxs-lookup"><span data-stu-id="a5806-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="a5806-115">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5806-115">Open Visual Studio</span></span>
-   <span data-ttu-id="a5806-116">**Zobrazení –&gt; Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="a5806-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="a5806-117">Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="a5806-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="a5806-118">Pokud nejsou připojeni k databázi z **Průzkumníka serveru** předtím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="a5806-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Vyberte zdroj dat](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="a5806-120">Připojte se k instanci LocalDB a zadejte **blogovací** jako název databáze</span><span class="sxs-lookup"><span data-stu-id="a5806-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDB připojení](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="a5806-122">Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**</span><span class="sxs-lookup"><span data-stu-id="a5806-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Vytvoření dialogového okna databáze](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="a5806-124">Nové databáze se teď budou zobrazovat v Průzkumníku serveru, klikněte pravým tlačítkem myši na něj a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="a5806-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="a5806-125">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="a5806-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="a5806-126">2. Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="a5806-126">2. Create the Application</span></span>

<span data-ttu-id="a5806-127">Pro zjednodušení budeme vytvářet základní Konzolová aplikace, které používá Code First pro přístup k datům:</span><span class="sxs-lookup"><span data-stu-id="a5806-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="a5806-128">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5806-128">Open Visual Studio</span></span>
-   <span data-ttu-id="a5806-129">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="a5806-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="a5806-130">Vyberte **Windows** v levé nabídce a **konzolové aplikace**</span><span class="sxs-lookup"><span data-stu-id="a5806-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="a5806-131">Zadejte **CodeFirstExistingDatabaseSample** jako název</span><span class="sxs-lookup"><span data-stu-id="a5806-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="a5806-132">Vyberte **OK**</span><span class="sxs-lookup"><span data-stu-id="a5806-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="a5806-133">3. Zpětná analýza modelu</span><span class="sxs-lookup"><span data-stu-id="a5806-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="a5806-134">My budeme používat Entity Framework Tools for Visual Studio nám generování počátečního kódu pro mapovat do databáze.</span><span class="sxs-lookup"><span data-stu-id="a5806-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="a5806-135">Tyto nástroje jsou prostě generuje kód, který můžete také zadat ručně Pokud dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="a5806-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="a5806-136">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="a5806-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="a5806-137">Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="a5806-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="a5806-138">Zadejte **BloggingContext** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="a5806-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="a5806-139">Tím se spustí **Průvodce datovým modelem Entity**</span><span class="sxs-lookup"><span data-stu-id="a5806-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="a5806-140">Vyberte **Code First z databáze** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="a5806-140">Select **Code First from Database** and click **Next**</span></span>

    ![Průvodce jeden CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="a5806-142">Vyberte připojení k databázi vytvořené v první části a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="a5806-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Průvodce dvě CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="a5806-144">Klikněte na zaškrtávací políčko vedle položky **tabulky** importovat všechny tabulky a klikněte na tlačítko **dokončit**</span><span class="sxs-lookup"><span data-stu-id="a5806-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Průvodce tři CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="a5806-146">Po dokončení procesu zpětné analýzy, počet položek, které se byly přidány do projektu, můžeme podívejte se na co je přidán.</span><span class="sxs-lookup"><span data-stu-id="a5806-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="a5806-147">Konfigurační soubor</span><span class="sxs-lookup"><span data-stu-id="a5806-147">Configuration file</span></span>

<span data-ttu-id="a5806-148">Soubor App.config se přidala do projektu, tento soubor obsahuje připojovací řetězec k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="a5806-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="a5806-149">*Můžete si všimnout některými jinými nastaveními v konfiguračním souboru příliš, jedná se o výchozí nastavení EF, které v Code First k vytváření databází. Protože jsme v naší aplikaci jsou mapování k existující databázi tato nastavení se bude ignorovat.*</span><span class="sxs-lookup"><span data-stu-id="a5806-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="a5806-150">Odvozené kontextu</span><span class="sxs-lookup"><span data-stu-id="a5806-150">Derived Context</span></span>

<span data-ttu-id="a5806-151">A **BloggingContext** třídy je přidaný do projektu.</span><span class="sxs-lookup"><span data-stu-id="a5806-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="a5806-152">Kontext představuje relaci s databází, což nám pro dotazování a uložit data.</span><span class="sxs-lookup"><span data-stu-id="a5806-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="a5806-153">Poskytuje kontext **DbSet&lt;TEntity&gt;**  pro každý typ v náš model.</span><span class="sxs-lookup"><span data-stu-id="a5806-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="a5806-154">Uvidíte také, že volá výchozí konstruktor, konstruktor základní třídy pomocí **název =** syntaxe.</span><span class="sxs-lookup"><span data-stu-id="a5806-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="a5806-155">To říká Code First, že připojovací řetězec má použít pro tento kontext by měly být načteny z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="a5806-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="a5806-156">*Vždycky byste měli použít **název =** syntaxe při použití připojovacího řetězce v konfiguračním souboru. Tím se zajistí, že pokud se připojovací řetězec není k dispozici pak Entity Framework vyvolá místo vytvoření nové databáze podle konvence.*</span><span class="sxs-lookup"><span data-stu-id="a5806-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="a5806-157">Třídy modelu</span><span class="sxs-lookup"><span data-stu-id="a5806-157">Model classes</span></span>

<span data-ttu-id="a5806-158">A konečně **blogu** a **příspěvek** třídy také byly přidány do projektu.</span><span class="sxs-lookup"><span data-stu-id="a5806-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="a5806-159">Jedná se o domény třídy, které tvoří náš model.</span><span class="sxs-lookup"><span data-stu-id="a5806-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="a5806-160">Zobrazí se vám Data poznámky u tříd a zadejte konfiguraci kde Code First konvence by odpovídaly struktuře existující databázi.</span><span class="sxs-lookup"><span data-stu-id="a5806-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="a5806-161">Například, zobrazí se vám **StringLength** Poznámka **Blog.Name** a **Blog.Url** vzhledem k tomu, že má maximální délku **200** v databáze (Code First výchozí hodnota je určený maximální délka podporována zprostředkovatelem databáze - **nvarchar(max)** v systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="a5806-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="a5806-162">4. Čtení a zápis dat</span><span class="sxs-lookup"><span data-stu-id="a5806-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="a5806-163">Když teď máme modelu je čas ho používat pro přístup k nějaká data.</span><span class="sxs-lookup"><span data-stu-id="a5806-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="a5806-164">Implementace **hlavní** metoda ve **Program.cs** jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="a5806-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="a5806-165">Tento kód vytvoří novou instanci třídy náš kontext a použije ho k vložení nového **blogu**.</span><span class="sxs-lookup"><span data-stu-id="a5806-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="a5806-166">Potom použije LINQ dotaz pro načtení všech **blogy** z databáze, seřazené podle abecedy podle **Title**.</span><span class="sxs-lookup"><span data-stu-id="a5806-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="a5806-167">Teď můžete aplikaci spustit a otestování.</span><span class="sxs-lookup"><span data-stu-id="a5806-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="a5806-168">Co když Moje databáze změní?</span><span class="sxs-lookup"><span data-stu-id="a5806-168">What if My Database Changes?</span></span>

<span data-ttu-id="a5806-169">Code First pro Průvodce databáze je určena ke generování počáteční bod sadu tříd, které pak můžete upravit a změnit.</span><span class="sxs-lookup"><span data-stu-id="a5806-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="a5806-170">Pokud se změní schéma databáze můžete ručně upravit třídy nebo provádět jiné zpětné analýzy, přepsat třídy.</span><span class="sxs-lookup"><span data-stu-id="a5806-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="a5806-171">Pomocí migrace Code First k existující databázi</span><span class="sxs-lookup"><span data-stu-id="a5806-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="a5806-172">Pokud chcete použít migrace Code First s existující databázi, přečtěte si téma [migrace Code First k existující databázi](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="a5806-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="a5806-173">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a5806-173">Summary</span></span>

<span data-ttu-id="a5806-174">V tomto názorném postupu jsme se podívali na vývoji Code First pomocí stávající databáze.</span><span class="sxs-lookup"><span data-stu-id="a5806-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="a5806-175">Provést zpětnou analýzu sadu tříd, které namapované k databázi a může použít k ukládání a načítání dat jsme použili Entity Framework Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a5806-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
