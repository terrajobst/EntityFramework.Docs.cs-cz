---
title: Vazby dat s WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 48e6d997875a25a5954484f854953df69a267d05
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384849"
---
# <a name="databinding-with-winforms"></a>Vazby dat s WinForms
Tento podrobný návod ukazuje, jak svázat ovládací prvky formuláře Windows (WinForms) ve formě "hlavní podrobnosti" typy POCO. Aplikace používá Entity Framework pro naplnění objekty s daty z databáze, sledování změn a uložení dat do databáze.

Model definuje dva typy, které se účastní vztahu jednoho k několika: kategorie (hlavní\\hlavní) a produktu (závislé\\podrobnosti). Nástroje sady Visual Studio se použije k vytvoření vazby typy definované v modelu, který má ovládacích prvků WinForms. Datová vazba rozhraní WinForms umožňuje navigaci mezi související objekty: výběr řádků v zobrazení předlohy způsobí, že podrobné zobrazení aktualizace pomocí odpovídající podřízená data.

Snímky obrazovky a výpis kódu v tomto názorném postupu pocházejí ze sady Visual Studio 2013, ale můžete dokončit tento návod s Visual Studio 2012 nebo Visual Studio 2010.

## <a name="pre-requisites"></a>Předpoklady

Musíte mít Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010 nainstalovaný k dokončení tohoto návodu.

Pokud používáte Visual Studio 2010, musíte také nainstalovat NuGet. Další informace najdete v tématu [instalace balíčků NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Vytvoření aplikace

-   Otevřít Visual Studio
-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Windows** v levém podokně a **Windows FormsApplication** v pravém podokně
-   Zadejte **WinFormswithEFSample** jako název
-   Vyberte **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Nainstalovat balíček NuGet pro rozhraní Entity Framework

-   V Průzkumníku řešení klikněte pravým tlačítkem myši na **WinFormswithEFSample** projektu
-   Vyberte **spravovat balíčky NuGet...**
-   V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku
-   Klikněte na tlačítko **instalace**  
    > [!NOTE]
    > Kromě EntityFramework sestavení je také přidán odkaz na System.ComponentModel.DataAnnotations. Pokud projekt obsahuje odkaz na System.Data.Entity, pak se odebere při instalaci balíčku objektu EntityFramework. Sestavení System.Data.Entity se už používá pro aplikace Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Implementace rozhraní IListSource pro kolekce

Vlastnosti kolekce musí implementovat rozhraní IListSource obousměrný datové vazby s řazení při používání formulářů Windows. K tomu budeme rozšiřovat observablecollection – přidání funkce rozhraní IListSource.

-   Přidat **ObservableListSource** třídy do projektu:
    -   Klikněte pravým tlačítkem na název projektu
    -   Vyberte **Add -&gt; nová položka**
    -   Vyberte **třídy** a zadejte **ObservableListSource** pro název třídy
-   Nahraďte kód vygenerovaný ve výchozím nastavení s následujícím kódem:

*Tato třída umožňuje obousměrný datové vazby, stejně jako řazení. Třída je odvozena z kolekci ObservableCollection&lt;T&gt; a přidá explicitní implementace rozhraní IListSource. Metoda GetList() IListSource implementované tak, aby vrátila implementace rozhraní IBindingList, které zůstávají synchronizované s kolekci ObservableCollection. Implementace IBindingList generovaných ToBindingList podporuje řazení. Metoda rozšíření ToBindingList je definována v sestavení objektu EntityFramework.*

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Definovat Model

V tomto návodu jste se rozhodli implementují model Code First nebo EF designeru. Proveďte jeden z následujících dvou částech.

### <a name="option-1-define-a-model-using-code-first"></a>Možnost 1: Definujte Model pomocí Code First

Tato část ukazuje, jak vytvořit model a jeho přidružená databáze pomocí Code First. Přejít k další části (**možnost 2: definovat model pomocí Database First)** Pokud byste chtěli raději použít první databázi vrátit pracovníkovi modelu z databáze pomocí EF designeru

Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény).

-   Přidat nový **produktu** třídy do projektu
-   Nahraďte kód vygenerovaný ve výchozím nastavení s následujícím kódem:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   Přidat **kategorie** třídy do projektu.
-   Nahraďte kód vygenerovaný ve výchozím nastavení s následujícím kódem:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

Kromě definování entit, budete muset definovat třídu, která je odvozena z **DbContext** a zveřejňuje **DbSet&lt;TEntity&gt;**  vlastnosti. **DbSet** vlastnosti umožňují kontextu vědět, jaké typy, které chcete zahrnout do modelu. **DbContext** a **DbSet** typy jsou definovány v sestavení objektu EntityFramework.

Instance typu DbContext odvozené spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, změňte sledování a zachovává data do databáze.

-   Přidat nový **ProductContext** třídy do projektu.
-   Nahraďte kód vygenerovaný ve výchozím nastavení s následujícím kódem:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
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

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   Připojení k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali a zadejte **produkty** jako název databáze

    ![Přidat připojení LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat připojení Express](~/ef6/media/addconnectionexpress.png)

