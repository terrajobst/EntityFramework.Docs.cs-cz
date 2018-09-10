---
title: Asynchronní dotazy a save - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 35604fc16ea37415d39801831aa162d0d42c2a2f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250748"
---
# <a name="async-query-and-save"></a><span data-ttu-id="ce76d-102">Asynchronní dotazy a uložit</span><span class="sxs-lookup"><span data-stu-id="ce76d-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="ce76d-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ce76d-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="ce76d-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="ce76d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="ce76d-105">Zavedena podpora asynchronního dotazu a uložit pomocí EF6 [async a operátoru await klíčová slova](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , která byla zavedena v rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="ce76d-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="ce76d-106">Zatímco u některých aplikací není můžou mít užitek z asynchronii, můžete použít ke zlepšení škálovatelnosti rychlost reakce a server klienta při zpracování dlouhodobě spuštěných, sítě nebo vstupně-výstupní úlohy.</span><span class="sxs-lookup"><span data-stu-id="ce76d-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="ce76d-107">Kdy k opravdu použít operátory async</span><span class="sxs-lookup"><span data-stu-id="ce76d-107">When to really use async</span></span>

<span data-ttu-id="ce76d-108">Účelem tohoto návodu je zavést koncepty asynchronní způsobem, který umožňuje snadno sledovat rozdíl mezi provádění asynchronní a synchronní programu.</span><span class="sxs-lookup"><span data-stu-id="ce76d-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="ce76d-109">Tento názorný postup není určený k objasnění některé z klíčových scénářů, kde asynchronní programování poskytuje výhody.</span><span class="sxs-lookup"><span data-stu-id="ce76d-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="ce76d-110">Asynchronní programování je primárně zaměřen na aktuální spravované vlákno (vlákno spuštěním kódu .NET) provádět další činnosti, kdy čeká operace, která nevyžaduje žádné výpočetní čas věnovat ze spravované vlákno.</span><span class="sxs-lookup"><span data-stu-id="ce76d-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="ce76d-111">Například zpracovávání databázový stroj je dotaz nic udělat kód .NET.</span><span class="sxs-lookup"><span data-stu-id="ce76d-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="ce76d-112">V klientských aplikacích (WinForms, WPF atd.) aktuální vlákno je možné zachovat interaktivní uživatelské rozhraní během provádění asynchronní operace.</span><span class="sxs-lookup"><span data-stu-id="ce76d-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="ce76d-113">V serverových aplikací (ASP.NET atd.) do vlákna je možné zpracovávat jiné příchozí žádosti – to může snížit využití paměti a/nebo zvýšit propustnost ze serveru.</span><span class="sxs-lookup"><span data-stu-id="ce76d-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="ce76d-114">Ve většině aplikací použití modifikátoru async, bude mít žádné výrazné výhody a dokonce může být škodlivé.</span><span class="sxs-lookup"><span data-stu-id="ce76d-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="ce76d-115">Pomocí testování, profilaci a zdravý měřit dopad asynchronní v konkrétním případě před potvrzením do něj.</span><span class="sxs-lookup"><span data-stu-id="ce76d-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="ce76d-116">Tady jsou některé další zdroje informací o asynchronní:</span><span class="sxs-lookup"><span data-stu-id="ce76d-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="ce76d-117">Přehled Brandon Bray async/await v rozhraní .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ce76d-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="ce76d-118">[Asynchronní programování](https://msdn.microsoft.com/library/hh191443.aspx) stránek v knihovně MSDN</span><span class="sxs-lookup"><span data-stu-id="ce76d-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="ce76d-119">[Jak vytvořit asynchronní pomocí technologie ASP.NET webové aplikace](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (včetně ukázku propustnost vyšší server)</span><span class="sxs-lookup"><span data-stu-id="ce76d-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="ce76d-120">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="ce76d-120">Create the model</span></span>

<span data-ttu-id="ce76d-121">Budeme používat [Code First pracovního postupu](~/ef6/modeling/code-first/workflows/new-database.md) vytvoření náš model a generovat databázi, ale asynchronní funkcí bude fungovat s všechny modely EF, včetně těch, které vytvořili s EF designeru.</span><span class="sxs-lookup"><span data-stu-id="ce76d-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="ce76d-122">Vytvořte konzolovou aplikaci a jeho volání **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="ce76d-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="ce76d-123">Přidání balíčku NuGet objektu EntityFramework</span><span class="sxs-lookup"><span data-stu-id="ce76d-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="ce76d-124">V Průzkumníku řešení klikněte pravým tlačítkem myši na **AsyncDemo** projektu</span><span class="sxs-lookup"><span data-stu-id="ce76d-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="ce76d-125">Vyberte **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="ce76d-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="ce76d-126">V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku</span><span class="sxs-lookup"><span data-stu-id="ce76d-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="ce76d-127">Klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="ce76d-127">Click **Install**</span></span>
-   <span data-ttu-id="ce76d-128">Přidat **Model.cs** třídy následující implementaci</span><span class="sxs-lookup"><span data-stu-id="ce76d-128">Add a **Model.cs** class with the following implementation</span></span>

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="ce76d-129">Vytvoření synchronní aplikace</span><span class="sxs-lookup"><span data-stu-id="ce76d-129">Create a synchronous program</span></span>

<span data-ttu-id="ce76d-130">Když teď máme modelu EF, napište kód, který používá některá přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="ce76d-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="ce76d-131">Nahraďte obsah **Program.cs** následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="ce76d-131">Replace the contents of **Program.cs** with the following code</span></span>

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

                Console.WriteLine();
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
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="ce76d-132">Tento kód volá **PerformDatabaseOperations** metodu, která uloží nový **blogu** do databáze a pak načte všechny **blogy** z databáze a vytiskne jim **Konzoly**.</span><span class="sxs-lookup"><span data-stu-id="ce76d-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="ce76d-133">Po této, program zapíše nabídky dne **konzoly**.</span><span class="sxs-lookup"><span data-stu-id="ce76d-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="ce76d-134">Protože kód je synchronní, můžete podle našich zkušeností následující postup provádění když spustíme program:</span><span class="sxs-lookup"><span data-stu-id="ce76d-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="ce76d-135">**SaveChanges** začíná tak, aby nabízel nové **blogu** do databáze</span><span class="sxs-lookup"><span data-stu-id="ce76d-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="ce76d-136">**SaveChanges** dokončení</span><span class="sxs-lookup"><span data-stu-id="ce76d-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="ce76d-137">Dotaz na všechny **blogy** je odeslán do databáze</span><span class="sxs-lookup"><span data-stu-id="ce76d-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="ce76d-138">Dotaz vrátí a výsledky se zapisují do **konzoly**</span><span class="sxs-lookup"><span data-stu-id="ce76d-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="ce76d-139">Nabídka dne je zapsán do **konzoly**</span><span class="sxs-lookup"><span data-stu-id="ce76d-139">Quote of the day is written to **Console**</span></span>

![Výstup synchronizace](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="ce76d-141">Díky tomu je asynchronní</span><span class="sxs-lookup"><span data-stu-id="ce76d-141">Making it asynchronous</span></span>

<span data-ttu-id="ce76d-142">Když teď máme náš program rychle zprovoznit, můžeme začít vytváření využívání nových async a operátoru await klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="ce76d-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="ce76d-143">Provedli jsme následující změny do souboru Program.cs</span><span class="sxs-lookup"><span data-stu-id="ce76d-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="ce76d-144">Řádek 2: Nástroje pomocí příkazu pro **System.Data.Entity** obor názvů poskytuje nám přístup k EF asynchronní rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="ce76d-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="ce76d-145">Řádek 4: Using příkazu pro **System.Threading.Tasks** obor názvů umožňuje, abyste mohli používat **úloh** typu.</span><span class="sxs-lookup"><span data-stu-id="ce76d-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="ce76d-146">Řádek 12 a 18: jsme zachycování jako úlohu, která monitoruje průběh **PerformSomeDatabaseOperations** (řádkem 12) a pak blokování provádění programu pro tento úkol kompletní jednou všechnu práci pro program provádí (řádek 18).</span><span class="sxs-lookup"><span data-stu-id="ce76d-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="ce76d-147">Řádek 25: Jsme aktualizace **PerformSomeDatabaseOperations** být označený jako **asynchronní** a vraťte se **úloh**.</span><span class="sxs-lookup"><span data-stu-id="ce76d-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="ce76d-148">Řádek 35: Jsme teď volá asynchronní verzi SaveChanges a čekáním na její dokončení.</span><span class="sxs-lookup"><span data-stu-id="ce76d-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="ce76d-149">Řádek 42: Jsme teď volá asynchronní verzi ToList – a očekává na výsledek.</span><span class="sxs-lookup"><span data-stu-id="ce76d-149">Line 42: We're now calling hte Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="ce76d-150">Úplný seznam dostupných rozšiřujících metod v oboru názvů System.Data.Entity odkazovat na třídu QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="ce76d-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="ce76d-151">*Bude také potřeba přidat "pomocí System.Data.Entity" k pomocí příkazů.*</span><span class="sxs-lookup"><span data-stu-id="ce76d-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

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

<span data-ttu-id="ce76d-152">Teď, když je asynchronní kód, můžete podle našich zkušeností různých provádění toku když spustíme program:</span><span class="sxs-lookup"><span data-stu-id="ce76d-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="ce76d-153">**SaveChanges** začíná tak, aby nabízel nové **blogu** do databáze *po odesláním příkazu do databáze žádné další výpočetní čas je potřebný pro aktuální vlákno spravované. **PerformDatabaseOperations** metoda vrátí hodnotu (přestože nebyl dokončen, provádění) a pokračuje v toku programu v hlavní metodě.*</span><span class="sxs-lookup"><span data-stu-id="ce76d-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="ce76d-154">**Nabídka dne je zapsán do konzoly**
    \*protože neexistuje žádná další práce do metody Main, spravované vlákno je blokována v čekání volat, dokud se nedokončí operace databáze. Po jeho dokončení, zbývající část naší **PerformDatabaseOperations** \* se spustí.</span><span class="sxs-lookup"><span data-stu-id="ce76d-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations*** will be executed.</span></span>
3.  <span data-ttu-id="ce76d-155">**SaveChanges** dokončení</span><span class="sxs-lookup"><span data-stu-id="ce76d-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="ce76d-156">Dotaz na všechny **blogy** je odeslán do databáze *spravované vlákno je opět, můžete provádět další operace, zatímco zpracování dotazu se v databázi. Protože všechny další spuštění bylo dokončeno, vlákno právě zastaví při volání čekání ale.*</span><span class="sxs-lookup"><span data-stu-id="ce76d-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="ce76d-157">Dotaz vrátí a výsledky se zapisují do **konzoly**</span><span class="sxs-lookup"><span data-stu-id="ce76d-157">Query returns and results are written to **Console**</span></span>

![Asynchronní výstupu](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="ce76d-159">Hlavní, co vyplývá</span><span class="sxs-lookup"><span data-stu-id="ce76d-159">The takeaway</span></span>

<span data-ttu-id="ce76d-160">Jsme teď viděli, jak snadné je vytvořit použití asynchronních metod na EF.</span><span class="sxs-lookup"><span data-stu-id="ce76d-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="ce76d-161">I když výhody asynchronní nemusí být jasné, s jednoduchou konzolovou aplikaci, tyto stejné strategie lze použít v situacích, kde dlouhotrvajících nebo vázané na síť aktivity může být jinak blokovat aplikaci, nebo způsobit, že velký počet vláken zvýšit nároky na paměť.</span><span class="sxs-lookup"><span data-stu-id="ce76d-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
