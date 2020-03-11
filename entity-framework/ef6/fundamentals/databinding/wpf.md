---
title: Vázání dat pomocí WPF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416594"
---
# <a name="databinding-with-wpf"></a>Vázání dat pomocí WPF
V tomto podrobném návodu se dozvíte, jak navazovat POCO typy na ovládací prvky WPF ve formuláři "Master-Detail". Aplikace používá rozhraní Entity Framework API k naplnění objektů daty z databáze, sledování změn a zachování dat v databázi.

Model definuje dva typy, které se účastní vztahu 1:1: **kategorie** (hlavní\\hlavní server) a **produkt** (podrobnosti o závislých\\ch). Nástroje sady Visual Studio pak slouží k navázání typů definovaných v modelu na ovládací prvky WPF. Rozhraní WPF pro datovou vazbu umožňuje navigaci mezi souvisejícími objekty: výběrem řádků v zobrazení Předloha způsobí, že se podrobné zobrazení aktualizuje s odpovídajícími podřízenými daty.

Snímky obrazovky a výpisy kódu v tomto návodu jsou pořízeny z Visual Studio 2013, ale můžete tento návod dokončit pomocí sady Visual Studio 2012 nebo Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Použití možnosti Object pro vytváření zdrojů dat WPF

V předchozí verzi Entity Framework jsme použili k doporučení použití možnosti **databáze** při vytváření nového zdroje dat založeného na modelu vytvořeném pomocí návrháře EF. Důvodem je, že návrhář vygeneroval kontext, který je odvozen z třídy ObjectContext a entity, které jsou odvozeny z objektů EntityObject. Použití možnosti databáze vám pomůže při psaní nejlepšího kódu pro interakci s tímto povrchem rozhraní API.

Návrháři EF pro Visual Studio 2012 a Visual Studio 2013 generují kontext, který je odvozený z DbContext společně s jednoduchými třídami POCO entit. V rámci sady Visual Studio 2010 doporučujeme odkládací šablonu pro generování kódu, která používá DbContext, jak je popsáno dále v tomto návodu.

Při použití plochy rozhraní API DbContext byste měli použít možnost **objektu** při vytváření nového zdroje dat, jak je znázorněno v tomto návodu.

V případě potřeby se můžete [vrátit na generování kódu založeného na ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) pro modely vytvořené pomocí návrháře EF.

## <a name="pre-requisites"></a>Požadavky

Pro dokončení tohoto Názorného postupu musíte mít nainstalovanou Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010.

