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
# <a name="code-first-migrations"></a><span data-ttu-id="02961-102">První migrace kódu</span><span class="sxs-lookup"><span data-stu-id="02961-102">Code First Migrations</span></span>
<span data-ttu-id="02961-103">Migrace code first je doporučený způsob, jak vyvíjet schéma databáze vaší aplikace, pokud používáte pracovní postup Code First.</span><span class="sxs-lookup"><span data-stu-id="02961-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="02961-104">Migrace poskytují sadu nástrojů, které umožňují:</span><span class="sxs-lookup"><span data-stu-id="02961-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="02961-105">Vytvoření počáteční databáze, která funguje s modelem EF</span><span class="sxs-lookup"><span data-stu-id="02961-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="02961-106">Generování migrace pro sledování změn, které provedete v modelu EF</span><span class="sxs-lookup"><span data-stu-id="02961-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="02961-107">Udržujte databázi aktuální s těmito změnami</span><span class="sxs-lookup"><span data-stu-id="02961-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="02961-108">Následující návod bude poskytovat přehled migrace code first v rámci entity.</span><span class="sxs-lookup"><span data-stu-id="02961-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="02961-109">Můžete buď dokončit celý návod, nebo přeskočit na téma, které vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="02961-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="02961-110">Jsou popsána následující témata:</span><span class="sxs-lookup"><span data-stu-id="02961-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="02961-111">Vytváření počátečního modelu & databáze</span><span class="sxs-lookup"><span data-stu-id="02961-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="02961-112">Než začneme používat migrace, potřebujeme projekt a model Code First pro práci s.</span><span class="sxs-lookup"><span data-stu-id="02961-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="02961-113">Pro tento návod budeme používat kanonický **blog** a **post** model.</span><span class="sxs-lookup"><span data-stu-id="02961-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="02961-114">Vytvoření nové aplikace **MigrationsDemo** Console</span><span class="sxs-lookup"><span data-stu-id="02961-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="02961-115">Přidání nejnovější verze balíčku **EntityFramework** NuGet do projektu</span><span class="sxs-lookup"><span data-stu-id="02961-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="02961-116">**Nástroje&gt; – Správce&gt; balíčků knihovny – konzola Správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="02961-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="02961-117">Spuštění příkazu **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="02961-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="02961-118">Přidejte **soubor Model.cs** s níže uvedeným kódem.</span><span class="sxs-lookup"><span data-stu-id="02961-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="02961-119">Tento kód definuje jednu třídu **Blog,** která tvoří náš model domény a třídu **BlogContext,** která je naším kontextem EF Code First</span><span class="sxs-lookup"><span data-stu-id="02961-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="02961-120">Teď, když máme model, je čas ho použít k provedení přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="02961-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="02961-121">Aktualizujte soubor **Program.cs** s níže uvedeným kódem.</span><span class="sxs-lookup"><span data-stu-id="02961-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="02961-122">Spusťte aplikaci a uvidíte, že **MigrationsCodeDemo.BlogContext** databáze je vytvořena pro vás.</span><span class="sxs-lookup"><span data-stu-id="02961-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![Databáze LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="02961-124">Povolení migrace</span><span class="sxs-lookup"><span data-stu-id="02961-124">Enabling Migrations</span></span>

<span data-ttu-id="02961-125">Je čas udělat nějaké další změny v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="02961-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="02961-126">Pojďme představit url vlastnost blog třídy.</span><span class="sxs-lookup"><span data-stu-id="02961-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="02961-127">Pokud byste měli spustit aplikaci znovu byste získat InvalidOperationException oznamující *model podporující kontext 'BlogContext' kontext se změnil od vytvoření databáze. Zvažte použití migrace code first k aktualizaci databáze (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="02961-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="02961-128">Jak naznačuje výjimka, je čas začít používat migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="02961-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="02961-129">Prvním krokem je umožnit migraci pro náš kontext.</span><span class="sxs-lookup"><span data-stu-id="02961-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="02961-130">Spuštění příkazu **Enable-Migrations** v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="02961-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="02961-131">Tento příkaz přidal do našeho projektu složku **Migrace.**</span><span class="sxs-lookup"><span data-stu-id="02961-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="02961-132">Tato nová složka obsahuje dva soubory:</span><span class="sxs-lookup"><span data-stu-id="02961-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="02961-133">**Configuration třídy.**</span><span class="sxs-lookup"><span data-stu-id="02961-133">**The Configuration class.**</span></span> <span data-ttu-id="02961-134">Tato třída umožňuje nakonfigurovat, jak se migrace chová pro váš kontext.</span><span class="sxs-lookup"><span data-stu-id="02961-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="02961-135">Pro tento návod použijeme pouze výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="02961-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="02961-136">*Vzhledem k tomu, že je pouze jeden kontext Code First v projektu, Enable-Migrations automaticky vyplnil v typu kontextu této konfigurace se vztahuje.*</span><span class="sxs-lookup"><span data-stu-id="02961-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="02961-137">**Počáteční Vytvoření migrace**.</span><span class="sxs-lookup"><span data-stu-id="02961-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="02961-138">Tato migrace byla vygenerována, protože jsme již měli Code First vytvořit databázi pro nás, než jsme povolili migrace.</span><span class="sxs-lookup"><span data-stu-id="02961-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="02961-139">Kód v této šástavlo migrace představuje objekty, které již byly vytvořeny v databázi.</span><span class="sxs-lookup"><span data-stu-id="02961-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="02961-140">V našem případě je tabulka **Blog** se **sloupci BlogId** a **Name.**</span><span class="sxs-lookup"><span data-stu-id="02961-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="02961-141">Název souboru obsahuje časové razítko, které vám pomůže s řazením.</span><span class="sxs-lookup"><span data-stu-id="02961-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="02961-142">*Pokud databáze nebyla již vytvořena tato migrace InitialCreate by nebyly přidány do projektu. Místo toho při prvním volání Add-Migration kód k vytvoření těchto tabulek by být šetrné k vytvoření nové migrace.*</span><span class="sxs-lookup"><span data-stu-id="02961-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="02961-143">Více modelů zaměřených na stejnou databázi</span><span class="sxs-lookup"><span data-stu-id="02961-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="02961-144">Při použití verze před EF6 pouze jeden model Code First lze použít ke generování nebo správě schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="02961-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="02961-145">Toto je výsledek jedné \*\* \_ \_MigrationsHistory\*\* tabulka na databázi s žádný způsob, jak zjistit, které položky patří do které modelu.</span><span class="sxs-lookup"><span data-stu-id="02961-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="02961-146">Počínaje EF6, **Configuration** třída obsahuje **ContextKey** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="02961-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="02961-147">To funguje jako jedinečný identifikátor pro každý model Code First.</span><span class="sxs-lookup"><span data-stu-id="02961-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="02961-148">Odpovídající sloupec v \*\* \_ \_tabulce MigrationsHistory\*\* umožňuje položkám z více modelů sdílet tabulku.</span><span class="sxs-lookup"><span data-stu-id="02961-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="02961-149">Ve výchozím nastavení je tato vlastnost nastavena na plně kvalifikovaný název kontextu.</span><span class="sxs-lookup"><span data-stu-id="02961-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="02961-150">Generování & běžícímigrace</span><span class="sxs-lookup"><span data-stu-id="02961-150">Generating & Running Migrations</span></span>

<span data-ttu-id="02961-151">Migrace Code First má dva primární příkazy, se kterými se seznámíte.</span><span class="sxs-lookup"><span data-stu-id="02961-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="02961-152">**Přidání migrace** bude zasévat další migraci na základě změn, které jste provedli v modelu od vytvoření poslední migrace</span><span class="sxs-lookup"><span data-stu-id="02961-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="02961-153">**Update-Database** použije všechny čekající migrace do databáze</span><span class="sxs-lookup"><span data-stu-id="02961-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="02961-154">Musíme vytvořit uživatelské zařízení pro migraci, abychom se postarali o novou vlastnost URL, kterou jsme přidali.</span><span class="sxs-lookup"><span data-stu-id="02961-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="02961-155">**Příkaz Přidat-migrace** nám umožňuje dát tyto migrace název, pojďme jen volat naše **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="02961-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="02961-156">Spuštění příkazu **Add-Migration AddBlogUrl** v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="02961-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="02961-157">Ve složce **Migrace** máme nyní novou migraci **AddBlogUrl.**</span><span class="sxs-lookup"><span data-stu-id="02961-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="02961-158">Název migračního souboru je předem opraven časovým razítkem, které vám pomůže s řazením</span><span class="sxs-lookup"><span data-stu-id="02961-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="02961-159">Nyní bychom mohli upravit nebo přidat k této migraci, ale všechno vypadá docela dobře.</span><span class="sxs-lookup"><span data-stu-id="02961-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="02961-160">Pojďme použít **Update-Database** použít tuto migraci do databáze.</span><span class="sxs-lookup"><span data-stu-id="02961-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="02961-161">Spuštění příkazu **Aktualizovat databázi** v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="02961-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="02961-162">Migrace Code First porovná migrace ve složce **Migrace** s těmi, které byly použity v databázi.</span><span class="sxs-lookup"><span data-stu-id="02961-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="02961-163">Uvidí, že migrace **AddBlogUrl** musí být použita a spustit.</span><span class="sxs-lookup"><span data-stu-id="02961-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="02961-164">Databáze **MigrationsDemo.BlogContext** je nyní aktualizována tak, aby zahrnovala sloupec **URL** v tabulce **Blogy.**</span><span class="sxs-lookup"><span data-stu-id="02961-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="02961-165">Přizpůsobení migrace</span><span class="sxs-lookup"><span data-stu-id="02961-165">Customizing Migrations</span></span>

<span data-ttu-id="02961-166">Zatím jsme vygenerovali a spouštěli migraci bez jakýchkoli změn.</span><span class="sxs-lookup"><span data-stu-id="02961-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="02961-167">Nyní se podívejme na úpravy kódu, který se generuje ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="02961-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="02961-168">Je čas provést další změny v našem modelu, přidáme novou vlastnost **Hodnocení** do třídy **Blog**</span><span class="sxs-lookup"><span data-stu-id="02961-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="02961-169">Přidáme také novou třídu **Post**</span><span class="sxs-lookup"><span data-stu-id="02961-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="02961-170">Přidáme také **kolekci Příspěvky** do třídy **Blog,** abychom vytvořili druhý konec vztahu mezi **blogem** a **příspěvkem**</span><span class="sxs-lookup"><span data-stu-id="02961-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="02961-171">Použijeme příkaz **Přidat-migrace,** abychom nechali code first migrations zakódovat svůj nejlepší odhad migrace pro nás.</span><span class="sxs-lookup"><span data-stu-id="02961-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="02961-172">Budeme volat tuto migraci **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="02961-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="02961-173">Spusťte příkaz **Add-Migration AddPostClass** v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="02961-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="02961-174">Code First Migrations odvedli docela dobrou práci při lešení těchto změn, ale existují některé věci, které bychom mohli chtít změnit:</span><span class="sxs-lookup"><span data-stu-id="02961-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="02961-175">Nejprve přidáme do sloupce **Posts.Title** jedinečný index (Přidání do řádku 22 & 29 v níže uvedeném kódu).</span><span class="sxs-lookup"><span data-stu-id="02961-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="02961-176">Přidáváme také sloupec **Blogs,** který neuplatní její platnost.</span><span class="sxs-lookup"><span data-stu-id="02961-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="02961-177">Pokud jsou v tabulce nějaká existující data, bude mu přiřazeno výchozí nastavení CLR datového typu pro nový sloupec (Hodnocení je celé číslo, takže by to bylo **0).**</span><span class="sxs-lookup"><span data-stu-id="02961-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="02961-178">Chceme však určit výchozí hodnotu **3,** aby stávající řádky v tabulce **Blogy začaly** se slušným hodnocením.</span><span class="sxs-lookup"><span data-stu-id="02961-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="02961-179">(Výchozí hodnotu zadanou na řádku 24 níže uvedeného kódu)</span><span class="sxs-lookup"><span data-stu-id="02961-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="02961-180">Naše upravená migrace je připravena k přechodu, proto pomocí **aktualizace databáze** aktualizujte databázi.</span><span class="sxs-lookup"><span data-stu-id="02961-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="02961-181">Tentokrát pojďme zadat **–Verbose** příznak tak, aby můžete vidět SQL, které je spuštěna migrace code first.</span><span class="sxs-lookup"><span data-stu-id="02961-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="02961-182">Spusťte příkaz **Update-Database –Verbose** v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="02961-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="02961-183">Pohyb dat / Vlastní SQL</span><span class="sxs-lookup"><span data-stu-id="02961-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="02961-184">Zatím jsme se podívali na migrační operace, které nemění ani nepřesouvají žádná data, nyní se podívejme na něco, co potřebuje přesunout některá data.</span><span class="sxs-lookup"><span data-stu-id="02961-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="02961-185">Neexistuje žádná nativní podpora pro pohyb dat ještě, ale můžeme spustit některé libovolné příkazy SQL v libovolném bodě v našem skriptu.</span><span class="sxs-lookup"><span data-stu-id="02961-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="02961-186">Pojďme přidat **Post.Abstract** vlastnost našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="02961-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="02961-187">Později budeme předem vyplnit **Abstrakt** pro existující příspěvky pomocí nějakého textu od začátku sloupce **Obsah.**</span><span class="sxs-lookup"><span data-stu-id="02961-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="02961-188">Použijeme příkaz **Přidat-migrace,** abychom nechali code first migrations zakódovat svůj nejlepší odhad migrace pro nás.</span><span class="sxs-lookup"><span data-stu-id="02961-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="02961-189">Spusťte příkaz **Add-Migration Add-Migration AddPostAbstract** v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="02961-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="02961-190">Vygenerovaná migrace se postará o změny schématu, ale chceme také předem vyplnit sloupec **Abstract** pomocí prvních 100 znaků obsahu pro každý příspěvek.</span><span class="sxs-lookup"><span data-stu-id="02961-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="02961-191">Můžeme to udělat tím, že klesne na SQL a spuštění **příkazu UPDATE** po přidání sloupce.</span><span class="sxs-lookup"><span data-stu-id="02961-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="02961-192">(Přidání do řádku 12 v níže uvedeném kódu)</span><span class="sxs-lookup"><span data-stu-id="02961-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="02961-193">Naše upravená migrace vypadá dobře, proto použijte **update-database** k aktualizaci databáze aktuální.</span><span class="sxs-lookup"><span data-stu-id="02961-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="02961-194">Zadáme příznak **–Verbose,** abychom viděli, že sql je spuštěna proti databázi.</span><span class="sxs-lookup"><span data-stu-id="02961-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="02961-195">Spusťte příkaz **Update-Database –Verbose** v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="02961-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="02961-196">Migrace na konkrétní verzi (včetně přechodu na nižší verzi)</span><span class="sxs-lookup"><span data-stu-id="02961-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="02961-197">Zatím jsme vždy upgradovali na nejnovější migraci, ale mohou nastaly časy, kdy chcete upgradovat / downgrade na konkrétní migraci.</span><span class="sxs-lookup"><span data-stu-id="02961-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="02961-198">Řekněme, že chceme migrovat naši databázi do stavu, ve kterém byla po spuštění migrace **AddBlogUrl.**</span><span class="sxs-lookup"><span data-stu-id="02961-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="02961-199">Můžeme použít **-TargetMigration** přepínač downgrade na tuto migraci.</span><span class="sxs-lookup"><span data-stu-id="02961-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="02961-200">Spusťte příkaz **Update-Database –TargetMigration: AddBlogUrl** v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="02961-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="02961-201">Tento příkaz spustí skript Dolů pro naše migrace **AddBlogAbstract** a **AddPostClass.**</span><span class="sxs-lookup"><span data-stu-id="02961-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="02961-202">Pokud chcete vrátit celou cestu zpět do prázdné databáze pak můžete použít **Update-Database -TargetMigration: $InitialDatabase** příkaz.</span><span class="sxs-lookup"><span data-stu-id="02961-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="02961-203">Získání skriptu SQL</span><span class="sxs-lookup"><span data-stu-id="02961-203">Getting a SQL Script</span></span>

<span data-ttu-id="02961-204">Pokud jiný vývojář chce tyto změny na svém počítači, které můžete synchronizovat, jakmile zkontrolujeme naše změny do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="02961-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="02961-205">Jakmile mají naše nové migrace, stačí spustit příkaz Aktualizovat databázi, aby změny byly použity místně.</span><span class="sxs-lookup"><span data-stu-id="02961-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="02961-206">Nicméně pokud chceme tlačit tyto změny na testovací server a nakonec výroby, pravděpodobně chceme skript SQL můžeme předat naše DBA.</span><span class="sxs-lookup"><span data-stu-id="02961-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="02961-207">Spusťte příkaz **Aktualizovat databázi,** ale tentokrát zadejte příznak **–Script** tak, aby změny byly zapsány do skriptu, nikoli použity.</span><span class="sxs-lookup"><span data-stu-id="02961-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="02961-208">Také určíme zdrojovou a cílovou migraci, pro kterou bude skript vygenerován.</span><span class="sxs-lookup"><span data-stu-id="02961-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="02961-209">Chceme, aby skript přešel z prázdné databáze **($InitialDatabase)** na nejnovější verzi (migrace **AddPostAbstract).**</span><span class="sxs-lookup"><span data-stu-id="02961-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="02961-210">*Pokud cílovou migraci nezadáte, migrace použije jako cíl nejnovější migraci. Pokud nezadáte zdrojové migrace, migrace bude používat aktuální stav databáze.*</span><span class="sxs-lookup"><span data-stu-id="02961-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="02961-211">Spuštění příkazu **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="02961-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="02961-212">Migrace Code First spustí kanál migrace, ale místo toho, aby skutečně použilzměny, zapíše je do souboru .sql za vás.</span><span class="sxs-lookup"><span data-stu-id="02961-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="02961-213">Jakmile je skript vygenerován, otevře se pro vás v sadě Visual Studio, připravený k zobrazení nebo uložení.</span><span class="sxs-lookup"><span data-stu-id="02961-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="02961-214">Generování idempotentních skriptů</span><span class="sxs-lookup"><span data-stu-id="02961-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="02961-215">Počínaje EF6, pokud zadáte **–SourceMigration $InitialDatabase** pak vygenerovaný skript bude "idempotentní".</span><span class="sxs-lookup"><span data-stu-id="02961-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="02961-216">Idempotentní skripty můžete upgradovat databázi aktuálně v libovolné verzi na nejnovější verzi (nebo zadanou verzi, pokud používáte **-TargetMigration**).</span><span class="sxs-lookup"><span data-stu-id="02961-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="02961-217">Vygenerovaný skript obsahuje \*\* \_ \_\*\* logiku pro kontrolu tabulky MigrationsHistory a platí pouze změny, které nebyly dříve použity.</span><span class="sxs-lookup"><span data-stu-id="02961-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="02961-218">Automatická inovace při spuštění aplikace (MigrateDatabaseToLatestVersion Initializer)</span><span class="sxs-lookup"><span data-stu-id="02961-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="02961-219">Pokud nasazujete aplikaci, můžete chtít, aby automaticky upgradovala databázi (použitím všech čekajících migrací) při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="02961-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="02961-220">Můžete to provést registrací **MigrateDatabaseToLatestVersion** inicializátoru databáze.</span><span class="sxs-lookup"><span data-stu-id="02961-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="02961-221">Inicializátor databáze jednoduše obsahuje některé logiky, která se používá k ujistěte se, že databáze je správně nastavena.</span><span class="sxs-lookup"><span data-stu-id="02961-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="02961-222">Tato logika je spuštěna při prvním použití kontextu v rámci procesu aplikace **(AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="02961-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="02961-223">Můžeme aktualizovat **Program.cs** soubor, jak je znázorněno níže, nastavit **MigrateDatabaseToLatestVersion** inicializátor pro BlogContext před použitím kontextu (Řádek 14).</span><span class="sxs-lookup"><span data-stu-id="02961-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="02961-224">Všimněte si, že je také nutné přidat using prohlášení pro **System.Data.Entity** obor názvů (řádek 5).</span><span class="sxs-lookup"><span data-stu-id="02961-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="02961-225">*Když vytvoříme instanci tohoto inicializátoru, musíme zadat typ kontextu (**BlogContext**) a konfiguraci migrace (**Konfigurace**) - konfigurace migrace je třída, která byla přidána do naší složky **Migrace,** když jsme povolili migrace.*</span><span class="sxs-lookup"><span data-stu-id="02961-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="02961-226">Nyní vždy, když naše aplikace spustí, bude nejprve zkontrolovat, zda databáze je cílení je aktuální a použít všechny čekající migrace, pokud tomu tak není.</span><span class="sxs-lookup"><span data-stu-id="02961-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
