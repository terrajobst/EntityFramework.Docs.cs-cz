---
title: Code First existující databáze – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 0a51f826422d7e2bff33b968605eace1e754c425
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418873"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="25df8-102">Code First do existující databáze</span><span class="sxs-lookup"><span data-stu-id="25df8-102">Code First to an Existing Database</span></span>
<span data-ttu-id="25df8-103">Toto video a podrobný návod vám poskytnou Úvod do Code First vývoje cílící na stávající databázi.</span><span class="sxs-lookup"><span data-stu-id="25df8-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="25df8-104">Code First umožňuje definovat model pomocí tříd C\# nebo VB.Net.</span><span class="sxs-lookup"><span data-stu-id="25df8-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="25df8-105">Volitelně můžete provést další konfiguraci pomocí atributů u tříd a vlastností nebo pomocí rozhraní API Fluent.</span><span class="sxs-lookup"><span data-stu-id="25df8-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="25df8-106">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="25df8-106">Watch the video</span></span>
<span data-ttu-id="25df8-107">Toto video je [teď k dispozici na Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="25df8-107">This video is [now available on Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="25df8-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="25df8-108">Pre-Requisites</span></span>

<span data-ttu-id="25df8-109">Abyste mohli dokončit tento návod, budete muset mít nainstalovanou **aplikaci Visual Studio 2012** nebo **Visual Studio 2013** .</span><span class="sxs-lookup"><span data-stu-id="25df8-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="25df8-110">Budete také potřebovat verzi **6,1** (nebo novější) Entity Framework Tools nainstalované sady **Visual Studio** .</span><span class="sxs-lookup"><span data-stu-id="25df8-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="25df8-111">Informace o instalaci nejnovější verze Entity Framework Tools najdete v tématu věnovaném [získání Entity Framework](~/ef6/fundamentals/install.md) .</span><span class="sxs-lookup"><span data-stu-id="25df8-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="25df8-112">1. vytvoření existující databáze</span><span class="sxs-lookup"><span data-stu-id="25df8-112">1. Create an Existing Database</span></span>

<span data-ttu-id="25df8-113">Když cílíte na existující databázi, bude už vytvořená, ale pro tento návod musíme pro přístup vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="25df8-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="25df8-114">Pojďme dopředu a vygenerovat databázi.</span><span class="sxs-lookup"><span data-stu-id="25df8-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="25df8-115">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25df8-115">Open Visual Studio</span></span>
-   <span data-ttu-id="25df8-116">**Zobrazení-&gt; Průzkumník serveru**</span><span class="sxs-lookup"><span data-stu-id="25df8-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="25df8-117">Klikněte pravým tlačítkem na **datová připojení –&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="25df8-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="25df8-118">Pokud jste se k databázi nepřipojili z **Průzkumník serveru** před tím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="25df8-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Výběr zdroje dat](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="25df8-120">Připojte se k instanci LocalDB a jako název databáze zadejte **blog** .</span><span class="sxs-lookup"><span data-stu-id="25df8-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Připojení LocalDB](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="25df8-122">Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .</span><span class="sxs-lookup"><span data-stu-id="25df8-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Dialogové okno vytvořit databázi](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="25df8-124">Nová databáze se nyní zobrazí v Průzkumník serveru, klikněte na ni pravým tlačítkem myši a vyberte **Nový dotaz** .</span><span class="sxs-lookup"><span data-stu-id="25df8-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="25df8-125">Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .</span><span class="sxs-lookup"><span data-stu-id="25df8-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="25df8-126">2. Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="25df8-126">2. Create the Application</span></span>

<span data-ttu-id="25df8-127">Abychom mohli dělat jednoduché věci, vytvoříme základní konzolovou aplikaci, která používá Code First k přístupu k datům:</span><span class="sxs-lookup"><span data-stu-id="25df8-127">To keep things simple we will build a basic console application that uses Code First to do the data access:</span></span>

-   <span data-ttu-id="25df8-128">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25df8-128">Open Visual Studio</span></span>
-   <span data-ttu-id="25df8-129">**Soubor –&gt; projekt New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="25df8-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="25df8-130">V levé nabídce a v **konzolové aplikaci** vyberte **Windows** .</span><span class="sxs-lookup"><span data-stu-id="25df8-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="25df8-131">Jako název zadejte **CodeFirstExistingDatabaseSample** .</span><span class="sxs-lookup"><span data-stu-id="25df8-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="25df8-132">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="25df8-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="25df8-133">3. zpětná analýza modelu</span><span class="sxs-lookup"><span data-stu-id="25df8-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="25df8-134">Použijeme Entity Framework Tools pro Visual Studio, abychom nám pomohli vygenerovat nějaký počáteční kód pro mapování na databázi.</span><span class="sxs-lookup"><span data-stu-id="25df8-134">We will use the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="25df8-135">Tyto nástroje právě generují kód, který můžete zadat také v případě, že dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="25df8-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="25df8-136">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="25df8-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="25df8-137">V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**</span><span class="sxs-lookup"><span data-stu-id="25df8-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="25df8-138">Jako název zadejte **BloggingContext** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="25df8-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="25df8-139">Spustí se **průvodce model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="25df8-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="25df8-140">Vyberte **Code First z databáze** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="25df8-140">Select **Code First from Database** and click **Next**</span></span>

    ![Průvodce One CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="25df8-142">Vyberte připojení k databázi, kterou jste vytvořili v první části, a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="25df8-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Průvodce – dva CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="25df8-144">Kliknutím na zaškrtávací políčko vedle **tabulky** naimportujte všechny tabulky a klikněte na **Dokončit** .</span><span class="sxs-lookup"><span data-stu-id="25df8-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Průvodce třemi CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="25df8-146">Po dokončení procesu zpětného analýz se do projektu přidají několik položek, které se podívejme na to, co se přidalo.</span><span class="sxs-lookup"><span data-stu-id="25df8-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="25df8-147">Konfigurační soubor</span><span class="sxs-lookup"><span data-stu-id="25df8-147">Configuration file</span></span>

<span data-ttu-id="25df8-148">Do projektu byl přidán soubor App. config, tento soubor obsahuje připojovací řetězec k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="25df8-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="25df8-149">*Všimněte si, že některá další nastavení jsou také v konfiguračním souboru. Jedná se o výchozí nastavení EF, které sděluje Code First, kde vytvořit databáze. Vzhledem k tomu, že jsme provedli mapování na existující databázi, tato nastavení budou v naší aplikaci ignorována.*</span><span class="sxs-lookup"><span data-stu-id="25df8-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="25df8-150">Odvozený kontext</span><span class="sxs-lookup"><span data-stu-id="25df8-150">Derived Context</span></span>

<span data-ttu-id="25df8-151">Do projektu se přidala třída **BloggingContext** .</span><span class="sxs-lookup"><span data-stu-id="25df8-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="25df8-152">Kontext představuje relaci s databází a umožňuje nám dotazovat se na data a ukládat je.</span><span class="sxs-lookup"><span data-stu-id="25df8-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="25df8-153">Kontext zpřístupňuje **negenerickými&lt;TEntity&gt;** pro každý typ v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="25df8-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="25df8-154">Všimněte si také, že výchozí konstruktor volá základní konstruktor pomocí syntaxe **Name =** .</span><span class="sxs-lookup"><span data-stu-id="25df8-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="25df8-155">To oznamuje Code First, že připojovací řetězec, který se má použít pro tento kontext, by měl být načten z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="25df8-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

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

<span data-ttu-id="25df8-156">*Při použití připojovacího řetězce v konfiguračním souboru byste vždy měli použít syntaxi **Name =** . Tím je zajištěno, že pokud připojovací řetězec není k dispozici, Entity Framework bude vyvolávat místo vytvoření nové databáze podle konvence.*</span><span class="sxs-lookup"><span data-stu-id="25df8-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="25df8-157">Třídy modelu</span><span class="sxs-lookup"><span data-stu-id="25df8-157">Model classes</span></span>

<span data-ttu-id="25df8-158">Nakonec byly do projektu přidány také třídy **blog** a **příspěvek** .</span><span class="sxs-lookup"><span data-stu-id="25df8-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="25df8-159">Jedná se o třídy domény, které tvoří náš model.</span><span class="sxs-lookup"><span data-stu-id="25df8-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="25df8-160">V případě, že se konvence Code First neshodují se strukturou existující databáze, uvidíte datové poznámky použité na třídy.</span><span class="sxs-lookup"><span data-stu-id="25df8-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="25df8-161">Například se zobrazí anotace **StringLength** na **blog.Name** a **blogu. URL** , protože v databázi mají maximální délku **200** (Code First výchozí hodnota je použití maximun podporovaného poskytovatelem databáze- **nvarchar (max)** v SQL Server).</span><span class="sxs-lookup"><span data-stu-id="25df8-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

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

## <a name="4-reading--writing-data"></a><span data-ttu-id="25df8-162">4. čtení & zápisu dat</span><span class="sxs-lookup"><span data-stu-id="25df8-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="25df8-163">Teď, když máme model, je čas ho použít pro přístup k některým datům.</span><span class="sxs-lookup"><span data-stu-id="25df8-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="25df8-164">Implementujte metodu **Main** v **program.cs** , jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="25df8-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="25df8-165">Tento kód vytvoří novou instanci našeho kontextu a pak ho použije k vložení nového **blogu**.</span><span class="sxs-lookup"><span data-stu-id="25df8-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="25df8-166">Pak použije dotaz LINQ k načtení všech **blogů** z databáze seřazené abecedně podle **názvu**.</span><span class="sxs-lookup"><span data-stu-id="25df8-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="25df8-167">Nyní můžete spustit aplikaci a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="25df8-167">You can now run the application and test it.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="25df8-168">Co když se změní moje databáze?</span><span class="sxs-lookup"><span data-stu-id="25df8-168">What if My Database Changes?</span></span>

<span data-ttu-id="25df8-169">Průvodce Code First k databázi je navržený tak, aby vygeneroval počáteční sadu tříd, které pak můžete upravit a upravit.</span><span class="sxs-lookup"><span data-stu-id="25df8-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="25df8-170">Pokud se vaše schéma databáze změní, můžete buď ručně upravit třídy, nebo provést jinou zpětnou analýzu pro přepsání tříd.</span><span class="sxs-lookup"><span data-stu-id="25df8-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="25df8-171">Použití Migrace Code First do existující databáze</span><span class="sxs-lookup"><span data-stu-id="25df8-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="25df8-172">Pokud chcete použít Migrace Code First s existující databází, přečtěte si téma [migrace Code First do existující databáze](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="25df8-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="25df8-173">Souhrn</span><span class="sxs-lookup"><span data-stu-id="25df8-173">Summary</span></span>

<span data-ttu-id="25df8-174">V tomto návodu jsme se podívali na Code First vývoje pomocí existující databáze.</span><span class="sxs-lookup"><span data-stu-id="25df8-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="25df8-175">V sadě Visual Studio jsme použili Entity Framework Tools pro zpětnou analýzu sady tříd, které jsou namapované na databázi, a dají se použít k ukládání a načítání dat.</span><span class="sxs-lookup"><span data-stu-id="25df8-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
