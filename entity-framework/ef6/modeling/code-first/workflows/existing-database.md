---
title: Code First existující databáze – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 0a51f826422d7e2bff33b968605eace1e754c425
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418873"
---
# <a name="code-first-to-an-existing-database"></a>Code First do existující databáze
Toto video a podrobný návod vám poskytnou Úvod do Code First vývoje cílící na stávající databázi. Code First umožňuje definovat model pomocí tříd C\# nebo VB.Net. Volitelně můžete provést další konfiguraci pomocí atributů u tříd a vlastností nebo pomocí rozhraní API Fluent.

## <a name="watch-the-video"></a>Přehrát video
Toto video je [teď k dispozici na Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Požadavky

Abyste mohli dokončit tento návod, budete muset mít nainstalovanou **aplikaci Visual Studio 2012** nebo **Visual Studio 2013** .

Budete také potřebovat verzi **6,1** (nebo novější) Entity Framework Tools nainstalované sady **Visual Studio** . Informace o instalaci nejnovější verze Entity Framework Tools najdete v tématu věnovaném [získání Entity Framework](~/ef6/fundamentals/install.md) .

## <a name="1-create-an-existing-database"></a>1. vytvoření existující databáze

Když cílíte na existující databázi, bude už vytvořená, ale pro tento návod musíme pro přístup vytvořit databázi.

Pojďme dopředu a vygenerovat databázi.

-   Otevřete sadu Visual Studio.
-   **Zobrazení-&gt; Průzkumník serveru**
-   Klikněte pravým tlačítkem na **datová připojení –&gt; přidat připojení...**
-   Pokud jste se k databázi nepřipojili z **Průzkumník serveru** před tím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat

    ![Výběr zdroje dat](~/ef6/media/selectdatasource.png)

-   Připojte se k instanci LocalDB a jako název databáze zadejte **blog** .

    ![Připojení LocalDB](~/ef6/media/localdbconnection.png)

-   Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .

    ![Dialogové okno vytvořit databázi](~/ef6/media/createdatabasedialog.png)

-   Nová databáze se nyní zobrazí v Průzkumník serveru, klikněte na ni pravým tlačítkem myši a vyberte **Nový dotaz** .
-   Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .

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

Abychom mohli dělat jednoduché věci, vytvoříme základní konzolovou aplikaci, která používá Code First k přístupu k datům:

-   Otevřete sadu Visual Studio.
-   **Soubor –&gt; projekt New-&gt;...**
-   V levé nabídce a v **konzolové aplikaci** vyberte **Windows** .
-   Jako název zadejte **CodeFirstExistingDatabaseSample** .
-   Vyberte **OK**.

 

## <a name="3-reverse-engineer-model"></a>3. zpětná analýza modelu

Použijeme Entity Framework Tools pro Visual Studio, abychom nám pomohli vygenerovat nějaký počáteční kód pro mapování na databázi. Tyto nástroje právě generují kód, který můžete zadat také v případě, že dáváte přednost.

-   **Projekt –&gt; přidat novou položku...**
-   V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**
-   Jako název zadejte **BloggingContext** a klikněte na **OK** .
-   Spustí se **průvodce model EDM (Entity Data Model)** .
-   Vyberte **Code First z databáze** a klikněte na **Další** .

    ![Průvodce One CFE](~/ef6/media/wizardonecfe.png)

-   Vyberte připojení k databázi, kterou jste vytvořili v první části, a klikněte na **Další** .

    ![Průvodce – dva CFE](~/ef6/media/wizardtwocfe.png)

-   Kliknutím na zaškrtávací políčko vedle **tabulky** naimportujte všechny tabulky a klikněte na **Dokončit** .

    ![Průvodce třemi CFE](~/ef6/media/wizardthreecfe.png)

Po dokončení procesu zpětného analýz se do projektu přidají několik položek, které se podívejme na to, co se přidalo.

### <a name="configuration-file"></a>Konfigurační soubor

Do projektu byl přidán soubor App. config, tento soubor obsahuje připojovací řetězec k existující databázi.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Všimněte si, že některá další nastavení jsou také v konfiguračním souboru. Jedná se o výchozí nastavení EF, které sděluje Code First, kde vytvořit databáze. Vzhledem k tomu, že jsme provedli mapování na existující databázi, tato nastavení budou v naší aplikaci ignorována.*

### <a name="derived-context"></a>Odvozený kontext

Do projektu se přidala třída **BloggingContext** . Kontext představuje relaci s databází a umožňuje nám dotazovat se na data a ukládat je.
Kontext zpřístupňuje **negenerickými&lt;TEntity&gt;** pro každý typ v našem modelu. Všimněte si také, že výchozí konstruktor volá základní konstruktor pomocí syntaxe **Name =** . To oznamuje Code First, že připojovací řetězec, který se má použít pro tento kontext, by měl být načten z konfiguračního souboru.

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

*Při použití připojovacího řetězce v konfiguračním souboru byste vždy měli použít syntaxi **Name =** . Tím je zajištěno, že pokud připojovací řetězec není k dispozici, Entity Framework bude vyvolávat místo vytvoření nové databáze podle konvence.*

### <a name="model-classes"></a>Třídy modelu

Nakonec byly do projektu přidány také třídy **blog** a **příspěvek** . Jedná se o třídy domény, které tvoří náš model. V případě, že se konvence Code First neshodují se strukturou existující databáze, uvidíte datové poznámky použité na třídy. Například se zobrazí anotace **StringLength** na **blog.Name** a **blogu. URL** , protože v databázi mají maximální délku **200** (Code First výchozí hodnota je použití maximun podporovaného poskytovatelem databáze- **nvarchar (max)** v SQL Server).

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

## <a name="4-reading--writing-data"></a>4. čtení & zápisu dat

Teď, když máme model, je čas ho použít pro přístup k některým datům. Implementujte metodu **Main** v **program.cs** , jak je znázorněno níže. Tento kód vytvoří novou instanci našeho kontextu a pak ho použije k vložení nového **blogu**. Pak použije dotaz LINQ k načtení všech **blogů** z databáze seřazené abecedně podle **názvu**.

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

Nyní můžete spustit aplikaci a otestovat ji.

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Co když se změní moje databáze?

Průvodce Code First k databázi je navržený tak, aby vygeneroval počáteční sadu tříd, které pak můžete upravit a upravit. Pokud se vaše schéma databáze změní, můžete buď ručně upravit třídy, nebo provést jinou zpětnou analýzu pro přepsání tříd.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Použití Migrace Code First do existující databáze

Pokud chcete použít Migrace Code First s existující databází, přečtěte si téma [migrace Code First do existující databáze](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na Code First vývoje pomocí existující databáze. V sadě Visual Studio jsme použili Entity Framework Tools pro zpětnou analýzu sady tříd, které jsou namapované na databázi, a dají se použít k ukládání a načítání dat.
