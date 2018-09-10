---
title: Kód nejprve do nové databáze - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 8ed1bfbc3536acc0d83b9c8ecdd180aeb44eff83
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251047"
---
# <a name="code-first-to-a-new-database"></a>Kód nejprve do nové databáze
Tato videa a podrobný návod poskytuje úvod k vývoji Code First cílení na novou databázi. Tento scénář obsahuje cílení na databázi, která neexistuje a vytvoří Code First nebo prázdnou databázi této Code First přidá nové tabulky. Kód nejprve umožňuje definovat model pomocí jazyka C\# nebo VB.Net třídy. Další konfigurace můžete volitelně provést pomocí atributů ve třídách a vlastnostech nebo s použitím rozhraní API fluent.

## <a name="watch-the-video"></a>Podívejte se na video
Toto video obsahuje úvod k vývoji Code First cílení na novou databázi. Tento scénář obsahuje cílení na databázi, která neexistuje a vytvoří Code First nebo prázdnou databázi této Code First přidá nové tabulky. Kód nejprve umožňuje definovat model pomocí tříd jazyka C# nebo VB.Net. Další konfigurace můžete volitelně provést pomocí atributů ve třídách a vlastnostech nebo s použitím rozhraní API fluent.

**Přednášející:**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

# <a name="pre-requisites"></a>Předpoklady

Budete muset mít alespoň Visual Studio 2010 nebo Visual Studio 2012 nainstalovat k dokončení tohoto návodu.

Pokud používáte Visual Studio 2010, musíte také mít [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) nainstalované.

## <a name="1-create-the-application"></a>1. Vytvoření aplikace

Pro zjednodušení budeme vytvářet základní Konzolová aplikace, které používá Code First pro přístup k datům.

-   Otevřít Visual Studio
-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Windows** v levé nabídce a **konzolové aplikace**
-   Zadejte **CodeFirstNewDatabaseSample** jako název
-   Vyberte **OK**

## <a name="2-create-the-model"></a>2. Vytvoření modelu

Umožňuje definovat velmi jednoduchý model s použitím třídy. Jsme právě definujete je v souboru Program.cs, ale v reálné aplikaci tříd si by rozdělit na samostatné soubory a potenciálně samostatného projektu.

Pod Program definice třídy v souboru Program.cs přidejte následující dvě třídy.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

Můžete si všimnout, že provádíme dvě navigační vlastnosti (Blog.Posts a Post.Blog) virtuální. To umožňuje funkci opožděné načtení Entity Framework. Opožděné načtení znamená, že obsah těchto vlastností se automaticky načtou z databáze při pokusu o přístup k nim.

## <a name="3-create-a-context"></a>3. Vytvoření kontextu

Nyní je možné definovat odvozené kontext, který reprezentuje relaci s databází, což nám pro dotazování a uložit data. Kontext, který je odvozen od System.Data.Entity.DbContext a zveřejňuje typu DbSet definujeme&lt;TEntity&gt; pro každou třídu v náš model.

Nyní zapínáme využíval typy z Entity Framework, takže je potřeba přidat balíček NuGet objektu EntityFramework.

