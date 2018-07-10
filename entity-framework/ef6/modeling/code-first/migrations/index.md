---
title: Migrace Code First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
caps.latest.revision: 3
ms.openlocfilehash: 5c7431985e2e404060197615bf281fcf3b318403
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914281"
---
# <a name="code-first-migrations"></a><span data-ttu-id="21de8-102">Migrace Code First</span><span class="sxs-lookup"><span data-stu-id="21de8-102">Code First Migrations</span></span>
<span data-ttu-id="21de8-103">Migrace Code First je doporučeným způsobem, jak vyvíjet schéma databáze vaší aplikace používáte Code First pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="21de8-103">Code First Migrations is the recommended way to evolve you application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="21de8-104">Migrace poskytují sadu nástrojů, které umožňují:</span><span class="sxs-lookup"><span data-stu-id="21de8-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="21de8-105">Vytvoření počáteční databáze, která funguje s EF modelu</span><span class="sxs-lookup"><span data-stu-id="21de8-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="21de8-106">Generování migrace můžete sledovat změny provedené do modelu EF</span><span class="sxs-lookup"><span data-stu-id="21de8-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="21de8-107">Zachovat aktuální tyto změny databáze</span><span class="sxs-lookup"><span data-stu-id="21de8-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="21de8-108">Následující návod přináší přehled migrace Code First v rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="21de8-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="21de8-109">Můžete buď dokončení celého postupu nebo přejděte k tématu, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="21de8-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="21de8-110">Jsou pokryta následující témata:</span><span class="sxs-lookup"><span data-stu-id="21de8-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="21de8-111">Vytváření počáteční modelu & databáze</span><span class="sxs-lookup"><span data-stu-id="21de8-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="21de8-112">Než začneme pomocí migrace musíte projekt a model Code First pro práci s.</span><span class="sxs-lookup"><span data-stu-id="21de8-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="21de8-113">V tomto návodu budeme používat kanonickém **blogu** a **příspěvek** modelu.</span><span class="sxs-lookup"><span data-stu-id="21de8-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="21de8-114">Vytvořte nový **MigrationsDemo** konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="21de8-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="21de8-115">Přidejte nejnovější verzi **EntityFramework** balíček NuGet do projektu</span><span class="sxs-lookup"><span data-stu-id="21de8-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="21de8-116">**Nástroje –&gt; Správce balíčků knihoven –&gt; Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="21de8-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="21de8-117">Spustit **Install-Package EntityFramework** příkazu</span><span class="sxs-lookup"><span data-stu-id="21de8-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="21de8-118">Přidat **Model.cs** souboru s kódem je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="21de8-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="21de8-119">Tento kód definuje jedinou **blogu** třídu, která tvoří náš model domény a **BlogContext** třídu, která je náš kontext platforem EF Code First</span><span class="sxs-lookup"><span data-stu-id="21de8-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="21de8-120">Když teď máme modelu je čas ji používat pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="21de8-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="21de8-121">Aktualizace **Program.cs** souboru s kódem je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="21de8-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="21de8-122">Spusťte aplikaci a uvidíte, že **MigrationsCodeDemo.BlogContext** databáze se vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="21de8-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="21de8-124">Povolení migrace</span><span class="sxs-lookup"><span data-stu-id="21de8-124">Enabling Migrations</span></span>

