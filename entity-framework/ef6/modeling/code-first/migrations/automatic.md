---
title: Automatické Migrace Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418998"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="28ade-102">Automatické Migrace Code First</span><span class="sxs-lookup"><span data-stu-id="28ade-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="28ade-103">Automatické migrace umožňují použít Migrace Code First bez souboru kódu v projektu pro každou změnu, kterou provedete.</span><span class="sxs-lookup"><span data-stu-id="28ade-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="28ade-104">Ne všechny změny lze použít automaticky – například přejmenování sloupců vyžaduje použití migrace založené na kódu.</span><span class="sxs-lookup"><span data-stu-id="28ade-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="28ade-105">V tomto článku se dozvíte, jak používat Migrace Code First v základních scénářích.</span><span class="sxs-lookup"><span data-stu-id="28ade-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="28ade-106">Pokud to neuděláte, budete muset před pokračováním přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) .</span><span class="sxs-lookup"><span data-stu-id="28ade-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="28ade-107">Doporučení pro Týmová prostředí</span><span class="sxs-lookup"><span data-stu-id="28ade-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="28ade-108">Můžete prokládat automatické migrace a migrace na základě kódu, ale nedoporučuje se ve scénářích vývoje týmu.</span><span class="sxs-lookup"><span data-stu-id="28ade-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="28ade-109">Pokud jste součástí týmu vývojářů, kteří používají správu zdrojového kódu, měli byste buď použít čistě automatické migrace nebo čistě migrace založené na kódu.</span><span class="sxs-lookup"><span data-stu-id="28ade-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="28ade-110">Vzhledem k omezením automatických migrací doporučujeme použití migrace na základě kódu v týmových prostředích.</span><span class="sxs-lookup"><span data-stu-id="28ade-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="28ade-111">Vytvoření počátečního & databáze modelu</span><span class="sxs-lookup"><span data-stu-id="28ade-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="28ade-112">Než začneme používat migrace, potřebujeme projekt a model Code First, se kterým pracujete.</span><span class="sxs-lookup"><span data-stu-id="28ade-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="28ade-113">V tomto návodu budeme používat kanonický **blog** a model **post** .</span><span class="sxs-lookup"><span data-stu-id="28ade-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="28ade-114">Vytvořit novou konzolovou aplikaci **MigrationsAutomaticDemo**</span><span class="sxs-lookup"><span data-stu-id="28ade-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="28ade-115">Přidejte do projektu nejnovější verzi balíčku NuGet **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="28ade-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="28ade-116">**Nástroje –&gt; správce balíčků knihovny –&gt; konzolu Správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="28ade-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="28ade-117">Spuštění příkazu **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="28ade-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="28ade-118">Přidejte soubor **model.cs** s kódem zobrazeným níže.</span><span class="sxs-lookup"><span data-stu-id="28ade-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="28ade-119">Tento kód definuje jednu třídu **blogu** , která poskytuje náš doménový model a třídu **BlogContext** , která je náš Code First kontextem EF.</span><span class="sxs-lookup"><span data-stu-id="28ade-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
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

-   <span data-ttu-id="28ade-120">Teď, když máme model, je čas ho použít k provedení přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="28ade-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="28ade-121">Aktualizujte soubor **program.cs** pomocí kódu uvedeného níže.</span><span class="sxs-lookup"><span data-stu-id="28ade-121">Update the **Program.cs** file with the code shown below.</span></span>

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
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

-   <span data-ttu-id="28ade-122">Spusťte aplikaci a zobrazí se vám pro vás vytvořená databáze **MigrationsAutomaticCodeDemo. BlogContext** .</span><span class="sxs-lookup"><span data-stu-id="28ade-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![LocalDB databáze](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="28ade-124">Povolování migrací</span><span class="sxs-lookup"><span data-stu-id="28ade-124">Enabling Migrations</span></span>

