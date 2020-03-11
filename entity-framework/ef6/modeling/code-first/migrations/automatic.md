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
# <a name="automatic-code-first-migrations"></a>Automatické Migrace Code First
Automatické migrace umožňují použít Migrace Code First bez souboru kódu v projektu pro každou změnu, kterou provedete. Ne všechny změny lze použít automaticky – například přejmenování sloupců vyžaduje použití migrace založené na kódu.

> [!NOTE]
> V tomto článku se dozvíte, jak používat Migrace Code First v základních scénářích. Pokud to neuděláte, budete muset před pokračováním přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) .

## <a name="recommendation-for-team-environments"></a>Doporučení pro Týmová prostředí

Můžete prokládat automatické migrace a migrace na základě kódu, ale nedoporučuje se ve scénářích vývoje týmu. Pokud jste součástí týmu vývojářů, kteří používají správu zdrojového kódu, měli byste buď použít čistě automatické migrace nebo čistě migrace založené na kódu. Vzhledem k omezením automatických migrací doporučujeme použití migrace na základě kódu v týmových prostředích.

## <a name="building-an-initial-model--database"></a>Vytvoření počátečního & databáze modelu

Než začneme používat migrace, potřebujeme projekt a model Code First, se kterým pracujete. V tomto návodu budeme používat kanonický **blog** a model **post** .

-   Vytvořit novou konzolovou aplikaci **MigrationsAutomaticDemo**
-   Přidejte do projektu nejnovější verzi balíčku NuGet **EntityFramework** .
    -   **Nástroje –&gt; správce balíčků knihovny –&gt; konzolu Správce balíčků**
    -   Spuštění příkazu **Install-Package EntityFramework**
-   Přidejte soubor **model.cs** s kódem zobrazeným níže. Tento kód definuje jednu třídu **blogu** , která poskytuje náš doménový model a třídu **BlogContext** , která je náš Code First kontextem EF.

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

-   Teď, když máme model, je čas ho použít k provedení přístupu k datům. Aktualizujte soubor **program.cs** pomocí kódu uvedeného níže.

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

