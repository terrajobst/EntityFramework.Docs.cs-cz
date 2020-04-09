---
title: Migrace prvního kódu – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78418962"
---
# <a name="code-first-migrations"></a>První migrace kódu
Migrace code first je doporučený způsob, jak vyvíjet schéma databáze vaší aplikace, pokud používáte pracovní postup Code First. Migrace poskytují sadu nástrojů, které umožňují:

1. Vytvoření počáteční databáze, která funguje s modelem EF
2. Generování migrace pro sledování změn, které provedete v modelu EF
2. Udržujte databázi aktuální s těmito změnami

Následující návod bude poskytovat přehled migrace code first v rámci entity. Můžete buď dokončit celý návod, nebo přeskočit na téma, které vás zajímá. Jsou popsána následující témata:

## <a name="building-an-initial-model--database"></a>Vytváření počátečního modelu & databáze

Než začneme používat migrace, potřebujeme projekt a model Code First pro práci s. Pro tento návod budeme používat kanonický **blog** a **post** model.

-   Vytvoření nové aplikace **MigrationsDemo** Console
-   Přidání nejnovější verze balíčku **EntityFramework** NuGet do projektu
    -   **Nástroje&gt; – Správce&gt; balíčků knihovny – konzola Správce balíčků**
    -   Spuštění příkazu **Install-Package EntityFramework**
-   Přidejte **soubor Model.cs** s níže uvedeným kódem. Tento kód definuje jednu třídu **Blog,** která tvoří náš model domény a třídu **BlogContext,** která je naším kontextem EF Code First

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

-   Teď, když máme model, je čas ho použít k provedení přístupu k datům. Aktualizujte soubor **Program.cs** s níže uvedeným kódem.

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

