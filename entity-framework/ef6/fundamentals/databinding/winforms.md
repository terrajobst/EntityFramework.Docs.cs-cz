---
title: Vazba s WinForms – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 3c7c58f5ded29c136bbdca1d81c64b07c53ce583
ms.sourcegitcommit: 7391cc31193c1216ec9ed485709042ad0c2106cf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/23/2019
ms.locfileid: "69985478"
---
# <a name="databinding-with-winforms"></a>Datová vazba s WinForms
V tomto podrobném návodu se dozvíte, jak navazovat POCO typy na ovládací prvky WinForms (Window Forms) ve formuláři "Master-Detail". Aplikace používá Entity Framework k naplnění objektů daty z databáze, sledování změn a zachování dat v databázi.

Model definuje dva typy, které se účastní relace 1: n: Kategorie (\\hlavní hlavní server) a produkt (\\podrobnosti závislé na). Nástroje sady Visual Studio pak slouží k navázání typů definovaných v modelu na ovládací prvky WinForms. Rozhraní WinForms pro vázání dat umožňuje navigaci mezi souvisejícími objekty: výběrem řádků v zobrazení Předloha způsobí, že se podrobné zobrazení aktualizuje s odpovídajícími podřízenými daty.

Snímky obrazovky a výpisy kódu v tomto návodu jsou pořízeny z Visual Studio 2013, ale můžete tento návod dokončit pomocí sady Visual Studio 2012 nebo Visual Studio 2010.

## <a name="pre-requisites"></a>Předpoklady

Pro dokončení tohoto Názorného postupu musíte mít nainstalovanou Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010.