-   Spusťte aplikaci a zobrazí se vám pro vás vytvořená databáze **MigrationsAutomaticCodeDemo. BlogContext** .

    ![LocalDB databáze](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Povolování migrací

Je čas udělat další změny v našem modelu.

-   Pojďme do třídy blogu přivést vlastnost URL.

``` csharp
    public string Url { get; set; }
```

Pokud byste chtěli aplikaci znovu spustit, měli byste obdržet zprávu, že došlo ke *změně modelu zálohování kontextu ' BlogContext ', protože databáze byla vytvořena. Pokud chcete aktualizovat databázi (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *)* , zvažte použití migrace Code First.

Vzhledem k tomu, že výjimka navrhuje, je čas začít používat Migrace Code First. Vzhledem k tomu, že chceme používat automatické migrace, zadáváme přepínač **– EnableAutomaticMigrations** .

-   V konzole správce balíčků spusťte příkaz **Povolit – migrace – EnableAutomaticMigrations** . Tento příkaz přidal do našeho projektu složku **migrace** . Tato nová složka obsahuje jeden soubor:

-   **Třída Configuration** Tato třída umožňuje nakonfigurovat, jak se budou migrace chovat pro váš kontext. V tomto návodu použijeme jenom výchozí konfiguraci.
    *Vzhledem k tomu, že v projektu existuje jen jeden Code First kontext, Enable – migrace se automaticky vyplní v kontextu typu, pro který je tato konfigurace platná.*

 

## <a name="your-first-automatic-migration"></a>Vaše první Automatická migrace

Migrace Code First má dva primární příkazy, se kterými se seznámíte.

-   **Přidání – při migraci** dojde k další migraci vygenerované na základě změn, které jste provedli v modelu od vytvoření poslední migrace.
-   **Aktualizace – databáze** bude používat všechny nedokončené migrace do databáze.

Nebudeme používat příkaz Přidat migraci (pokud to opravdu nepotřebujeme) a soustředit se na to, aby Migrace Code First automaticky počítaly a používaly změny. Pojďme použít **Update-Database** a získat migrace Code First pro vložení změn do našeho modelu (nová vlastnost **blog. ur**l) do databáze.

-   Spusťte příkaz **Update-Database** v konzole správce balíčků.

Databáze **MigrationsAutomaticDemo. BlogContext** se teď aktualizovala tak, aby obsahovala sloupec **URL** v tabulce **Blogy** .

 

## <a name="your-second-automatic-migration"></a>Druhá Automatická migrace

Pojďme udělat další změnu a nechat Migrace Code First automaticky vložit změny do databáze pro nás.

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

Nyní použijte **příkaz Update-Database** k uvedení databáze do aktuálního stavu. Tentokrát je nutné zadat příznak **– verbose** , abyste viděli SQL, na kterém je spuštěný migrace Code First.

-   Spusťte příkaz **Update-Database – verbose** v konzole správce balíčků.

## <a name="adding-a-code-based-migration"></a>Přidání migrace na základě kódu

Teď se podíváme na něco, co můžeme použít k použití migrace na základě kódu pro.

-   Pojďme do třídy **blogu** přidat vlastnost **hodnocení** .

``` csharp
    public int Rating { get; set; }
```

Můžeme pouze spustit rutinu **Update-Database** , aby byly tyto změny nabízeny do databáze. Přidáváme ale do tabulky Blogy, které neumožňují hodnotu null **.** Pokud v tabulce existují nějaká existující data, zobrazí se mu výchozí hodnota CLR pro nový sloupec (hodnocení je celé číslo, takže by to bylo **0**). Ale chceme zadat výchozí hodnotu **3** , aby se stávající řádky v tabulce **Blogy** spouštěly se hodnocením dát.
Pojďme pomocí příkazu Add-Migration zapsat tuto změnu do migrace založené na kódu, aby ji bylo možné upravovat. Příkaz **Add-Migration** nám umožňuje dát těmto migracim název, Pojďme ale volat náš **AddBlogRating**.

-   Spusťte příkaz **Add-Migration AddBlogRating** v konzole správce balíčků.
-   Ve složce **migrace** teď máme novou migraci **AddBlogRating** . Název souboru migrace je předem vyřešen s časovým razítkem, které vám pomůžou s řazením. Pojďme upravit generovaný kód a zadat výchozí hodnotu 3 pro blog. hodnocení (řádek 10 v kódu níže)

*Migrace má také soubor kódu na pozadí, který zachycuje některá metadata. Tato metadata umožní Migrace Code First replikovat automatické migrace, které jsme provedli před migrací na základě kódu. To je důležité, pokud jiný vývojář chce spustit naše migrace nebo když je čas nasadit naši aplikaci.*

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

Naše upravená migrace je dobrá, takže použijeme **příkaz Update-Database** , aby se databáze aktualizovala v aktuálním stavu.

-   Spusťte příkaz **Update-Database** v konzole správce balíčků.

## <a name="back-to-automatic-migrations"></a>Zpět na automatické migrace

Nyní je zdarma přejít zpět na automatické migrace pro naše jednodušší změny. Migrace Code First se postará o provádění automatických migrací a migrace na základě kódu ve správném pořadí na základě metadat, která ukládá do souboru kódu na pozadí pro každou migraci na základě kódu.

-   Pojďme do našeho modelu přidat vlastnost post. abstract.

``` csharp
    public string Abstract { get; set; }
```

Nyní můžeme pomocí funkce **Update-Database** získat migrace Code First k odeslání této změny do databáze pomocí automatické migrace.

-   Spusťte příkaz **Update-Database** v konzole správce balíčků.

## <a name="summary"></a>Souhrn

V tomto návodu jste zjistili, jak používat automatické migrace k vložení změn modelu do databáze. Zjistili jste také, jak vygenerovat a spustit migrace na základě kódu v rámci automatických migrací, když potřebujete větší kontrolu.
