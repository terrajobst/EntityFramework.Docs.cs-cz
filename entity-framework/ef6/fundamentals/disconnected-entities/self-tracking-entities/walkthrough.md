---
title: Vlastní sledování entity návod – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 1c450bbb20c246d9b9d58707ac03eb48eadfa970
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251281"
---
# <a name="self-tracking-entities-walkthrough"></a>Vlastní sledování návod entity
> [!IMPORTANT]
> Už doporučujeme použít šablonu samoobslužného tracking entity. Pouze bude nadále být k dispozici pro podporu stávající aplikace. Pokud aplikace potřebuje pracovat s grafy odpojených entit, zvažte další možnosti, jako [organizovaným entity](http://trackableentities.github.io/), což je technologie, podobně jako u samoobslužného-Tracking-entit, které je více aktivně vyvíjen Komunita, nebo psaní vlastního kódu pomocí nízké úrovně rozhraní API pro sledování změn.

Tento návod ukazuje scénář, ve kterém služby Windows Communication Foundation (WCF) poskytuje operace, která se vrátí entit grafu. V dalším kroku klientská aplikace provádí úpravy tohoto grafu a odešle změny operace služby, která ověřuje a uloží aktualizace k databázi pomocí Entity Frameworku.

Před dokončením tohoto návodu Ujistěte se, že čtete [Self-Tracking entity](index.md) stránky.

Tento návod provede následující akce:

-   Vytvoří databázi přístup.
-   Vytvoří knihovnu tříd, která obsahuje model.
-   Záměna Self-Tracking Entity generátor šablony.
-   Přesune tříd entit do samostatného projektu.
-   Vytvoří službu WCF, která poskytuje operace pro dotazování a uložení entity.
-   Vytvoří klienta aplikace (konzoly a WPF), které využívají službu.

Použijeme první databáze v tomto podrobném návodu, ale stejné techniky stejnou měrou vztahují na první Model.

## <a name="pre-requisites"></a>Předpoklady

K dokončení tohoto návodu potřebujete novější verzi sady Visual Studio.

## <a name="create-a-database"></a>Vytvoření databáze

Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:

-   Pokud používáte sadu Visual Studio 2012 pak vytvoříte databázi LocalDB.
-   Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.

Pojďme tedy vygenerovala databáze.

-   Otevřít Visual Studio
-   **Zobrazení –&gt; Průzkumníka serveru**
-   Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**
-   Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat
-   Připojte se k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali
-   Zadejte **STESample** jako název databáze
-   Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**
-   Nová databáze se teď budou zobrazovat v Průzkumníku serveru
-   Pokud používáte sadu Visual Studio 2012
    -   Klikněte pravým tlačítkem na databázi v Průzkumníku serveru a vyberte **nový dotaz**
    -   Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**
-   Pokud používáte Visual Studio 2010
    -   Vyberte **dat –&gt; jazyka Transact SQL Editor –&gt; nové připojení dotazu...**
    -   Zadejte **.\\ SQLEXPRESS** jako název serveru a klikněte na **OK**
    -   Vyberte **STESample** databázi z rozevíracího seznamu v horní části editoru dotazů
    -   Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **provést SQL**

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

Až, nejprve je třeba projekt se umístí modelu.

-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Visual C\#**  v levém podokně a pak **knihovny tříd**
-   Zadejte **STESample** jako název a klikněte na **OK**

Teď vytvoříme jednoduchý model v EF designeru pro přístup k naší databázi:

-   **Projekt –&gt; přidat novou položku...**
-   Vyberte **Data** v levém podokně a pak **datový Model Entity ADO.NET**
-   Zadejte **BloggingModel** jako název a klikněte na **OK**
-   Vyberte **Generovat z databáze** a klikněte na tlačítko **další**
-   Zadejte informace o připojení pro databázi, kterou jste vytvořili v předchozí části
-   Zadejte **BloggingContext** jako název připojovacího řetězce a klikněte na **další**
-   Zaškrtněte políčko vedle položky **tabulky** a klikněte na tlačítko **dokončit**

## <a name="swap-to-ste-code-generation"></a>Zaměnit STE generování kódu

Nyní potřebujeme zakázat výchozí generování kódu a přepnutí na Self-Tracking entity.

### <a name="if-you-are-using-visual-studio-2012"></a>Pokud používáte sadu Visual Studio 2012

-   Rozbalte **BloggingModel.edmx** v **Průzkumníka řešení** a odstranit **BloggingModel.tt** a **BloggingModel.Context.tt** 
     *Tato akce zakáže výchozí generování kódu*
-   Klikněte pravým tlačítkem na prázdnou oblast na EF designeru ploše a vyberte možnost **přidat položku generování kódu...**
-   Vyberte **Online** v levém podokně a vyhledejte **generátor STE**
-   Vyberte **STE generátor pro jazyk C\#**  šablony, zadejte **STETemplate** jako název a klikněte na **přidat**
-   **STETemplate.tt** a **STETemplate.Context.tt** soubory budou přidány pod BloggingModel.edmx souboru

### <a name="if-you-are-using-visual-studio-2010"></a>Pokud používáte Visual Studio 2010

-   Klikněte pravým tlačítkem na prázdnou oblast na EF designeru ploše a vyberte možnost **přidat položku generování kódu...**
-   Vyberte **kód** v levém podokně a pak **generátor Entity Self-Tracking ADO.NET**
-   Zadejte **STETemplate** jako název a klikněte na **přidat**
-   **STETemplate.tt** a **STETemplate.Context.tt** se přidají soubory přímo do vašeho projektu

## <a name="move-entity-types-into-separate-project"></a>Přesuňte typy entit do samostatného projektu

Použití Self-Tracking entity naše klientská aplikace potřebuje přístup k entity prostor tříd vygenerovaných z našeho modelu. Protože nechceme zpřístupnit celý model do klientské aplikace teď ještě chvíli Zůstaneme k přesunutí tříd entit do samostatného projektu.

Prvním krokem je se přestávají generovat tříd entit v existující projekt:

-   Klikněte pravým tlačítkem na **STETemplate.tt** v **Průzkumníka řešení** a vyberte **vlastnosti**
-   V **vlastnosti** okno vymazat **TextTemplatingFileGenerator** z **CustomTool** vlastnost
-   Rozbalte **STETemplate.tt** v **Průzkumníka řešení** a odstraňte všechny soubory vnořená dole

V dalším kroku budeme přidat nový projekt a v něm generování tříd entit

-   **Soubor –&gt; Add -&gt; projektu...**
-   Vyberte **Visual C\#**  v levém podokně a pak **knihovny tříd**
-   Zadejte **STESample.Entities** jako název a klikněte na **OK**
-   **Projekt –&gt; přidat existující položku...**
-   Přejděte **STESample** složky projektu
-   Vyberte, chcete-li zobrazit **všechny soubory (\*.\*)**
-   Vyberte **STETemplate.tt** souboru
-   Klikněte na šipku rozevíracího seznamu vedle **přidat** tlačítko a vyberte **přidat jako odkaz**

    ![Přidat propojené šablony](~/ef6/media/addlinkedtemplate.png)

My budeme také zajistit, aby že vygeneruje tříd entit ve stejném oboru názvů jako kontext. Právě to snižuje počet pomocí příkazů, které je potřeba přidat v celé aplikaci.

-   Klikněte pravým tlačítkem na propojený **STETemplate.tt** v **Průzkumníka řešení** a vyberte **vlastnosti**
-   V **vlastnosti** okno sady **Custom Tool Namespace** k **STESample**

Kód vygenerovaný šablony STE potřebovat odkaz na **System.Runtime.Serialization** kompilaci. Tato knihovna je potřeba pro WCF **kontraktu dat DataContract** a **DataMember** atributy, které se používají na typy serializovatelné entit.

-   Klikněte pravým tlačítkem na **STESample.Entities** projekt **Průzkumníka řešení** a vyberte **přidat odkaz...**
    -   V sadě Visual Studio 2012 – zaškrtněte políčko vedle položky **System.Runtime.Serialization** a klikněte na tlačítko **OK**
    -   V sadě Visual Studio 2010 – vyberte **System.Runtime.Serialization** a klikněte na tlačítko **OK**

A konečně projekt se náš kontext v něm potřebovat odkaz na typy entit.

-   Klikněte pravým tlačítkem na **STESample** projekt **Průzkumníka řešení** a vyberte **přidat odkaz...**
    -   V sadě Visual Studio 2012 – vyberte **řešení** v levém podokně zaškrtněte políčko vedle položky **STESample.Entities** a klikněte na tlačítko **OK**
    -   V sadě Visual Studio 2010 – vyberte **projekty** kartu, vyberte možnost **STESample.Entities** a klikněte na tlačítko **OK**

>[!NOTE]
> Další možností pro typy entit Přesun do samostatného projektu je přesunout soubor šablony, nikoli propojení z výchozího umístění. Pokud to uděláte, budete muset aktualizovat **inputFile** proměnné v šabloně zadat relativní cestu k souboru edmx (v tomto příkladu, která by byla **... \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>Vytvoření služby WCF

Teď je čas na přidání zpřístupnit naše data ve službě WCF, začneme vytvořením projektu.

-   **Soubor –&gt; Add -&gt; projektu...**
-   Vyberte **Visual C\#**  v levém podokně a pak **aplikace služby WCF**
-   Zadejte **STESample.Service** jako název a klikněte na **OK**
-   Přidejte odkaz na **System.Data.Entity** sestavení
-   Přidejte odkaz na **STESample** a **STESample.Entities** projekty

Potřebujeme zkopírujte připojovací řetězec EF do tohoto projektu tak, aby se nachází za běhu.

-   Otevřít **App.Config** souboru ** STESample ** projektu a zkopírujte **connectionStrings** – element
-   Vložit **connectionStrings** prvek jako podřízený prvek **konfigurace** elementu **Web.Config** soubor **STESample.Service** projektu

Nyní je čas k implementaci aktuální služby.

-   Otevřít **IService1.cs** a nahraďte jeho obsah následujícím kódem

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

-   Otevřít **Service1.svc** a nahraďte jeho obsah následujícím kódem

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

## <a name="consume-the-service-from-a-console-application"></a>Používání této služby z konzolové aplikace

Pojďme vytvořit konzolovou aplikaci, která využívá naši službu.

-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Visual C\#**  v levém podokně a pak **konzolové aplikace**
-   Zadejte **STESample.ConsoleTest** jako název a klikněte na **OK**
-   Přidejte odkaz na **STESample.Entities** projektu

Potřebujeme odkazu na službu do naší službě WCF

-   Klikněte pravým tlačítkem myši **STESample.ConsoleTest** projekt **Průzkumníka řešení** a vyberte **přidat odkaz na službu...**
-   Klikněte na tlačítko **zjišťování**
-   Zadejte **BloggingService** jako obor názvů a klikněte na **OK**

Nyní jsme můžete napsat kód k používání této služby.

-   Otevřít **Program.cs** a nahraďte jeho obsah následujícím kódem.

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

Nyní můžete spustit aplikaci sledujte v akci.

-   Klikněte pravým tlačítkem **STESample.ConsoleTest** projekt **Průzkumníku řešení** a vyberte **ladění -&gt; zahájit novou instanci**

Pokud aplikace provádí, zobrazí se následující výstup.

```
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

## <a name="consume-the-service-from-a-wpf-application"></a>Využívat služby z aplikace WPF

Vytvoříme aplikaci WPF, která využívá naši službu.

-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Visual C\#**  v levém podokně a pak **aplikace WPF**
-   Zadejte **STESample.WPFTest** jako název a klikněte na **OK**
-   Přidejte odkaz na **STESample.Entities** projektu

Potřebujeme odkazu na službu do naší službě WCF

-   Klikněte pravým tlačítkem myši **STESample.WPFTest** projekt **Průzkumníka řešení** a vyberte **přidat odkaz na službu...**
-   Klikněte na tlačítko **zjišťování**
-   Zadejte **BloggingService** jako obor názvů a klikněte na **OK**

Nyní jsme můžete napsat kód k používání této služby.

-   Otevřít **souboru MainWindow.xaml** a nahraďte jeho obsah následujícím kódem.

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

-   Otevřete kódu pro hlavní okno MainWindow (**MainWindow.xaml.cs**) a nahraďte jeho obsah následujícím kódem

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

Nyní můžete spustit aplikaci sledujte v akci.

-   Klikněte pravým tlačítkem **STESample.WPFTest** projekt **Průzkumníku řešení** a vyberte **ladění -&gt; zahájit novou instanci**
-   Můžete pracovat s daty na obrazovce a uložte ho prostřednictvím používání služby **Uložit** tlačítko

![WPF hlavní okno](~/ef6/media/wpf.png)
