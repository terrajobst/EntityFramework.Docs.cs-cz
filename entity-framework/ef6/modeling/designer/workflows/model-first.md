---
title: Model First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418103"
---
# <a name="model-first"></a>Model First
Toto video a podrobný návod vám poskytnou Úvod do Model Firstho vývoje pomocí Entity Framework. Model First umožňuje vytvořit nový model pomocí Entity Framework Designer a pak z modelu vygenerovat schéma databáze. Model je uložen v souboru EDMX (. edmx rozšíření) a lze jej zobrazit a upravit v Entity Framework Designer. Třídy, ve kterých pracujete ve vaší aplikaci, jsou automaticky generovány ze souboru EDMX.

## <a name="watch-the-video"></a>Přehrát video
Toto video a podrobný návod vám poskytnou Úvod do Model Firstho vývoje pomocí Entity Framework. Model First umožňuje vytvořit nový model pomocí Entity Framework Designer a pak z modelu vygenerovat schéma databáze. Model je uložen v souboru EDMX (. edmx rozšíření) a lze jej zobrazit a upravit v Entity Framework Designer. Třídy, ve kterých pracujete ve vaší aplikaci, jsou automaticky generovány ze souboru EDMX.

**Prezentující**: [Rowan Miller](https://romiller.com/)

**Video**: [wmv](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Požadavky

Abyste mohli dokončit tento návod, budete muset mít nainstalovanou aplikaci Visual Studio 2010 nebo Visual Studio 2012.

Pokud používáte Visual Studio 2010, budete také muset nainstalovat [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

## <a name="1-create-the-application"></a>1. Vytvoření aplikace

Aby se zajistilo něco jednoduchého, vytvoříme základní konzolovou aplikaci, která používá Model First k provádění přístupu k datům:

-   Otevřete sadu Visual Studio.
-   **Soubor –&gt; projekt New-&gt;...**
-   V levé nabídce a v **konzolové aplikaci** vyberte **Windows** .
-   Jako název zadejte **ModelFirstSample** .
-   Vyberte **OK**.

## <a name="2-create-model"></a>2. vytvoření modelu

Budeme používat Entity Framework Designer, který je součástí sady Visual Studio, a vytvořit náš model.

-   **Projekt –&gt; přidat novou položku...**
-   V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**
-   Jako název zadejte **BloggingModel** a klikněte na **OK**. tím se spustí Průvodce model EDM (Entity Data Model).
-   Vyberte **prázdný model** a klikněte na **Dokončit** .

    ![Vytvořit prázdný model](~/ef6/media/createemptymodel.png)

Entity Framework Designer se otevře s prázdným modelem. Nyní můžeme do modelu začít přidávat entity, vlastnosti a přidružení.

-   Klikněte pravým tlačítkem na návrhovou plochu a vyberte **vlastnosti** .
-   V okno Vlastnosti změňte **název kontejneru entity** na **BloggingContext**
    *je to název odvozeného kontextu, který bude vygenerován za vás, kontext představuje relaci s databází, což nám umožní dotazovat se na data a uložit* je.
-   Klikněte pravým tlačítkem na návrhovou plochu a vyberte **Přidat novou&gt; entitu...**
-   Jako název entity zadejte **blog** a jako název klíče **BlogId** a klikněte na **OK** .

    ![Přidat entitu blogu](~/ef6/media/addblogentity.png)

-   V návrhové ploše klikněte pravým tlačítkem myši na novou entitu a vyberte **Přidat novou&gt; skalární vlastnost**, jako název vlastnosti zadejte **název** .
-   Opakujte tento postup, chcete-li přidat vlastnost **URL** .
-   Klikněte pravým tlačítkem na vlastnost **URL** na návrhové ploše a vyberte možnost **vlastnosti**. v části okno Vlastnosti změňte nastavení **Nullable** na **hodnotu true**
    *to nám umožní uložit blog do databáze bez přiřazení adresy URL* .
-   Pomocí technik, které jste právě seznámili, přidejte entitu **post** s vlastností klíče **PostId** .
-   Přidání skalárních vlastností **nadpisu** a **obsahu** k entitě **post**

Teď, když máme několik entit, je čas Přidat přidružení (nebo relaci) mezi nimi.

-   Klikněte pravým tlačítkem na návrhovou plochu a vyberte **Přidat přidružení nové&gt;...**
-   Nastavte jeden konec bodu relace na **blog** s násobností **jednoho** a druhého koncového bodu pro **publikování** s násobností **mnoha**
    *to znamená, že blog obsahuje mnoho příspěvků a příspěvek patří do jednoho blogu* .
-   Ujistěte se, že je zaškrtnuté políčko **Přidat vlastnosti cizího klíče do pole "post" entity** , a klikněte na **OK** .

    ![Přidat MF přidružení](~/ef6/media/addassociationmf.png)

Teď máme jednoduchý model, který umožňuje vygenerovat databázi a použít ji ke čtení a zápisu dat.

![Počáteční model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Další kroky v aplikaci Visual Studio 2010

Pokud pracujete v aplikaci Visual Studio 2010, je nutné provést další kroky k upgradu na nejnovější verzi Entity Framework. Upgrade je důležitý, protože poskytuje přístup k vylepšenému povrchu rozhraní API, který je mnohem jednodušší, a také o nejnovějších opravách chyb.

Nejdřív potřebujeme získat nejnovější verzi Entity Framework z NuGetu.

-   **Projekt –&gt; spravovat balíčky NuGet...** 
    , *jestli nemáte balíčky pro **správu NuGet...** , měli byste nainstalovat [nejnovější verzi nugetu](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .*
-   Vyberte **online** kartu.
-   Výběr balíčku **EntityFramework**
-   Klikněte na **nainstalovat** .

Dále je potřeba prohodit náš model a vygenerovat kód, který využívá rozhraní DbContext API, které bylo představeno v novějších verzích Entity Framework.

-   V Návrháři EF klikněte pravým tlačítkem na prázdný bod modelu a vyberte **Přidat položku pro generování kódu...**
-   V nabídce vlevo vyberte **online šablony** a vyhledejte **DbContext** .
-   Vyberte generátor EF **5. x DbContext pro jazyk C\#** , jako název zadejte **BloggingModel** a klikněte na **Přidat** .

    ![Šablona DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. vygenerování databáze

Vzhledem k našemu modelu Entity Framework může vypočítat schéma databáze, které nám umožní ukládat a načítat data pomocí modelu.

Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:

-   Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.
-   Pokud používáte Visual Studio 2012, budete vytvářet databázi [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .

Pojďme dopředu a vygenerovat databázi.

-   Klikněte pravým tlačítkem na návrhovou plochu a vyberte **Generovat databázi z modelu...**
-   Klikněte na **nové připojení...** a zadejte buď LocalDB nebo SQL Express, v závislosti na verzi sady Visual Studio, kterou používáte, jako název databáze zadejte **ModelFirst. blog** .

    ![LocalDB propojení MF](~/ef6/media/localdbconnectionmf.png)

    ![MF propojení SQL Express](~/ef6/media/sqlexpressconnectionmf.png)

-   Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .
-   Vyberte **Další** a Entity Framework Designer vypočítá skript pro vytvoření schématu databáze.
-   Po zobrazení skriptu klikněte na tlačítko **Dokončit** a skript se přidá do projektu a otevře se.
-   Klikněte pravým tlačítkem na skript a vyberte **Spustit**, budete vyzváni k zadání databáze, ke které se chcete připojit, a zadáním LocalDB nebo SQL Server Express v závislosti na tom, kterou verzi sady Visual Studio používáte.

## <a name="4-reading--writing-data"></a>4. čtení & zápisu dat

Teď, když máme model, je čas ho použít pro přístup k některým datům. Třídy, které použijeme pro přístup k datům, se pro vás automaticky generují na základě souboru EDMX.

*Tento snímek obrazovky je ze sady Visual Studio 2012, pokud používáte Visual Studio 2010 soubory BloggingModel.tt a BloggingModel.Context.tt budou přímo v rámci projektu, a ne jako vnořené do souboru EDMX.*

![Generované třídy](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. zvládnutí změn modelu

Teď je čas udělat v našem modelu nějaké změny. když tyto změny provedeme, musíme také aktualizovat schéma databáze.

Začneme přidáním nové entity uživatele do našeho modelu.

-   Přidejte nový název entity **uživatele** s **uživatelským** jménem jako název klíče a **řetězec** jako typ vlastnosti pro klíč.

    ![Přidat entitu uživatele](~/ef6/media/adduserentity.png)

-   Na návrhové ploše klikněte pravým tlačítkem na vlastnost **username** a vyberte **vlastnosti**. v okno Vlastnosti změňte nastavení **MaxLength** na **50**
    *tím se omezí data, která se dají ukládat v uživatelském jménu na 50 znaků* .
-   Přidání skalární vlastnosti **DisplayName** do entity **uživatele**

Teď máme aktualizovaný model a teď jsme připraveni aktualizovat databázi tak, aby vyhovovala našemu novému typu entity uživatele.

-   Klikněte pravým tlačítkem myši na návrhovou plochu a vyberte možnost **Generovat databázi z modelu...** , Entity Framework vypočítá skript pro opětovné vytvoření schématu na základě aktualizovaného modelu.
-   Klikněte na **Dokončit**.
-   Můžete obdržet upozornění týkající se přepsání stávajícího skriptu DDL a částí mapování a úložiště v modelu, pro obě tato upozornění klikněte na **Ano** .
-   Otevře se aktualizovaný skript SQL pro vytvoření databáze.  
    *Skript, který je vygenerován, odstraní všechny existující tabulky a znovu vytvoří schéma od začátku. To může fungovat pro místní vývoj, ale není životaschopná pro doručování změn do databáze, která už je nasazená. Pokud potřebujete publikovat změny v databázi, která již byla nasazena, budete muset skript upravit nebo použít nástroj porovnání schématu pro výpočet migračního skriptu.*
-   Klikněte pravým tlačítkem na skript a vyberte **Spustit**, budete vyzváni k zadání databáze, ke které se chcete připojit, a zadáním LocalDB nebo SQL Server Express v závislosti na tom, kterou verzi sady Visual Studio používáte.

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na Model First vývoj, což nám umožnilo vytvořit model v Návrháři EF a pak z tohoto modelu vygenerovat databázi. Pak jsme model používali ke čtení a zápisu dat z databáze. Nakonec jsme model aktualizovali a pak znovu vytvořili schéma databáze, aby odpovídalo modelu.
