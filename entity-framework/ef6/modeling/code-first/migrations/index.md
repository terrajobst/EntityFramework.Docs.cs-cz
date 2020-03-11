---
title: Migrace Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418962"
---
# <a name="code-first-migrations"></a>Migrace Code First
Migrace Code First je doporučený způsob, jak vyvíjet schéma databáze vaší aplikace, pokud používáte pracovní postup Code First. Migrace poskytují sadu nástrojů, které umožňují:

1. Vytvoření počáteční databáze, která funguje s vaším modelem EF
2. Generování migrace pro sledování změn, které provedete v modelu EF
2. Udržujte databázi v aktuálním stavu pomocí těchto změn

Následující návod vám poskytne přehled Migrace Code First v Entity Framework. Můžete buď dokončit celý návod, nebo přeskočit k tématu, které vás zajímá. Jsou pokryta následující témata:

## <a name="building-an-initial-model--database"></a>Vytvoření počátečního & databáze modelu

Než začneme používat migrace, potřebujeme projekt a model Code First, se kterým pracujete. V tomto návodu budeme používat kanonický **blog** a model **post** .

-   Vytvořit novou konzolovou aplikaci **MigrationsDemo**
-   Přidejte do projektu nejnovější verzi balíčku NuGet **EntityFramework** .
    -   **Nástroje –&gt; správce balíčků knihovny –&gt; konzolu Správce balíčků**
    -   Spuštění příkazu **Install-Package EntityFramework**
-   Přidejte soubor **model.cs** s kódem zobrazeným níže. Tento kód definuje jednu třídu **blogu** , která poskytuje náš doménový model a třídu **BlogContext** , která je náš Code First kontextem EF.

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

-   Teď, když máme model, je čas ho použít k provedení přístupu k datům. Aktualizujte soubor **program.cs** pomocí kódu uvedeného níže.

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