-   Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**

    ![Vytvoření databáze](~/ef6/media/createdatabase.png)

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

    ![Zvolte připojení](~/ef6/media/chooseyourconnection.png)

-   Klikněte na zaškrtávací políčko vedle "Tables" k importu všech tabulek a klikněte na tlačítko 'Dokončit'

    ![Vyberte objekty](~/ef6/media/chooseyourobjects.png)

Po dokončení procesu zpětné analýzy nový model je přidána do projektu a zpřístupnili k zobrazení v Návrháři Entity Framework. Soubor App.config má také byla přidána do projektu s podrobnostmi o připojení pro databázi.

#### <a name="additional-steps-in-visual-studio-2010"></a>Další kroky v sadě Visual Studio 2010

Pokud pracujete v sadě Visual Studio 2010 je potřeba aktualizovat EF designeru používat EF6 generování kódu.

-   Klikněte pravým tlačítkem na prázdné místo modelu v EF designeru a vyberte **přidat položku generování kódu...**
-   Vyberte **Online šablon** v levé nabídce a vyhledejte **DbContext**
-   Vyberte **EF 6.x DbContext generátor pro jazyk C\#,** zadejte **ProductsModel** jako název a klikněte na tlačítko Přidat

#### <a name="updating-code-generation-for-data-binding"></a>Aktualizuje se generování kódu pro vytváření datových vazeb

EF generuje kód z modelu pomocí šablon T4. Šablony součástí sady Visual Studio nebo stáhnout z Galerie sady Visual Studio jsou určené pro obecné účely použití. To znamená, že entity vygenerované z těchto šablon mít jednoduché rozhraní ICollection&lt;T&gt; vlastnosti. Při vytváření datových vazeb je však třeba mít nastavenou vlastnost kolekce, které implementují rozhraní IListSource. To je důvod, proč jsme vytvořili výše ObservableListSource třídy a teď budeme upravovat šablony, aby pomocí této třídy.

-   Otevřít **Průzkumníka řešení** a najít **ProductModel.edmx** souboru
-   Najít **ProductModel.tt** souboru, který se vnoří pod ProductModel.edmx souboru

    ![Šablona modelu produktu](~/ef6/media/productmodeltemplate.png)

-   Poklikejte na soubor ProductModel.tt ho otevřete v editoru sady Visual Studio
-   Najít a nahradit dva výskyty "**rozhraní ICollection**"s"**ObservableListSource**". Tyto jsou umístěny na přibližně řádky 296 a 484.
-   Najít a nahradit první výskyt "**HashSet**"s"**ObservableListSource**". Výskyt této události se nachází na řádku přibližně 50. **Ne** nahraďte druhým výskytem HashSet najdete dále v kódu.
-   Uložte soubor ProductModel.tt. To by měl způsobí, že kód pro entity být znovu vygenerován. Pokud kód automaticky neobnoví, klikněte pravým tlačítkem na ProductModel.tt a zvolte možnost "Spustit vlastní nástroj".

Pokud nyní otevřete soubor Category.cs (což je vnořený ProductModel.tt), pak byste měli vidět, že má kolekce produktů typ **ObservableListSource&lt;produktu&gt;**.

Zkompilujte projekt.

## <a name="lazy-loading"></a>Opožděné načtení

**Produkty** vlastnost **kategorie** třídy a **kategorie** vlastnost **produktu** třídy jsou navigační vlastnosti. Navigační vlastnosti v Entity Framework, poskytují způsob, jak procházet vztah mezi dvěma typy entit.

EF poskytuje možnost načtení souvisejících entit z databáze automaticky při prvním přístupu navigační vlastnost. S tímto typem načítání (označované jako opožděné načtení) mějte na paměti, že při prvním přístupu ke každé vlastnosti navigace samostatný dotaz se spustí na databázi není-li obsah již v kontextu.

Při použití typů entit POCO, EF dosahuje opožděné načtení vytváření instancí typů odvozených proxy za běhu a potom přepsáním virtuálních vlastností ve třídách přidat hook načítání. Pokud chcete získat opožděné načtení souvisejících objektů, je třeba deklarovat navigace gettery vlastností jako **veřejné** a **virtuální** (**Overridable** v jazyce Visual Basic), a je třída nesmí být **zapečetěné** (**NotOverridable** v jazyce Visual Basic). Při použití databáze první navigačních vlastností se automaticky provede virtuální povolit opožděné načtení. V části Code First jsme zvolili, navigačních vlastností virtuální ze stejného důvodu

## <a name="bind-object-to-controls"></a>Vytvoření vazby objektů k ovládacím prvkům

Přidání třídy, které jsou definovány v modelu jako zdroj dat pro tuto aplikaci WinForms.

-   V hlavní nabídce vyberte **projekt –&gt; přidat nový zdroj dat...**
    (v sadě Visual Studio 2010, je nutné vybrat **dat –&gt; přidat nový zdroj dat...** )