-   Spusťte aplikaci a uvidíte, že **MigrationsCodeDemo.BlogContext** databáze je vytvořena pro vás.

    ![Databáze LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Povolení migrace

Je čas udělat nějaké další změny v našem modelu.

-   Pojďme představit url vlastnost blog třídy.

``` csharp
    public string Url { get; set; }
```

Pokud byste měli spustit aplikaci znovu byste získat InvalidOperationException oznamující *model podporující kontext 'BlogContext' kontext se změnil od vytvoření databáze. Zvažte použití migrace code first k aktualizaci databáze (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Jak naznačuje výjimka, je čas začít používat migrace Code First. Prvním krokem je umožnit migraci pro náš kontext.

-   Spuštění příkazu **Enable-Migrations** v konzole Správce balíčků

    Tento příkaz přidal do našeho projektu složku **Migrace.** Tato nová složka obsahuje dva soubory:

-   **Configuration třídy.** Tato třída umožňuje nakonfigurovat, jak se migrace chová pro váš kontext. Pro tento návod použijeme pouze výchozí konfiguraci.
    *Vzhledem k tomu, že je pouze jeden kontext Code First v projektu, Enable-Migrations automaticky vyplnil v typu kontextu této konfigurace se vztahuje.*
-   **Počáteční Vytvoření migrace**. Tato migrace byla vygenerována, protože jsme již měli Code First vytvořit databázi pro nás, než jsme povolili migrace. Kód v této šástavlo migrace představuje objekty, které již byly vytvořeny v databázi. V našem případě je tabulka **Blog** se **sloupci BlogId** a **Name.** Název souboru obsahuje časové razítko, které vám pomůže s řazením.
    *Pokud databáze nebyla již vytvořena tato migrace InitialCreate by nebyly přidány do projektu. Místo toho při prvním volání Add-Migration kód k vytvoření těchto tabulek by být šetrné k vytvoření nové migrace.*

### <a name="multiple-models-targeting-the-same-database"></a>Více modelů zaměřených na stejnou databázi

Při použití verze před EF6 pouze jeden model Code First lze použít ke generování nebo správě schématu databáze. Toto je výsledek jedné ** \_ \_MigrationsHistory** tabulka na databázi s žádný způsob, jak zjistit, které položky patří do které modelu.

Počínaje EF6, **Configuration** třída obsahuje **ContextKey** vlastnost. To funguje jako jedinečný identifikátor pro každý model Code First. Odpovídající sloupec v ** \_ \_tabulce MigrationsHistory** umožňuje položkám z více modelů sdílet tabulku. Ve výchozím nastavení je tato vlastnost nastavena na plně kvalifikovaný název kontextu.

## <a name="generating--running-migrations"></a>Generování & běžícímigrace

Migrace Code First má dva primární příkazy, se kterými se seznámíte.

-   **Přidání migrace** bude zasévat další migraci na základě změn, které jste provedli v modelu od vytvoření poslední migrace
-   **Update-Database** použije všechny čekající migrace do databáze

Musíme vytvořit uživatelské zařízení pro migraci, abychom se postarali o novou vlastnost URL, kterou jsme přidali. **Příkaz Přidat-migrace** nám umožňuje dát tyto migrace název, pojďme jen volat naše **AddBlogUrl**.

-   Spuštění příkazu **Add-Migration AddBlogUrl** v konzole Správce balíčků
-   Ve složce **Migrace** máme nyní novou migraci **AddBlogUrl.** Název migračního souboru je předem opraven časovým razítkem, které vám pomůže s řazením

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

Nyní bychom mohli upravit nebo přidat k této migraci, ale všechno vypadá docela dobře. Pojďme použít **Update-Database** použít tuto migraci do databáze.

-   Spuštění příkazu **Aktualizovat databázi** v konzole Správce balíčků
-   Migrace Code First porovná migrace ve složce **Migrace** s těmi, které byly použity v databázi. Uvidí, že migrace **AddBlogUrl** musí být použita a spustit.

Databáze **MigrationsDemo.BlogContext** je nyní aktualizována tak, aby zahrnovala sloupec **URL** v tabulce **Blogy.**

## <a name="customizing-migrations"></a>Přizpůsobení migrace

Zatím jsme vygenerovali a spouštěli migraci bez jakýchkoli změn. Nyní se podívejme na úpravy kódu, který se generuje ve výchozím nastavení.

-   Je čas provést další změny v našem modelu, přidáme novou vlastnost **Hodnocení** do třídy **Blog**

``` csharp
    public int Rating { get; set; }
```

-   Přidáme také novou třídu **Post**

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

-   Přidáme také **kolekci Příspěvky** do třídy **Blog,** abychom vytvořili druhý konec vztahu mezi **blogem** a **příspěvkem**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Použijeme příkaz **Přidat-migrace,** abychom nechali code first migrations zakódovat svůj nejlepší odhad migrace pro nás. Budeme volat tuto migraci **AddPostClass**.

-   Spusťte příkaz **Add-Migration AddPostClass** v konzole Správce balíčků.

Code First Migrations odvedli docela dobrou práci při lešení těchto změn, ale existují některé věci, které bychom mohli chtít změnit:

1.  Nejprve přidáme do sloupce **Posts.Title** jedinečný index (Přidání do řádku 22 & 29 v níže uvedeném kódu).
2.  Přidáváme také sloupec **Blogs,** který neuplatní její platnost. Pokud jsou v tabulce nějaká existující data, bude mu přiřazeno výchozí nastavení CLR datového typu pro nový sloupec (Hodnocení je celé číslo, takže by to bylo **0).** Chceme však určit výchozí hodnotu **3,** aby stávající řádky v tabulce **Blogy začaly** se slušným hodnocením.
    (Výchozí hodnotu zadanou na řádku 24 níže uvedeného kódu)

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

Naše upravená migrace je připravena k přechodu, proto pomocí **aktualizace databáze** aktualizujte databázi. Tentokrát pojďme zadat **–Verbose** příznak tak, aby můžete vidět SQL, které je spuštěna migrace code first.

-   Spusťte příkaz **Update-Database –Verbose** v konzole Správce balíčků.

## <a name="data-motion--custom-sql"></a>Pohyb dat / Vlastní SQL

Zatím jsme se podívali na migrační operace, které nemění ani nepřesouvají žádná data, nyní se podívejme na něco, co potřebuje přesunout některá data. Neexistuje žádná nativní podpora pro pohyb dat ještě, ale můžeme spustit některé libovolné příkazy SQL v libovolném bodě v našem skriptu.

-   Pojďme přidat **Post.Abstract** vlastnost našeho modelu. Později budeme předem vyplnit **Abstrakt** pro existující příspěvky pomocí nějakého textu od začátku sloupce **Obsah.**

``` csharp
    public string Abstract { get; set; }
```

Použijeme příkaz **Přidat-migrace,** abychom nechali code first migrations zakódovat svůj nejlepší odhad migrace pro nás.

-   Spusťte příkaz **Add-Migration Add-Migration AddPostAbstract** v konzole Správce balíčků.
-   Vygenerovaná migrace se postará o změny schématu, ale chceme také předem vyplnit sloupec **Abstract** pomocí prvních 100 znaků obsahu pro každý příspěvek. Můžeme to udělat tím, že klesne na SQL a spuštění **příkazu UPDATE** po přidání sloupce.
    (Přidání do řádku 12 v níže uvedeném kódu)

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

Naše upravená migrace vypadá dobře, proto použijte **update-database** k aktualizaci databáze aktuální. Zadáme příznak **–Verbose,** abychom viděli, že sql je spuštěna proti databázi.

-   Spusťte příkaz **Update-Database –Verbose** v konzole Správce balíčků.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Migrace na konkrétní verzi (včetně přechodu na nižší verzi)

Zatím jsme vždy upgradovali na nejnovější migraci, ale mohou nastaly časy, kdy chcete upgradovat / downgrade na konkrétní migraci.

Řekněme, že chceme migrovat naši databázi do stavu, ve kterém byla po spuštění migrace **AddBlogUrl.** Můžeme použít **-TargetMigration** přepínač downgrade na tuto migraci.

-   Spusťte příkaz **Update-Database –TargetMigration: AddBlogUrl** v konzole Správce balíčků.

Tento příkaz spustí skript Dolů pro naše migrace **AddBlogAbstract** a **AddPostClass.**

Pokud chcete vrátit celou cestu zpět do prázdné databáze pak můžete použít **Update-Database -TargetMigration: $InitialDatabase** příkaz.

## <a name="getting-a-sql-script"></a>Získání skriptu SQL

Pokud jiný vývojář chce tyto změny na svém počítači, které můžete synchronizovat, jakmile zkontrolujeme naše změny do správy zdrojového kódu. Jakmile mají naše nové migrace, stačí spustit příkaz Aktualizovat databázi, aby změny byly použity místně. Nicméně pokud chceme tlačit tyto změny na testovací server a nakonec výroby, pravděpodobně chceme skript SQL můžeme předat naše DBA.

-   Spusťte příkaz **Aktualizovat databázi,** ale tentokrát zadejte příznak **–Script** tak, aby změny byly zapsány do skriptu, nikoli použity. Také určíme zdrojovou a cílovou migraci, pro kterou bude skript vygenerován. Chceme, aby skript přešel z prázdné databáze **($InitialDatabase)** na nejnovější verzi (migrace **AddPostAbstract).**
    *Pokud cílovou migraci nezadáte, migrace použije jako cíl nejnovější migraci. Pokud nezadáte zdrojové migrace, migrace bude používat aktuální stav databáze.*
-   Spuštění příkazu **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** v konzole Správce balíčků

Migrace Code First spustí kanál migrace, ale místo toho, aby skutečně použilzměny, zapíše je do souboru .sql za vás. Jakmile je skript vygenerován, otevře se pro vás v sadě Visual Studio, připravený k zobrazení nebo uložení.

### <a name="generating-idempotent-scripts"></a>Generování idempotentních skriptů

Počínaje EF6, pokud zadáte **–SourceMigration $InitialDatabase** pak vygenerovaný skript bude "idempotentní". Idempotentní skripty můžete upgradovat databázi aktuálně v libovolné verzi na nejnovější verzi (nebo zadanou verzi, pokud používáte **-TargetMigration**). Vygenerovaný skript obsahuje ** \_ \_** logiku pro kontrolu tabulky MigrationsHistory a platí pouze změny, které nebyly dříve použity.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Automatická inovace při spuštění aplikace (MigrateDatabaseToLatestVersion Initializer)

Pokud nasazujete aplikaci, můžete chtít, aby automaticky upgradovala databázi (použitím všech čekajících migrací) při spuštění aplikace. Můžete to provést registrací **MigrateDatabaseToLatestVersion** inicializátoru databáze. Inicializátor databáze jednoduše obsahuje některé logiky, která se používá k ujistěte se, že databáze je správně nastavena. Tato logika je spuštěna při prvním použití kontextu v rámci procesu aplikace **(AppDomain**).

Můžeme aktualizovat **Program.cs** soubor, jak je znázorněno níže, nastavit **MigrateDatabaseToLatestVersion** inicializátor pro BlogContext před použitím kontextu (Řádek 14). Všimněte si, že je také nutné přidat using prohlášení pro **System.Data.Entity** obor názvů (řádek 5).

*Když vytvoříme instanci tohoto inicializátoru, musíme zadat typ kontextu (**BlogContext**) a konfiguraci migrace (**Konfigurace**) - konfigurace migrace je třída, která byla přidána do naší složky **Migrace,** když jsme povolili migrace.*

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
                Database.SetInitializer(new MigrateDatabaseToLatestVersion<BlogContext, Configuration>());

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

Nyní vždy, když naše aplikace spustí, bude nejprve zkontrolovat, zda databáze je cílení je aktuální a použít všechny čekající migrace, pokud tomu tak není.