-   Spusťte aplikaci a zobrazí se vám pro vás vytvořená databáze **MigrationsCodeDemo. BlogContext** .

    ![LocalDB databáze](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Povolování migrací

Je čas udělat další změny v našem modelu.

-   Pojďme do třídy blogu přivést vlastnost URL.

``` csharp
    public string Url { get; set; }
```

Pokud byste chtěli aplikaci znovu spustit, měli byste obdržet zprávu, že došlo ke *změně modelu zálohování kontextu ' BlogContext ', protože databáze byla vytvořena. Pokud chcete aktualizovat databázi (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *)* , zvažte použití migrace Code First.

Vzhledem k tomu, že výjimka navrhuje, je čas začít používat Migrace Code First. Prvním krokem je povolit migrace pro náš kontext.

-   Spuštění příkazu **Povolit – migrace** v konzole správce balíčků

    Tento příkaz přidal do našeho projektu složku **migrace** . Tato nová složka obsahuje dva soubory:

-   **Třída Configuration** Tato třída umožňuje nakonfigurovat, jak se budou migrace chovat pro váš kontext. V tomto návodu použijeme jenom výchozí konfiguraci.
    *Vzhledem k tomu, že v projektu existuje jen jeden Code First kontext, Enable – migrace se automaticky vyplní v kontextu typu, pro který je tato konfigurace platná.*
-   **Migrace InitialCreate** Tato migrace se vygenerovala, protože už jsme Code First vytvořit databázi pro nás, než jsme povolili migrace. Kód v této vygenerované migraci představuje objekty, které již byly vytvořeny v databázi. V našem případě je to tabulka **blogu** se sloupci **BlogId** a **Name** . Název souboru obsahuje časové razítko, které vám pomůžou s řazením.
    *Pokud databáze ještě není vytvořená, migrace InitialCreate by se do projektu nepřidala. Místo toho se při prvním volání metody přidání migrace kód pro vytvoření těchto tabulek vytvoří při nové migraci na základě uživatelského rozhraní.*

### <a name="multiple-models-targeting-the-same-database"></a>Několik modelů cílících na stejnou databázi

Při používání verzí starších než EF6 může být pro generování a správu schématu databáze použit pouze jeden model Code First. Jedná se o výsledek jednoho **\_\_tabulce MigrationsHistory** na databázi bez možnosti určit, které položky patří do daného modelu.

Počínaje EF6 třída **Configuration** zahrnuje vlastnost **ContextKey** . To funguje jako jedinečný identifikátor pro každý model Code First. Odpovídající sloupec v tabulce **\_\_MigrationsHistory** umožňuje položkám z více modelů sdílet tabulku. Ve výchozím nastavení je tato vlastnost nastavena na plně kvalifikovaný název vašeho kontextu.

## <a name="generating--running-migrations"></a>Generují se & spouští migrace.

Migrace Code First má dva primární příkazy, se kterými se seznámíte.

-   **Přidání – při migraci** dojde k další migraci vygenerované na základě změn, které jste provedli v modelu od vytvoření poslední migrace.
-   **Aktualizace – databáze** bude používat všechny nedokončené migrace do databáze.

Abychom se postaral o novou vlastnost URL, kterou jsme přidali, potřebujeme vytvořit nové uživatelské rozhraní. Příkaz **Add-Migration** nám umožňuje dát těmto migracim název, Pojďme ale volat náš **AddBlogUrl**.

-   Spuštění příkazu **Add-Migration AddBlogUrl** v konzole správce balíčků
-   Ve složce **migrace** teď máme novou migraci **AddBlogUrl** . Název souboru migrace je předem vyřešen s časovým razítkem, které vám pomůžou s řazením.

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

Tuto migraci teď můžeme upravit nebo přidat, ale vše vypadá poměrně dobré. Pojďme použít tuto migraci do databáze pomocí **Update-Database** .

-   Spuštění příkazu **Update-Database** v konzole správce balíčků
-   Migrace Code First porovná migrace v našich složkách **migrace** s těmi, které byly použity pro databázi. Uvidí, že je potřeba použít migraci **AddBlogUrl** a spustit ji.

Databáze **MigrationsDemo. BlogContext** se teď aktualizovala tak, aby obsahovala sloupec **URL** v tabulce **Blogy** .

## <a name="customizing-migrations"></a>Přizpůsobení migrací

Zatím jsme vygenerovali a spustili migraci bez provedení změn. Teď se podíváme na úpravu kódu, který se vygeneruje ve výchozím nastavení.

-   Je čas udělat další změny v našem modelu. Pojďme do třídy **blogu** přidat novou vlastnost **hodnocení** .

``` csharp
    public int Rating { get; set; }
```

-   Pojďme také přidat novou třídu **post**

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

-   Do třídy **blog** přidáme také kolekci **příspěvky** , která bude tvořit druhý konec relace mezi **blogem** a **příspěvkem** .

``` csharp
    public virtual List<Post> Posts { get; set; }
```

K umožnění Migrace Code Firstho uživatelského rozhraní využijeme k dispozici nejlepší odhad na migraci pro nás pomocí příkazu **Add-Migration** . Budeme volat tuto **AddPostClass**migrace.

-   Spusťte příkaz **Add-Migration AddPostClass** v konzole správce balíčků.

Migrace Code First pro tyto změny bylo poměrně dobré úlohy, ale můžete chtít změnit několik věcí:

1.  Nejprve přidáme jedinečný index do **příspěvku. sloupec title** (přidávání na řádku 22 & 29 v kódu níže).
2.  Také přidáváme do tohoto sloupce **hodnocení** , které neumožňují hodnotu null. Pokud v tabulce existují nějaká existující data, zobrazí se jim výchozí hodnota CLR datového typu pro nový sloupec (hodnocení je celé číslo, takže by to bylo **0**). Ale chceme zadat výchozí hodnotu **3** , aby se stávající řádky v tabulce **Blogy** spouštěly se hodnocením dát.
    (Můžete zobrazit výchozí hodnotu zadanou na řádku 24 v kódu níže)

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

Naše upravená migrace je připravená k tomu, takže pomocí **Update-Database** přineseme databázi do aktuálního stavu. Tentokrát je nutné zadat příznak **– verbose** , abyste viděli SQL, na kterém je spuštěný migrace Code First.

-   Spusťte příkaz **Update-Database – verbose** v konzole správce balíčků.

## <a name="data-motion--custom-sql"></a>Pohyb dat/vlastní SQL

Zatím jsme si prohlédli operace migrace, které nemění ani nepřesouvá žádná data, teď se podíváme na něco, co potřebuje přesunout některá data. Ještě není k dispozici žádná nativní podpora pro pohyb dat, ale v jakémkoli okamžiku v našem skriptu můžeme spustit libovolné příkazy SQL.

-   Pojďme do našeho modelu přidat vlastnost **post. Abstract** . Později teď vyplníme **abstrakci** pro existující příspěvky pomocí textu, který se nachází na začátku sloupce **Content (obsah** ).

``` csharp
    public string Abstract { get; set; }
```

K umožnění Migrace Code Firstho uživatelského rozhraní využijeme k dispozici nejlepší odhad na migraci pro nás pomocí příkazu **Add-Migration** .

-   Spusťte příkaz **Add-Migration AddPostAbstract** v konzole správce balíčků.
-   Vygenerovaná migrace se stará o změny schématu, ale chceme také předem naplnit **abstraktní** sloupec s použitím prvních 100 znaků obsahu pro každý příspěvek. To můžeme udělat vyřazením dolů na SQL a spuštěním příkazu **Update** po přidání sloupce.
    (Přidání na řádku 12 v kódu níže)

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

Naše upravená migrace je dobrá, takže použijeme **příkaz Update-Database** , aby se databáze aktualizovala v aktuálním stavu. Určíme příznak **– verbose** , aby bylo možné vidět, že je SQL spuštěný proti databázi.

-   Spusťte příkaz **Update-Database – verbose** v konzole správce balíčků.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Migrace na konkrétní verzi (včetně downgradu)

Zatím jsme se vždycky upgradovali na nejnovější migraci, ale může nastat situace, kdy budete chtít upgradovat nebo downgradovat na konkrétní migraci.

Řekněme, že chceme migrovat naši databázi do stavu, ve kterém byl po spuštění naší migrace **AddBlogUrl** . Pro přechod na tuto migraci můžeme použít přepínač **– TargetMigration** .

-   Spusťte příkaz **Update-Database – TargetMigration: AddBlogUrl** v konzole správce balíčků.

Tento příkaz spustí skript pro migrace našich **AddBlogAbstract** a **AddPostClass** .

Pokud chcete vrátit všechny možnosti zpátky do prázdné databáze, můžete použít příkaz **Update-Database – TargetMigration: $InitialDatabase** .

## <a name="getting-a-sql-script"></a>Získání skriptu SQL

Pokud jiný vývojář tuto změnu na svém počítači přeje, může se po kontrole změn do správy zdrojových kódů synchronizovat jenom jednou. Jakmile naši nové migrace dostanou, stačí spustit příkaz Update-Database, aby se změny používaly lokálně. Pokud ale chceme tyto změny nabízet na testovacím serveru a nakonec v produkčním prostředí, nejspíš chceme, aby se skript SQL, který můžeme předat našímu DBA.

-   Spusťte příkaz **Update-Database** , ale tentokrát určete příznak **– Script** , aby se změny zapsaly do skriptu místo použití. Také určíme migraci zdroje a cíle pro vygenerování skriptu pro. Chceme, aby se skript přešel z prázdné databáze ( **$InitialDatabase**) na nejnovější verzi ( **AddPostAbstract**migrace).
    *Pokud nezadáte cílovou migraci, budou migrace používat jako cíl nejnovější migraci. Pokud neurčíte zdrojová migrace, budou migrace používat aktuální stav databáze.*
-   Spusťte příkaz **Update-Database-Script-SourceMigration: $InitialDatabase-TargetMigration: AddPostAbstract** v konzole správce balíčků.

Migrace Code First spustí kanál migrace, ale místo toho, aby se změny projevily, ho za vás zapíše do souboru. SQL. Po vygenerování skriptu je tento skript otevřený pro vás v aplikaci Visual Studio, který jste připraveni k zobrazení nebo uložení.

### <a name="generating-idempotent-scripts"></a>Generování skriptů Idempotentní

Začínáte-li s EF6, pokud zadáte **– SourceMigration $InitialDatabase** pak bude vygenerovaný skript "idempotentní". Idempotentní skripty mohou upgradovat databázi aktuálně v libovolné verzi na nejnovější verzi (nebo na zadanou verzi, pokud používáte **– TargetMigration**). Vygenerovaný skript obsahuje logiku pro kontrolu **\_tabulky \_MigrationsHistory** a pouze změny, které se předtím nepoužily.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Automatické upgradování při spuštění aplikace (inicializátor MigrateDatabaseToLatestVersion)

Pokud nasazujete aplikaci, možná budete chtít, aby při spuštění aplikace automaticky upgradovali databázi (pomocí všech nedokončených migrací). To můžete provést tak, že zaregistrujete inicializátor databáze **MigrateDatabaseToLatestVersion** . Inicializátor databáze jednoduše obsahuje logiku, která se používá k zajištění správného nastavení databáze. Tato logika se spustí při prvním použití kontextu v procesu aplikace (**AppDomain**).

Soubor **program.cs** můžeme aktualizovat, jak je vidět níže, pro nastavení inicializátoru **MigrateDatabaseToLatestVersion** pro BlogContext předtím, než použijeme kontext (řádek 14). Všimněte si, že je také nutné přidat příkaz using pro obor názvů **System. data. entity** (řádek 5).

*Když vytvoříme instanci tohoto inicializátoru, musíme zadat typ kontextu (**BlogContext**) a konfiguraci migrace (**Konfigurace**) – konfigurace migrace je třída, která se přidala do naší složky **migrace** po povolení migrace.*

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

Teď, když se aplikace spustí, nejdřív zkontroluje, jestli je databáze, na kterou cílí, v aktuálním stavu, a pokud není, použijte všechny nedokončené migrace.
