---
title: Database First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182453"
---
# <a name="database-first"></a>Database First
Toto video a podrobný návod vám poskytnou Úvod do Database Firstho vývoje pomocí Entity Framework. Database First umožňuje zpětně analyzovat model z existující databáze. Model je uložen v souboru EDMX (. edmx rozšíření) a lze jej zobrazit a upravit v Entity Framework Designer. Třídy, ve kterých pracujete ve vaší aplikaci, jsou automaticky generovány ze souboru EDMX.

## <a name="watch-the-video"></a>Přehrát video
Toto video představuje úvod do Database First vývoje pomocí Entity Framework. Database First umožňuje zpětně analyzovat model z existující databáze. Model je uložen v souboru EDMX (. edmx rozšíření) a lze jej zobrazit a upravit v Entity Framework Designer. Třídy, ve kterých pracujete ve vaší aplikaci, jsou automaticky generovány ze souboru EDMX.

**Prezentující**: [Rowan Miller](https://romiller.com/)

**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Předpoklady

Abyste mohli dokončit tento návod, budete muset mít nainstalovanou aspoň Visual Studio 2010 nebo Visual Studio 2012.

Pokud používáte Visual Studio 2010, budete také muset nainstalovat [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

 

## <a name="1-create-an-existing-database"></a>1. Vytvoření existující databáze

Když cílíte na existující databázi, bude už vytvořená, ale pro tento návod musíme pro přístup vytvořit databázi.

Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:

-   Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.
-   Pokud používáte Visual Studio 2012, budete vytvářet databázi [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .

 

Pojďme dopředu a vygenerovat databázi.

-   Otevřít Visual Studio
-   **Zobrazení-&gt; Průzkumník serveru**
-   Klikněte pravým tlačítkem na **datová připojení – &gt; Přidat připojení...**
-   Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat Microsoft SQL Server jako zdroj dat

    ![Vybrat zdroj dat](~/ef6/media/selectdatasource.png)

-   Připojte se buď k LocalDB nebo SQL Express v závislosti na tom, který z nich jste nainstalovali, a jako název databáze zadejte **DatabaseFirst. blog.**

    ![Připojení SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Připojení LocalDB DF](~/ef6/media/localdbconnectiondf.png)

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
```

## <a name="2-create-the-application"></a>2. Vytvoření aplikace

Aby se zajistilo něco jednoduchého, vytvoříme základní konzolovou aplikaci, která používá Database First k provádění přístupu k datům:

-   Otevřít Visual Studio
-   **Soubor-&gt; nový-&gt; projekt...**
-   V levé nabídce a v **konzolové aplikaci** vyberte **Windows** .
-   Jako název zadejte **DatabaseFirstSample** .
-   Vybrat **OK**

 

## <a name="3-reverse-engineer-model"></a>3. Model zpětného analýz

Budeme používat Entity Framework Designer, který je součástí sady Visual Studio, a vytvořit náš model.

-   **Projekt-&gt; Přidat novou položku...**
-   V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**
-   Jako název zadejte **BloggingModel** a klikněte na **OK** .
-   Spustí se **průvodce model EDM (Entity Data Model)** .
-   Vyberte **Generovat z databáze** a klikněte na **Další** .

    ![Krok 1 Průvodce](~/ef6/media/wizardstep1.png)

-   Vyberte připojení k databázi, kterou jste vytvořili v první části, jako název připojovacího řetězce zadejte **BloggingContext** a klikněte na **Další** .

    ![Krok 2 Průvodce](~/ef6/media/wizardstep2.png)

-   Kliknutím na zaškrtávací políčko vedle tabulky naimportujete všechny tabulky a kliknete na Dokončit.

    ![Krok 3 Průvodce](~/ef6/media/wizardstep3.png)

 

Po dokončení procesu zpětného zpracování se do projektu přidá nový model, který jste otevřeli, abyste mohli zobrazit Entity Framework Designer. Do projektu byl také přidán soubor App. config s podrobnostmi o připojení pro databázi.

![Počáteční model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Další kroky v aplikaci Visual Studio 2010

Pokud pracujete v aplikaci Visual Studio 2010, je nutné provést další kroky k upgradu na nejnovější verzi Entity Framework. Upgrade je důležitý, protože poskytuje přístup k vylepšenému povrchu rozhraní API, který je mnohem jednodušší, a také o nejnovějších opravách chyb.

Nejdřív potřebujeme získat nejnovější verzi Entity Framework z NuGetu.

-   **Projekt – &gt; spravovat balíčky NuGet...** 
    ,*Pokud nemáte balíčky pro **správu NuGet...** , měli byste nainstalovat [nejnovější verzi nugetu](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .*
-   Vyberte **online** kartu.
-   Výběr balíčku **EntityFramework**
-   Klikněte na **nainstalovat** .

Dále je potřeba prohodit náš model a vygenerovat kód, který využívá rozhraní DbContext API, které bylo představeno v novějších verzích Entity Framework.

-   V Návrháři EF klikněte pravým tlačítkem na prázdný bod modelu a vyberte **Přidat položku pro generování kódu...**
-   V nabídce vlevo vyberte **online šablony** a vyhledejte **DbContext** .
-   Vyberte generátor EF **5. x DbContext pro jazyk C @ no__t-1**, jako název zadejte **BloggingModel** a klikněte na **Přidat** .

    ![Šablona DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Čtení & zápisu dat

Teď, když máme model, je čas ho použít pro přístup k některým datům. Třídy, které použijeme pro přístup k datům, se pro vás automaticky generují na základě souboru EDMX.

*Tento snímek obrazovky je ze sady Visual Studio 2012, pokud používáte Visual Studio 2010 soubory BloggingModel.tt a BloggingModel.Context.tt budou přímo v rámci projektu, a ne jako vnořené do souboru EDMX.*

![Generované třídy DF](~/ef6/media/generatedclassesdf.png)

 

Implementujte metodu Main v Program.cs, jak je znázorněno níže. Tento kód vytvoří novou instanci našeho kontextu a pak ho použije k vložení nového blogu. Pak použije dotaz LINQ k načtení všech blogů z databáze seřazené abecedně podle názvu.

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
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a>5. Práce se změnami databáze

Teď je čas udělat nějaké změny ve schématu databáze, když tyto změny provedeme, abychom tyto změny provedli, i když aktualizujeme náš model.

Prvním krokem je udělat změny ve schématu databáze. Přidáváme do schématu tabulku uživatelů.

-   Klikněte pravým tlačítkem na databázi **DatabaseFirst. blogů** v Průzkumník serveru a vyberte **Nový dotaz** .
-   Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Teď, když je schéma aktualizované, je čas aktualizovat model pomocí těchto změn.

-   Klikněte pravým tlačítkem myši na prázdný bod modelu v Návrháři EF a vyberte aktualizovat model z databáze.... tím se spustí Průvodce aktualizací.
-   Na kartě Přidat v Průvodci aktualizací zaškrtněte políčko vedle pole tabulky, což znamená, že chceme do schématu přidat jakékoli nové tabulky.
    karta aktualizace *The zobrazuje všechny existující tabulky v modelu, u kterých budou během aktualizace zkontrolovány změny. Karty odstranit zobrazují všechny tabulky, které byly odebrány ze schématu a budou také odebrány z modelu jako součást aktualizace. Informace na těchto dvou kartách se automaticky zjišťují a jsou dostupné jenom pro informativní účely, nemůžete změnit žádné nastavení.*

    ![Průvodce aktualizací](~/ef6/media/refreshwizard.png)

-   V Průvodci aktualizací klikněte na Dokončit.

 

Model se teď aktualizoval tak, aby zahrnoval novou entitu uživatele, která se mapuje na tabulku uživatelů, kterou jsme přidali do databáze.

![Model se aktualizoval](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na Database First vývoj, což nám umožnilo vytvořit model v Návrháři EF na základě existující databáze. Pak jsme tento model použili ke čtení a zápisu dat z databáze. Nakonec jsme model aktualizovali tak, aby odrážel změny schématu databáze.
