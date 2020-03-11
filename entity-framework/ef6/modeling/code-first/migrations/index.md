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
# <a name="code-first-migrations"></a><span data-ttu-id="fb88d-102">Migrace Code First</span><span class="sxs-lookup"><span data-stu-id="fb88d-102">Code First Migrations</span></span>
<span data-ttu-id="fb88d-103">Migrace Code First je doporučený způsob, jak vyvíjet schéma databáze vaší aplikace, pokud používáte pracovní postup Code First.</span><span class="sxs-lookup"><span data-stu-id="fb88d-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="fb88d-104">Migrace poskytují sadu nástrojů, které umožňují:</span><span class="sxs-lookup"><span data-stu-id="fb88d-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="fb88d-105">Vytvoření počáteční databáze, která funguje s vaším modelem EF</span><span class="sxs-lookup"><span data-stu-id="fb88d-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="fb88d-106">Generování migrace pro sledování změn, které provedete v modelu EF</span><span class="sxs-lookup"><span data-stu-id="fb88d-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="fb88d-107">Udržujte databázi v aktuálním stavu pomocí těchto změn</span><span class="sxs-lookup"><span data-stu-id="fb88d-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="fb88d-108">Následující návod vám poskytne přehled Migrace Code First v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fb88d-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="fb88d-109">Můžete buď dokončit celý návod, nebo přeskočit k tématu, které vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="fb88d-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="fb88d-110">Jsou pokryta následující témata:</span><span class="sxs-lookup"><span data-stu-id="fb88d-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="fb88d-111">Vytvoření počátečního & databáze modelu</span><span class="sxs-lookup"><span data-stu-id="fb88d-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="fb88d-112">Než začneme používat migrace, potřebujeme projekt a model Code First, se kterým pracujete.</span><span class="sxs-lookup"><span data-stu-id="fb88d-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="fb88d-113">V tomto návodu budeme používat kanonický **blog** a model **post** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="fb88d-114">Vytvořit novou konzolovou aplikaci **MigrationsDemo**</span><span class="sxs-lookup"><span data-stu-id="fb88d-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="fb88d-115">Přidejte do projektu nejnovější verzi balíčku NuGet **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="fb88d-116">**Nástroje –&gt; správce balíčků knihovny –&gt; konzolu Správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="fb88d-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="fb88d-117">Spuštění příkazu **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="fb88d-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="fb88d-118">Přidejte soubor **model.cs** s kódem zobrazeným níže.</span><span class="sxs-lookup"><span data-stu-id="fb88d-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="fb88d-119">Tento kód definuje jednu třídu **blogu** , která poskytuje náš doménový model a třídu **BlogContext** , která je náš Code First kontextem EF.</span><span class="sxs-lookup"><span data-stu-id="fb88d-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="fb88d-120">Teď, když máme model, je čas ho použít k provedení přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="fb88d-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="fb88d-121">Aktualizujte soubor **program.cs** pomocí kódu uvedeného níže.</span><span class="sxs-lookup"><span data-stu-id="fb88d-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="fb88d-122">Spusťte aplikaci a zobrazí se vám pro vás vytvořená databáze **MigrationsCodeDemo. BlogContext** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![LocalDB databáze](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="fb88d-124">Povolování migrací</span><span class="sxs-lookup"><span data-stu-id="fb88d-124">Enabling Migrations</span></span>

<span data-ttu-id="fb88d-125">Je čas udělat další změny v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="fb88d-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="fb88d-126">Pojďme do třídy blogu přivést vlastnost URL.</span><span class="sxs-lookup"><span data-stu-id="fb88d-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="fb88d-127">Pokud byste chtěli aplikaci znovu spustit, měli byste obdržet zprávu, že došlo ke *změně modelu zálohování kontextu ' BlogContext ', protože databáze byla vytvořena. Pokud chcete aktualizovat databázi (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *)* , zvažte použití migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="fb88d-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="fb88d-128">Vzhledem k tomu, že výjimka navrhuje, je čas začít používat Migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="fb88d-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="fb88d-129">Prvním krokem je povolit migrace pro náš kontext.</span><span class="sxs-lookup"><span data-stu-id="fb88d-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="fb88d-130">Spuštění příkazu **Povolit – migrace** v konzole správce balíčků</span><span class="sxs-lookup"><span data-stu-id="fb88d-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="fb88d-131">Tento příkaz přidal do našeho projektu složku **migrace** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="fb88d-132">Tato nová složka obsahuje dva soubory:</span><span class="sxs-lookup"><span data-stu-id="fb88d-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="fb88d-133">**Třída Configuration**</span><span class="sxs-lookup"><span data-stu-id="fb88d-133">**The Configuration class.**</span></span> <span data-ttu-id="fb88d-134">Tato třída umožňuje nakonfigurovat, jak se budou migrace chovat pro váš kontext.</span><span class="sxs-lookup"><span data-stu-id="fb88d-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="fb88d-135">V tomto návodu použijeme jenom výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fb88d-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="fb88d-136">*Vzhledem k tomu, že v projektu existuje jen jeden Code First kontext, Enable – migrace se automaticky vyplní v kontextu typu, pro který je tato konfigurace platná.*</span><span class="sxs-lookup"><span data-stu-id="fb88d-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="fb88d-137">**Migrace InitialCreate**</span><span class="sxs-lookup"><span data-stu-id="fb88d-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="fb88d-138">Tato migrace se vygenerovala, protože už jsme Code First vytvořit databázi pro nás, než jsme povolili migrace.</span><span class="sxs-lookup"><span data-stu-id="fb88d-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="fb88d-139">Kód v této vygenerované migraci představuje objekty, které již byly vytvořeny v databázi.</span><span class="sxs-lookup"><span data-stu-id="fb88d-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="fb88d-140">V našem případě je to tabulka **blogu** se sloupci **BlogId** a **Name** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="fb88d-141">Název souboru obsahuje časové razítko, které vám pomůžou s řazením.</span><span class="sxs-lookup"><span data-stu-id="fb88d-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="fb88d-142">*Pokud databáze ještě není vytvořená, migrace InitialCreate by se do projektu nepřidala. Místo toho se při prvním volání metody přidání migrace kód pro vytvoření těchto tabulek vytvoří při nové migraci na základě uživatelského rozhraní.*</span><span class="sxs-lookup"><span data-stu-id="fb88d-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="fb88d-143">Několik modelů cílících na stejnou databázi</span><span class="sxs-lookup"><span data-stu-id="fb88d-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="fb88d-144">Při používání verzí starších než EF6 může být pro generování a správu schématu databáze použit pouze jeden model Code First.</span><span class="sxs-lookup"><span data-stu-id="fb88d-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="fb88d-145">Jedná se o výsledek jednoho **\_\_tabulce MigrationsHistory** na databázi bez možnosti určit, které položky patří do daného modelu.</span><span class="sxs-lookup"><span data-stu-id="fb88d-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="fb88d-146">Počínaje EF6 třída **Configuration** zahrnuje vlastnost **ContextKey** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="fb88d-147">To funguje jako jedinečný identifikátor pro každý model Code First.</span><span class="sxs-lookup"><span data-stu-id="fb88d-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="fb88d-148">Odpovídající sloupec v tabulce **\_\_MigrationsHistory** umožňuje položkám z více modelů sdílet tabulku.</span><span class="sxs-lookup"><span data-stu-id="fb88d-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="fb88d-149">Ve výchozím nastavení je tato vlastnost nastavena na plně kvalifikovaný název vašeho kontextu.</span><span class="sxs-lookup"><span data-stu-id="fb88d-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="fb88d-150">Generují se & spouští migrace.</span><span class="sxs-lookup"><span data-stu-id="fb88d-150">Generating & Running Migrations</span></span>

<span data-ttu-id="fb88d-151">Migrace Code First má dva primární příkazy, se kterými se seznámíte.</span><span class="sxs-lookup"><span data-stu-id="fb88d-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="fb88d-152">**Přidání – při migraci** dojde k další migraci vygenerované na základě změn, které jste provedli v modelu od vytvoření poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="fb88d-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="fb88d-153">**Aktualizace – databáze** bude používat všechny nedokončené migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="fb88d-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="fb88d-154">Abychom se postaral o novou vlastnost URL, kterou jsme přidali, potřebujeme vytvořit nové uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb88d-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="fb88d-155">Příkaz **Add-Migration** nám umožňuje dát těmto migracim název, Pojďme ale volat náš **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="fb88d-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="fb88d-156">Spuštění příkazu **Add-Migration AddBlogUrl** v konzole správce balíčků</span><span class="sxs-lookup"><span data-stu-id="fb88d-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="fb88d-157">Ve složce **migrace** teď máme novou migraci **AddBlogUrl** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="fb88d-158">Název souboru migrace je předem vyřešen s časovým razítkem, které vám pomůžou s řazením.</span><span class="sxs-lookup"><span data-stu-id="fb88d-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="fb88d-159">Tuto migraci teď můžeme upravit nebo přidat, ale vše vypadá poměrně dobré.</span><span class="sxs-lookup"><span data-stu-id="fb88d-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="fb88d-160">Pojďme použít tuto migraci do databáze pomocí **Update-Database** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="fb88d-161">Spuštění příkazu **Update-Database** v konzole správce balíčků</span><span class="sxs-lookup"><span data-stu-id="fb88d-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="fb88d-162">Migrace Code First porovná migrace v našich složkách **migrace** s těmi, které byly použity pro databázi.</span><span class="sxs-lookup"><span data-stu-id="fb88d-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="fb88d-163">Uvidí, že je potřeba použít migraci **AddBlogUrl** a spustit ji.</span><span class="sxs-lookup"><span data-stu-id="fb88d-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="fb88d-164">Databáze **MigrationsDemo. BlogContext** se teď aktualizovala tak, aby obsahovala sloupec **URL** v tabulce **Blogy** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="fb88d-165">Přizpůsobení migrací</span><span class="sxs-lookup"><span data-stu-id="fb88d-165">Customizing Migrations</span></span>

<span data-ttu-id="fb88d-166">Zatím jsme vygenerovali a spustili migraci bez provedení změn.</span><span class="sxs-lookup"><span data-stu-id="fb88d-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="fb88d-167">Teď se podíváme na úpravu kódu, který se vygeneruje ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fb88d-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="fb88d-168">Je čas udělat další změny v našem modelu. Pojďme do třídy **blogu** přidat novou vlastnost **hodnocení** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="fb88d-169">Pojďme také přidat novou třídu **post**</span><span class="sxs-lookup"><span data-stu-id="fb88d-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="fb88d-170">Do třídy **blog** přidáme také kolekci **příspěvky** , která bude tvořit druhý konec relace mezi **blogem** a **příspěvkem** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="fb88d-171">K umožnění Migrace Code Firstho uživatelského rozhraní využijeme k dispozici nejlepší odhad na migraci pro nás pomocí příkazu **Add-Migration** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="fb88d-172">Budeme volat tuto **AddPostClass**migrace.</span><span class="sxs-lookup"><span data-stu-id="fb88d-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="fb88d-173">Spusťte příkaz **Add-Migration AddPostClass** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fb88d-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="fb88d-174">Migrace Code First pro tyto změny bylo poměrně dobré úlohy, ale můžete chtít změnit několik věcí:</span><span class="sxs-lookup"><span data-stu-id="fb88d-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="fb88d-175">Nejprve přidáme jedinečný index do **příspěvku. sloupec title** (přidávání na řádku 22 & 29 v kódu níže).</span><span class="sxs-lookup"><span data-stu-id="fb88d-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="fb88d-176">Také přidáváme do tohoto sloupce **hodnocení** , které neumožňují hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="fb88d-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="fb88d-177">Pokud v tabulce existují nějaká existující data, zobrazí se jim výchozí hodnota CLR datového typu pro nový sloupec (hodnocení je celé číslo, takže by to bylo **0**).</span><span class="sxs-lookup"><span data-stu-id="fb88d-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="fb88d-178">Ale chceme zadat výchozí hodnotu **3** , aby se stávající řádky v tabulce **Blogy** spouštěly se hodnocením dát.</span><span class="sxs-lookup"><span data-stu-id="fb88d-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="fb88d-179">(Můžete zobrazit výchozí hodnotu zadanou na řádku 24 v kódu níže)</span><span class="sxs-lookup"><span data-stu-id="fb88d-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="fb88d-180">Naše upravená migrace je připravená k tomu, takže pomocí **Update-Database** přineseme databázi do aktuálního stavu.</span><span class="sxs-lookup"><span data-stu-id="fb88d-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="fb88d-181">Tentokrát je nutné zadat příznak **– verbose** , abyste viděli SQL, na kterém je spuštěný migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="fb88d-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="fb88d-182">Spusťte příkaz **Update-Database – verbose** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fb88d-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="fb88d-183">Pohyb dat/vlastní SQL</span><span class="sxs-lookup"><span data-stu-id="fb88d-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="fb88d-184">Zatím jsme si prohlédli operace migrace, které nemění ani nepřesouvá žádná data, teď se podíváme na něco, co potřebuje přesunout některá data.</span><span class="sxs-lookup"><span data-stu-id="fb88d-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="fb88d-185">Ještě není k dispozici žádná nativní podpora pro pohyb dat, ale v jakémkoli okamžiku v našem skriptu můžeme spustit libovolné příkazy SQL.</span><span class="sxs-lookup"><span data-stu-id="fb88d-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="fb88d-186">Pojďme do našeho modelu přidat vlastnost **post. Abstract** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="fb88d-187">Později teď vyplníme **abstrakci** pro existující příspěvky pomocí textu, který se nachází na začátku sloupce **Content (obsah** ).</span><span class="sxs-lookup"><span data-stu-id="fb88d-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="fb88d-188">K umožnění Migrace Code Firstho uživatelského rozhraní využijeme k dispozici nejlepší odhad na migraci pro nás pomocí příkazu **Add-Migration** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="fb88d-189">Spusťte příkaz **Add-Migration AddPostAbstract** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fb88d-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="fb88d-190">Vygenerovaná migrace se stará o změny schématu, ale chceme také předem naplnit **abstraktní** sloupec s použitím prvních 100 znaků obsahu pro každý příspěvek.</span><span class="sxs-lookup"><span data-stu-id="fb88d-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="fb88d-191">To můžeme udělat vyřazením dolů na SQL a spuštěním příkazu **Update** po přidání sloupce.</span><span class="sxs-lookup"><span data-stu-id="fb88d-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="fb88d-192">(Přidání na řádku 12 v kódu níže)</span><span class="sxs-lookup"><span data-stu-id="fb88d-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="fb88d-193">Naše upravená migrace je dobrá, takže použijeme **příkaz Update-Database** , aby se databáze aktualizovala v aktuálním stavu.</span><span class="sxs-lookup"><span data-stu-id="fb88d-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="fb88d-194">Určíme příznak **– verbose** , aby bylo možné vidět, že je SQL spuštěný proti databázi.</span><span class="sxs-lookup"><span data-stu-id="fb88d-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="fb88d-195">Spusťte příkaz **Update-Database – verbose** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fb88d-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="fb88d-196">Migrace na konkrétní verzi (včetně downgradu)</span><span class="sxs-lookup"><span data-stu-id="fb88d-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="fb88d-197">Zatím jsme se vždycky upgradovali na nejnovější migraci, ale může nastat situace, kdy budete chtít upgradovat nebo downgradovat na konkrétní migraci.</span><span class="sxs-lookup"><span data-stu-id="fb88d-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="fb88d-198">Řekněme, že chceme migrovat naši databázi do stavu, ve kterém byl po spuštění naší migrace **AddBlogUrl** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="fb88d-199">Pro přechod na tuto migraci můžeme použít přepínač **– TargetMigration** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="fb88d-200">Spusťte příkaz **Update-Database – TargetMigration: AddBlogUrl** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fb88d-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="fb88d-201">Tento příkaz spustí skript pro migrace našich **AddBlogAbstract** a **AddPostClass** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="fb88d-202">Pokud chcete vrátit všechny možnosti zpátky do prázdné databáze, můžete použít příkaz **Update-Database – TargetMigration: $InitialDatabase** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="fb88d-203">Získání skriptu SQL</span><span class="sxs-lookup"><span data-stu-id="fb88d-203">Getting a SQL Script</span></span>

<span data-ttu-id="fb88d-204">Pokud jiný vývojář tuto změnu na svém počítači přeje, může se po kontrole změn do správy zdrojových kódů synchronizovat jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="fb88d-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="fb88d-205">Jakmile naši nové migrace dostanou, stačí spustit příkaz Update-Database, aby se změny používaly lokálně.</span><span class="sxs-lookup"><span data-stu-id="fb88d-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="fb88d-206">Pokud ale chceme tyto změny nabízet na testovacím serveru a nakonec v produkčním prostředí, nejspíš chceme, aby se skript SQL, který můžeme předat našímu DBA.</span><span class="sxs-lookup"><span data-stu-id="fb88d-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="fb88d-207">Spusťte příkaz **Update-Database** , ale tentokrát určete příznak **– Script** , aby se změny zapsaly do skriptu místo použití.</span><span class="sxs-lookup"><span data-stu-id="fb88d-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="fb88d-208">Také určíme migraci zdroje a cíle pro vygenerování skriptu pro.</span><span class="sxs-lookup"><span data-stu-id="fb88d-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="fb88d-209">Chceme, aby se skript přešel z prázdné databáze ( **$InitialDatabase**) na nejnovější verzi ( **AddPostAbstract**migrace).</span><span class="sxs-lookup"><span data-stu-id="fb88d-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="fb88d-210">*Pokud nezadáte cílovou migraci, budou migrace používat jako cíl nejnovější migraci. Pokud neurčíte zdrojová migrace, budou migrace používat aktuální stav databáze.*</span><span class="sxs-lookup"><span data-stu-id="fb88d-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="fb88d-211">Spusťte příkaz **Update-Database-Script-SourceMigration: $InitialDatabase-TargetMigration: AddPostAbstract** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fb88d-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="fb88d-212">Migrace Code First spustí kanál migrace, ale místo toho, aby se změny projevily, ho za vás zapíše do souboru. SQL.</span><span class="sxs-lookup"><span data-stu-id="fb88d-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="fb88d-213">Po vygenerování skriptu je tento skript otevřený pro vás v aplikaci Visual Studio, který jste připraveni k zobrazení nebo uložení.</span><span class="sxs-lookup"><span data-stu-id="fb88d-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="fb88d-214">Generování skriptů Idempotentní</span><span class="sxs-lookup"><span data-stu-id="fb88d-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="fb88d-215">Začínáte-li s EF6, pokud zadáte **– SourceMigration $InitialDatabase** pak bude vygenerovaný skript "idempotentní".</span><span class="sxs-lookup"><span data-stu-id="fb88d-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="fb88d-216">Idempotentní skripty mohou upgradovat databázi aktuálně v libovolné verzi na nejnovější verzi (nebo na zadanou verzi, pokud používáte **– TargetMigration**).</span><span class="sxs-lookup"><span data-stu-id="fb88d-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="fb88d-217">Vygenerovaný skript obsahuje logiku pro kontrolu **\_tabulky \_MigrationsHistory** a pouze změny, které se předtím nepoužily.</span><span class="sxs-lookup"><span data-stu-id="fb88d-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="fb88d-218">Automatické upgradování při spuštění aplikace (inicializátor MigrateDatabaseToLatestVersion)</span><span class="sxs-lookup"><span data-stu-id="fb88d-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="fb88d-219">Pokud nasazujete aplikaci, možná budete chtít, aby při spuštění aplikace automaticky upgradovali databázi (pomocí všech nedokončených migrací).</span><span class="sxs-lookup"><span data-stu-id="fb88d-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="fb88d-220">To můžete provést tak, že zaregistrujete inicializátor databáze **MigrateDatabaseToLatestVersion** .</span><span class="sxs-lookup"><span data-stu-id="fb88d-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="fb88d-221">Inicializátor databáze jednoduše obsahuje logiku, která se používá k zajištění správného nastavení databáze.</span><span class="sxs-lookup"><span data-stu-id="fb88d-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="fb88d-222">Tato logika se spustí při prvním použití kontextu v procesu aplikace (**AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="fb88d-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="fb88d-223">Soubor **program.cs** můžeme aktualizovat, jak je vidět níže, pro nastavení inicializátoru **MigrateDatabaseToLatestVersion** pro BlogContext předtím, než použijeme kontext (řádek 14).</span><span class="sxs-lookup"><span data-stu-id="fb88d-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="fb88d-224">Všimněte si, že je také nutné přidat příkaz using pro obor názvů **System. data. entity** (řádek 5).</span><span class="sxs-lookup"><span data-stu-id="fb88d-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="fb88d-225">*Když vytvoříme instanci tohoto inicializátoru, musíme zadat typ kontextu (**BlogContext**) a konfiguraci migrace (**Konfigurace**) – konfigurace migrace je třída, která se přidala do naší složky **migrace** po povolení migrace.*</span><span class="sxs-lookup"><span data-stu-id="fb88d-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="fb88d-226">Teď, když se aplikace spustí, nejdřív zkontroluje, jestli je databáze, na kterou cílí, v aktuálním stavu, a pokud není, použijte všechny nedokončené migrace.</span><span class="sxs-lookup"><span data-stu-id="fb88d-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
