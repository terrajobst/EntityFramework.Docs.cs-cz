---
title: Vazby dat s WPF – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
caps.latest.revision: 3
ms.openlocfilehash: 1756ec14fe83d80199b6040bd345dc2fe6294281
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912745"
---
# <a name="databinding-with-wpf"></a>Vazby dat s WPF
Tento podrobný návod ukazuje, jak svázat ovládací prvky WPF ve formě "hlavní podrobnosti" typy POCO. Aplikace používá rozhraní API Entity Framework pro naplnění objekty s daty z databáze, sledování změn a uložení dat do databáze.

Model definuje dva typy, které se účastní vztahu jednoho k několika: **kategorie** (hlavní\\hlavní) a **produktu** (závislé\\podrobnosti). Nástroje sady Visual Studio se použije k vytvoření vazby typy definované v modelu, který má ovládacích prvků WPF. Navigace mezi související objekty umožňuje rozhraní datové vazby WPF: výběr řádků v zobrazení předlohy způsobí, že podrobné zobrazení aktualizace pomocí odpovídající podřízená data.

Snímky obrazovky a výpis kódu v tomto názorném postupu pocházejí ze sady Visual Studio 2013, ale můžete dokončit tento návod s Visual Studio 2012 nebo Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Pomocí možnosti 'Object' pro vytvoření zdroje dat pro WPF

V předchozí verzi rozhraní Entity Framework jsme použili doporučujeme používat **databáze** při vytváření nového zdroje dat založené na modelu vytvářené pomocí návrháře EF možnost. To bylo, protože kontext, který je odvozen od objektu ObjectContext a tříd entit, které jsou odvozeny z EntityObject vygeneruje návrháře. Pomocí možnosti databáze by usnadňuje psaní nejlepší kód pro interakci s této plochy rozhraní API.

Ať už EF pro Visual Studio 2012 a Visual Studio 2013 vygenerovat kontext, který je odvozen od položky DbContext spolu s jednoduchou tříd entit POCO. Pomocí sady Visual Studio 2010 doporučujeme přechodem na šablony generování kódu, který používá kontext databáze, jak je popsáno dále v tomto návodu.

Při použití plochy rozhraní DbContext API byste měli použít **objekt** možnosti při vytváření nového zdroje dat, jak je znázorněno v tomto názorném postupu.

V případě potřeby můžete [vrátit k generování kódu na základě ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) pro modely vytvořené pomocí EF designeru.

## <a name="pre-requisites"></a>Předpoklady

Musíte mít Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010 nainstalovaný k dokončení tohoto návodu.

Pokud používáte Visual Studio 2010, musíte také nainstalovat NuGet. Další informace najdete v tématu [instalace balíčků NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).  

## <a name="create-the-application"></a>Vytvoření aplikace

-   Otevřít Visual Studio
-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Windows** v levém podokně a **WPFApplication** v pravém podokně
-   Zadejte **WPFwithEFSample** jako název
-   Vyberte **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Nainstalovat balíček NuGet pro rozhraní Entity Framework

-   V Průzkumníku řešení klikněte pravým tlačítkem myši na **WinFormswithEFSample** projektu
-   Vyberte **spravovat balíčky NuGet...**
-   V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku
-   Klikněte na tlačítko **instalace**  
    >[!NOTE]
    > Kromě EntityFramework sestavení je také přidán odkaz na System.ComponentModel.DataAnnotations. Pokud projekt obsahuje odkaz na System.Data.Entity, pak se odebere při instalaci balíčku objektu EntityFramework. Sestavení System.Data.Entity se už používá pro aplikace Entity Framework 6.

## <a name="define-a-model"></a>Definovat Model

V tomto návodu jste se rozhodli implementují model Code First nebo EF designeru. Proveďte jeden z následujících dvou částech.

### <a name="option-1-define-a-model-using-code-first"></a>Možnost 1: Definujte Model pomocí Code First

Tato část ukazuje, jak vytvořit model a jeho přidružená databáze pomocí Code First. Přejít k další části (**možnost 2: definovat model pomocí Database First)** Pokud byste chtěli raději použít první databázi vrátit pracovníkovi modelu z databáze pomocí EF designeru

Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény).

