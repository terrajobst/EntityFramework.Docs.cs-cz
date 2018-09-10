---
title: Automatické migrace Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 256d1c774a2165dc12daf3d04550566c1a44b751
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250449"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="8b15d-102">Migrace automatické Code First</span><span class="sxs-lookup"><span data-stu-id="8b15d-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="8b15d-103">Automatické migrace můžete pomocí migrace Code First bez nutnosti soubor kódu ve vašem projektu u každé změny, které provedete.</span><span class="sxs-lookup"><span data-stu-id="8b15d-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="8b15d-104">Ne všechny změny mohou být automaticky použity – například přejmenování sloupců vyžadují použití migrace založené na kódu.</span><span class="sxs-lookup"><span data-stu-id="8b15d-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="8b15d-105">Tento článek předpokládá, že víte, jak pomocí migrace Code First v základní scénáře.</span><span class="sxs-lookup"><span data-stu-id="8b15d-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="8b15d-106">Pokud ne, pak budete muset přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="8b15d-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="8b15d-107">Doporučení pro Týmová prostředí</span><span class="sxs-lookup"><span data-stu-id="8b15d-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="8b15d-108">Můžete intersperse migrace automatické a založený na kódu, ale to se nedoporučuje v týmu vývojové scénáře.</span><span class="sxs-lookup"><span data-stu-id="8b15d-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="8b15d-109">Pokud jsou součástí týmu vývojářů, které používají správy zdrojových kódů měli byste použít buď čistě automatickou migrací nebo migrací čistě založený na kódu.</span><span class="sxs-lookup"><span data-stu-id="8b15d-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="8b15d-110">Dané omezení automatické migrace doporučujeme pomocí migrace založené na kódu v prostředí team.</span><span class="sxs-lookup"><span data-stu-id="8b15d-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="8b15d-111">Vytváření počáteční modelu & databáze</span><span class="sxs-lookup"><span data-stu-id="8b15d-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="8b15d-112">Než začneme pomocí migrace musíte projekt a model Code First pro práci s.</span><span class="sxs-lookup"><span data-stu-id="8b15d-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="8b15d-113">V tomto návodu budeme používat kanonickém **blogu** a **příspěvek** modelu.</span><span class="sxs-lookup"><span data-stu-id="8b15d-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="8b15d-114">Vytvořte nový **MigrationsAutomaticDemo** konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="8b15d-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="8b15d-115">Přidejte nejnovější verzi **EntityFramework** balíček NuGet do projektu</span><span class="sxs-lookup"><span data-stu-id="8b15d-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="8b15d-116">**Nástroje –&gt; Správce balíčků knihoven –&gt; Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="8b15d-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="8b15d-117">Spustit **Install-Package EntityFramework** příkazu</span><span class="sxs-lookup"><span data-stu-id="8b15d-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="8b15d-118">Přidat **Model.cs** souboru s kódem je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="8b15d-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="8b15d-119">Tento kód definuje jedinou **blogu** třídu, která tvoří náš model domény a **BlogContext** třídu, která je náš kontext platforem EF Code First</span><span class="sxs-lookup"><span data-stu-id="8b15d-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="8b15d-120">Když teď máme modelu je čas ji používat pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="8b15d-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="8b15d-121">Aktualizace **Program.cs** souboru s kódem je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="8b15d-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="8b15d-122">Spusťte aplikaci a uvidíte, že **MigrationsAutomaticCodeDemo.BlogContext** databáze se vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="8b15d-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![Databáze LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="8b15d-124">Povolení migrace</span><span class="sxs-lookup"><span data-stu-id="8b15d-124">Enabling Migrations</span></span>

<span data-ttu-id="8b15d-125">Je čas provést některé další změny našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="8b15d-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="8b15d-126">Umožňuje zavést vlastnost adresa Url blogu třídy.</span><span class="sxs-lookup"><span data-stu-id="8b15d-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="8b15d-127">Pokud chcete aplikaci spustit znovu získali byste s oznámením InvalidOperationException *model zálohování kontextu 'BlogContext' byl změněn, protože byla vytvořena databáze. Zvažte použití migrace Code First k aktualizaci databáze (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](http://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="8b15d-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="8b15d-128">Jak výjimku naznačuje, je čas začít pomocí migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="8b15d-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="8b15d-129">Protože chceme použít automatické migrace teď ještě chvíli Zůstaneme k určení **– EnableAutomaticMigrations** přepnout.</span><span class="sxs-lookup"><span data-stu-id="8b15d-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="8b15d-130">Spustit **povolení migrace – EnableAutomaticMigrations** příkaz v balíčku správce konzoly tento příkaz má přidat **migrace** složku do projektu.</span><span class="sxs-lookup"><span data-stu-id="8b15d-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="8b15d-131">Tato nová složka obsahuje jeden soubor:</span><span class="sxs-lookup"><span data-stu-id="8b15d-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="8b15d-132">**Třída konfigurace.**</span><span class="sxs-lookup"><span data-stu-id="8b15d-132">**The Configuration class.**</span></span> <span data-ttu-id="8b15d-133">Tato třída umožňuje konfigurovat chování migrace pro váš kontext.</span><span class="sxs-lookup"><span data-stu-id="8b15d-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="8b15d-134">V tomto návodu budeme používat jenom výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8b15d-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="8b15d-135">*Vzhledem k tomu, že existuje pouze jeden kontext Code First ve vašem projektu, má povolení migrace automaticky vyplněna tato konfigurace se vztahuje na typ kontextu.*</span><span class="sxs-lookup"><span data-stu-id="8b15d-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="8b15d-136">První automatické migrace</span><span class="sxs-lookup"><span data-stu-id="8b15d-136">Your First Automatic Migration</span></span>

<span data-ttu-id="8b15d-137">Migrace Code First má dva primární příkazy, které se chystáte seznámit se s.</span><span class="sxs-lookup"><span data-stu-id="8b15d-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="8b15d-138">**Přidejte migraci** bude generování uživatelského rozhraní další migrace na základě změn, které jste provedli pro váš model, od vytvoření posledního migrace</span><span class="sxs-lookup"><span data-stu-id="8b15d-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="8b15d-139">**Aktualizace databáze** se použije čekající migrace do databáze</span><span class="sxs-lookup"><span data-stu-id="8b15d-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="8b15d-140">Budeme vyhnout pomocí migrace přidat (Pokud jsme skutečně potřeba) a zaměřit se na umožníte tím migrace Code First automaticky vypočítá a změny aplikujte.</span><span class="sxs-lookup"><span data-stu-id="8b15d-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="8b15d-141">Použijeme **aktualizace databáze** zobrazíte migrace Code First chcete nasdílet změny do našeho modelu (Nový **Blog.Ur**vlastnost l) k databázi.</span><span class="sxs-lookup"><span data-stu-id="8b15d-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="8b15d-142">Spustit **aktualizace databáze** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="8b15d-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="8b15d-143">**MigrationsAutomaticDemo.BlogContext** databáze je teď aktualizovaný zahrnout **Url** sloupec v **blogy** tabulky.</span><span class="sxs-lookup"><span data-stu-id="8b15d-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="8b15d-144">Druhý automatické migrace</span><span class="sxs-lookup"><span data-stu-id="8b15d-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="8b15d-145">Vytvoříme další změnit a nechat migrace Code First automaticky odešlete změny do databáze pro nás.</span><span class="sxs-lookup"><span data-stu-id="8b15d-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="8b15d-146">Můžeme také přidat nový **příspěvek** třídy</span><span class="sxs-lookup"><span data-stu-id="8b15d-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="8b15d-147">Přidáme také **příspěvky** kolekce **blogu** třídy formuláře druhém konci vztahu mezi **blogu** a **příspěvku**</span><span class="sxs-lookup"><span data-stu-id="8b15d-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="8b15d-148">Teď pomocí **aktualizace databáze** uveďte aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="8b15d-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="8b15d-149">Nyní můžeme zadat **– Verbose** příznak, kde můžete zobrazit SQL, na kterém běží migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="8b15d-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="8b15d-150">Spustit **aktualizace databáze – Verbose** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="8b15d-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="8b15d-151">Přidání kódu na základě migrace</span><span class="sxs-lookup"><span data-stu-id="8b15d-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="8b15d-152">Nyní Pojďme se podívat na něco jsme mohli chtít používat založený na kódu pro migraci.</span><span class="sxs-lookup"><span data-stu-id="8b15d-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="8b15d-153">Přidejme **hodnocení** vlastnost **blogu** třídy</span><span class="sxs-lookup"><span data-stu-id="8b15d-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="8b15d-154">Právě spouštět **aktualizace databáze** tak, aby nabízel tyto změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="8b15d-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="8b15d-155">Ale přidáváme neumožňující **Blogs.Rating** sloupce, pokud není žádná existující data v tabulce ji získat přiřadí výchozí CLR datového typu pro nový sloupec (hodnocení je celé číslo, takže, který bude **0**).</span><span class="sxs-lookup"><span data-stu-id="8b15d-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="8b15d-156">Chcete zadat výchozí hodnotu, ale **3** tak v tomto existujícím řádků **blogy** tabulky budou začínat vrazíme hodnocení.</span><span class="sxs-lookup"><span data-stu-id="8b15d-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="8b15d-157">Teď pomocí příkazu přidat migrace zapsat tuto změnu si migrace založené na kódu tak, že jsme ho můžete upravit.</span><span class="sxs-lookup"><span data-stu-id="8b15d-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="8b15d-158">**Přidat migrace** příkazu, umožníte nám tyto migrace pojmenujte, stačí pojmenujme náš **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="8b15d-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="8b15d-159">Spustit **přidat migrace AddBlogRating** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="8b15d-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="8b15d-160">V **migrace** složky teď máme nový **AddBlogRating** migrace.</span><span class="sxs-lookup"><span data-stu-id="8b15d-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="8b15d-161">Název souboru migraci je pevně předem s časovým razítkem Nápověda k řazení.</span><span class="sxs-lookup"><span data-stu-id="8b15d-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="8b15d-162">Pojďme upravit generovaného kódu můžete zadat výchozí hodnotu 3 pro Blog.Rating (řádek 10 v následujícím kódu)</span><span class="sxs-lookup"><span data-stu-id="8b15d-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="8b15d-163">*Migrace obsahuje také soubor kódu na pozadí, který zachycuje některá metadata. Tato metadata vám umožní migrace Code First k replikaci automatické migrace, které jsme provedli před migrací tento založený na kódu. To je důležité, pokud jiný vývojář chce spusťte naše migrace, nebo když je čas nasazení naší aplikace.*</span><span class="sxs-lookup"><span data-stu-id="8b15d-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="8b15d-164">Naše upravených migrace je v pořádku, takže použijeme **aktualizace databáze** uveďte aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="8b15d-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="8b15d-165">Spustit **aktualizace databáze** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="8b15d-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="8b15d-166">Zpět na automatické migrace</span><span class="sxs-lookup"><span data-stu-id="8b15d-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="8b15d-167">Jsme teď můžete přepnout zpět na automatické migrace pro jednodušší změny.</span><span class="sxs-lookup"><span data-stu-id="8b15d-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="8b15d-168">Migrace Code First se postará o provedení migrace automatické a založený na kódu ve správném pořadí na základě metadat, který ukládá v souboru kódu na pozadí pro každou migraci založený na kódu.</span><span class="sxs-lookup"><span data-stu-id="8b15d-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="8b15d-169">Přidejme do našeho modelu Post.Abstract vlastnost</span><span class="sxs-lookup"><span data-stu-id="8b15d-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="8b15d-170">Teď můžeme použít **aktualizace databáze** zobrazíte migrace Code First nasdílejte tuto změnu do databáze pomocí automatickou migraci.</span><span class="sxs-lookup"><span data-stu-id="8b15d-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="8b15d-171">Spustit **aktualizace databáze** příkazu v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="8b15d-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="8b15d-172">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8b15d-172">Summary</span></span>

<span data-ttu-id="8b15d-173">V tomto návodu jste viděli, jak používat automatické migrace tak, aby nabízel modelu změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="8b15d-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="8b15d-174">Také jste viděli, jak generování uživatelského rozhraní a spusťte založený na kódu migrace mezi automatické migrace, když potřebujete větší kontrolu.</span><span class="sxs-lookup"><span data-stu-id="8b15d-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
