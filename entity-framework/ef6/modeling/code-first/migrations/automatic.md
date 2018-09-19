---
title: Automatické migrace Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283911"
---
# <a name="automatic-code-first-migrations"></a>Migrace automatické Code First
Automatické migrace můžete pomocí migrace Code First bez nutnosti soubor kódu ve vašem projektu u každé změny, které provedete. Ne všechny změny mohou být automaticky použity – například přejmenování sloupců vyžadují použití migrace založené na kódu.

> [!NOTE]
> Tento článek předpokládá, že víte, jak pomocí migrace Code First v základní scénáře. Pokud ne, pak budete muset přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) než budete pokračovat.

## <a name="recommendation-for-team-environments"></a>Doporučení pro Týmová prostředí

Můžete intersperse migrace automatické a založený na kódu, ale to se nedoporučuje v týmu vývojové scénáře. Pokud jsou součástí týmu vývojářů, které používají správy zdrojových kódů měli byste použít buď čistě automatickou migrací nebo migrací čistě založený na kódu. Dané omezení automatické migrace doporučujeme pomocí migrace založené na kódu v prostředí team.

## <a name="building-an-initial-model--database"></a>Vytváření počáteční modelu & databáze

Než začneme pomocí migrace musíte projekt a model Code First pro práci s. V tomto návodu budeme používat kanonickém **blogu** a **příspěvek** modelu.

-   Vytvořte nový **MigrationsAutomaticDemo** konzolové aplikace
-   Přidejte nejnovější verzi **EntityFramework** balíček NuGet do projektu
    -   **Nástroje –&gt; Správce balíčků knihoven –&gt; Konzola správce balíčků**
    -   Spustit **Install-Package EntityFramework** příkazu
-   Přidat **Model.cs** souboru s kódem je uvedeno níže. Tento kód definuje jedinou **blogu** třídu, která tvoří náš model domény a **BlogContext** třídu, která je náš kontext platforem EF Code First

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

-   Když teď máme modelu je čas ji používat pro přístup k datům. Aktualizace **Program.cs** souboru s kódem je uvedeno níže.

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