-   Přidejte novou třídu do **WPFwithEFSample:**
    -   Klikněte pravým tlačítkem na název projektu
    -   Vyberte **přidat**, pak **nová položka**
    -   Vyberte **třídy** a zadejte **produktu** pro název třídy
-   Nahradit **produktu** třídy definice s následujícím kódem:

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

**Produkty** vlastnost **kategorie** třídy a **kategorie** vlastnost **produktu** třídy jsou navigační vlastnosti. Navigační vlastnosti v Entity Framework, poskytují způsob, jak procházet vztah mezi dvěma typy entit.

Kromě definování entit, budete muset definovat třídu, která je odvozena od položky DbContext a zveřejňuje DbSet&lt;TEntity&gt; vlastnosti. DbSet&lt;TEntity&gt; vlastnosti umožňují kontextu vědět, jaké typy, které chcete zahrnout do modelu.

Instance typu DbContext odvozené spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, změňte sledování a zachovává data do databáze.

-   Přidat nový **ProductContext** třídy do projektu s následující definice:

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Zkompilujte projekt.

### <a name="option-2-define-a-model-using-database-first"></a>Možnost 2: Definujte model pomocí Database First

Tato část ukazuje způsob použití Database First provést zpětnou analýzu modelu z databáze pomocí EF designeru. Pokud jste dokončili předchozí části (**možnost 1: definovat model pomocí Code First)**, tuto část přeskočit a přejít rovnou do **opožděné načtení** oddílu.

#### <a name="create-an-existing-database"></a>Vytvořit existující databáze

Obvykle Pokud cílíte na existující databázi, které se už vytvořili, ale v tomto návodu budeme potřebovat vytvořit databázi pro přístup k.

Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:

-   Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.
-   Pokud používáte sadu Visual Studio 2012 pak vytvoříte [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) databáze.

Pojďme tedy vygenerovala databáze.

-   **Zobrazení –&gt; Průzkumníka serveru**
-   Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**
-   Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat jako zdroj dat Microsoft SQL Server

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   Připojení k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali a zadejte **produkty** jako název databáze

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   Nové databáze se teď budou zobrazovat v Průzkumníku serveru, klikněte pravým tlačítkem myši na něj a vyberte **nový dotaz**
-   Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a>Zpětná analýza modelu

My budeme používat Entity Framework Designer, která je součástí sady Visual Studio, k vytvoření našeho modelu.

-   **Projekt –&gt; přidat novou položku...**
-   Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**
-   Zadejte **ProductModel** jako název a klikněte na **OK**
-   Tím se spustí **Průvodce datovým modelem Entity**
-   Vyberte **Generovat z databáze** a klikněte na tlačítko **další**

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   Vyberte připojení k databázi vytvořené v první části, zadejte **ProductContext** jako název připojovacího řetězce a klikněte na tlačítko **další**

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   Klikněte na zaškrtávací políčko vedle "Tables" k importu všech tabulek a klikněte na tlačítko 'Dokončit'

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

Po dokončení procesu zpětné analýzy nový model je přidána do projektu a zpřístupnili k zobrazení v Návrháři Entity Framework. Soubor App.config má také byla přidána do projektu s podrobnostmi o připojení pro databázi.

#### <a name="additional-steps-in-visual-studio-2010"></a>Další kroky v sadě Visual Studio 2010

Pokud pracujete v sadě Visual Studio 2010 je potřeba aktualizovat EF designeru používat EF6 generování kódu.

-   Klikněte pravým tlačítkem na prázdné místo modelu v EF designeru a vyberte **přidat položku generování kódu...**
-   Vyberte **Online šablon** v levé nabídce a vyhledejte **DbContext**
-   Vyberte **EF 6.x DbContext generátor pro jazyk C\#,** zadejte **ProductsModel** jako název a klikněte na tlačítko Přidat

#### <a name="updating-code-generation-for-data-binding"></a>Aktualizuje se generování kódu pro vytváření datových vazeb

