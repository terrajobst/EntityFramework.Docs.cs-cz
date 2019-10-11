---
title: Asynchronní dotazování a ukládání – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 0642dc13e7aa3906fa1495031c62701fc16f0192
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181836"
---
# <a name="async-query-and-save"></a>Asynchronní dotazování a ukládání
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

EF6 zavádí podporu pro asynchronní dotazy a ukládá je pomocí [klíčových slov Async a await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , která byla představena v rozhraní .NET 4,5. I když ne všechny aplikace můžou těžit z asynchronii, je možné ji použít ke zlepšení odezvy klienta a škálovatelnosti serveru při zpracování dlouhotrvajících, síťových nebo vstupně-výstupních úloh.

## <a name="when-to-really-use-async"></a>Kdy skutečně použít asynchronní

Účelem tohoto návodu je zavedení asynchronních konceptů způsobem, který usnadňuje sledování rozdílů mezi asynchronním a synchronním prováděním programu. Tento návod není určen k ilustraci žádných klíčových scénářů, kde asynchronní programování poskytuje výhody.

Asynchronní programování je primárně zaměřeno na uvolnění aktuálního spravovaného vlákna (vlákno spouštějící kód .NET) k provedení jiné práce v době, kdy čeká na operaci, která nevyžaduje výpočetní čas ze spravovaného vlákna. Například když databázový stroj zpracovává dotaz, nemusíte nic dělat pomocí kódu .NET.

V klientských aplikacích (WinForms, WPF atd.) je možné použít aktuální vlákno k udržení odezvy uživatelského rozhraní během provádění asynchronní operace. V serverových aplikacích (ASP.NET atd.) se vlákno dá použít ke zpracování dalších příchozích požadavků – to může snížit využití paměti nebo zvýšit propustnost serveru.

Ve většině aplikací s využitím Async nebudou žádné znatelné výhody a dokonce by mohlo být škodlivé. Používejte testy, profilaci a běžné smysly k měření dopadu asynchronního testu ve vašem konkrétním scénáři před jeho potvrzením.

Zde jsou některé další zdroje informací o asynchronních prostředcích:

-   [Brandon Bray – přehled Async/await v rozhraní .NET 4,5](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   Stránky [asynchronního programování](https://msdn.microsoft.com/library/hh191443.aspx) v knihovně MSDN
-   [Sestavování webových aplikací v ASP.NET pomocí Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (zahrnuje ukázku zvýšené propustnosti serveru)

## <a name="create-the-model"></a>Vytvoření modelu

Pomocí [pracovního postupu Code First](~/ef6/modeling/code-first/workflows/new-database.md) vytvoříte náš model a vygenerujete databázi, ale asynchronní funkce budou fungovat se všemi modely EF, včetně těch, které jsou vytvořené pomocí návrháře EF.

-   Vytvořte konzolovou aplikaci a zavolejte ji **AsyncDemo**
-   Přidat balíček NuGet EntityFramework
    -   V Průzkumník řešení klikněte pravým tlačítkem na projekt **AsyncDemo** .
    -   Vyberte **Spravovat balíčky NuGet...**
    -   V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .
    -   Klikněte na **nainstalovat** .
-   Přidat třídu **model.cs** s následující implementací

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

 

## <a name="create-a-synchronous-program"></a>Vytvořit synchronní program

Teď, když máme model EF, napíšeme kód, který ho používá k provedení určitého přístupu k datům.

-   Nahraďte obsah **program.cs** následujícím kódem.

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

Tento kód volá metodu **PerformDatabaseOperations** , která uloží nový **blog** do databáze a potom načte všechny **Blogy** z databáze a vytiskne je do **konzoly**. Potom program zapíše do **konzoly**nabídku dne.

Vzhledem k tomu, že kód je synchronní, můžeme při spuštění programu sledovat následující tok spuštění:

1.  **SaveChanges** zahajuje vložení nového **blogu** do databáze.
2.  **SaveChanges** se dokončí.
3.  Dotaz pro všechny **Blogy** jsou odesílány do databáze.
4.  Dotaz vrátí a výsledky se zapisují do **konzoly** .
5.  Nabídka dne se zapisuje do **konzoly** .

![Synchronizovat výstup](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Asynchronní vytváření

Teď, když máme náš program v provozu, můžeme začít používat nová klíčová slova Async a await. Provedli jsme následující změny Program.cs

1.  Řádek 2: Příkaz using pro obor názvů **System. data. entity** poskytne přístup k metodám asynchronního rozšíření EF.
2.  Řádek 4: Příkaz using pro obor názvů **System. Threading. Tasks** nám umožňuje použít typ **úkolu** .
3.  Řádek 12 & 18: Zachycujeme jako úkol, který monitoruje průběh **PerformSomeDatabaseOperations** (řádek 12) a potom zablokuje spuštění programu pro tuto úlohu, aby se dokončila i po dokončení veškeré práce pro daný program (řádek 18).
4.  Řádek 25: **PerformSomeDatabaseOperations** jsme aktualizovali tak, aby byla označena jako **asynchronní** a vrátila **úlohu**.
5.  Řádek 35: Nyní voláme asynchronní verzi metody SaveChanges a čeká se na její dokončení.
6.  Řádek 42: Nyní voláme asynchronní verzi ToList – a čekáme na výsledek.

Úplný seznam dostupných metod rozšíření v oboru názvů System. data. entity naleznete v tématu Třída QueryableExtensions. *Pro příkazy using budete také muset přidat "using System. data. entity".*

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

Teď, když je kód asynchronní, můžeme při spuštění programu sledovat jiný tok spuštění:

1. **SaveChanges** zahajuje vložení nového **blogu** do databáze.  
    @no__t – 0Once se příkaz pošle do databáze. v aktuálním spravovaném vlákně není potřeba žádné další výpočetní čas. Metoda **PerformDatabaseOperations** vrací (i když není dokončená) a tok programu v metodě Main pokračuje. *
2. **Nabídka dne se zapisuje do konzoly.**  
    @no__t – 0Since neexistuje žádná další práce v metodě Main, spravované vlákno je ve volání čekání blokované, dokud se operace databáze nedokončí. Po dokončení bude zbytek našich **PerformDatabaseOperations** spuštěn. *
3.  **SaveChanges** se dokončí.  
4.  Dotaz pro všechny **Blogy** jsou odesílány do databáze.  
    @no__t – 0Again je spravované vlákno bezplatné v průběhu zpracování dotazu v databázi. Vzhledem k tomu, že bylo dokončeno jakékoli jiné spuštění, vlákno bude pouze zastaveno při volání čekání, ale. *
5.  Dotaz vrátí a výsledky se zapisují do **konzoly** .  

![Asynchronní výstup](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Poznatkem

Nyní jsme viděli, jak snadné je využít asynchronní metody EF. I když se výhody asynchronního asynchronního typu nemusí velmi vyhodnotit pomocí jednoduché konzolové aplikace, je možné tyto stejné strategie použít v situacích, kdy dlouhotrvající nebo síťové aktivity může jinak blokovat aplikaci nebo způsobit velký počet vláken. Zvětšete nároky na paměť.