<span data-ttu-id="21de8-125">Je čas provést některé další změny našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="21de8-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="21de8-126">Umožňuje zavést vlastnost adresa Url blogu třídy.</span><span class="sxs-lookup"><span data-stu-id="21de8-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="21de8-127">Pokud chcete aplikaci spustit znovu získali byste s oznámením InvalidOperationException *model zálohování kontextu 'BlogContext' byl změněn, protože byla vytvořena databáze. Zvažte použití migrace Code First k aktualizaci databáze (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](http://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="21de8-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="21de8-128">Jak výjimku naznačuje, je čas začít pomocí migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="21de8-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="21de8-129">Prvním krokem je povolení migrace pro náš kontext.</span><span class="sxs-lookup"><span data-stu-id="21de8-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="21de8-130">Spustit **povolení migrace** příkazu v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="21de8-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="21de8-131">Tento příkaz má přidat **migrace** složku do projektu.</span><span class="sxs-lookup"><span data-stu-id="21de8-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="21de8-132">Tato nová složka obsahuje dva soubory:</span><span class="sxs-lookup"><span data-stu-id="21de8-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="21de8-133">**Třída konfigurace.**</span><span class="sxs-lookup"><span data-stu-id="21de8-133">**The Configuration class.**</span></span> <span data-ttu-id="21de8-134">Tato třída umožňuje konfigurovat chování migrace pro váš kontext.</span><span class="sxs-lookup"><span data-stu-id="21de8-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="21de8-135">V tomto návodu budeme používat jenom výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="21de8-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="21de8-136">*Vzhledem k tomu, že existuje pouze jeden kontext Code First ve vašem projektu, má povolení migrace automaticky vyplněna tato konfigurace se vztahuje na typ kontextu.*</span><span class="sxs-lookup"><span data-stu-id="21de8-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="21de8-137">**Při migraci InitialCreate**.</span><span class="sxs-lookup"><span data-stu-id="21de8-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="21de8-138">Tato migrace se vygenerovat, protože jsme už měli Code First vytvořit databázi pro nás, než jsme povolili migrace.</span><span class="sxs-lookup"><span data-stu-id="21de8-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="21de8-139">Kód v této vygenerované migraci představuje objekty, které již byly vytvořeny v databázi.</span><span class="sxs-lookup"><span data-stu-id="21de8-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="21de8-140">V našem, který je **blogu** tabulky s **BlogId** a **název** sloupce.</span><span class="sxs-lookup"><span data-stu-id="21de8-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="21de8-141">Název souboru obsahuje časové razítko, které vám pomůžou s řazení.</span><span class="sxs-lookup"><span data-stu-id="21de8-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="21de8-142">*Pokud databázi nebyl už vytvořili InitialCreate migrace by byly přidány do projektu. Při prvním říkáme migrace přidat kód k vytvoření těchto tabulek by místo toho automaticky generovaný nové migrace.*</span><span class="sxs-lookup"><span data-stu-id="21de8-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="21de8-143">Více modelů, které cílí na stejné databáze</span><span class="sxs-lookup"><span data-stu-id="21de8-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="21de8-144">Při použití verze starší než EF6, pouze jeden model Code First může vygenerovat a spravovat schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="21de8-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="21de8-145">To je výsledkem jednoho  **\_ \_MigrationsHistory** tabulky na databázi s způsob, jak zjistit položky, které patří do které modelu.</span><span class="sxs-lookup"><span data-stu-id="21de8-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="21de8-146">Počínaje EF6, **konfigurace** obsahuje třídy **ContextKey** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="21de8-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="21de8-147">To slouží jako jedinečný identifikátor pro každý model Code First.</span><span class="sxs-lookup"><span data-stu-id="21de8-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="21de8-148">Odpovídající sloupec ve  **\_ \_MigrationsHistory** tabulky umožňuje položky z více modelů pro sdílení v tabulce.</span><span class="sxs-lookup"><span data-stu-id="21de8-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="21de8-149">Ve výchozím nastavení je tato vlastnost nastavena na plně kvalifikovaný název kontextu.</span><span class="sxs-lookup"><span data-stu-id="21de8-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="21de8-150">Generování a spuštění migrace</span><span class="sxs-lookup"><span data-stu-id="21de8-150">Generating & Running Migrations</span></span>

<span data-ttu-id="21de8-151">Migrace Code First má dva primární příkazy, které se chystáte seznámit se s.</span><span class="sxs-lookup"><span data-stu-id="21de8-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="21de8-152">**Přidejte migraci** bude generování uživatelského rozhraní další migrace na základě změn, které jste provedli pro váš model, od vytvoření posledního migrace</span><span class="sxs-lookup"><span data-stu-id="21de8-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="21de8-153">**Aktualizace databáze** se použije čekající migrace do databáze</span><span class="sxs-lookup"><span data-stu-id="21de8-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="21de8-154">Potřebujeme scaffold migrace, která se stará o novou vlastnost adresa Url, kterou jsme přidali.</span><span class="sxs-lookup"><span data-stu-id="21de8-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="21de8-155">**Přidat migrace** příkazu, umožníte nám tyto migrace pojmenujte, stačí pojmenujme náš **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="21de8-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="21de8-156">Spustit **přidat migrace AddBlogUrl** příkazu v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="21de8-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="21de8-157">V **migrace** složky teď máme nový **AddBlogUrl** migrace.</span><span class="sxs-lookup"><span data-stu-id="21de8-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="21de8-158">Název souboru migrace předem vyřešen s časovým razítkem usnadňující řazení</span><span class="sxs-lookup"><span data-stu-id="21de8-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="21de8-159">Nyní jsme může upravit nebo přidat do této migrace ale všechno, co vypadá poměrně dobře.</span><span class="sxs-lookup"><span data-stu-id="21de8-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="21de8-160">Použijeme **aktualizace databáze** použít tuto migraci do databáze.</span><span class="sxs-lookup"><span data-stu-id="21de8-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="21de8-161">Spustit **aktualizace databáze** příkazu v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="21de8-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="21de8-162">Migrace Code First při porovnání migrace v našich **migrace** složky na základě těch, které se použily k databázi.</span><span class="sxs-lookup"><span data-stu-id="21de8-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="21de8-163">Je vidět, že **AddBlogUrl** migrace je potřeba použít a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="21de8-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="21de8-164">**MigrationsDemo.BlogContext** databáze je teď aktualizovaný zahrnout **Url** sloupec v **blogy** tabulky.</span><span class="sxs-lookup"><span data-stu-id="21de8-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="21de8-165">Přizpůsobení migrace</span><span class="sxs-lookup"><span data-stu-id="21de8-165">Customizing Migrations</span></span>

<span data-ttu-id="21de8-166">Zatím jsme vygeneruje a spuštění migrace beze změn.</span><span class="sxs-lookup"><span data-stu-id="21de8-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="21de8-167">Nyní Pojďme se podívat na úpravy kódu, který získá vygenerována ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="21de8-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="21de8-168">Je čas provést některé další změny náš model, přidáme nový **hodnocení** vlastnost **blogu** třídy</span><span class="sxs-lookup"><span data-stu-id="21de8-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="21de8-169">Můžeme také přidat nový **příspěvek** třídy</span><span class="sxs-lookup"><span data-stu-id="21de8-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="21de8-170">Přidáme také **příspěvky** kolekce **blogu** třídy formuláře druhém konci vztahu mezi **blogu** a **příspěvku**</span><span class="sxs-lookup"><span data-stu-id="21de8-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="21de8-171">Použijeme **přidat migrace** příkaz, který umožní migrace Code First generování uživatelského rozhraní na migrace pro nás jeho co nejlepší odhad.</span><span class="sxs-lookup"><span data-stu-id="21de8-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="21de8-172">My budeme volat tuto migraci **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="21de8-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="21de8-173">Spustit **přidat migrace AddPostClass** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="21de8-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="21de8-174">Migrace Code First nebyla tom docela dobře práce pro generování uživatelského rozhraní tyto změny, ale existují některé možnosti, co chceme změnit:</span><span class="sxs-lookup"><span data-stu-id="21de8-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="21de8-175">Nejprve nahoru, přidáme jedinečný index k **Posts.Title** sloupec (přidání řádku 22 & 29 v následujícím kódu).</span><span class="sxs-lookup"><span data-stu-id="21de8-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="21de8-176">Také přidáváme neumožňující **Blogs.Rating** sloupce.</span><span class="sxs-lookup"><span data-stu-id="21de8-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="21de8-177">Pokud není žádná existující data v tabulce ji získat přiřadí výchozí CLR datového typu pro nový sloupec (hodnocení je celé číslo, takže, který bude **0**).</span><span class="sxs-lookup"><span data-stu-id="21de8-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="21de8-178">Chcete zadat výchozí hodnotu, ale **3** tak v tomto existujícím řádků **blogy** tabulky budou začínat vrazíme hodnocení.</span><span class="sxs-lookup"><span data-stu-id="21de8-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="21de8-179">(Zobrazí se výchozí hodnota zadaná na řádku 24 kód uvedený níže)</span><span class="sxs-lookup"><span data-stu-id="21de8-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="21de8-180">Naše upravených migrace je připraven k přejít, takže použijeme **aktualizace databáze** uveďte aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="21de8-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="21de8-181">Nyní můžeme zadat **– Verbose** příznak, kde můžete zobrazit SQL, na kterém běží migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="21de8-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="21de8-182">Spustit **aktualizace databáze – Verbose** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="21de8-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="21de8-183">Pohyb dat nebo vlastními SQL</span><span class="sxs-lookup"><span data-stu-id="21de8-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="21de8-184">Zatím jsme se podívat na migraci, které operace, které nechcete změnit nebo přesunout všechna data, teď Pojďme se podívat na něco, kterou je potřeba pohyb nějaká data.</span><span class="sxs-lookup"><span data-stu-id="21de8-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="21de8-185">Neexistuje žádná nativní podpora pro pohybu dat ještě, ale můžeme spustit některé libovolné příkazy SQL v libovolném bodě ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="21de8-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="21de8-186">Přidejme **Post.Abstract** vlastnost našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="21de8-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="21de8-187">Později, přejdeme k předběžnému naplnění **abstraktní** existující příspěvky od začátku pomocí nějaký text **obsahu** sloupce.</span><span class="sxs-lookup"><span data-stu-id="21de8-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="21de8-188">Použijeme **přidat migrace** příkaz, který umožní migrace Code First generování uživatelského rozhraní na migrace pro nás jeho co nejlepší odhad.</span><span class="sxs-lookup"><span data-stu-id="21de8-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="21de8-189">Spustit **přidat migrace AddPostAbstract** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="21de8-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="21de8-190">Vygenerovaný migrace se postará o změny schématu, ale také chceme k předběžnému naplnění **abstraktní** sloupce pomocí prvních 100 znaků obsahu pro jednotlivé příspěvky.</span><span class="sxs-lookup"><span data-stu-id="21de8-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="21de8-191">Můžeme to udělat přetažením na SQL a spuštění **aktualizace** příkaz poté, co se má přidat sloupec.</span><span class="sxs-lookup"><span data-stu-id="21de8-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="21de8-192">(Přidání řádku 12 v následujícím kódu)</span><span class="sxs-lookup"><span data-stu-id="21de8-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="21de8-193">Naše upravených migrace je v pořádku, takže použijeme **aktualizace databáze** uveďte aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="21de8-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="21de8-194">Budete určíme **– Verbose** příznak, jsme viděli SQL spuštěn na databázi.</span><span class="sxs-lookup"><span data-stu-id="21de8-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="21de8-195">Spustit **aktualizace databáze – Verbose** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="21de8-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="21de8-196">Migrace na konkrétní verzi (včetně Downgrade)</span><span class="sxs-lookup"><span data-stu-id="21de8-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="21de8-197">Zatím jsme provedli vždy upgrade na nejnovější migrace, ale může nastat situace, kdy chcete upgradovat nebo downgradovat na konkrétní migrace.</span><span class="sxs-lookup"><span data-stu-id="21de8-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="21de8-198">Řekněme, že chceme migrovat databáze do stavu byl po spuštění našich **AddBlogUrl** migrace.</span><span class="sxs-lookup"><span data-stu-id="21de8-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="21de8-199">Můžeme použít **– TargetMigration** přepínač na starší verzi této migrace.</span><span class="sxs-lookup"><span data-stu-id="21de8-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="21de8-200">Spustit **aktualizace databáze – TargetMigration: AddBlogUrl** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="21de8-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="21de8-201">Tento příkaz spustí skript dolů pro naše **AddBlogAbstract** a **AddPostClass** migrace.</span><span class="sxs-lookup"><span data-stu-id="21de8-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="21de8-202">Pokud chcete úplně vrátit zpět na prázdnou databázi, pak můžete použít **aktualizace databáze – TargetMigration: $InitialDatabase** příkazu.</span><span class="sxs-lookup"><span data-stu-id="21de8-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="21de8-203">Získávání skript SQL</span><span class="sxs-lookup"><span data-stu-id="21de8-203">Getting a SQL Script</span></span>

<span data-ttu-id="21de8-204">Pokud jiný vývojář chce, aby se tyto změny v jejich počítači se jenom synchronizovat až zkontrolujeme naše změny do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="21de8-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="21de8-205">Jakmile budou mít naše nové migrace jsou pouze spustit příkaz aktualizace databáze chcete-li změny použít místně.</span><span class="sxs-lookup"><span data-stu-id="21de8-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="21de8-206">Ale pokud se mají tyto změny vydat na testovací server a nakonec produkčního prostředí, chceme pravděpodobně skriptu SQL, který jsme můžete předat do našich DBA.</span><span class="sxs-lookup"><span data-stu-id="21de8-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="21de8-207">Spustit **aktualizace databáze** příkazu, ale tentokrát zadejte **– skript** příznak tak, že změny jsou zapsány do skriptu namísto použití.</span><span class="sxs-lookup"><span data-stu-id="21de8-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="21de8-208">Také určíte zdrojovou a cílovou migrace se vygenerovat skript pro.</span><span class="sxs-lookup"><span data-stu-id="21de8-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="21de8-209">Chceme, aby skript, který bude směřovat prázdnou databázi (**$InitialDatabase**) na nejnovější verzi (migrace **AddPostAbstract**).</span><span class="sxs-lookup"><span data-stu-id="21de8-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="21de8-210">*Pokud nezadáte target migrace, migrace použije nejnovější migrace jako cíl. Pokud nechcete zadat zdroj migrace, migrace použije aktuální stav databáze.*</span><span class="sxs-lookup"><span data-stu-id="21de8-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="21de8-211">Spustit **aktualizace databáze-skript - SourceMigration: $InitialDatabase - TargetMigration: AddPostAbstract** příkazu v konzole Správce balíčků</span><span class="sxs-lookup"><span data-stu-id="21de8-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="21de8-212">Migrace Code First, spustí se kanál migrace, ale místo ve skutečnosti použití změn ji bude vypsat je do souboru .sql za vás.</span><span class="sxs-lookup"><span data-stu-id="21de8-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="21de8-213">Po vygenerování skript ho je pro vás otevřen v aplikaci Visual Studio, které jsou připravené k zobrazení nebo uložení.</span><span class="sxs-lookup"><span data-stu-id="21de8-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="21de8-214">Generování skriptů Idempotentní</span><span class="sxs-lookup"><span data-stu-id="21de8-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="21de8-215">Počínaje EF6, pokud zadáte **– SourceMigration $InitialDatabase** generovaný skript bude "idempotentní".</span><span class="sxs-lookup"><span data-stu-id="21de8-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="21de8-216">Idempotentní skripty můžete upgradovat databázi aktuálně na žádné verze na nejnovější verzi (nebo zadaná verze, pokud používáte **– TargetMigration**).</span><span class="sxs-lookup"><span data-stu-id="21de8-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="21de8-217">Generovaný skript obsahuje logiku pro kontrolu  **\_ \_MigrationsHistory** tabulky a pouze použití změn, které dříve nebyly použity.</span><span class="sxs-lookup"><span data-stu-id="21de8-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="21de8-218">Automaticky upgrade při spuštění aplikace (MigrateDatabaseToLatestVersion se inicializátor).</span><span class="sxs-lookup"><span data-stu-id="21de8-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="21de8-219">Pokud nasazujete aplikaci můžete ho automaticky upgradovat databázi (s použitím čekající migrace) při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="21de8-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="21de8-220">Můžete to provést tak, že zaregistrujete **MigrateDatabaseToLatestVersion** inicializátor databáze.</span><span class="sxs-lookup"><span data-stu-id="21de8-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="21de8-221">Inicializátor databáze obsahuje některé logiku, která se používá k Ujistěte se, že databáze je správně nastavený.</span><span class="sxs-lookup"><span data-stu-id="21de8-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="21de8-222">Tato logika je spustit při prvním kontextu se používá v rámci procesu aplikace (**AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="21de8-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="21de8-223">Abychom mohli aktualizovat **Program.cs** souboru, jak je znázorněno níže, chcete-li nastavit **MigrateDatabaseToLatestVersion** inicializátor pro BlogContext dřív, než kontextu (řádek 14).</span><span class="sxs-lookup"><span data-stu-id="21de8-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="21de8-224">Všimněte si, že musíte také přidat sadu pomocí příkazu pro **System.Data.Entity** obor názvů (řádku 5).</span><span class="sxs-lookup"><span data-stu-id="21de8-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="21de8-225">*Když vytvoříme instanci této inicializátoru, musíme určit typ kontextu (**BlogContext**) a konfiguraci migrace (**konfigurace**) – konfigurace migrace je třída, která je teď Přidá do našich **migrace** složky, pokud jsme povolili migrace.*</span><span class="sxs-lookup"><span data-stu-id="21de8-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="21de8-226">Nyní pokaždé, když naše spouštět aplikace, nejprve zjistí, pokud databáze je zaměřen na aktuální a použít všechny probíhající migrace, pokud není.</span><span class="sxs-lookup"><span data-stu-id="21de8-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
