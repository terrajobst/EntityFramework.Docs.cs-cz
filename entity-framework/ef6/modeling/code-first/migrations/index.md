---
title: Migrace Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: f408ef861a2992783142fa1483d1433ca710399a
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415793"
---
# <a name="code-first-migrations"></a>Migrace Code First
Migrace Code First je doporučeným způsobem, jak vyvíjet schéma databáze vaší aplikace, pokud používáte Code First pracovního postupu. Migrace poskytují sadu nástrojů, které umožňují:

1. Vytvoření počáteční databáze, která funguje s EF modelu
2. Generování migrace můžete sledovat změny provedené do modelu EF
2. Zachovat aktuální tyto změny databáze

Následující návod přináší přehled migrace Code First v rozhraní Entity Framework. Můžete buď dokončení celého postupu nebo přejděte k tématu, které vás zajímají. Jsou pokryta následující témata:

## <a name="building-an-initial-model--database"></a>Vytváření počáteční modelu & databáze

Než začneme pomocí migrace musíte projekt a model Code First pro práci s. V tomto návodu budeme používat kanonickém **blogu** a **příspěvek** modelu.

-   Vytvořte nový **MigrationsDemo** konzolové aplikace
-   Přidejte nejnovější verzi **EntityFramework** balíček NuGet do projektu
    -   **Nástroje –&gt; Správce balíčků knihoven –&gt; Konzola správce balíčků**
    -   Spustit **Install-Package EntityFramework** příkazu