EF generuje kód z modelu pomocí šablon T4. Šablony součástí sady Visual Studio nebo stáhnout z Galerie sady Visual Studio jsou určené pro obecné účely použití. To znamená, že entity vygenerované z těchto šablon mít jednoduché rozhraní ICollection&lt;T&gt; vlastnosti. Při provádění data vazby pomocí grafického subsystému WPF je však žádoucí použít **kolekci ObservableCollection** pro vlastnosti kolekce tak, že WPF může udržovat přehled o provedené změny kolekce. Za tímto účelem jsme se k úpravě šablony pro použití kolekci ObservableCollection.

-   Otevřít **Průzkumníka řešení** a najít **ProductModel.edmx** souboru
-   Najít **ProductModel.tt** souboru, který se vnoří pod ProductModel.edmx souboru

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   Poklikejte na soubor ProductModel.tt ho otevřete v editoru sady Visual Studio
-   Najít a nahradit dva výskyty "**rozhraní ICollection**"s"**kolekci ObservableCollection**". Toto jsou přibližně v řádcích 296 a 484.
-   Najít a nahradit první výskyt "**HashSet**"s"**kolekci ObservableCollection**". Výskyt této události je umístěn přibližně na řádku 50. **Ne** nahraďte druhým výskytem HashSet najdete dále v kódu.
-   Najít a nahradit jenom výskyt "**System.Collections.Generic**"s"**System.Collections.ObjectModel**". Tento soubor je umístěn přibližně na řádku 424.
-   Uložte soubor ProductModel.tt. To by měl způsobí, že kód pro entity být znovu vygenerován. Pokud kód automaticky neobnoví, klikněte pravým tlačítkem na ProductModel.tt a zvolte možnost "Spustit vlastní nástroj".

Pokud nyní otevřete soubor Category.cs (což je vnořený ProductModel.tt), pak byste měli vidět, že má kolekce produktů typ **kolekci ObservableCollection&lt;produktu&gt;**.

Zkompilujte projekt.

## <a name="lazy-loading"></a>Opožděné načtení

**Produkty** vlastnost **kategorie** třídy a **kategorie** vlastnost **produktu** třídy jsou navigační vlastnosti. Navigační vlastnosti v Entity Framework, poskytují způsob, jak procházet vztah mezi dvěma typy entit.

EF poskytuje možnost načtení souvisejících entit z databáze automaticky při prvním přístupu navigační vlastnost. S tímto typem načítání (označované jako opožděné načtení) mějte na paměti, že při prvním přístupu ke každé vlastnosti navigace samostatný dotaz se spustí na databázi není-li obsah již v kontextu.

Při použití typů entit POCO, EF dosahuje opožděné načtení vytváření instancí typů odvozených proxy za běhu a potom přepsáním virtuálních vlastností ve třídách přidat hook načítání. Pokud chcete získat opožděné načtení souvisejících objektů, je třeba deklarovat navigace gettery vlastností jako **veřejné** a **virtuální** (**Overridable** v jazyce Visual Basic), a je třída nesmí být **zapečetěné** (**NotOverridable** v jazyce Visual Basic). Při použití databáze první navigačních vlastností se automaticky provede virtuální povolit opožděné načtení. V části Code First jsme zvolili, navigačních vlastností virtuální ze stejného důvodu

## <a name="bind-object-to-controls"></a>Vytvoření vazby objektů k ovládacím prvkům

Přidání třídy, které jsou definovány v modelu jako zdroj dat pro tuto aplikaci WPF.

-   Dvakrát klikněte na panel **souboru MainWindow.xaml** v Průzkumníku řešení otevřete hlavní formulář
-   V hlavní nabídce vyberte **projekt –&gt; přidat nový zdroj dat...**
    (v sadě Visual Studio 2010, je nutné vybrat **dat –&gt; přidat nový zdroj dat...** )
-   V seznamu zvolte Typewindow zdroje dat, vyberte **objekt** a klikněte na tlačítko **další**
-   Vyberte datové objekty dialogu nejextrémnějších **WPFwithEFSample** dvěma časy a vyberte **kategorie**  
    *Není nutné vybrat **produktu** zdroje dat, protože jsme se na portálu **produktu**vlastnost **kategorie** zdroj dat*  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   Klikněte na tlačítko **dokončit.**
