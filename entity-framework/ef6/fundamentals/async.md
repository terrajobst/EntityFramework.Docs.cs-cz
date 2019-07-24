---
title: Asynchronní dotazování a ukládání – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: bf2039110962e8dd114242dcd0b9454963750774
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306587"
---
# <a name="async-query-and-save"></a><span data-ttu-id="00ad3-102">Asynchronní dotazování a ukládání</span><span class="sxs-lookup"><span data-stu-id="00ad3-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="00ad3-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="00ad3-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="00ad3-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="00ad3-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="00ad3-105">EF6 zavádí podporu pro asynchronní dotazy a ukládá je pomocí [klíčových slov Async a await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , která byla představena v rozhraní .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="00ad3-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="00ad3-106">I když ne všechny aplikace můžou těžit z asynchronii, je možné ji použít ke zlepšení odezvy klienta a škálovatelnosti serveru při zpracování dlouhotrvajících, síťových nebo vstupně-výstupních úloh.</span><span class="sxs-lookup"><span data-stu-id="00ad3-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="00ad3-107">Kdy skutečně použít asynchronní</span><span class="sxs-lookup"><span data-stu-id="00ad3-107">When to really use async</span></span>

<span data-ttu-id="00ad3-108">Účelem tohoto návodu je zavedení asynchronních konceptů způsobem, který usnadňuje sledování rozdílů mezi asynchronním a synchronním prováděním programu.</span><span class="sxs-lookup"><span data-stu-id="00ad3-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="00ad3-109">Tento návod není určen k ilustraci žádných klíčových scénářů, kde asynchronní programování poskytuje výhody.</span><span class="sxs-lookup"><span data-stu-id="00ad3-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="00ad3-110">Asynchronní programování je primárně zaměřeno na uvolnění aktuálního spravovaného vlákna (vlákno spouštějící kód .NET) k provedení jiné práce v době, kdy čeká na operaci, která nevyžaduje výpočetní čas ze spravovaného vlákna.</span><span class="sxs-lookup"><span data-stu-id="00ad3-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="00ad3-111">Například když databázový stroj zpracovává dotaz, nemusíte nic dělat pomocí kódu .NET.</span><span class="sxs-lookup"><span data-stu-id="00ad3-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="00ad3-112">V klientských aplikacích (WinForms, WPF atd.) je možné použít aktuální vlákno k udržení odezvy uživatelského rozhraní během provádění asynchronní operace.</span><span class="sxs-lookup"><span data-stu-id="00ad3-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="00ad3-113">V serverových aplikacích (ASP.NET atd.) se vlákno dá použít ke zpracování dalších příchozích požadavků – to může snížit využití paměti nebo zvýšit propustnost serveru.</span><span class="sxs-lookup"><span data-stu-id="00ad3-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="00ad3-114">Ve většině aplikací s využitím Async nebudou žádné znatelné výhody a dokonce by mohlo být škodlivé.</span><span class="sxs-lookup"><span data-stu-id="00ad3-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="00ad3-115">Používejte testy, profilaci a běžné smysly k měření dopadu asynchronního testu ve vašem konkrétním scénáři před jeho potvrzením.</span><span class="sxs-lookup"><span data-stu-id="00ad3-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="00ad3-116">Zde jsou některé další zdroje informací o asynchronních prostředcích:</span><span class="sxs-lookup"><span data-stu-id="00ad3-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="00ad3-117">Brandon Bray – přehled Async/await v rozhraní .NET 4,5</span><span class="sxs-lookup"><span data-stu-id="00ad3-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="00ad3-118">Stránky [asynchronního programování](https://msdn.microsoft.com/library/hh191443.aspx) v knihovně MSDN</span><span class="sxs-lookup"><span data-stu-id="00ad3-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="00ad3-119">[Sestavování webových aplikací v ASP.NET pomocí Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (zahrnuje ukázku zvýšené propustnosti serveru)</span><span class="sxs-lookup"><span data-stu-id="00ad3-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="00ad3-120">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="00ad3-120">Create the model</span></span>

<span data-ttu-id="00ad3-121">Pomocí [pracovního postupu Code First](~/ef6/modeling/code-first/workflows/new-database.md) vytvoříte náš model a vygenerujete databázi, ale asynchronní funkce budou fungovat se všemi modely EF, včetně těch, které jsou vytvořené pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="00ad3-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="00ad3-122">Vytvořte konzolovou aplikaci a zavolejte ji **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="00ad3-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="00ad3-123">Přidat balíček NuGet EntityFramework</span><span class="sxs-lookup"><span data-stu-id="00ad3-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="00ad3-124">V Průzkumník řešení klikněte pravým tlačítkem na projekt **AsyncDemo** .</span><span class="sxs-lookup"><span data-stu-id="00ad3-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="00ad3-125">Vyberte **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="00ad3-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="00ad3-126">V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="00ad3-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="00ad3-127">Klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="00ad3-127">Click **Install**</span></span>
-   <span data-ttu-id="00ad3-128">Přidat třídu **model.cs** s následující implementací</span><span class="sxs-lookup"><span data-stu-id="00ad3-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
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
    }