<span data-ttu-id="28ade-125">Je čas udělat další změny v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="28ade-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="28ade-126">Pojďme do třídy blogu přivést vlastnost URL.</span><span class="sxs-lookup"><span data-stu-id="28ade-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="28ade-127">Pokud byste chtěli aplikaci znovu spustit, měli byste obdržet zprávu, že došlo ke *změně modelu zálohování kontextu ' BlogContext ', protože databáze byla vytvořena. Pokud chcete aktualizovat databázi (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *)* , zvažte použití migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="28ade-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="28ade-128">Vzhledem k tomu, že výjimka navrhuje, je čas začít používat Migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="28ade-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="28ade-129">Vzhledem k tomu, že chceme používat automatické migrace, zadáváme přepínač **– EnableAutomaticMigrations** .</span><span class="sxs-lookup"><span data-stu-id="28ade-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="28ade-130">V konzole správce balíčků spusťte příkaz **Povolit – migrace – EnableAutomaticMigrations** . Tento příkaz přidal do našeho projektu složku **migrace** .</span><span class="sxs-lookup"><span data-stu-id="28ade-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="28ade-131">Tato nová složka obsahuje jeden soubor:</span><span class="sxs-lookup"><span data-stu-id="28ade-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="28ade-132">**Třída Configuration**</span><span class="sxs-lookup"><span data-stu-id="28ade-132">**The Configuration class.**</span></span> <span data-ttu-id="28ade-133">Tato třída umožňuje nakonfigurovat, jak se budou migrace chovat pro váš kontext.</span><span class="sxs-lookup"><span data-stu-id="28ade-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="28ade-134">V tomto návodu použijeme jenom výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="28ade-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="28ade-135">*Vzhledem k tomu, že v projektu existuje jen jeden Code First kontext, Enable – migrace se automaticky vyplní v kontextu typu, pro který je tato konfigurace platná.*</span><span class="sxs-lookup"><span data-stu-id="28ade-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="28ade-136">Vaše první Automatická migrace</span><span class="sxs-lookup"><span data-stu-id="28ade-136">Your First Automatic Migration</span></span>

<span data-ttu-id="28ade-137">Migrace Code First má dva primární příkazy, se kterými se seznámíte.</span><span class="sxs-lookup"><span data-stu-id="28ade-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="28ade-138">**Přidání – při migraci** dojde k další migraci vygenerované na základě změn, které jste provedli v modelu od vytvoření poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="28ade-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="28ade-139">**Aktualizace – databáze** bude používat všechny nedokončené migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="28ade-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="28ade-140">Nebudeme používat příkaz Přidat migraci (pokud to opravdu nepotřebujeme) a soustředit se na to, aby Migrace Code First automaticky počítaly a používaly změny.</span><span class="sxs-lookup"><span data-stu-id="28ade-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="28ade-141">Pojďme použít **Update-Database** a získat migrace Code First pro vložení změn do našeho modelu (nová vlastnost **blog. ur**l) do databáze.</span><span class="sxs-lookup"><span data-stu-id="28ade-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="28ade-142">Spusťte příkaz **Update-Database** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="28ade-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="28ade-143">Databáze **MigrationsAutomaticDemo. BlogContext** se teď aktualizovala tak, aby obsahovala sloupec **URL** v tabulce **Blogy** .</span><span class="sxs-lookup"><span data-stu-id="28ade-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="28ade-144">Druhá Automatická migrace</span><span class="sxs-lookup"><span data-stu-id="28ade-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="28ade-145">Pojďme udělat další změnu a nechat Migrace Code First automaticky vložit změny do databáze pro nás.</span><span class="sxs-lookup"><span data-stu-id="28ade-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="28ade-146">Pojďme také přidat novou třídu **post**</span><span class="sxs-lookup"><span data-stu-id="28ade-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="28ade-147">Do třídy **blog** přidáme také kolekci **příspěvky** , která bude tvořit druhý konec relace mezi **blogem** a **příspěvkem** .</span><span class="sxs-lookup"><span data-stu-id="28ade-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="28ade-148">Nyní použijte **příkaz Update-Database** k uvedení databáze do aktuálního stavu.</span><span class="sxs-lookup"><span data-stu-id="28ade-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="28ade-149">Tentokrát je nutné zadat příznak **– verbose** , abyste viděli SQL, na kterém je spuštěný migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="28ade-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="28ade-150">Spusťte příkaz **Update-Database – verbose** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="28ade-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="28ade-151">Přidání migrace na základě kódu</span><span class="sxs-lookup"><span data-stu-id="28ade-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="28ade-152">Teď se podíváme na něco, co můžeme použít k použití migrace na základě kódu pro.</span><span class="sxs-lookup"><span data-stu-id="28ade-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="28ade-153">Pojďme do třídy **blogu** přidat vlastnost **hodnocení** .</span><span class="sxs-lookup"><span data-stu-id="28ade-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="28ade-154">Můžeme pouze spustit rutinu **Update-Database** , aby byly tyto změny nabízeny do databáze.</span><span class="sxs-lookup"><span data-stu-id="28ade-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="28ade-155">Přidáváme ale do tabulky Blogy, které neumožňují hodnotu null **.** Pokud v tabulce existují nějaká existující data, zobrazí se mu výchozí hodnota CLR pro nový sloupec (hodnocení je celé číslo, takže by to bylo **0**).</span><span class="sxs-lookup"><span data-stu-id="28ade-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="28ade-156">Ale chceme zadat výchozí hodnotu **3** , aby se stávající řádky v tabulce **Blogy** spouštěly se hodnocením dát.</span><span class="sxs-lookup"><span data-stu-id="28ade-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="28ade-157">Pojďme pomocí příkazu Add-Migration zapsat tuto změnu do migrace založené na kódu, aby ji bylo možné upravovat.</span><span class="sxs-lookup"><span data-stu-id="28ade-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="28ade-158">Příkaz **Add-Migration** nám umožňuje dát těmto migracim název, Pojďme ale volat náš **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="28ade-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="28ade-159">Spusťte příkaz **Add-Migration AddBlogRating** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="28ade-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="28ade-160">Ve složce **migrace** teď máme novou migraci **AddBlogRating** .</span><span class="sxs-lookup"><span data-stu-id="28ade-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="28ade-161">Název souboru migrace je předem vyřešen s časovým razítkem, které vám pomůžou s řazením.</span><span class="sxs-lookup"><span data-stu-id="28ade-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="28ade-162">Pojďme upravit generovaný kód a zadat výchozí hodnotu 3 pro blog. hodnocení (řádek 10 v kódu níže)</span><span class="sxs-lookup"><span data-stu-id="28ade-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="28ade-163">*Migrace má také soubor kódu na pozadí, který zachycuje některá metadata. Tato metadata umožní Migrace Code First replikovat automatické migrace, které jsme provedli před migrací na základě kódu. To je důležité, pokud jiný vývojář chce spustit naše migrace nebo když je čas nasadit naši aplikaci.*</span><span class="sxs-lookup"><span data-stu-id="28ade-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