-   Přidat **Model.cs** souboru s kódem je uvedeno níže. Tento kód definuje jedinou **blogu** třídu, která tvoří náš model domény a **BlogContext** třídu, která je náš kontext platforem EF Code First

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   Když teď máme modelu je čas ji používat pro přístup k datům. Aktualizace **Program.cs** souboru s kódem je uvedeno níže.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   Spusťte aplikaci a uvidíte, že **MigrationsCodeDemo.BlogContext** databáze se vytvoří za vás.

    ![Databáze LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Povolení migrace

Je čas provést některé další změny našeho modelu.

-   Umožňuje zavést vlastnost adresa Url blogu třídy.

``` csharp
    public string Url { get; set; }
```

Pokud chcete aplikaci spustit znovu získali byste s oznámením InvalidOperationException *model zálohování kontextu 'BlogContext' byl změněn, protože byla vytvořena databáze. Zvažte použití migrace Code First k aktualizaci databáze (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Jak výjimku naznačuje, je čas začít pomocí migrace Code First. Prvním krokem je povolení migrace pro náš kontext.

-   Spustit **povolení migrace** příkazu v konzole Správce balíčků

    Tento příkaz má přidat **migrace** složku do projektu. Tato nová složka obsahuje dva soubory:

-   **Třída konfigurace.** Tato třída umožňuje konfigurovat chování migrace pro váš kontext. V tomto návodu budeme používat jenom výchozí konfiguraci.
    *Vzhledem k tomu, že existuje pouze jeden kontext Code First ve vašem projektu, má povolení migrace automaticky vyplněna tato konfigurace se vztahuje na typ kontextu.*
-   **Při migraci InitialCreate**. Tato migrace se vygenerovat, protože jsme už měli Code First vytvořit databázi pro nás, než jsme povolili migrace. Kód v této vygenerované migraci představuje objekty, které již byly vytvořeny v databázi. V našem, který je **blogu** tabulky s **BlogId** a **název** sloupce. Název souboru obsahuje časové razítko, které vám pomůžou s řazení.
    *Pokud databázi nebyl už vytvořili InitialCreate migrace by byly přidány do projektu. Při prvním říkáme migrace přidat kód k vytvoření těchto tabulek by místo toho automaticky generovaný nové migrace.*

### <a name="multiple-models-targeting-the-same-database"></a>Více modelů, které cílí na stejné databáze

Při použití verze starší než EF6, pouze jeden model Code First může vygenerovat a spravovat schématu databáze. To je výsledkem jednoho  **\_ \_MigrationsHistory** tabulky na databázi s způsob, jak zjistit položky, které patří do které modelu.

Počínaje EF6, **konfigurace** obsahuje třídy **ContextKey** vlastnost. To slouží jako jedinečný identifikátor pro každý model Code First. Odpovídající sloupec ve  **\_ \_MigrationsHistory** tabulky umožňuje položky z více modelů pro sdílení v tabulce. Ve výchozím nastavení je tato vlastnost nastavena na plně kvalifikovaný název kontextu.

## <a name="generating--running-migrations"></a>Generování a spuštění migrace

Migrace Code First má dva primární příkazy, které se chystáte seznámit se s.

-   **Přidejte migraci** bude generování uživatelského rozhraní další migrace na základě změn, které jste provedli pro váš model, od vytvoření posledního migrace
-   **Aktualizace databáze** se použije čekající migrace do databáze

Potřebujeme scaffold migrace, která se stará o novou vlastnost adresa Url, kterou jsme přidali. **Přidat migrace** příkazu, umožníte nám tyto migrace pojmenujte, stačí pojmenujme náš **AddBlogUrl**.

-   Spustit **přidat migrace AddBlogUrl** příkazu v konzole Správce balíčků
-   V **migrace** složky teď máme nový **AddBlogUrl** migrace. Název souboru migrace předem vyřešen s časovým razítkem usnadňující řazení

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogUrl : DbMigration
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

Nyní jsme může upravit nebo přidat do této migrace ale všechno, co vypadá poměrně dobře. Použijeme **aktualizace databáze** použít tuto migraci do databáze.

-   Spustit **aktualizace databáze** příkazu v konzole Správce balíčků
-   Migrace Code First při porovnání migrace v našich **migrace** složky na základě těch, které se použily k databázi. Je vidět, že **AddBlogUrl** migrace je potřeba použít a spustíme ji.

**MigrationsDemo.BlogContext** databáze je teď aktualizovaný zahrnout **Url** sloupec v **blogy** tabulky.

## <a name="customizing-migrations"></a>Přizpůsobení migrace

Zatím jsme vygeneruje a spuštění migrace beze změn. Nyní Pojďme se podívat na úpravy kódu, který získá vygenerována ve výchozím nastavení.

-   Je čas provést některé další změny náš model, přidáme nový **hodnocení** vlastnost **blogu** třídy

``` csharp
    public int Rating { get; set; }
```

-   Můžeme také přidat nový **příspěvek** třídy

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   Přidáme také **příspěvky** kolekce **blogu** třídy formuláře druhém konci vztahu mezi **blogu** a **příspěvku**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Použijeme **přidat migrace** příkaz, který umožní migrace Code First generování uživatelského rozhraní na migrace pro nás jeho co nejlepší odhad. My budeme volat tuto migraci **AddPostClass**.

-   Spustit **přidat migrace AddPostClass** příkazu v konzole Správce balíčků.

Migrace Code First nebyla tom docela dobře práce pro generování uživatelského rozhraní tyto změny, ale existují některé možnosti, co chceme změnit:

1.  Nejprve nahoru, přidáme jedinečný index k **Posts.Title** sloupec (přidání řádku 22 & 29 v následujícím kódu).
2.  Také přidáváme neumožňující **Blogs.Rating** sloupce. Pokud není žádná existující data v tabulce ji získat přiřadí výchozí CLR datového typu pro nový sloupec (hodnocení je celé číslo, takže, který bude **0**). Chcete zadat výchozí hodnotu, ale **3** tak v tomto existujícím řádků **blogy** tabulky budou začínat vrazíme hodnocení.
    (Zobrazí se výchozí hodnota zadaná na řádku 24 kód uvedený níže)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostClass : DbMigration
        {
            public override void Up()
            {
                CreateTable(
                    "dbo.Posts",
                    c => new
                        {
                            PostId = c.Int(nullable: false, identity: true),
                            Title = c.String(maxLength: 200),
                            Content = c.String(),
                            BlogId = c.Int(nullable: false),
                        })
                    .PrimaryKey(t => t.PostId)
                    .ForeignKey("dbo.Blogs", t => t.BlogId, cascadeDelete: true)
                    .Index(t => t.BlogId)
                    .Index(p => p.Title, unique: true);

                AddColumn("dbo.Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropIndex("dbo.Posts", new[] { "Title" });
                DropIndex("dbo.Posts", new[] { "BlogId" });
                DropForeignKey("dbo.Posts", "BlogId", "dbo.Blogs");
                DropColumn("dbo.Blogs", "Rating");
                DropTable("dbo.Posts");
            }
        }
    }
```

Naše upravených migrace je připraven k přejít, takže použijeme **aktualizace databáze** uveďte aktuální databázi. Nyní můžeme zadat **– Verbose** příznak, kde můžete zobrazit SQL, na kterém běží migrace Code First.

-   Spustit **aktualizace databáze – Verbose** příkazu v konzole Správce balíčků.

## <a name="data-motion--custom-sql"></a>Pohyb dat nebo vlastními SQL

Zatím jsme se podívat na migraci, které operace, které nechcete změnit nebo přesunout všechna data, teď Pojďme se podívat na něco, kterou je potřeba pohyb nějaká data. Neexistuje žádná nativní podpora pro pohybu dat ještě, ale můžeme spustit některé libovolné příkazy SQL v libovolném bodě ve skriptu.

-   Přidejme **Post.Abstract** vlastnost našeho modelu. Později, přejdeme k předběžnému naplnění **abstraktní** existující příspěvky od začátku pomocí nějaký text **obsahu** sloupce.

``` csharp
    public string Abstract { get; set; }
```

Použijeme **přidat migrace** příkaz, který umožní migrace Code First generování uživatelského rozhraní na migrace pro nás jeho co nejlepší odhad.

-   Spustit **přidat migrace AddPostAbstract** příkazu v konzole Správce balíčků.
-   Vygenerovaný migrace se postará o změny schématu, ale také chceme k předběžnému naplnění **abstraktní** sloupce pomocí prvních 100 znaků obsahu pro jednotlivé příspěvky. Můžeme to udělat přetažením na SQL a spuštění **aktualizace** příkaz poté, co se má přidat sloupec.
    (Přidání řádku 12 v následujícím kódu)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostAbstract : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Posts", "Abstract", c => c.String());

                Sql("UPDATE dbo.Posts SET Abstract = LEFT(Content, 100) WHERE Abstract IS NULL");
            }

            public override void Down()
            {
                DropColumn("dbo.Posts", "Abstract");
            }
        }
    }
```

Naše upravených migrace je v pořádku, takže použijeme **aktualizace databáze** uveďte aktuální databázi. Budete určíme **– Verbose** příznak, jsme viděli SQL spuštěn na databázi.

-   Spustit **aktualizace databáze – Verbose** příkazu v konzole Správce balíčků.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Migrace na konkrétní verzi (včetně Downgrade)

Zatím jsme provedli vždy upgrade na nejnovější migrace, ale může nastat situace, kdy chcete upgradovat nebo downgradovat na konkrétní migrace.

Řekněme, že chceme migrovat databáze do stavu byl po spuštění našich **AddBlogUrl** migrace. Můžeme použít **– TargetMigration** přepínač na starší verzi této migrace.

-   Spustit **aktualizace databáze – TargetMigration: AddBlogUrl** příkazu v konzole Správce balíčků.

Tento příkaz spustí skript dolů pro naše **AddBlogAbstract** a **AddPostClass** migrace.

Pokud chcete úplně vrátit zpět na prázdnou databázi, pak můžete použít **aktualizace databáze – TargetMigration: $InitialDatabase** příkazu.

## <a name="getting-a-sql-script"></a>Získávání skript SQL

Pokud jiný vývojář chce, aby se tyto změny v jejich počítači se jenom synchronizovat až zkontrolujeme naše změny do správy zdrojového kódu. Jakmile budou mít naše nové migrace jsou pouze spustit příkaz aktualizace databáze chcete-li změny použít místně. Ale pokud se mají tyto změny vydat na testovací server a nakonec produkčního prostředí, chceme pravděpodobně skriptu SQL, který jsme můžete předat do našich DBA.

-   Spustit **aktualizace databáze** příkazu, ale tentokrát zadejte **– skript** příznak tak, že změny jsou zapsány do skriptu namísto použití. Také určíte zdrojovou a cílovou migrace se vygenerovat skript pro. Chceme, aby skript, který bude směřovat prázdnou databázi (**$InitialDatabase**) na nejnovější verzi (migrace **AddPostAbstract**).
    *Pokud nezadáte target migrace, migrace použije nejnovější migrace jako cíl. Pokud nechcete zadat zdroj migrace, migrace použije aktuální stav databáze.*
-   Spustit **aktualizace databáze-skript - SourceMigration: $InitialDatabase - TargetMigration: AddPostAbstract** příkazu v konzole Správce balíčků

Migrace Code First, spustí se kanál migrace, ale místo ve skutečnosti použití změn ji bude vypsat je do souboru .sql za vás. Po vygenerování skript ho je pro vás otevřen v aplikaci Visual Studio, které jsou připravené k zobrazení nebo uložení.

### <a name="generating-idempotent-scripts"></a>Generování skriptů Idempotentní

Počínaje EF6, pokud zadáte **– SourceMigration $InitialDatabase** generovaný skript bude "idempotentní". Idempotentní skripty můžete upgradovat databázi aktuálně na žádné verze na nejnovější verzi (nebo zadaná verze, pokud používáte **– TargetMigration**). Generovaný skript obsahuje logiku pro kontrolu  **\_ \_MigrationsHistory** tabulky a pouze použití změn, které dříve nebyly použity.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Automaticky upgrade při spuštění aplikace (MigrateDatabaseToLatestVersion se inicializátor).

Pokud nasazujete aplikaci můžete ho automaticky upgradovat databázi (s použitím čekající migrace) při spuštění aplikace. Můžete to provést tak, že zaregistrujete **MigrateDatabaseToLatestVersion** inicializátor databáze. Inicializátor databáze obsahuje některé logiku, která se používá k Ujistěte se, že databáze je správně nastavený. Tato logika je spustit při prvním kontextu se používá v rámci procesu aplikace (**AppDomain**).

Abychom mohli aktualizovat **Program.cs** souboru, jak je znázorněno níže, chcete-li nastavit **MigrateDatabaseToLatestVersion** inicializátor pro BlogContext dřív, než kontextu (řádek 14). Všimněte si, že musíte také přidat sadu pomocí příkazu pro **System.Data.Entity** obor názvů (řádku 5).

*Když vytvoříme instanci této inicializátoru, musíme určit typ kontextu (**BlogContext**) a konfiguraci migrace (**konfigurace**) – konfigurace migrace je třída, která je teď Přidá do našich **migrace** složky, pokud jsme povolili migrace.*

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Data.Entity;
    using MigrationsDemo.Migrations;

    namespace MigrationsDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                Database.SetInitializer(new MigrateDatabaseToLatestVersion\<BlogContext, Configuration>());

                using (var db = new BlogContext())
                {
                    db.Blogs.Add(new Blog { Name = "Another Blog " });
                    db.SaveChanges();

                    foreach (var blog in db.Blogs)
                    {
                        Console.WriteLine(blog.Name);
                    }
                }

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }
        }
    }
```

Nyní pokaždé, když naše spouštět aplikace, nejprve zjistí, pokud databáze je zaměřen na aktuální a použít všechny probíhající migrace, pokud není.