Pokud používáte Visual Studio 2010, je také nutné nainstalovat NuGet. Další informace najdete v tématu [instalace NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Vytvoření aplikace

-   Otevřít Visual Studio
-   **Soubor –&gt; nový-&gt; projekt....**
-   V levém podokně vyberte **Windows** a v pravém podokně klikněte na **Windows FormsApplication** .
-   Jako název zadejte **WinFormswithEFSample** .
-   Vybrat **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instalace balíčku Entity Framework NuGet

-   V Průzkumník řešení klikněte pravým tlačítkem na projekt **WinFormswithEFSample** .
-   Vyberte **Spravovat balíčky NuGet...**
-   V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .
-   Klikněte na **nainstalovat** .  
    > [!NOTE]
    > Kromě sestavení EntityFramework je také přidáno odkaz na System. ComponentModel. DataAnnotations. Pokud má projekt odkaz na System. data. entity, bude po instalaci balíčku EntityFramework odebrán. Sestavení System. data. entity již není používáno pro Entity Framework 6 aplikací.

## <a name="implementing-ilistsource-for-collections"></a>Implementace IListSource pro kolekce

Vlastnosti kolekce musí implementovat rozhraní IListSource, aby bylo možné při použití model Windows Forms Povolit obousměrnou datovou vazbu s řazením. Za tímto účelem rozšíříme kolekci ObservableCollection, aby se přidaly funkce IListSource.

-   Přidejte do projektu třídu **ObservableListSource** :
    -   Klikněte pravým tlačítkem myši na název projektu.
    -   Vyberte **položku Přidat&gt; a nová položka** .
    -   Vyberte **třídu** a jako název třídy zadejte **ObservableListSource** .
-   Nahraďte kód vygenerovaný ve výchozím nastavení následujícím kódem:

*Tato třída umožňuje obousměrnou datovou vazbu a řazení. Třída je odvozena z kolekci ObservableCollection&lt;T&gt; a přidává explicitní implementaci IListSource. Metoda GetList () IListSource je implementována pro vrácení implementace IBindingList, která zůstává synchronizována s kolekci ObservableCollection. Implementace IBindingList vygenerovaná ToBindingList podporuje řazení. Metoda rozšíření ToBindingList je definována v sestavení EntityFramework.*

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

## <a name="define-a-model"></a>Definování modelu

V tomto návodu můžete zvolit implementaci modelu pomocí Code First nebo návrháře EF. Proveďte jednu ze dvou následujících částí.

### <a name="option-1-define-a-model-using-code-first"></a>Možnost 1: Definice modelu pomocí Code First

V této části se dozvíte, jak vytvořit model a jeho přidruženou databázi pomocí Code First. Přejděte k další části (**možnost 2: Definice modelu pomocí Database First)** Pokud byste místo toho použili Database First k zpětné analýze modelu z databáze pomocí návrháře EF

Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model.

-   Přidat novou třídu **produktu** do projektu
-   Nahraďte kód vygenerovaný ve výchozím nastavení následujícím kódem:

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

-   Přidejte do projektu třídu **kategorie** .
-   Nahraďte kód vygenerovaný ve výchozím nastavení následujícím kódem:

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

Kromě definování entit musíte definovat třídu, která je odvozena z **DbContext** a zpřístupňuje **negenerickými&lt;vlastnosti TEntity&gt;**  . Vlastnosti **negenerickými** umožňují, aby kontext věděl, které typy chcete do modelu zahrnout. Typy **DbContext** a **negenerickými** jsou definovány v sestavení EntityFramework.

Instance DbContext odvozeného typu spravuje objekty entit za běhu, což zahrnuje vyplnění objektů daty z databáze, sledování změn a uchování dat do databáze.

-   Přidejte do projektu novou třídu **ProductContext** .
-   Nahraďte kód vygenerovaný ve výchozím nastavení následujícím kódem:

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

### <a name="option-2-define-a-model-using-database-first"></a>Možnost 2: Definice modelu pomocí Database First

V této části se dozvíte, jak použít Database First k zpětné analýze modelu z databáze pomocí návrháře EF. Pokud jste dokončili předchozí část (**možnost 1: Definujte model pomocí Code First)** , pak tuto část přeskočte a přejděte rovnou na oddíl **opožděné načítání** .

#### <a name="create-an-existing-database"></a>Vytvoření existující databáze

Když cílíte na existující databázi, bude už vytvořená, ale pro tento návod musíme pro přístup vytvořit databázi.

Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:

-   Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.
-   Pokud používáte Visual Studio 2012, budete vytvářet databázi [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .

Pojďme dopředu a vygenerovat databázi.

-   **Zobrazení –&gt; Průzkumník serveru**
-   Klikněte pravým tlačítkem na **datová&gt; připojení – přidat připojení...**
-   Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat Microsoft SQL Server jako zdroj dat

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   Připojte se buď k LocalDB nebo SQL Express v závislosti na tom, který z nich jste nainstalovali, a jako název databáze zadejte **produkty** .

    ![Přidat LocalDB připojení](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat Express připojení](~/ef6/media/addconnectionexpress.png)

-   Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .

    ![Vytvořit databázi](~/ef6/media/createdatabase.png)

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

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

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

EF generuje kód z modelu pomocí šablon T4. Šablony dodávané se sadou Visual Studio nebo stažené z galerie sady Visual Studio jsou určené pro účely obecného použití. To znamená, že entity vygenerované z těchto šablon mají jednoduché&lt;vlastnosti&gt; ICollection T. Při provádění datových vazeb je ale žádoucí, aby měly vlastnosti kolekce, které implementují IListSource. To je důvod, proč jsme vytvořili třídu ObservableListSource výše a teď se chystáme upravit šablony, aby bylo možné tuto třídu používat.

-   Otevřete **Průzkumník řešení** a vyhledejte soubor **ProductModel. edmx.**
-   Vyhledejte soubor **ProductModel.TT** , který bude vnořen do souboru ProductModel. edmx.

    ![Šablona modelu produktu](~/ef6/media/productmodeltemplate.png)

-   Poklikejte na soubor ProductModel.tt a otevře se v editoru Visual studia.
-   Vyhledejte a nahraďte dva výskyty "**ICollection**" pomocí "**ObservableListSource**". Tyto jsou umístěné přibližně na řádcích 296 a 484.
-   Najde první výskyt "**HashSet –** " a nahradí ho "**ObservableListSource**". Tento výskyt je umístěný přibližně na řádku 50. Neměňte druhý výskyt HashSet –, který byl nalezen později v kódu.
-   Uložte soubor ProductModel.tt. To by mělo způsobit opětovné vygenerování kódu pro entity. Pokud se kód znovu negeneruje automaticky, klikněte pravým tlačítkem na ProductModel.tt a zvolte spustit vlastní nástroj.

Pokud teď otevřete soubor Category.cs (který je vnořený pod ProductModel.TT), měli byste vidět, že kolekce Products má typ **ObservableListSource&lt;produkt&gt;** .

Zkompilujte projekt.

## <a name="lazy-loading"></a>Opožděné načítání

Vlastnost **Products** u vlastnosti Category třídy a **kategorie** u třídy **produkt** je vlastností navigace. V Entity Framework navigační vlastnosti poskytují způsob, jak procházet relaci mezi dvěma typy entit.

EF vám nabízí možnost načítat související entity z databáze automaticky při prvním přístupu k vlastnosti navigace. U tohoto typu načítání (tzv. opožděné načítání) mějte na paměti, že při prvním přístupu k jednotlivým vlastnostem navigace se v databázi spustí samostatný dotaz, pokud obsah ještě není v kontextu.

Při použití typů entit POCO nahrazuje EF opožděné načítání vytvořením instancí odvozených typů proxy během běhu a následným přepsáním virtuálních vlastností ve třídách, aby bylo možné přidat zavěšení zatížení. Chcete-li získat opožděné načítání souvisejících objektů, je nutné deklarovat metody Get vlastnosti navigace jako **veřejné** a **virtuální** (**přepsatelné** v Visual Basic) a třídu nesmí být **zapečetěna** (**NotOverridable** v Visual Basic). Při použití Database First navigační vlastnosti se automaticky vypnuly virtuálním, aby bylo možné povolit opožděné načítání. V části Code First jsme se rozhodli, aby navigační vlastnosti byly ve stejném důsledku virtuální.

## <a name="bind-object-to-controls"></a>Svázání objektu s ovládacími prvky

Přidejte třídy, které jsou definovány v modelu jako zdroje dat pro tuto aplikaci WinForms.

-   V hlavní nabídce vyberte **&gt; projekt – přidat nový zdroj dat...**
    (v aplikaci Visual Studio 2010 je nutné vybrat **&gt; data – přidat nový zdroj dat...** )
-   V okně zvolte typ zdroje dat vyberte **objekt** a klikněte na **Další** .
-   V dialogovém okně Vybrat datové objekty rozložte **WinFormswithEFSample** dvakrát a vyberte **kategorie** . není nutné vybírat zdroj dat produktu, protože se k němu dostanete prostřednictvím vlastnosti produktu ve zdroji dat kategorie.

    ![Zdroj dat](~/ef6/media/datasource.png)

-   Klikněte na tlačítko **Dokončit.**
    Pokud se nezobrazí okno zdroje dat, vyberte možnost **Zobrazit –&gt; &gt; ostatní zdroje dat systému Windows.**
-   Stiskněte ikonu připnutí, aby se okno zdroje dat neautomaticky skrylo. Pokud je okno již viditelné, může být nutné spustit tlačítko Aktualizovat.

    ![Zdroj dat 2](~/ef6/media/datasource2.png)

-   V Průzkumník řešení dvakrát klikněte na soubor **Form1.cs** a otevřete tak hlavní formulář v návrháři.
-   Vyberte zdroj dat **kategorie** a přetáhněte jej na formuláři. Ve výchozím nastavení jsou nové ovládací prvky DataGridView (**categoryDataGridView**) a navigační panely nástrojů přidány do návrháře. Tyto ovládací prvky jsou vázány na komponenty BindingSource (**categoryBindingSource**) a vazby navigátor (**categoryBindingNavigator**), které jsou vytvořeny také.
-   Upravte sloupce v **categoryDataGridView**. Chceme nastavit sloupec **KódKategorie** na hodnotu jen pro čtení. Hodnota vlastnosti **CategoryID** je vygenerována databází poté, co data uložíme.
    -   Klikněte pravým tlačítkem myši na ovládací prvek DataGridView a vyberte možnost Upravit sloupce...
    -   Vyberte sloupec KódKategorie a nastavte vlastnost ReadOnly na hodnotu true.
    -   Stiskněte OK
-   Vyberte možnost produkty ze zdroje dat kategorie a přetáhněte ji do formuláře. Do formuláře jsou přidány productDataGridView a productBindingSource.
-   Upravte sloupce v productDataGridView. Chceme skrýt sloupce KódKategorie a Category a nastavit ProductId jen pro čtení. Po uložení dat se hodnota vlastnosti ProductId vygeneruje v databázi.
    -   Klikněte pravým tlačítkem myši na ovládací prvek DataGridView a vyberte možnost **Upravit sloupce...** .
    -   Vyberte sloupec **ProductID** a nastavte vlastnost **ReadOnly** na **hodnotu true**.
    -   Vyberte sloupec **CategoryID** a stiskněte tlačítko **Odebrat** . Totéž proveďte se sloupcem **Category (kategorie** ).
    -   Stisknutím klávesy **OK**.

    Zatím jsme přidružili ovládací prvky DataGridView k komponentám BindingSource v návrháři. V další části přidáme kód do kódu na pozadí pro nastavení categoryBindingSource. DataSource na kolekci entit, které jsou aktuálně sledovány pomocí DbContext. Když jsme přetáhli produkty z kategorie do kategorie, WinForms se postará o nastavení vlastnosti productsBindingSource. DataSource na categoryBindingSource a vlastnost productsBindingSource. DataMember na Products. Z důvodu této vazby budou v productDataGridView zobrazeny pouze produkty patřící do aktuálně vybrané kategorie.
-   Na panelu nástrojů navigace povolte tlačítko **Uložit** kliknutím pravým tlačítkem myši a výběrem možnosti **povoleno**.

    ![Návrhář – formulář 1](~/ef6/media/form1-designer.png)

-   Přidejte obslužnou rutinu události pro tlačítko Uložit dvojitým kliknutím na tlačítko. Tím se přidá obslužná rutina události a budete přinášet k kódu za jeho pozadí. Do další části se přidá kód obslužné rutiny události **kliknutí na categoryBindingNavigatorSaveItem\_** .

## <a name="add-the-code-that-handles-data-interaction"></a>Přidat kód, který zpracovává interakci s daty

Nyní přidáme kód pro použití ProductContext k provedení přístupu k datům. Aktualizujte kód pro hlavní okno formuláře, jak je znázorněno níže.

Kód deklaruje dlouhodobě běžící instanci ProductContext. Objekt ProductContext se používá k dotazování a ukládání dat do databáze. Metoda Dispose () v instanci ProductContext je pak volána z přepsané metody při zavírání. Komentáře kódu poskytují podrobné informace o tom, co kód dělá.

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

## <a name="test-the-windows-forms-application"></a>Testování aplikace model Windows Forms

-   Zkompilujte a spusťte aplikaci a můžete otestovat její funkčnost.

    ![Formulář 1 před uložením](~/ef6/media/form1beforesave.png)

-   Po uložení klíčů vygenerovaných úložiště se zobrazí na obrazovce.

    ![Formulář 1 po uložení](~/ef6/media/form1aftersave.png)

-   Pokud jste použili Code First, uvidíte také, že se pro vás vytvoří databáze **WinFormswithEFSample. ProductContext** .

    ![Průzkumník objektů serveru](~/ef6/media/serverobjexplorer.png)