-   V okně zvolte typ zdroje dat, vyberte **objekt** a klikněte na tlačítko **další**
-   Vyberte datové objekty dialogu nejextrémnějších **WinFormswithEFSample** dvěma časy a vyberte **kategorie** není nutné vybrat zdroj dat produktu, protože se dostaneme k němu prostřednictvím produktu Vlastnost ve zdroji dat kategorie.

    ![Zdroj dat](~/ef6/media/datasource.png)

-   Klikněte na tlačítko **dokončit.** 
     *Pokud okna zdroje dat se nezobrazuje, vyberte *** zobrazení –&gt; ostatní Windows -&gt; zdroje dat**
-   Stiskněte ikonu připínáčku tak okna zdroje dat není automaticky skrýt. Budete muset stiskněte tlačítko Aktualizovat, pokud už je okno viditelné.

    ![Zdroj dat 2](~/ef6/media/datasource2.png)

-   V Průzkumníku řešení poklikejte **Form1.cs** soubor otevřete hlavní formulář v návrháři.
-   Vyberte **kategorie** zdroje dat a přetáhněte ji na formuláři. Ve výchozím nastavení nového ovládacího prvku DataGridView (**categoryDataGridView**) a nástrojů navigační ovládací prvky jsou přidány do návrháře. Tyto ovládací prvky, které jsou vázány na objekt BindingSource (**categoryBindingSource**) a Navigátor vazby (**categoryBindingNavigator**) komponent, které jsou vytvořeny také.
-   Upravit sloupce na **categoryDataGridView**. Chceme nastavit **CategoryId** sloupec jen pro čtení. Hodnota **CategoryId** vlastnost je generován databází, poté, co jsme uložit data.
    -   Klikněte pravým tlačítkem na ovládací prvek DataGridView a vyberte Upravit sloupce...
    -   Vyberte sloupec ID kategorie a nastaven na hodnotu True jen pro čtení
    -   Kliknutím na tlačítko OK
-   Vyberte produkty ze zdroje dat kategorii a přetáhněte ho ve formuláři. ProductDataGridView a productBindingSource se přidají do formuláře.
-   Upravte sloupce na productDataGridView. Chcete skrýt sloupce CategoryId a kategorie a nastavte ProductId jen pro čtení. Hodnota vlastnosti ProductId je generován databází, poté, co jsme uložit data.
    -   Klikněte pravým tlačítkem na ovládací prvek DataGridView a vyberte **upravit sloupce...** .
    -   Vyberte **ProductId** sloupec a sadu **jen pro čtení** k **True**.
    -   Vyberte **CategoryId** sloupce a stiskněte klávesu **odebrat** tlačítko. Postupujte stejným způsobem pracovat s **kategorie** sloupce.
    -   Stisknutím klávesy **OK**.

    Zatím jsme naše ovládacích prvků DataGridView spojené s BindingSource součásti v návrháři. V další části přidáme kód do kódu nastavit categoryBindingSource.DataSource ke kolekci entit, které jsou aktuálně sledovány objektem DbContext. Když jsme kvůli usnadnění použití vypsány a vyřadit produkty z kategorie WinForms trvalo stará o nastavení vlastnost productsBindingSource.DataSource categoryBindingSource a productsBindingSource.DataMember vlastnosti k produktům. Z důvodu této vazby v productDataGridView zobrazí pouze produkty, které patří do vybrané kategorie.
-   Povolit **Uložit** tlačítko na navigačním panelu kliknete pravým tlačítkem myši a výběrem **povoleno**.

    ![Návrhář formulářů 1](~/ef6/media/form1-designer.png)

-   Přidat obslužnou rutinu události pro uložení tlačítko dvojitým kliknutím na tlačítko. Tím přidáte obslužné rutiny události a můžete přenést do kódu pro formulář. Kód **categoryBindingNavigatorSaveItem\_klikněte na tlačítko** obslužné rutiny události bude přidána v další části.

## <a name="add-the-code-that-handles-data-interaction"></a>Přidejte kód, který zpracovává Data interakce

Teď přidáme kód pro použití ProductContext přístup k datům. Aktualizace kódu pro okno hlavního formuláře, jak je znázorněno níže.

Kód deklaruje instanci dlouhotrvající ProductContext. Objekt ProductContext se používá k dotazování a ukládání dat do databáze. Metoda Dispose() na instanci ProductContext se pak volá z přepsané metody OnClosing. V komentářích ke kódu obsahují podrobné informace o co kód dělá.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a>Testování aplikace Windows Forms

-   Kompilace a spuštění, které můžete otestovat aplikaci a budete si moct funkce.

    ![Formulář 1 před uložit](~/ef6/media/form1beforesave.png)

-   Po uložení klíče úložiště, vygeneruje se zobrazí na obrazovce.

    ![Formuláře 1 po uložení](~/ef6/media/form1aftersave.png)

-   Pokud jste použili Code First, pak bude také uvidíte, že **WinFormswithEFSample.ProductContext** databáze se vytvoří za vás.

    ![Průzkumník objektů serveru](~/ef6/media/serverobjexplorer.png)