```

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="00ad3-129">Vytvořit synchronní program</span><span class="sxs-lookup"><span data-stu-id="00ad3-129">Create a synchronous program</span></span>

<span data-ttu-id="00ad3-130">Teď, když máme model EF, napíšeme kód, který ho používá k provedení určitého přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="00ad3-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="00ad3-131">Nahraďte obsah **program.cs** následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="00ad3-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="00ad3-132">Tento kód volá metodu **PerformDatabaseOperations** , která uloží nový **blog** do databáze a potom načte všechny **Blogy** z databáze a vytiskne je do **konzoly**.</span><span class="sxs-lookup"><span data-stu-id="00ad3-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="00ad3-133">Potom program zapíše do **konzoly**nabídku dne.</span><span class="sxs-lookup"><span data-stu-id="00ad3-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="00ad3-134">Vzhledem k tomu, že kód je synchronní, můžeme při spuštění programu sledovat následující tok spuštění:</span><span class="sxs-lookup"><span data-stu-id="00ad3-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="00ad3-135">**SaveChanges** zahajuje vložení nového **blogu** do databáze.</span><span class="sxs-lookup"><span data-stu-id="00ad3-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="00ad3-136">**SaveChanges** se dokončí.</span><span class="sxs-lookup"><span data-stu-id="00ad3-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="00ad3-137">Dotaz pro všechny **Blogy** jsou odesílány do databáze.</span><span class="sxs-lookup"><span data-stu-id="00ad3-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="00ad3-138">Dotaz vrátí a výsledky se zapisují do **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="00ad3-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="00ad3-139">Nabídka dne se zapisuje do **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="00ad3-139">Quote of the day is written to **Console**</span></span>

![Synchronizovat výstup](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="00ad3-141">Asynchronní vytváření</span><span class="sxs-lookup"><span data-stu-id="00ad3-141">Making it asynchronous</span></span>

<span data-ttu-id="00ad3-142">Teď, když máme náš program v provozu, můžeme začít používat nová klíčová slova Async a await.</span><span class="sxs-lookup"><span data-stu-id="00ad3-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="00ad3-143">Provedli jsme následující změny Program.cs</span><span class="sxs-lookup"><span data-stu-id="00ad3-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="00ad3-144">Řádek 2: Příkaz using pro obor názvů **System. data. entity** poskytne přístup k metodám asynchronního rozšíření EF.</span><span class="sxs-lookup"><span data-stu-id="00ad3-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="00ad3-145">Řádek 4: Příkaz using pro obor názvů **System. Threading. Tasks** nám umožňuje použít typ **úkolu** .</span><span class="sxs-lookup"><span data-stu-id="00ad3-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="00ad3-146">Řádek 12 & 18: Zachycujeme jako úkol, který monitoruje průběh **PerformSomeDatabaseOperations** (řádek 12) a potom zablokuje spuštění programu pro tuto úlohu, aby se dokončila i po dokončení veškeré práce pro daný program (řádek 18).</span><span class="sxs-lookup"><span data-stu-id="00ad3-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="00ad3-147">Řádek 25: **PerformSomeDatabaseOperations** jsme aktualizovali tak, aby byla označena jako **asynchronní** a vrátila **úlohu**.</span><span class="sxs-lookup"><span data-stu-id="00ad3-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="00ad3-148">Řádek 35: Nyní voláme asynchronní verzi metody SaveChanges a čeká se na její dokončení.</span><span class="sxs-lookup"><span data-stu-id="00ad3-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="00ad3-149">Řádek 42: Nyní voláme asynchronní verzi ToList – a čekáme na výsledek.</span><span class="sxs-lookup"><span data-stu-id="00ad3-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="00ad3-150">Úplný seznam dostupných metod rozšíření v oboru názvů System. data. entity naleznete v tématu Třída QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="00ad3-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="00ad3-151">*Pro příkazy using budete také muset přidat "using System. data. entity".*</span><span class="sxs-lookup"><span data-stu-id="00ad3-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="00ad3-152">Teď, když je kód asynchronní, můžeme při spuštění programu sledovat jiný tok spuštění:</span><span class="sxs-lookup"><span data-stu-id="00ad3-152">Now that the code is asynchronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="00ad3-153">**SaveChanges** začíná po odeslání příkazu do  databáze odeslat nový blog *do databáze. v aktuálním spravovaném vlákně není potřeba žádné další výpočetní čas. Metoda **PerformDatabaseOperations** vrací (i když ještě nedokončila) a tok programu v metodě Main pokračuje.*</span><span class="sxs-lookup"><span data-stu-id="00ad3-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="00ad3-154">**Nabídka dne je zapsána do konzoly**
     *, protože v metodě Main neexistuje více práce, spravované vlákno je blokováno ve volání čekání, dokud se operace databáze nedokončí. Po dokončení bude zbytek našich **PerformDatabaseOperations** spuštěn.*</span><span class="sxs-lookup"><span data-stu-id="00ad3-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="00ad3-155">**SaveChanges** se dokončí.</span><span class="sxs-lookup"><span data-stu-id="00ad3-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="00ad3-156">Dotaz pro všechny **Blogy** se znovu pošle do *databáze, spravované vlákno se zadarmo provede v průběhu zpracování dotazu v databázi. Vzhledem k tomu, že bylo dokončeno jakékoli jiné spuštění, vlákno bude pouze zastaveno ve volání čekání, i když.*</span><span class="sxs-lookup"><span data-stu-id="00ad3-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="00ad3-157">Dotaz vrátí a výsledky se zapisují do **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="00ad3-157">Query returns and results are written to **Console**</span></span>

![Asynchronní výstup](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="00ad3-159">Poznatkem</span><span class="sxs-lookup"><span data-stu-id="00ad3-159">The takeaway</span></span>

<span data-ttu-id="00ad3-160">Nyní jsme viděli, jak snadné je využít asynchronní metody EF.</span><span class="sxs-lookup"><span data-stu-id="00ad3-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="00ad3-161">I když se výhody asynchronního asynchronního typu nemusí velmi vyhodnotit pomocí jednoduché konzolové aplikace, je možné tyto stejné strategie použít v situacích, kdy dlouhotrvající nebo síťové aktivity může jinak blokovat aplikaci nebo způsobit velký počet vláken. Zvětšete nároky na paměť.</span><span class="sxs-lookup"><span data-stu-id="00ad3-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
