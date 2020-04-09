---
title: Datová vazba s WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639147"
---
> [!IMPORTANT]
> **Tento dokument je platný pouze pro wpf v rozhraní .NET Framework.**
>
> Tento dokument popisuje datové vazby pro WPF v rozhraní .NET Framework. Pro nové projekty .NET Core doporučujeme použít [EF Core](/ef/core) namísto entity Framework 6. Dokumentace pro datové vazby v EF Core je sledována v [problém #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).

# <a name="databinding-with-wpf"></a>Datová vazba s WPF
Tento podrobný návod ukazuje, jak svázat typy POCO s ovládacími prvky WPF ve formuláři "hlavní podrobnosti". Aplikace používá rozhraní API entity framework k naplnění objektů daty z databáze, sledování změn a zachování dat do databáze.

Model definuje dva typy, které se účastní vztahu 1:N: **Kategorie** \\(hlavní\\hlavní) a **Product** (závislý detail). Potom Visual Studio nástroje se používají k vazbě typy definované v modelu wpf ovládací prvky. WPF rozhraní pro vázání dat umožňuje navigaci mezi souvisejícíobjekty: výběr řádků v hlavním zobrazení způsobí, že zobrazení podrobností aktualizovat s odpovídající podřízené údaje.

Snímky obrazovky a výpisy kódu v tomto návodu jsou převzaty z Visual Studia 2013, ale tento návod můžete dokončit pomocí sady Visual Studio 2012 nebo Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Použití možnosti "Objekt" pro vytváření zdrojů dat WPF

S předchozí verzí entity framework jsme použili k doporučení používat **možnost Databáze** při vytváření nového zdroje dat na základě modelu vytvořeného pomocí EF Designer. Důvodem bylo, že návrhář by generovat kontext, který je odvozen z ObjectContext a entity třídy, které jsou odvozeny z EntityObject. Použití možnost databáze by vám pomůže napsat nejlepší kód pro interakci s tímto povrchem rozhraní API.

Návrháři EF pro Visual Studio 2012 a Visual Studio 2013 generovat kontext, který je odvozen z DbContext spolu s jednoduchými třídami entit POCO. S Visual Studio 2010 doporučujeme prohození na šablonu generování kódu, která používá DbContext, jak je popsáno dále v tomto návodu.

Při použití povrchu rozhraní API DbContext byste měli použít **možnost Object** při vytváření nového zdroje dat, jak je znázorněno v tomto návodu.

V případě potřeby se můžete [vrátit k generování kódu založeného na ObjektContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) pro modely vytvořené pomocí EF Designer.

## <a name="pre-requisites"></a>Požadavky

K dokončení tohoto návodu je potřeba mít nainstalovanou Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010.

Pokud používáte Visual Studio 2010, budete muset také nainstalovat NuGet. Další informace naleznete [v tématu Instalace NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Vytvoření aplikace

-   Otevřete sadu Visual Studio.
-   **Soubor&gt; -&gt; Nový - Projekt....**
-   V levém podokně a WPFApplication v pravém podokně vyberte **Windows** a **WPFApplication.**
-   Jako název zadejte **WPFwithEFSample.** 
-   Vybrat **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instalace balíčku Entity Framework NuGet

-   V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **WinFormswithEFSample**
-   Vyberte **Spravovat balíčky NuGet...**
-   V dialogovém okně Spravovat balíčky NuGet vyberte kartu **Online** a zvolte **balíček EntityFramework.**
-   Klikněte na **Instalovat.**  
    >[!NOTE]
    > Kromě entityFramework sestavení je také přidán odkaz na System.ComponentModel.DataAnnotations. Pokud projekt obsahuje odkaz na System.Data.Entity, pak bude odebrán při instalaci balíčku EntityFramework. Sestavení System.Data.Entity se již nepoužívá pro aplikace entity Framework 6.

## <a name="define-a-model"></a>Definování modelu

V tomto návodu můžete zvolit implementaci modelu pomocí Code First nebo EF Designer. Dokončete jednu ze dvou následujících částí.

### <a name="option-1-define-a-model-using-code-first"></a>Možnost 1: Definujte model pomocí kódu jako první

Tato část ukazuje, jak vytvořit model a jeho přidružené databáze pomocí Code First. Přejít na další část **(Možnost 2: Definujte model pomocí databáze první),** pokud byste raději použít Database First pro zpětnou analýzu modelu z databáze pomocí návrháře EF

Při použití vývoje Code First obvykle začínáte zápisem tříd rozhraní .NET Framework, které definují váš koncepční (doménový) model.

-   Přidejte novou třídu do **wpfwithEFSample:**
    -   Klikněte pravým tlačítkem myši na název projektu
    -   Vyberte **Přidat**a pak **Novou položku.**
    -   Vyberte **Třídu** a zadejte **produkt** pro název třídy.
-   Nahraďte definici **třídy produktu** následujícím kódem:

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

**Vlastnost Products** ve vlastnosti **Category** a **Category** ve třídě **Product** jsou navigační vlastnosti. V rozhraní Entity Framework poskytují navigační vlastnosti způsob navigace pro navigaci ve vztahu mezi dvěma typy entit.

Kromě definování entit je třeba definovat třídu, která je odvozena od&lt;DbContext a zpřístupňuje vlastnosti DbSet TEntity.&gt; Vlastnosti&lt;DbSet&gt; TEntity umožňují kontextu vědět, které typy chcete zahrnout do modelu.

Instance odvozeného typu DbContext spravuje objekty entity za běhu, který zahrnuje naplnění objektů s daty z databáze, sledování změn a uchování dat do databáze.

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

### <a name="option-2-define-a-model-using-database-first"></a>Možnost 2: Definování modelu pomocí databáze jako první

Tato část ukazuje, jak pomocí database first zpětnou analýzu modelu z databáze pomocí ef návrháře. Pokud jste dokončili předchozí část **(Možnost 1: Definujte model pomocí kódu jako první)**, přeskočte tuto část a přejděte přímo do části **Opožděné načítání.**

#### <a name="create-an-existing-database"></a>Vytvoření existující databáze

Obvykle, když cílíte na existující databázi, bude již vytvořena, ale pro tento návod musíme vytvořit databázi pro přístup.

Databázový server nainstalovaný v sadě Visual Studio se liší v závislosti na nainstalované verzi sady Visual Studio:

-   Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.
-   Pokud používáte Visual Studio 2012 pak budete vytvářet databázi [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)

Pojďme do toho a vygenerujme databázi.

-   **Zobrazení&gt; – Průzkumník serveru**
-   Klikněte pravým tlačítkem na **Datová připojení -&gt; Přidat připojení...**
-   Pokud jste se nepřipojili k databázi z Průzkumníka serveru, budete muset jako zdroj dat vybrat Microsoft SQL Server

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   Připojte se k LocalDB nebo SQL Express, v závislosti na tom, který z nich jste nainstalovali, a zadejte **produkty** jako název databáze

    ![Přidat místní databázi připojení](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat expres připojení](~/ef6/media/addconnectionexpress.png)

-   Vyberte **OK** a budete dotázáni, zda chcete vytvořit novou databázi, vyberte **Ano**

    ![Create Database](~/ef6/media/createdatabase.png)

-   Nová databáze se nyní zobrazí v Průzkumníkovi serveru, klikněte na ni pravým tlačítkem myši a vyberte **Nový dotaz**
-   Zkopírujte následující SQL do nového dotazu, klikněte pravým tlačítkem myši na dotaz a vyberte **Spustit**

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

#### <a name="reverse-engineer-model"></a>Model zpětné analýzy

Budeme využívat Návrhář rozhraní entity, který je součástí sady Visual Studio, k vytvoření našeho modelu.

-   **Projekt&gt; – přidat novou položku...**
-   V yberte **Data** z levé nabídky a potom **ADO.NET datový model entity.**
-   Jako název zadejte **ProductModel** a klepněte na **ok.**
-   Tím se spustí **Průvodce datovým modelem entity.**
-   Vyberte **Generovat z databáze** a klepněte na **Další.**

    ![Zvolte obsah modelu](~/ef6/media/choosemodelcontents.png)

-   Vyberte připojení k databázi, kterou jste vytvořili v první části, zadejte **ProductContext** jako název připojovacího řetězce a klepněte na tlačítko **Další.**

    ![Vyberte si připojení](~/ef6/media/chooseyourconnection.png)

-   Kliknutím na zaškrtávací políčko vedle položky Tabulky importujete všechny tabulky a kliknete na tlačítko Dokončit.

    ![Vyberte si své objekty](~/ef6/media/chooseyourobjects.png)

Po dokončení procesu zpětné analýzy je nový model přidán do projektu a otevřen pro zobrazení v návrháři architektury entit. Do projektu byl také přidán soubor App.config s podrobnostmi o připojení pro databázi.

#### <a name="additional-steps-in-visual-studio-2010"></a>Další kroky v sadě Visual Studio 2010

Pokud pracujete v sadě Visual Studio 2010, budete muset aktualizovat návrháře EF, aby používal generování kódu EF6.

-   Klikněte pravým tlačítkem myši na prázdné místo modelu v návrháři EF a vyberte **Přidat položku generování kódu...**
-   V levé nabídce **vyberte online šablony** a vyhledejte **dbcontext.**
-   Vyberte **generátor EF 6.x\#DbContext pro C ,** zadejte jako název Název **ProductsModel** a klepněte na tlačítko Přidat

#### <a name="updating-code-generation-for-data-binding"></a>Aktualizace generování kódu pro datová vazba

EF generuje kód z vašeho modelu pomocí šablon T4. Šablony dodávané v sadě Visual Studio nebo stažené z galerie sady Visual Studio jsou určeny pro běžné použití. To znamená, že entity generované z těchto šablon&lt;&gt; mají jednoduché vlastnosti ICollection T. Však při provádění datové vazby pomocí WPF je žádoucí použít **ObservableCollection** pro vlastnosti kolekce tak, aby WPF můžete sledovat změny provedené v kolekcích. Za tímto účelem budeme upravovat šablony používat ObservableCollection.

-   Otevřete **Průzkumníka řešení** a najděte soubor **ProductModel.edmx**
-   Vyhledání **souboru ProductModel.tt,** který bude vnořen do souboru ProductModel.edmx

    ![Šablona modelu produktu WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Poklepáním na soubor ProductModel.tt soubor otevřete v editoru Sady Visual Studio.
-   Najděte a nahraďte dva výskyty "**ICollection**" s "**ObservableCollection**". Ty se nacházejí přibližně na linkách 296 a 484.
-   Najděte a nahraďte první výskyt "**HashSet**" "**observablecollection**". Tento výskyt se nachází přibližně na řádku 50. **Nenahrazujte** druhý výskyt HashSet nalezen později v kódu.
-   Najděte a nahraďte jediný výskyt "**System.Collections.Generic**" s "**System.Collections.ObjectModel**". Nachází se přibližně na lince 424.
-   Uložte soubor ProductModel.tt. To by mělo způsobit kód pro entity, které mají být obnoveny. Pokud se kód neregeneruje automaticky, klikněte pravým tlačítkem myši na ProductModel.tt a zvolte "Spustit vlastní nástroj".

Pokud nyní otevřete soubor Category.cs (který je vnořený pod ProductModel.tt), měli byste vidět, že kolekce Products má typ **ObservableCollection&lt;Product&gt;**.

Zkompilujte projekt.

## <a name="lazy-loading"></a>Opožděné načítání

**Vlastnost Products** ve vlastnosti **Category** a **Category** ve třídě **Product** jsou navigační vlastnosti. V rozhraní Entity Framework poskytují navigační vlastnosti způsob navigace pro navigaci ve vztahu mezi dvěma typy entit.

EF umožňuje automatické načítání souvisejících entit z databáze při prvním přístupu k vlastnosti navigace. S tímto typem načítání (tzv. opožděné načítání), uvědomte si, že při prvním přístupu ke každé navigační vlastnosti bude proveden samostatný dotaz proti databázi, pokud obsah již není v kontextu.

Při použití typů entit POCO ef dosahuje opožděné načítání vytvořením instance odvozených typů proxy během běhu a pak přepsání virtuální vlastnosti ve vašich třídách přidat zatížení háku. Chcete-li získat opožděné načítání souvisejících objektů, musíte deklarovat vlastnosti navigace getters jako **veřejné** a **virtuální** **(Overridable** v jazyce Visual Basic) a třídy nesmí být **zapečetěné** **(NotOverridable** v jazyce Visual Basic). Při použití databáze první navigační vlastnosti jsou automaticky virtuální povolit opožděné načítání. V části Kód první jsme se rozhodli, aby navigační vlastnosti virtuální ze stejného důvodu

## <a name="bind-object-to-controls"></a>Vazba objektu na ovládací prvky

Přidejte třídy, které jsou definovány v modelu jako zdroje dat pro tuto aplikaci WPF.

-   Poklepáním na **soubor MainWindow.xaml** v Průzkumníku řešení otevřete hlavní formulář.
-   V hlavní nabídce vyberte **Project -&gt; Add New Data Source ...**
    (V sadě Visual Studio 2010 je potřeba vybrat **data –&gt; přidat nový zdroj dat...**)
-   V okně Zvolit typ zdroje dat vyberte **Objekt** a klepněte na **Další.**
-   V dialogovém okně Vybrat datové objekty rozložte **wpfsEFsample** dvakrát a vyberte **kategorii**  
    *Není nutné vybírat zdroj dat **produktu,** protože se k němu dostaneme prostřednictvím **vlastnosti produktu**ve zdroji dat **kategorie.***  

    ![Vybrat datové objekty](~/ef6/media/selectdataobjects.png)

-   Klikněte na **Dokončit.**
-   Okno Zdroje dat se otevře vedle okna MainWindow.xaml *Pokud se okno Zdroje dat nezobrazuje, vyberte Zobrazit – **&gt; jiné zdroje dat windows.&gt; ** *
-   Stiskněte ikonu špendlíku, aby se okno Zdroje dat automaticky neskrývalo. Možná budete muset stisknout tlačítko aktualizace, pokud okno již bylo viditelné.

    ![Zdroje dat](~/ef6/media/datasources.png)

-   Vyberte zdroj dat **kategorie** a přetáhněte jej do formuláře.

Při přetažení tohoto zdroje došlo k následujícímu:

-   Prostředek **categoryViewSource** a ovládací prvek **categoryDataGrid** byly přidány do xaml. 
-   Vlastnost DataContext v nadřazeném elementu Grid byla nastavena na "{StaticResource **categoryViewSource** }".Prostředek **categoryViewSource** slouží jako zdroj vazby pro vnější\\nadřazený element Grid. Vnitřní Elementy Grid pak dědí hodnotu DataContext z nadřazené mřížky (vlastnost CategoryDataGrid ItemsSource je nastavena na {Binding})The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid's ItemsSource vlastnost is set to "{Binding}")

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

Nyní, když máme mřížku pro zobrazení kategorie, přidáme mřížku podrobností, která zobrazí přidružené produkty.

-   Vyberte vlastnost **Products** z části zdroje dat **Kategorie** a přetáhněte ji do formuláře.
    -   Do xaml jsou přidány mřížky prostředku a produktu ProductDataGrid **kategorieProductsViewSource** a **productDataGrid.**
    -   Cesta vazby pro tento prostředek je nastavena na produkty
    -   WPF rozhraní pro vázání dat zajišťuje, že pouze produkty související s vybranou kategorií se zobrazí v **produktu DataGrid**
-   Z panelu nástrojů přetáhněte **tlačítko** do formuláře. Nastavte vlastnost **Name** na **tlačítkoUložit** a vlastnost **Obsah** **uložit**.

Formulář by měl vypadat podobně jako tento:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Přidat kód, který zpracovává interakci dat

Je čas přidat některé obslužné rutiny událostí do hlavního okna.

-   V okně XAML klikněte ** &lt;** na element Window, který vybere hlavní okno
-   V okně **Vlastnosti** zvolte **Události** v pravém horním rohu a potom poklepejte na textové pole vpravo od popisku **Načteno.**

    ![Vlastnosti hlavního okna](~/ef6/media/mainwindowproperties.png)

-   Přidejte také událost **Click** pro tlačítko **Uložit** poklepáním na tlačítko Uložit v návrháři. 

Tím se dostanete do kódu za formuláře, budeme nyní upravit kód použít ProductContext k provedení přístupu k datům. Aktualizujte kód pro MainWindow, jak je znázorněno níže.

Kód deklaruje dlouhotrvající instanci **ProductContext**. **ProductContext** Objekt se používá k dotazování a ukládání dat do databáze. **Dispose()** na **ProductContext** instance je pak volána z přepsané **OnClosing** metoda.Komentáře kódu poskytují podrobnosti o tom, co kód dělá.

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

-   Zkompilujte a spusťte aplikaci. Pokud jste použili Kód První, pak uvidíte, že **wpfwithEFSample.ProductContext** databáze je vytvořen pro vás.
-   Zadejte název kategorie do horní mřížky a názvy produktů v dolní mřížce *Nezadávejte nic ve sloupcích ID, protože primární klíč je generován databází.*

    ![Hlavní okno s novými kategoriemi a produkty](~/ef6/media/screen1.png)

-   Stisknutím tlačítka **Uložit** data uložíte do databáze.

Po volání **DbContext SaveChanges()**, ID jsou naplněny databáze generované hodnoty. Protože jsme **volali Refresh()** po **SaveChanges()** Ovládací prvky **DataGrid** jsou aktualizovány s novými hodnotami také.

![Hlavní okno s vyplněnými ID](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Další zdroje

Další informace o datové vazbě na kolekce pomocí WPF naleznete v [tomto tématu](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) v dokumentaci WPF.  
