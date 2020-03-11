---
title: Rozdělení entity návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418460"
---
# <a name="designer-entity-splitting"></a>Rozdělení entity návrháře
Tento návod ukazuje, jak namapovat typ entity na dvě tabulky úpravou modelu pomocí Entity Framework Designer (EF Designer). Pokud tabulky sdílejí společný klíč, můžete ji namapovat na více tabulek. Koncepty, které platí pro mapování typu entity na dvě tabulky, lze snadno rozšířit a mapovat typ entity na více než dvě tabulky.

Následující obrázek ukazuje hlavní okna, která se používají při práci s návrhářem EF.

![Návrhář EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Předpoklady

Visual Studio 2012 nebo Visual Studio 2010, Ultimate, Premium, Professional nebo web Express Edition.

## <a name="create-the-database"></a>Vytvoření databáze

Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:

-   Pokud používáte Visual Studio 2012, budete vytvářet databázi LocalDB.
-   Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.

Nejprve vytvoříme databázi se dvěma tabulkami, které budeme zkombinovat do jedné entity.

-   Otevřete sadu Visual Studio.
-   **Zobrazení-&gt; Průzkumník serveru**
-   Klikněte pravým tlačítkem na **datová připojení –&gt; přidat připojení...**
-   Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat
-   Připojte se k LocalDB nebo SQL Express v závislosti na tom, který z nich máte nainstalovanou.
-   Jako název databáze zadejte **EntitySplitting** .
-   Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .
-   Nová databáze se nyní zobrazí v Průzkumník serveru
-   Pokud používáte Visual Studio 2012
    -   Klikněte pravým tlačítkem na databázi v Průzkumník serveru a vyberte **Nový dotaz** .
    -   Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .
-   Pokud používáte Visual Studio 2010
    -   Vyberte **data-&gt; Editor jazyka Transact SQL –&gt; nové připojení dotazu...**
    -   Jako název serveru zadejte **.\\SQLEXPRESS** a klikněte na **OK** .
    -   Vyberte databázi **EntitySplitting** v rozevíracím seznamu v horní části editoru dotazů.
    -   Zkopírujte následující příkaz SQL do nového dotazu, potom klikněte pravým tlačítkem na dotaz a vyberte **Spustit SQL** .

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

-   V nabídce **soubor** přejděte na příkaz **Nový**a klikněte na **projekt**.
-   V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **Konzolová aplikace** .
-   Jako název projektu zadejte **MapEntityToTablesSample** a klikněte na **OK**.
-   Pokud se zobrazí výzva k uložení dotazu SQL vytvořeného v první části, klikněte na **ne** .

## <a name="create-a-model-based-on-the-database"></a>Vytvoření modelu založeného na databázi

-   Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka**.
-   V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
-   Jako název souboru zadejte **MapEntityToTablesModel. edmx** a pak klikněte na **Přidat**.
-   V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další.**
-   V rozevíracím seznamu vyberte připojení **EntitySplitting** a klikněte na **Další**.
-   V dialogovém okně zvolit objekty databáze zaškrtněte políčko vedle **tabulky** uzel.
    Tato akce přidá všechny tabulky z databáze **EntitySplitting** do modelu.
-   Klikněte na tlačítko **Dokončit**.

Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.

## <a name="map-an-entity-to-two-tables"></a>Mapování entity na dvě tabulky

V tomto kroku aktualizujeme typ entity **Person** na kombinování dat z tabulek **Person** a **PersonInfo** .

-   Vyberte vlastnosti **e-mailu** a **telefonů** entity **PersonInfo **a stiskněte klávesy **CTRL + X** .
-   Vyberte entitu **osoba **a stiskněte klávesy **CTRL + V** .
-   Na návrhové ploše vyberte entitu  **PersonInfo** a stiskněte tlačítko **Odstranit** na klávesnici.
-   Po zobrazení výzvy klikněte na tlačítko **ne** , pokud chcete odebrat tabulku **PersonInfo** z modelu, chystáme se ji namapovat na entitu **Person** .

    ![Odstranit tabulky](~/ef6/media/deletetables.png)

Další kroky vyžadují okno s **podrobnostmi mapování** . Pokud toto okno nevidíte, klikněte pravým tlačítkem myši na návrhovou plochu a vyberte **mapování podrobností**.

-   Vyberte typ entity  **osoba** a v okně **Podrobnosti mapování** klikněte na **&lt;přidat tabulku nebo zobrazení&gt;**  .
-   V rozevíracím seznamu vyberte **PersonInfo ** .
    Okno  **podrobností mapování** se aktualizuje s výchozími mapováními sloupců, což je pro náš scénář přesné.

Typ entity  **osoba** je teď namapovaný na **uživatele** a tabulky **PersonInfo** .

![Mapování 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Použití modelu

-   Do metody Main vložte následující kód.

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

-   Zkompilujte a spusťte aplikaci.

V důsledku spuštění této aplikace byly v databázi provedeny následující příkazy jazyka T-SQL. 

-   Následující dva příkazy **INSERT** byly provedeny jako výsledek spuštěného kontextu. SaveChanges (). Přebírají data z entity **Person** a rozdělí je mezi tabulka **Person** a **PersonInfo** .

    ![Vložit 1](~/ef6/media/insert1.png)

    ![Vložit 2](~/ef6/media/insert2.png)
-   Následující **příkaz SELECT** byl proveden jako výsledek vytváření výčtu osob v databázi. Kombinuje data z tabulky **Person** a **PersonInfo** .

    ![Vyberte](~/ef6/media/select.png)
