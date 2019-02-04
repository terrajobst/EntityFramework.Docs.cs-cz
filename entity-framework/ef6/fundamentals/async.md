---
title: Asynchronní dotazy a save - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 89c7b9d533d37b4c9e123f37d8ab27c67ba26cc8
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668710"
---
# <a name="async-query-and-save"></a>Asynchronní dotazy a uložit
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Zavedena podpora asynchronního dotazu a uložit pomocí EF6 [async a operátoru await klíčová slova](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , která byla zavedena v rozhraní .NET 4.5. Zatímco u některých aplikací není můžou mít užitek z asynchronii, můžete použít ke zlepšení škálovatelnosti rychlost reakce a server klienta při zpracování dlouhodobě spuštěných, sítě nebo vstupně-výstupní úlohy.

## <a name="when-to-really-use-async"></a>Kdy k opravdu použít operátory async

Účelem tohoto návodu je zavést koncepty asynchronní způsobem, který umožňuje snadno sledovat rozdíl mezi provádění asynchronní a synchronní programu. Tento názorný postup není určený k objasnění některé z klíčových scénářů, kde asynchronní programování poskytuje výhody.

Asynchronní programování je primárně zaměřen na aktuální spravované vlákno (vlákno spuštěním kódu .NET) provádět další činnosti, kdy čeká operace, která nevyžaduje žádné výpočetní čas věnovat ze spravované vlákno. Například zpracovávání databázový stroj je dotaz nic udělat kód .NET.

V klientských aplikacích (WinForms, WPF atd.) aktuální vlákno je možné zachovat interaktivní uživatelské rozhraní během provádění asynchronní operace. V serverových aplikací (ASP.NET atd.) do vlákna je možné zpracovávat jiné příchozí žádosti – to může snížit využití paměti a/nebo zvýšit propustnost ze serveru.

Ve většině aplikací použití modifikátoru async, bude mít žádné výrazné výhody a dokonce může být škodlivé. Pomocí testování, profilaci a zdravý měřit dopad asynchronní v konkrétním případě před potvrzením do něj.

Tady jsou některé další zdroje informací o asynchronní:

-   [Přehled Brandon Bray async/await v rozhraní .NET 4.5](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Asynchronní programování](https://msdn.microsoft.com/library/hh191443.aspx) stránek v knihovně MSDN
-   [Jak vytvořit asynchronní pomocí technologie ASP.NET webové aplikace](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (včetně ukázku propustnost vyšší server)

## <a name="create-the-model"></a>Vytvoření modelu

Budeme používat [Code First pracovního postupu](~/ef6/modeling/code-first/workflows/new-database.md) vytvoření náš model a generovat databázi, ale asynchronní funkcí bude fungovat s všechny modely EF, včetně těch, které vytvořili s EF designeru.

-   Vytvořte konzolovou aplikaci a jeho volání **AsyncDemo**
-   Přidání balíčku NuGet objektu EntityFramework
    -   V Průzkumníku řešení klikněte pravým tlačítkem myši na **AsyncDemo** projektu
    -   Vyberte **spravovat balíčky NuGet...**
    -   V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku
    -   Klikněte na tlačítko **instalace**
-   Přidat **Model.cs** třídy následující implementaci

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

 

## <a name="create-a-synchronous-program"></a>Vytvoření synchronní aplikace

Když teď máme modelu EF, napište kód, který používá některá přístup k datům.

-   Nahraďte obsah **Program.cs** následujícím kódem

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

Tento kód volá **PerformDatabaseOperations** metodu, která uloží nový **blogu** do databáze a pak načte všechny **blogy** z databáze a vytiskne jim **Konzoly**. Po této, program zapíše nabídky dne **konzoly**.

Protože kód je synchronní, můžete podle našich zkušeností následující postup provádění když spustíme program:

1.  **SaveChanges** začíná tak, aby nabízel nové **blogu** do databáze
2.  **SaveChanges** dokončení
3.  Dotaz na všechny **blogy** je odeslán do databáze
4.  Dotaz vrátí a výsledky se zapisují do **konzoly**
5.  Nabídka dne je zapsán do **konzoly**

![Výstup synchronizace](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Díky tomu je asynchronní

Když teď máme náš program rychle zprovoznit, můžeme začít vytváření využívání nových async a operátoru await klíčová slova. Provedli jsme následující změny do souboru Program.cs

1.  Řádek 2: Použití příkazu pro **System.Data.Entity** obor názvů poskytuje nám přístup k EF asynchronní rozšiřující metody.
2.  Řádek 4: Použití příkazu pro **System.Threading.Tasks** obor názvů umožňuje, abyste mohli používat **úloh** typu.
3.  Řádek 12 a 18: Jsme zachycování jako úlohu, která monitoruje průběh **PerformSomeDatabaseOperations** (řádkem 12) a pak blokování provádění programu pro tento úkol kompletní jednou všechnu práci pro program provádí (řádek 18).
4.  25. řádek: Jsme aktualizace **PerformSomeDatabaseOperations** být označený jako **asynchronní** a vraťte se **úloh**.
5.  Řádek 35: Nyní jsme volání asynchronní verzi SaveChanges a čekáním na její dokončení.
6.  Řádek 42: Voláme na asynchronní verzi ToList a čekání na výsledek.

Úplný seznam dostupných rozšiřujících metod v oboru názvů System.Data.Entity odkazovat na třídu QueryableExtensions. *Bude také potřeba přidat "pomocí System.Data.Entity" k pomocí příkazů.*

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

Teď, když je asynchronní kód, můžete podle našich zkušeností různých provádění toku když spustíme program:

1.  **SaveChanges** začíná tak, aby nabízel nové **blogu** do databáze *po odesláním příkazu do databáze žádné další výpočetní čas je potřebný pro aktuální vlákno spravované. **PerformDatabaseOperations** metoda vrátí hodnotu (přestože nebyl dokončen, provádění) a pokračuje v toku programu v hlavní metodě.*
2.  **Nabídka dne je zapsán do konzoly**
    *protože neexistuje žádná další práce do metody Main, spravované vlákno je blokována v čekání volat, dokud se nedokončí operace databáze. Po jeho dokončení, zbývající část naší **PerformDatabaseOperations** se spustí.*
3.  **SaveChanges** dokončení
4.  Dotaz na všechny **blogy** je odeslán do databáze *spravované vlákno je opět, můžete provádět další operace, zatímco zpracování dotazu se v databázi. Protože všechny další spuštění bylo dokončeno, vlákno právě zastaví při volání čekání ale.*
5.  Dotaz vrátí a výsledky se zapisují do **konzoly**

![Asynchronní výstupu](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Hlavní, co vyplývá

Jsme teď viděli, jak snadné je vytvořit použití asynchronních metod na EF. I když výhody asynchronní nemusí být jasné, s jednoduchou konzolovou aplikaci, tyto stejné strategie lze použít v situacích, kde dlouhotrvajících nebo vázané na síť aktivity může být jinak blokovat aplikaci, nebo způsobit, že velký počet vláken zvýšit nároky na paměť.
