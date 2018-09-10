---
title: Rozdělení návrháře entit - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 06199be977276cd3656e2550df79bac24276ec51
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250593"
---
# <a name="designer-entity-splitting"></a>Rozdělení návrháře entit
Tento návod ukazuje, jak pro mapování typu entity na dvě tabulky tak, že upravíte model se Návrhář Entity Framework (EF designeru). Entitu můžete namapovat na několik tabulek při tabulky sdílet společný klíč. Koncepty, které platí pro mapování typu entity na dvě tabulky jsou snadno rozšířit mapování typu entity k více než dvě tabulky.

Následující obrázek znázorňuje hlavní windows, které se používají při práci s EF designeru.

![EF designeru](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Požadavky

Visual Studio 2012 nebo Visual Studio 2010, Ultimate, Premium, Professional nebo Web Express edition.

## <a name="create-the-database"></a>Vytvoření databáze

Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:

-   Pokud používáte sadu Visual Studio 2012 pak vytvoříte databázi LocalDB.
-   Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.

Nejprve vytvoříme databázi pomocí dvou tabulek, které budeme zkombinovat do jedné entity.

-   Otevřít Visual Studio
-   **Zobrazení –&gt; Průzkumníka serveru**
-   Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**
-   Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat
-   Připojte se k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali
-   Zadejte **EntitySplitting** jako název databáze
-   Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**
-   Nová databáze se teď budou zobrazovat v Průzkumníku serveru
-   Pokud používáte sadu Visual Studio 2012
    -   Klikněte pravým tlačítkem na databázi v Průzkumníku serveru a vyberte **nový dotaz**
    -   Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**
-   Pokud používáte Visual Studio 2010
    -   Vyberte **dat –&gt; jazyka Transact SQL Editor –&gt; nové připojení dotazu...**
    -   Zadejte **.\\ SQLEXPRESS** jako název serveru a klikněte na **OK**
    -   Vyberte **EntitySplitting** databázi z rozevíracího seznamu v horní části editoru dotazů
    -   Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **provést SQL**

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>Vytvoření projektu

-   Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**.
-   V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzolovou aplikaci** šablony.
-   Zadejte **MapEntityToTablesSample** jako název projektu a klikněte na tlačítko **OK**.
-   Klikněte na tlačítko **ne** Pokud se zobrazí výzva k uložení dotazu SQL vytvořené v první části.

## <a name="create-a-model-based-on-the-database"></a>Vytvořit Model založený na databázi

-   Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**.
-   Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.
-   Zadejte **MapEntityToTablesModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.
-   V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další.**
-   Vyberte **EntitySplitting** připojení z rozevíracího seznamu a klikněte na tlačítko **Další**.
-   V dialogovém okně Zvolte vaše databázové objekty, zaškrtněte políčko vedle položky **tabulky** uzlu.
    Tato možnost přidá všechny tabulky z **EntitySplitting** databáze do modelu.
-   Klikněte na tlačítko **Dokončit**.

V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.

## <a name="map-an-entity-to-two-tables"></a>Mapovat Entity na dvě tabulky

V tomto kroku budeme aktualizovat **osoba** typ entity kombinovat data z **osoba** a **PersonInfo** tabulky.

-   Vyberte **e-mailu** a **Phone** vlastnosti ** PersonInfo ** entity a stiskněte klávesu **Ctrl + X** klíče.
-   Vyberte ** osoba ** entity a stiskněte klávesu **Ctrl + V** klíče.
-   Na návrhové ploše, vyberte **PersonInfo** entity a stiskněte klávesu **odstranit** tlačítko na klávesnici.
-   Klikněte na tlačítko **ne** když se zobrazí výzva, pokud chcete odebrat **PersonInfo** tabulky z modelu, jsme ji namapovat na **osoba** entity.

    ![Odstranit tabulky](~/ef6/media/deletetables.png)

Vyžadovat další kroky **podrobnosti mapování** okna. Pokud toto okno nelze zobrazit, klikněte pravým tlačítkem na návrhové ploše a vyberte **podrobnosti mapování**.

-   Vyberte **osoba** typu entity a klikněte na tlačítko **&lt;přidat tabulku nebo zobrazení&gt;** v **podrobnosti mapování** okna.
-   Vyberte ** PersonInfo ** z rozevíracího seznamu.
    **Podrobnosti mapování** okno se aktualizuje s výchozí mapování sloupců, jedná se o jemné pro náš scénář.

**Osoba** typ entity je nyní namapována na **osoba** a **PersonInfo** tabulky.

![Mapování 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Použití modelu

-   Vložte následující kód do metody Main.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   Kompilace a spuštění aplikace.

Následující příkazy T-SQL byly provedeny na databázi v důsledku spuštění této aplikace. 

-   Následující dva **vložit** příkazy byly spuštěny v důsledku spuštění kontextu. SaveChanges(). Přijímají data z **osoba** entity a rozdělit ho mezi **osoba** a **PersonInfo** tabulky.

    ![Vložit 1](~/ef6/media/insert1.png)

    ![Vložit 2](~/ef6/media/insert2.png)
-   Následující **vyberte** se spustil v důsledku výčet uživatelů v databázi. Kombinuje data z **osoba** a **PersonInfo** tabulky.

    ![Vyberte](~/ef6/media/select.png)