<span data-ttu-id="28ade-164">Naše upravená migrace je dobrá, takže použijeme **příkaz Update-Database** , aby se databáze aktualizovala v aktuálním stavu.</span><span class="sxs-lookup"><span data-stu-id="28ade-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="28ade-165">Spusťte příkaz **Update-Database** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="28ade-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="28ade-166">Zpět na automatické migrace</span><span class="sxs-lookup"><span data-stu-id="28ade-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="28ade-167">Nyní je zdarma přejít zpět na automatické migrace pro naše jednodušší změny.</span><span class="sxs-lookup"><span data-stu-id="28ade-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="28ade-168">Migrace Code First se postará o provádění automatických migrací a migrace na základě kódu ve správném pořadí na základě metadat, která ukládá do souboru kódu na pozadí pro každou migraci na základě kódu.</span><span class="sxs-lookup"><span data-stu-id="28ade-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="28ade-169">Pojďme do našeho modelu přidat vlastnost post. abstract.</span><span class="sxs-lookup"><span data-stu-id="28ade-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="28ade-170">Nyní můžeme pomocí funkce **Update-Database** získat migrace Code First k odeslání této změny do databáze pomocí automatické migrace.</span><span class="sxs-lookup"><span data-stu-id="28ade-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="28ade-171">Spusťte příkaz **Update-Database** v konzole správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="28ade-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="28ade-172">Souhrn</span><span class="sxs-lookup"><span data-stu-id="28ade-172">Summary</span></span>

<span data-ttu-id="28ade-173">V tomto návodu jste zjistili, jak používat automatické migrace k vložení změn modelu do databáze.</span><span class="sxs-lookup"><span data-stu-id="28ade-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="28ade-174">Zjistili jste také, jak vygenerovat a spustit migrace na základě kódu v rámci automatických migrací, když potřebujete větší kontrolu.</span><span class="sxs-lookup"><span data-stu-id="28ade-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