-   Spusťte aplikaci a uvidíte, že **MigrationsAutomaticCodeDemo.BlogContext** databáze se vytvoří za vás.

    ![Databáze LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Povolení migrace

Je čas provést některé další změny našeho modelu.

-   Umožňuje zavést vlastnost adresa Url blogu třídy.

``` csharp
    public string Url { get; set; }
```

Pokud chcete aplikaci spustit znovu získali byste s oznámením InvalidOperationException *model zálohování kontextu 'BlogContext' byl změněn, protože byla vytvořena databáze. Zvažte použití migrace Code First k aktualizaci databáze (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Jak výjimku naznačuje, je čas začít pomocí migrace Code First. Protože chceme použít automatické migrace teď ještě chvíli Zůstaneme k určení **– EnableAutomaticMigrations** přepnout.

-   Spustit **povolení migrace – EnableAutomaticMigrations** příkaz v balíčku správce konzoly tento příkaz má přidat **migrace** složku do projektu. Tato nová složka obsahuje jeden soubor:

-   **Třída konfigurace.** Tato třída umožňuje konfigurovat chování migrace pro váš kontext. V tomto návodu budeme používat jenom výchozí konfiguraci.
    *Vzhledem k tomu, že existuje pouze jeden kontext Code First ve vašem projektu, má povolení migrace automaticky vyplněna tato konfigurace se vztahuje na typ kontextu.*

 

## <a name="your-first-automatic-migration"></a>První automatické migrace

Migrace Code First má dva primární příkazy, které se chystáte seznámit se s.

-   **Přidejte migraci** bude generování uživatelského rozhraní další migrace na základě změn, které jste provedli pro váš model, od vytvoření posledního migrace
-   **Aktualizace databáze** se použije čekající migrace do databáze

Budeme vyhnout pomocí migrace přidat (Pokud jsme skutečně potřeba) a zaměřit se na umožníte tím migrace Code First automaticky vypočítá a změny aplikujte. Použijeme **aktualizace databáze** zobrazíte migrace Code First chcete nasdílet změny do našeho modelu (Nový **Blog.Ur**vlastnost l) k databázi.

-   Spustit **aktualizace databáze** příkazu v konzole Správce balíčků.

**MigrationsAutomaticDemo.BlogContext** databáze je teď aktualizovaný zahrnout **Url** sloupec v **blogy** tabulky.

 

## <a name="your-second-automatic-migration"></a>Druhý automatické migrace

Vytvoříme další změnit a nechat migrace Code First automaticky odešlete změny do databáze pro nás.

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

Teď pomocí **aktualizace databáze** uveďte aktuální databázi. Nyní můžeme zadat **– Verbose** příznak, kde můžete zobrazit SQL, na kterém běží migrace Code First.

-   Spustit **aktualizace databáze – Verbose** příkazu v konzole Správce balíčků.

## <a name="adding-a-code-based-migration"></a>Přidání kódu na základě migrace

Nyní Pojďme se podívat na něco jsme mohli chtít používat založený na kódu pro migraci.

-   Přidejme **hodnocení** vlastnost **blogu** třídy

``` csharp
    public int Rating { get; set; }
```

Právě spouštět **aktualizace databáze** tak, aby nabízel tyto změny do databáze. Ale přidáváme neumožňující **Blogs.Rating** sloupce, pokud není žádná existující data v tabulce ji získat přiřadí výchozí CLR datového typu pro nový sloupec (hodnocení je celé číslo, takže, který bude **0**). Chcete zadat výchozí hodnotu, ale **3** tak v tomto existujícím řádků **blogy** tabulky budou začínat vrazíme hodnocení.
Teď pomocí příkazu přidat migrace zapsat tuto změnu si migrace založené na kódu tak, že jsme ho můžete upravit. **Přidat migrace** příkazu, umožníte nám tyto migrace pojmenujte, stačí pojmenujme náš **AddBlogRating**.

-   Spustit **přidat migrace AddBlogRating** příkazu v konzole Správce balíčků.
-   V **migrace** složky teď máme nový **AddBlogRating** migrace. Název souboru migraci je pevně předem s časovým razítkem Nápověda k řazení. Pojďme upravit generovaného kódu můžete zadat výchozí hodnotu 3 pro Blog.Rating (řádek 10 v následujícím kódu)

*Migrace obsahuje také soubor kódu na pozadí, který zachycuje některá metadata. Tato metadata vám umožní migrace Code First k replikaci automatické migrace, které jsme provedli před migrací tento založený na kódu. To je důležité, pokud jiný vývojář chce spusťte naše migrace, nebo když je čas nasazení naší aplikace.*

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

Naše upravených migrace je v pořádku, takže použijeme **aktualizace databáze** uveďte aktuální databázi.

-   Spustit **aktualizace databáze** příkazu v konzole Správce balíčků.

## <a name="back-to-automatic-migrations"></a>Zpět na automatické migrace

Jsme teď můžete přepnout zpět na automatické migrace pro jednodušší změny. Migrace Code First se postará o provedení migrace automatické a založený na kódu ve správném pořadí na základě metadat, který ukládá v souboru kódu na pozadí pro každou migraci založený na kódu.

-   Přidejme do našeho modelu Post.Abstract vlastnost

``` csharp
    public string Abstract { get; set; }
```

Teď můžeme použít **aktualizace databáze** zobrazíte migrace Code First nasdílejte tuto změnu do databáze pomocí automatickou migraci.

-   Spustit **aktualizace databáze** příkazu v konzole Správce balíčků.

## <a name="summary"></a>Souhrn

V tomto návodu jste viděli, jak používat automatické migrace tak, aby nabízel modelu změn v databázi. Také jste viděli, jak generování uživatelského rozhraní a spusťte založený na kódu migrace mezi automatické migrace, když potřebujete větší kontrolu.