Pokud používáte Visual Studio 2010, je také nutné nainstalovat NuGet. Další informace najdete v tématu [instalace NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Vytvoření aplikace

-   Otevřete sadu Visual Studio.
-   **Soubor-&gt; projekt New-&gt;...**
-   V levém podokně vyberte **Windows** a **WPFApplication** v pravém podokně.
-   Jako název zadejte **WPFwithEFSample** 
-   Vybrat **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instalace balíčku Entity Framework NuGet

-   V Průzkumník řešení klikněte pravým tlačítkem na projekt **WinFormswithEFSample** .
-   Vyberte **Spravovat balíčky NuGet...**
-   V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .
-   Klikněte na **nainstalovat** .  
    >[!NOTE]
    > Kromě sestavení EntityFramework je také přidáno odkaz na System. ComponentModel. DataAnnotations. Pokud má projekt odkaz na System. data. entity, bude po instalaci balíčku EntityFramework odebrán. Sestavení System. data. entity již není používáno pro Entity Framework 6 aplikací.

## <a name="define-a-model"></a>Definování modelu

V tomto návodu můžete zvolit implementaci modelu pomocí Code First nebo návrháře EF. Proveďte jednu ze dvou následujících částí.

### <a name="option-1-define-a-model-using-code-first"></a>Možnost 1: definování modelu pomocí Code First

V této části se dozvíte, jak vytvořit model a jeho přidruženou databázi pomocí Code First. Přejděte k další části (**možnost 2: definice modelu pomocí Database First)** , pokud byste chtěli použít Database First k zpětné analýze modelu z databáze pomocí návrháře EF.

Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model.

-   Přidat novou třídu do **WPFwithEFSample:**
    -   Klikněte pravým tlačítkem myši na název projektu.
    -   Výběr položky **Přidat**a **Nová položka**
    -   Vyberte **třídu** a jako název třídy zadejte  **produktu** .
-   Nahraďte definici třídy  **produktu** následujícím kódem:

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

Vlastnost **Products** **u vlastnosti Category třídy a** **kategorie** u třídy **produkt** je vlastností navigace. V Entity Framework navigační vlastnosti poskytují způsob, jak procházet relaci mezi dvěma typy entit.

Kromě definování entit musíte definovat třídu, která je odvozena z DbContext a zpřístupňuje Negenerickými&lt;vlastnosti TEntity&gt;. Vlastnosti Negenerickými&lt;TEntity&gt; umožňují kontextu zjistit, které typy chcete do modelu zahrnout.

Instance DbContext odvozeného typu spravuje objekty entit za běhu, což zahrnuje vyplnění objektů daty z databáze, sledování změn a uchování dat do databáze.

-   Přidejte do projektu novou třídu **ProductContext** s následující definicí:

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

### <a name="option-2-define-a-model-using-database-first"></a>Možnost 2: definování modelu pomocí Database First

V této části se dozvíte, jak použít Database First k zpětné analýze modelu z databáze pomocí návrháře EF. Pokud jste dokončili předchozí oddíl (**možnost 1: Definujte model pomocí Code First)** , pak tuto část přeskočte a přejděte rovnou k oddílu **opožděné načítání** .

#### <a name="create-an-existing-database"></a>Vytvoření existující databáze

Když cílíte na existující databázi, bude už vytvořená, ale pro tento návod musíme pro přístup vytvořit databázi.

Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:

-   Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.
-   Pokud používáte Visual Studio 2012, budete vytvářet databázi [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .

Pojďme dopředu a vygenerovat databázi.

-   **Zobrazení-&gt; Průzkumník serveru**
-   Klikněte pravým tlačítkem na **datová připojení –&gt; přidat připojení...**
-   Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat Microsoft SQL Server jako zdroj dat

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   Připojte se buď k LocalDB nebo SQL Express v závislosti na tom, který z nich jste nainstalovali, a jako název databáze zadejte **produkty** .

    ![Přidat LocalDB připojení](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat Express připojení](~/ef6/media/addconnectionexpress.png)

-   Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .

    ![Create Database](~/ef6/media/createdatabase.png)

-   Nová databáze se nyní zobrazí v Průzkumník serveru, klikněte na ni pravým tlačítkem myši a vyberte **Nový dotaz** .
-   Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .

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

#### <a name="reverse-engineer-model"></a>Model zpětného analýz

Budeme používat Entity Framework Designer, který je součástí sady Visual Studio, a vytvořit náš model.

-   **Projekt –&gt; přidat novou položku...**
-   V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**
-   Jako název zadejte **ProductModel** a klikněte na **OK** .
-   Spustí se **průvodce model EDM (Entity Data Model)** .
-   Vyberte **Generovat z databáze** a klikněte na **Další** .

    ![Zvolit obsah modelu](~/ef6/media/choosemodelcontents.png)

-   Vyberte připojení k databázi, kterou jste vytvořili v první části, jako název připojovacího řetězce zadejte **ProductContext** a klikněte na **Další** .

    ![Zvolit připojení](~/ef6/media/chooseyourconnection.png)

-   Kliknutím na zaškrtávací políčko vedle tabulky naimportujete všechny tabulky a kliknete na Dokončit.

    ![Zvolit vaše objekty](~/ef6/media/chooseyourobjects.png)

Po dokončení procesu zpětného zpracování se do projektu přidá nový model, který jste otevřeli, abyste mohli zobrazit Entity Framework Designer. Do projektu byl také přidán soubor App. config s podrobnostmi o připojení pro databázi.

#### <a name="additional-steps-in-visual-studio-2010"></a>Další kroky v aplikaci Visual Studio 2010

Pokud pracujete v aplikaci Visual Studio 2010, bude nutné aktualizovat návrháře EF, aby používal generování kódu EF6.

-   V Návrháři EF klikněte pravým tlačítkem na prázdný bod modelu a vyberte **Přidat položku pro generování kódu...**
-   V nabídce vlevo vyberte **online šablony** a vyhledejte **DbContext** .
-   Vyberte **generátor EF 6. x DbContext pro jazyk C\#,** jako název zadejte **ProductsModel** a klikněte na Přidat.

#### <a name="updating-code-generation-for-data-binding"></a>Aktualizuje se generování kódu pro datovou vazbu.

EF generuje kód z modelu pomocí šablon T4. Šablony dodávané se sadou Visual Studio nebo stažené z galerie sady Visual Studio jsou určené pro účely obecného použití. To znamená, že entity vygenerované z těchto šablon mají jednoduché vlastnosti rozhraní ICollection&lt;T&gt;. Při vytváření datových vazeb pomocí WPF je však žádoucí použít **kolekci ObservableCollection** pro vlastnosti kolekce, aby WPF mohl sledovat změny provedené v kolekcích. K tomuto účelu budeme upravovat šablony, aby používaly kolekci ObservableCollection.

-   Otevřete **Průzkumník řešení** a vyhledejte soubor **ProductModel. edmx.**
-   Vyhledejte soubor **ProductModel.TT** , který bude vnořen do souboru ProductModel. edmx.

    ![Šablona modelu produktu WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Poklikejte na soubor ProductModel.tt a otevře se v editoru Visual studia.
-   Vyhledejte a nahraďte dva výskyty "**ICollection**" pomocí "**kolekci ObservableCollection**". Ty se nacházejí přibližně na řádcích 296 a 484.
-   Najde první výskyt "**HashSet –** " a nahradí ho "**kolekci ObservableCollection**". Tento výskyt je umístěný přibližně na řádku 50. **Neměňte druhý** výskyt HashSet –, který byl nalezen později v kódu.
-   Vyhledejte a nahraďte pouze výskyt "**System. Collections. Generic**" pomocí "**System. Collections. ObjectModel**". To se nachází přibližně na řádku 424.
-   Uložte soubor ProductModel.tt. To by mělo způsobit opětovné vygenerování kódu pro entity. Pokud se kód znovu negeneruje automaticky, klikněte pravým tlačítkem na ProductModel.tt a zvolte spustit vlastní nástroj.

Pokud teď otevřete soubor Category.cs (který je vnořený pod ProductModel.tt), měli byste vidět, že kolekce Products má typ **kolekci observablecollection&lt;&gt;produktu** .

Zkompilujte projekt.

## <a name="lazy-loading"></a>Opožděné načítání

Vlastnost **Products** **u vlastnosti Category třídy a** **kategorie** u třídy **produkt** je vlastností navigace. V Entity Framework navigační vlastnosti poskytují způsob, jak procházet relaci mezi dvěma typy entit.

EF vám nabízí možnost načítat související entity z databáze automaticky při prvním přístupu k vlastnosti navigace. U tohoto typu načítání (tzv. opožděné načítání) mějte na paměti, že při prvním přístupu k jednotlivým vlastnostem navigace se v databázi spustí samostatný dotaz, pokud obsah ještě není v kontextu.

Při použití typů entit POCO nahrazuje EF opožděné načítání vytvořením instancí odvozených typů proxy během běhu a následným přepsáním virtuálních vlastností ve třídách, aby bylo možné přidat zavěšení zatížení. Chcete-li získat opožděné načítání souvisejících objektů, je nutné deklarovat metody Get vlastnosti navigace jako **veřejné** a **virtuální** (**přepsatelné** v Visual Basic) a třídu nesmí být **zapečetěna** (**NotOverridable** v Visual Basic). Při použití Database First navigační vlastnosti se automaticky vypnuly virtuálním, aby bylo možné povolit opožděné načítání. V části Code First jsme se rozhodli, aby navigační vlastnosti byly ve stejném důsledku virtuální.

## <a name="bind-object-to-controls"></a>Svázání objektu s ovládacími prvky

Přidejte třídy, které jsou definovány v modelu jako zdroje dat pro tuto aplikaci WPF.

-   Poklikejte na **MainWindow. XAML** v Průzkumník řešení k otevření hlavního formuláře.
-   V hlavní nabídce vyberte **projekt –&gt; přidat nový zdroj dat...**
    (v aplikaci Visual Studio 2010 je nutné vybrat **data –&gt; přidat nový zdroj dat...** )
-   Ve formuláři zvolte zdroj dat Typewindow vyberte **objekt** a klikněte na **Další** .
-   V dialogovém okně Vybrat datové objekty odložte **WPFwithEFSample** dvakrát a vyberte **kategorii** .  
    *Nemusíte vybírat zdroj dat **produktu** , protože se k němu dostanete prostřednictvím vlastnosti **produktu**ve zdroji dat **kategorie** .*  

    ![Vybrat datové objekty](~/ef6/media/selectdataobjects.png)

-   Klikněte na **Dokončit**.
-   Okno zdroje dat je otevřeno vedle okna MainWindow. XAML *, pokud se nezobrazí okno zdroje dat, vyberte možnost **zobrazit –&gt; jiné zdroje dat&gt; Windows***  .
-   Stiskněte ikonu připnutí, aby se okno zdroje dat neautomaticky skrylo. Pokud je okno již viditelné, může být nutné spustit tlačítko Aktualizovat.

    ![Zdroje dat](~/ef6/media/datasources.png)

-   Vyberte zdroj dat **kategorie** a přetáhněte jej na formuláři.

Při přetažení tohoto zdroje došlo k následujícímu:

-   Prostředek **categoryViewSource** a ovládací prvek **categoryDataGrid** byly přidány do jazyka XAML. 
-   Vlastnost DataContext v nadřazeném elementu gridu se nastavila na {StaticResource **categoryViewSource** }. Prostředek **categoryViewSource** slouží jako zdroj vazby pro vnější\\nadřazený prvek mřížky. Vnitřní elementy mřížky pak zdědí hodnotu DataContext z nadřazené mřížky (vlastnost ItemsSource categoryDataGrid je nastavená na {Binding}).

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

## <a name="adding-a-details-grid"></a>Přidání mřížky podrobností

Teď, když máme mřížku pro zobrazení kategorií, přidáme k zobrazení přidružených produktů tabulku podrobností.

-   Vyberte vlastnost **Products** ze zdroje dat **kategorie** a přetáhněte ji do formuláře.
    -   Prostředek **categoryProductsViewSource** a **productDataGrid** mřížka se přidají do XAML.
    -   Cesta vazby pro tento prostředek je nastavená na Products.
    -   Architektura pro datovou vazbu WPF zajišťuje, aby se v **productDataGrid** zobrazovaly jenom produkty související s vybranou kategorií.
-   Přetáhněte **tlačítko** myši na panelu nástrojů do formuláře. Nastavte vlastnost **Name** na **ButtonSave** a vlastnost **Content** , která se má **Uložit**.

Formulář by měl vypadat nějak takto:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Přidat kód, který zpracovává interakci s daty

Je čas přidat do hlavního okna nějaké obslužné rutiny událostí.

-   V okně XAML klikněte na prvek **&lt;okno** . tím se vybere hlavní okno.
-   V okně **vlastnosti** zvolte **události** v pravém horním rohu a potom poklikejte na textové pole napravo od **načteného** popisku.

    ![Vlastnosti hlavního okna](~/ef6/media/mainwindowproperties.png)

-   Přidejte také událost **Click** pro tlačítko **Uložit** dvojitým kliknutím na tlačítko Uložit v návrháři. 

Tím se vám zobrazí kód na pozadí pro formulář. teď kód upravíte, aby se k přístupu k datům používal ProductContext. Aktualizujte kód pro MainWindow, jak je znázorněno níže.

Kód deklaruje dlouhodobě běžící instanci **ProductContext**. Objekt **ProductContext** se používá k dotazování a ukládání dat do databáze. Metoda **Dispose ()** v instanci **ProductContext** je pak volána z přepsané metody při **zavírání** . Komentáře kódu poskytují podrobné informace o tom, co kód dělá.

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

-   Zkompilujte a spusťte aplikaci. Pokud jste použili Code First, zobrazí se vám pro vás vytvořená databáze **WPFwithEFSample. ProductContext** .
-   Zadejte název kategorie v horní mřížce a názvy produktů v dolním mřížce *nezadávají žádné údaje ve SLOUPCÍCH ID, protože primární klíč je vygenerovaný databází* .

    ![Hlavní okno s novými kategoriemi a produkty](~/ef6/media/screen1.png)

-   Kliknutím na tlačítko **Uložit** uložte data do databáze.

Po volání **metody SaveChanges ()** pro DbContext se identifikátory naplní hodnotami generovanými databází. Protože jsme volali **Refresh ()** po **metody SaveChanges ()** , ovládací prvky **DataGrid** jsou aktualizovány také novými hodnotami.

![Hlavní okno s vyplněnými ID](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Další prostředky

Další informace o datové vazbě na kolekce pomocí WPF naleznete v [tomto tématu](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) v dokumentaci k platformě WPF.  
