---
title: Návod k entitám s sebou samým sledováním – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 9bd644461f50a7eff1006cb8866ca9a3b08b6b8d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181713"
---
# <a name="self-tracking-entities-walkthrough"></a>Návod k entitám s sebou samým sledováním
> [!IMPORTANT]
> Nedoporučujeme používat šablonu samoobslužné sledování – entity. Bude dál k dispozici jenom pro podporu stávajících aplikací. Pokud vaše aplikace vyžaduje práci s odpojenými grafy entit, zvažte další alternativy, jako jsou například sledovací [entity](https://trackableentities.github.io/), což je technologie podobná entitám s vlastním sledováním, které jsou aktivně vyvíjené komunitou, nebo psaním vlastní kód s využitím rozhraní API pro sledování změn nízké úrovně.

Tento návod ukazuje scénář, ve kterém služba Windows Communication Foundation (WCF) zveřejňuje operaci, která vrací graf entity. V dalším kroku klientská aplikace pracuje s grafem a odesílá změny operace služby, která ověřuje a ukládá aktualizace databáze pomocí Entity Framework.

Před dokončením tohoto návodu se ujistěte, že si přečtete stránku [entit s vlastním sledováním](index.md) .

V tomto návodu se dokončí následující akce:

-   Vytvoří databázi pro přístup.
-   Vytvoří knihovnu tříd, která obsahuje model.
-   Zahodí se k šabloně generátoru entit na základě sebe.
-   Přesune třídy entit do samostatného projektu.
-   Vytvoří službu WCF, která zveřejňuje operace pro dotazování a ukládání entit.
-   Vytvoří klientské aplikace (konzolu a WPF), které tuto službu využívají.

V tomto návodu použijeme Database First, ale stejné postupy se použijí stejně jako Model First.

## <a name="pre-requisites"></a>Předpoklady

K dokončení tohoto návodu budete potřebovat nejnovější verzi sady Visual Studio.

## <a name="create-a-database"></a>Vytvoření databáze

Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:

-   Pokud používáte Visual Studio 2012, budete vytvářet databázi LocalDB.
-   Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.

Pojďme dopředu a vygenerovat databázi.

-   Otevřít Visual Studio
-   **Zobrazení-&gt; Průzkumník serveru**
-   Klikněte pravým tlačítkem na **datová připojení – &gt; Přidat připojení...**
-   Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat
-   Připojte se k LocalDB nebo SQL Express v závislosti na tom, který z nich máte nainstalovanou.
-   Jako název databáze zadejte **STESample** .
-   Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .
-   Nová databáze se nyní zobrazí v Průzkumník serveru
-   Pokud používáte Visual Studio 2012
    -   Klikněte pravým tlačítkem na databázi v Průzkumník serveru a vyberte **Nový dotaz** .
    -   Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .
-   Pokud používáte Visual Studio 2010
    -   Vyberte **data-&gt; Transact SQL Editor-&gt; nové připojení dotazu...**
    -   Jako název serveru zadejte **. \\SQLEXPRESS** a klikněte na **OK** .
    -   Vyberte databázi **STESample** v rozevíracím seznamu v horní části editoru dotazů.
    -   Zkopírujte následující příkaz SQL do nového dotazu, potom klikněte pravým tlačítkem na dotaz a vyberte **Spustit SQL** .

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a>Vytvoření modelu

Nejdřív potřebujeme projekt pro vložení modelu do.

-   **Soubor-&gt; nový-&gt; projekt...**
-   V levém podokně vyberte **Visual C @ no__t-1** a pak **knihovny tříd** .
-   Jako název zadejte **STESample** a klikněte na **OK** .

Nyní vytvoříme v Návrháři EF jednoduchý model pro přístup k naší databázi:

-   **Projekt-&gt; Přidat novou položku...**
-   V levém podokně vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**
-   Jako název zadejte **BloggingModel** a klikněte na **OK** .
-   Vyberte **Generovat z databáze** a klikněte na **Další** .
-   Zadejte informace o připojení pro databázi, kterou jste vytvořili v předchozí části.
-   Jako název připojovacího řetězce zadejte **BloggingContext** a klikněte na **Další** .
-   Zaškrtněte políčko vedle **tabulky** a klikněte na **Dokončit** .

## <a name="swap-to-ste-code-generation"></a>Přepnout na generování kódu STE

Teď je potřeba zakázat generování výchozích kódů a jejich přeměnu na entity, které se sledují sami.

### <a name="if-you-are-using-visual-studio-2012"></a>Pokud používáte Visual Studio 2012

-   V **Průzkumník řešení** rozbalte **BloggingModel. edmx** a odstraňte **BloggingModel.TT** a **BloggingModel.Context.TT**
    .*tím se zakáže výchozí generování kódu* .
-   Klikněte pravým tlačítkem myši na prázdnou oblast na povrchu návrháře EF a vyberte **Přidat položku pro generování kódu...**
-   V levém podokně vyberte **online** a vyhledejte **generátor ste** .
-   Vyberte **ste generátor pro šablonu jazyka C @ no__t-1** , jako název zadejte **STETemplate** a klikněte na **Přidat** .
-   Soubory **STETemplate.TT** a **STETemplate.Context.TT** se přidají do souboru BloggingModel. edmx.

### <a name="if-you-are-using-visual-studio-2010"></a>Pokud používáte Visual Studio 2010

-   Klikněte pravým tlačítkem myši na prázdnou oblast na povrchu návrháře EF a vyberte **Přidat položku pro generování kódu...**
-   V levém podokně vyberte **kód** a pak **ADO.NET generátor entit pro samoobslužné sledování** .
-   Jako název zadejte **STETemplate** a klikněte na **Přidat** .
-   Soubory **STETemplate.TT** a **STETemplate.Context.TT** se přidají přímo do vašeho projektu.

## <a name="move-entity-types-into-separate-project"></a>Přesunout typy entit do samostatného projektu

Chcete-li použít entity se sledováním, potřebuje přístup k třídám entit vygenerovaným z našeho modelu. Vzhledem k tomu, že nechceme zveřejnit celý model pro klientskou aplikaci, přesuneme třídy entit do samostatného projektu.

Prvním krokem je zastavit generování tříd entit v existujícím projektu:

-   V **Průzkumník řešení** klikněte pravým tlačítkem na **STETemplate.TT** a vyberte **vlastnosti** .
-   V okně **vlastnosti** zrušte **hodnotu TextTemplatingFileGenerator** z vlastnosti **CustomTool** .
-   V **Průzkumník řešení** rozbalte **STETemplate.TT** a odstraňte všechny soubory, které jsou v něm vnořené.

Nyní přidáme nový projekt a vygenerujeme třídy entit.

-   **Soubor-&gt; Add-&gt; projekt...**
-   V levém podokně vyberte **Visual C @ no__t-1** a pak **knihovny tříd** .
-   Jako název zadejte **STESample. Entities** a klikněte na **OK** .
-   **Projekt-&gt; Přidat existující položku...**
-   Přejděte do složky projektu **STESample** .
-   Tuto možnost vyberte, pokud chcete zobrazit **všechny soubory (\*. \*).**
-   Vybrat soubor **STETemplate.TT**
-   Klikněte na šipku rozevíracího seznamu vedle tlačítka **Přidat** a vyberte **Přidat jako odkaz** .

    ![Přidat propojenou šablonu](~/ef6/media/addlinkedtemplate.png)

Také se ujistěte, že třídy entit se generují ve stejném oboru názvů jako kontext. Tím se jenom zmenší počet příkazů using, které potřebujeme přidat v naší aplikaci.

-   Klikněte pravým tlačítkem na propojený **STETemplate.TT** v **Průzkumník řešení** a vyberte **vlastnosti** .
-   V okně **vlastnosti** nastavte **obor názvů vlastního nástroje** na **STESample**

Kód vygenerovaný šablonou STE bude pro zkompilování vyžadovat odkaz na **System. Runtime. Serialization** . Tato knihovna je potřebná pro atributy **DataContract** a **DataMember** WCF, které se používají v serializovatelných typech entit.

-   Klikněte pravým tlačítkem na projekt **STESample. Entities** v **Průzkumník řešení** a vyberte **Přidat odkaz...**
    -   V aplikaci Visual Studio 2012 – zaškrtněte políčko vedle pole **System. Runtime. Serialization** a klikněte na tlačítko **OK** .
    -   V aplikaci Visual Studio 2010 – vyberte **System. Runtime. Serialization** a klikněte na **OK** .

Nakonec projekt s naším kontextem bude potřebovat odkaz na typy entit.

-   V **Průzkumník řešení** klikněte pravým tlačítkem na projekt **STESample** a vyberte **Přidat odkaz...**
    -   V aplikaci Visual Studio 2012 – v levém podokně vyberte **řešení** , zaškrtněte políčko vedle položky **STESample. Entities** a klikněte na **OK** .
    -   V aplikaci Visual Studio 2010 – vyberte kartu **projekty** , vyberte **STESample. Entities** a klikněte na **OK** .

>[!NOTE]
> Další možností pro přesunutí typů entit do samostatného projektu je přesunout soubor šablony namísto jeho propojení z výchozího umístění. Pokud to uděláte, budete muset v šabloně aktualizovat proměnnou **Vstupní_soubor** , aby byla zajištěna relativní cesta k souboru EDMX (v tomto příkladu, který by byl **.. \\BloggingModel. edmx**).

## <a name="create-a-wcf-service"></a>Vytvoření služby WCF

Teď je čas přidat službu WCF, aby zveřejnila naše data, začneme vytvořením projektu.

-   **Soubor-&gt; Add-&gt; projekt...**
-   V levém podokně vyberte **Visual C @ no__t-1** a potom **aplikaci služby WCF** .
-   Jako název zadejte **STESample. Service** a klikněte na **OK** .
-   Přidat odkaz na sestavení **System. data. entity**
-   Přidat odkaz na projekty **STESample** a **STESample. Entities**

Musíme zkopírovat připojovací řetězec EF do tohoto projektu, aby byl nalezen za běhu.

-   Otevřete soubor **App. config** pro projekt **STESample **a zkopírujte element **connectionStrings** .
-   Vložte element **connectionStrings** jako podřízený element **konfiguračního** elementu v souboru **Web. config** v projektu **STESample. Service** .

Nyní je čas implementovat skutečnou službu.

-   Otevřete **IService1.cs** a nahraďte obsah následujícím kódem.

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   Otevřete **Service1. svc** a nahraďte obsah následujícím kódem.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a>Využití služby z konzolové aplikace

Pojďme vytvořit konzolovou aplikaci, která používá naši službu.

-   **Soubor-&gt; nový-&gt; projekt...**
-   V levém podokně vyberte **Visual C @ no__t-1** a pak **konzolovou aplikaci** .
-   Jako název zadejte **STESample. ConsoleTest** a klikněte na **OK** .
-   Přidat odkaz na projekt **STESample. Entities**

Pro naši službu WCF potřebujeme odkaz na službu.

-   V **Průzkumník řešení** klikněte pravým tlačítkem na projekt **STESample. ConsoleTest** a vyberte **Přidat odkaz na službu...**
-   Klikněte na **Vyhledat** .
-   Jako obor názvů zadejte **BloggingService** a klikněte na **OK** .

Nyní můžeme napsat kód pro využívání služby.

-   Otevřete **program.cs** a nahraďte obsah následujícím kódem.

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

Teď můžete aplikaci spustit, aby se zobrazila v akci.

-   V **Průzkumník řešení** klikněte pravým tlačítkem na projekt **STESample. ConsoleTest** a vyberte **ladit-&gt; Start New instance** .

Když se aplikace spustí, zobrazí se následující výstup.

```console
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a>Využívání služby z aplikace WPF

Pojďme vytvořit aplikaci WPF, která používá naši službu.

-   **Soubor-&gt; nový-&gt; projekt...**
-   V levém podokně vyberte **Visual C @ no__t-1** a pak **aplikace WPF** .
-   Jako název zadejte **STESample. WPFTest** a klikněte na **OK** .
-   Přidat odkaz na projekt **STESample. Entities**

Pro naši službu WCF potřebujeme odkaz na službu.

-   V **Průzkumník řešení** klikněte pravým tlačítkem na projekt **STESample. WPFTest** a vyberte **Přidat odkaz na službu...**
-   Klikněte na **Vyhledat** .
-   Jako obor názvů zadejte **BloggingService** a klikněte na **OK** .

Nyní můžeme napsat kód pro využívání služby.

-   Otevřete **MainWindow. XAML** a nahraďte obsah následujícím kódem.

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   Otevřete kód za MainWindow (**MainWindow.XAML.cs**) a nahraďte jeho obsah následujícím kódem.

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

Teď můžete aplikaci spustit, aby se zobrazila v akci.

-   V **Průzkumník řešení** klikněte pravým tlačítkem na projekt **STESample. WPFTest** a vyberte **ladit-&gt; Start New instance** .
-   Můžete manipulovat s daty pomocí obrazovky a uložit ji přes službu pomocí tlačítka **Uložit** .

![Hlavní okno WPF](~/ef6/media/wpf.png)
