---
title: Kód nejprve k existující databázi - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: f05420beb3dff2d632151fcbf48986b0d9cd18ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490607"
---
# <a name="code-first-to-an-existing-database"></a>Kód nejprve ke stávající databázi
Tato videa a podrobný návod poskytuje úvod k vývoji Code First cílení na existující databázi. Kód nejprve umožňuje definovat model pomocí jazyka C\# nebo VB.Net třídy. Volitelně další konfigurace je možné provádět pomocí atributů ve třídách a vlastnostech nebo s použitím rozhraní API fluent.

## <a name="watch-the-video"></a>Podívejte se na video
Toto video je [nyní k dispozici na webu Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Předpoklady

Budete muset mít **Visual Studio 2012** nebo **Visual Studio 2013** nainstalované k dokončení tohoto návodu.

Budete také potřebovat verze **6.1** (nebo novější) z **Entity Framework Tools for Visual Studio** nainstalované. Zobrazit [získat Entity Framework](~/ef6/fundamentals/install.md) informace o instalaci nejnovější verze Entity Framework Tools.

## <a name="1-create-an-existing-database"></a>1. Vytvořit existující databáze

Obvykle Pokud cílíte na existující databázi, které se už vytvořili, ale v tomto návodu budeme potřebovat vytvořit databázi pro přístup k.

Pojďme tedy vygenerovala databáze.

-   Otevřít Visual Studio
-   **Zobrazení –&gt; Průzkumníka serveru**
-   Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**
-   Pokud nejsou připojeni k databázi z **Průzkumníka serveru** předtím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat

    ![Vyberte zdroj dat](~/ef6/media/selectdatasource.png)

-   Připojte se k instanci LocalDB a zadejte **blogovací** jako název databáze

    ![LocalDB připojení](~/ef6/media/localdbconnection.png)

-   Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**

    ![Vytvoření dialogového okna databáze](~/ef6/media/createdatabasedialog.png)

-   Nové databáze se teď budou zobrazovat v Průzkumníku serveru, klikněte pravým tlačítkem myši na něj a vyberte **nový dotaz**
-   Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. Vytvoření aplikace

Pro zjednodušení budeme vytvářet základní Konzolová aplikace, které používá Code First pro přístup k datům:

-   Otevřít Visual Studio
-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Windows** v levé nabídce a **konzolové aplikace**
-   Zadejte **CodeFirstExistingDatabaseSample** jako název
-   Vyberte **OK**

 

## <a name="3-reverse-engineer-model"></a>3. Zpětná analýza modelu

My budeme používat Entity Framework Tools for Visual Studio nám generování počátečního kódu pro mapovat do databáze. Tyto nástroje jsou prostě generuje kód, který můžete také zadat ručně Pokud dáváte přednost.

-   **Projekt –&gt; přidat novou položku...**
-   Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**
-   Zadejte **BloggingContext** jako název a klikněte na **OK**
-   Tím se spustí **Průvodce datovým modelem Entity**
-   Vyberte **Code First z databáze** a klikněte na tlačítko **další**

    ![Průvodce jeden CFE](~/ef6/media/wizardonecfe.png)

-   Vyberte připojení k databázi vytvořené v první části a klikněte na tlačítko **další**

    ![Průvodce dvě CFE](~/ef6/media/wizardtwocfe.png)

-   Klikněte na zaškrtávací políčko vedle položky **tabulky** importovat všechny tabulky a klikněte na tlačítko **dokončit**

    ![Průvodce tři CFE](~/ef6/media/wizardthreecfe.png)

Po dokončení procesu zpětné analýzy, počet položek, které se byly přidány do projektu, můžeme podívejte se na co je přidán.

### <a name="configuration-file"></a>Konfigurační soubor

Soubor App.config se přidala do projektu, tento soubor obsahuje připojovací řetězec k existující databázi.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Můžete si všimnout některými jinými nastaveními v konfiguračním souboru příliš, jedná se o výchozí nastavení EF, které v Code First k vytváření databází. Protože jsme v naší aplikaci jsou mapování k existující databázi tato nastavení se bude ignorovat.*

### <a name="derived-context"></a>Odvozené kontextu

A **BloggingContext** třídy je přidaný do projektu. Kontext představuje relaci s databází, což nám pro dotazování a uložit data.
Poskytuje kontext **DbSet&lt;TEntity&gt;**  pro každý typ v náš model. Uvidíte také, že volá výchozí konstruktor, konstruktor základní třídy pomocí **název =** syntaxe. To říká Code First, že připojovací řetězec má použít pro tento kontext by měly být načteny z konfiguračního souboru.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*Vždycky byste měli použít **název =** syntaxe při použití připojovacího řetězce v konfiguračním souboru. Tím se zajistí, že pokud se připojovací řetězec není k dispozici pak Entity Framework vyvolá místo vytvoření nové databáze podle konvence.*

### <a name="model-classes"></a>Třídy modelu

A konečně **blogu** a **příspěvek** třídy také byly přidány do projektu. Jedná se o domény třídy, které tvoří náš model. Zobrazí se vám Data poznámky u tříd a zadejte konfiguraci kde Code First konvence by odpovídaly struktuře existující databázi. Například, zobrazí se vám **StringLength** Poznámka **Blog.Name** a **Blog.Url** vzhledem k tomu, že má maximální délku **200** v databáze (Code First výchozí hodnota je určený maximální délka podporována zprostředkovatelem databáze - **nvarchar(max)** v systému SQL Server).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. Čtení a zápis dat

Když teď máme modelu je čas ho používat pro přístup k nějaká data. Implementace **hlavní** metoda ve **Program.cs** jak je znázorněno níže. Tento kód vytvoří novou instanci třídy náš kontext a použije ho k vložení nového **blogu**. Potom použije LINQ dotaz pro načtení všech **blogy** z databáze, seřazené podle abecedy podle **Title**.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Co když Moje databáze změní?

Code First pro Průvodce databáze je určena ke generování počáteční bod sadu tříd, které pak můžete upravit a změnit. Pokud se změní schéma databáze můžete ručně upravit třídy nebo provádět jiné zpětné analýzy, přepsat třídy.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Pomocí migrace Code First k existující databázi

Pokud chcete použít migrace Code First s existující databázi, přečtěte si téma [migrace Code First k existující databázi](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Souhrn

V tomto názorném postupu jsme se podívali na vývoji Code First pomocí stávající databáze. Provést zpětnou analýzu sadu tříd, které namapované k databázi a může použít k ukládání a načítání dat jsme použili Entity Framework Tools for Visual Studio.