-   Otevření okna zdrojů dat vedle okna souboru MainWindow.xaml *Pokud okna zdroje dat se nezobrazuje, vyberte **zobrazení –&gt; ostatní Windows -&gt; zdroje dat***
-   Stiskněte ikonu připínáčku tak okna zdroje dat není automaticky skrýt. Budete muset stiskněte tlačítko Aktualizovat, pokud už je okno viditelné.

    ![Zdroje dat](~/ef6/media/datasources.png)

-   Vyberte ** kategorii ** zdroje dat a přetáhněte ji na formuláři.

Tímto se stalo při přetažení jsme tento zdroj:

-   **CategoryViewSource** prostředků a ** categoryDataGrid ** ovládací prvek byl přidán do XAML. Další informace o DataViewSources najdete v tématu http://bea.stollnitz.com/blog/?p=387.
-   Vlastnost DataContext v elementu nadřazené mřížky byla nastavená na "{StaticResource **categoryViewSource** }".  **CategoryViewSource** prostředků slouží jako zdroj vazby pro vnější\\nadřazeného elementu mřížky. Hodnota kontextu DataContext vnitřní elementy mřížky pak dědí z nadřazené mřížky (vlastnost ItemsSource categoryDataGrid je nastavená na "{vazba}"). 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>Přidání podrobností mřížky

Když teď máme mřížky zobrazíte kategorie Pojďme přidáte podrobnosti mřížce se zobrazí související produkty.

-   Vyberte ** produkty ** vlastnosti v rámci ** kategorii ** zdroje dat a přetáhněte ji na formuláři.
    -   **CategoryProductsViewSource** prostředků a **productDataGrid** mřížky jsou přidány do XAML
    -   Cesta vazby pro tento prostředek nastavená na produkty
    -   Rozhraní datové vazby WPF zajistí, že pouze produkty, které souvisejí se do vybrané kategorie se zobrazí v **productDataGrid**
-   Z panelu nástrojů přetáhněte **tlačítko** do formuláře. Nastavte **název** vlastnost **buttonSave** a **obsahu** vlastnost **Uložit**.

Formulář by měl vypadat nějak takto:

![Návrhář](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Přidejte kód, který zpracovává Data interakce

Je čas přidat několik obslužných rutin událostí do hlavního okna.

-   V okně, XAML, klikněte na  **&lt;okno** elementu, tato možnost vybere hlavní okno
-   V **vlastnosti** okna zvolte **události** v pravém horním rohu, pak poklikejte na textové pole na pravé straně **Loaded** popisek

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   Také přidat **klikněte na tlačítko** události **Uložit** tlačítko dvojitým kliknutím na tlačítko Uložit v návrháři. 

To přináší do kódu pro formulář, budeme vám teď upravovat kód, který použije ProductContext přístup k datům. Aktualizujte kód pro hlavního okna MainWindow, jak je znázorněno níže.

Kód deklaruje instanci dlouhotrvající **ProductContext**. **ProductContext** objekt se používá k dotazování a ukládání dat do databáze. **Dispose**() na **ProductContext** instance se nazývá pak z přepsané **OnClosing** metody. V komentářích ke kódu obsahují podrobné informace o co kód dělá.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>Testování aplikace WPF

-   Kompilace a spuštění aplikace. Pokud jste použili Code First, pak uvidíte, že **WPFwithEFSample.ProductContext** databáze se vytvoří za vás.
-   Zadejte název kategorie v mřížce a produktu důležití v mřížce dolů *nezadávejte nic v ID sloupce, protože primární klíč je generován databází*

    ![Screen1](~/ef6/media/screen1.png)

-   Stisknutím klávesy **Uložit** tlačítko pro uložení dat do databáze

Po volání na DbContext **SaveChanges**(), ID jsou vyplněna podle hodnot v databázi vygeneruje. Protože jsme volat **aktualizovat**() po **SaveChanges**() **DataGrid** ovládací prvky jsou aktualizovány s novými hodnotami.

![Screen2](~/ef6/media/screen2.png)