-   **Project –&gt; spravovat balíčky NuGet...**
    Poznámka: Pokud nemáte k dispozici **spravovat balíčky NuGet...** možnost byste měli nainstalovat [nejnovější verze Nugetu](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Vyberte **Online** kartu
-   Vyberte **EntityFramework** balíčku
-   Klikněte na tlačítko **instalace**

Přidat sadu pomocí příkazu pro System.Data.Entity v horní části souboru program.cs.

``` csharp
using System.Data.Entity;
```

Pod příspěvkem třída v souboru Program.cs přidejte následující odvozené kontextu.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Tady je úplný seznam všech co Program.cs teď může obsahovat.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

To je veškerý kód, který musíme začít ukládání a načítání dat. Samozřejmě je poměrně děje na pozadí a provedeme podívat, jestli ve chvíli, než se ale první Pojďme pozorování v akci.

## <a name="4-reading--writing-data"></a>4. Čtení a zápis dat

V souboru Program.cs implementujte metodu Main, jak je znázorněno níže. Tento kód vytvoří novou instanci třídy náš kontext a použije ho k vložení nového blogu. Pak používá dotaz LINQ k načtení všech blogy z databáze abecedně podle názvu.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Teď můžete aplikaci spustit a otestování.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Kde jsou moje Data?

Podle konvence DbContext vytvořila databázi za vás.

-   Pokud místní instance SQL Express je k dispozici (nainstalované ve výchozím nastavení se sadou Visual Studio 2010) pak Code First vytvořil databázi na instanci
-   Pokud systém SQL Express není k dispozici, pak Code First, pokusí se použít [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (nainstalované ve výchozím nastavení pomocí sady Visual Studio 2012)
-   Plně kvalifikovaný název odvozené kontextu, v našem, který se má stejný název databáze **CodeFirstNewDatabaseSample.BloggingContext**

Jedná se o pouze výchozích konvencí a existují různé způsoby, jak změnit databázi, která používá Code First, další informace najdete v **jak DbContext zjistí Model a připojení k databázi** tématu.
Můžete připojit k této databázi pomocí Průzkumníku serveru v sadě Visual Studio

-   **Zobrazení –&gt; Průzkumníka serveru**
-   Klikněte pravým tlačítkem na **datová připojení** a vyberte **přidat připojení...**
-   Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat jako zdroj dat Microsoft SQL Server

    ![Vyberte zdroj dat](~/ef6/media/selectdatasource.png)

-   Připojte se k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali

Nyní jsme mohli prohlédnout schématu, které Code First vytvořili.

![Výchozí schéma](~/ef6/media/schemainitial.png)

DbContext dobře fungoval jaké třídy, které zahrnují zobrazením vlastnosti DbSet, které jsme definovali v modelu. Potom použije výchozí sadu Code First konvence k určení názvy tabulek a sloupců, určit datové typy, najít primární klíče, atd. Později v tomto názorném postupu podíváme na potlačení Tato konvence.

## <a name="5-dealing-with-model-changes"></a>5. Práce se změnami modelu

Nyní je čas na něm provést nějaké změny náš model, když jsme provedení těchto změn je také potřeba aktualizovat schéma databáze. K tomu budeme používat funkci migrace Code First nebo migrace pro krátké.

Migrace umožňuje nám umožnily seřazené sady kroků, které popisují, jak (a) naše schéma databáze. Každý z těchto kroků, označované jako migrace, obsahuje část kódu, jsou zde popsány změny, které má být použita. 

Prvním krokem je povolení migrace Code First pro naše BloggingContext.

-   **Nástroje –&gt; Správce balíčků knihoven -&gt; Konzola správce balíčků**
-   Spustit **povolení migrace** příkazu v konzole Správce balíčků
-   Nová složka migrace je přidaný do projektu, který obsahuje dvě položky:
    -   **Configuration.cs** – tento soubor obsahuje nastavení, které budou používat migrace pro migraci BloggingContext. Nemusíme přitom něco změnit, ale tady je, kde můžete určit obor názvů změní počáteční hodnoty data, zaregistrujte poskytovatele pro ostatní databáze, migrace se generují v atd.
    -   **&lt;časové razítko&gt;\_InitialCreate.cs** – Toto je první migraci, představuje změny, které již byly implementovány do databáze se nebudou prázdnou databázi, který obsahuje tabulky blogů a příspěvky . I když umožňujeme Code First automaticky vytvoří tyto tabulky pro USA, teď, když jsme výslovného souhlasu pro migrace se převedl do migrace. Kód nejprve má také zaznamenána v místní databázi, tato migrace již byl použit. Časové razítko souboru se používá pro účely řazení.

    Teď můžeme změnit náš model, přidejte do třídy blogu vlastnost adresa Url:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Spustit **přidat migrace AddUrl** příkazu v konzole Správce balíčků.
    Příkaz Přidat migrace kontroluje změny od poslední migrace a vygeneruje uživatelské rozhraní nového migrace se všechny změny, které se nacházejí. Můžeme poskytnout migrace názvu; v tomto případě voláme "AddUrl" migrace.
    Automaticky generovaný kód říká, že potřebujeme přidat adresu Url sloupci, který může obsahovat data řetězce, na vlastníka. Blogy tabulky. V případě potřeby může upravíme automaticky generovaný kód, ale není to nutné v tomto případě.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
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

-   Spustit **aktualizace databáze** příkazu v konzole Správce balíčků. Tento příkaz použije všechny probíhající migrace do databáze. Již byla použita naší InitialCreate migrace, migrace se pouze použít naše nová AddUrl migrace.
    Tip: Můžete použít **– Verbose** přepnout při volání metody Update-databázi pro zobrazení, která je spouštěna na databázi SQL.

Blogy tabulky v databázi je nyní přidán nový sloupec adresy Url:

![Schéma URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Datové poznámky

Zatím jste pouze umožňujeme EF zjišťování modelu s použitím jeho výchozí konvence, ale že se chystáte nastat situace, kdy našich tříd není konvencím a musíme být schopni provést další konfiguraci. Existují dvě možnosti pro tento účel; Podíváme se na datových poznámek v této části a pak rozhraní fluent API v další části.

-   Přidejme do našeho modelu třídu uživatelů

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Musíme také přidat sadu náš odvozené kontext

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Pokud jsme se pokusili přidejte migraci dostali bychom o tom, že chyba "*EntityType 'Uživatele' nemá definovaný žádný klíč. Definujte klíč pro tento element EntityType."* protože EF nemá žádnou možnost zjistit, že uživatelské jméno by měly být primární klíč pro uživatele.
-   Vytvoříme nyní použít anotacemi dat, takže potřebujeme přidat, a s použitím příkazu v horní části souboru program.cs

```
using System.ComponentModel.DataAnnotations;
```

-   Nyní opatřit poznámkami vlastnost Username k určení, že se jedná o primární klíč

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Použití **přidat migrace AddUser** příkaz k migraci na použití těchto změn v databázi
-   Spustit **aktualizace databáze** příkaz pro použití nového migrace databáze

Nová tabulka se teď přidá do databáze:

![Schéma s uživateli](~/ef6/media/schemawithusers.png)

Úplný seznam podporovaných EF poznámky je:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. Rozhraní Fluent API

V předchozí části jsme se podívali na pomocí datové poznámky doplňují nebo potlačit, co bylo zjištěno konvencí. Druhý způsob konfigurace modelu je přes Code First rozhraní fluent API.

Většinu konfiguraci modelu lze provést pomocí jednoduchých datových poznámek. Rozhraní fluent API je rozšířený způsob určení konfigurace modelu, která zahrnuje všechno, co se anotacemi dat způsobů, jak kromě toho některé pokročilejší konfigurace s anotacemi dat nebylo možné. Datové poznámky a rozhraní fluent API můžete použít společně.

Pro přístup k rozhraní API fluent přepsat metodu OnModelCreating v DbContext. Řekněme, že jsme chtěli přejmenovat sloupec, který User.DisplayName je uložen v zobrazíte\_název.

-   Potlačí metodu OnModelCreating na BloggingContext následujícím kódem

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Použití **přidat migrace ChangeDisplayName** příkaz k migraci na použití těchto změn v databázi.
-   Spustit **aktualizace databáze** příkaz pro použití novou migraci databáze.

Sloupec DisplayName přejmenovává na zobrazení\_název:

![Schéma se zobrazovaným názvem přejmenovat](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Souhrn

V tomto názorném postupu jsme se podívali na vývoji Code First pomocí nové databáze. Jsme definována model s použitím tříd pak používá k vytvoření databáze a ukládání a načítání dat tohoto modelu. Po vytvoření databáze můžeme použít ke změně schématu jako náš model se migrace Code First. Také jsme viděli, jak nakonfigurovat model s použitím datových poznámek a rozhraní Fluent API.
