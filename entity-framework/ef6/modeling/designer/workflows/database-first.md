---
title: Database First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c81025fe7c3ad6398f003f7be2a3f9f072eec327
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284080"
---
# <a name="database-first"></a>První databáze
Tato videa a podrobný návod poskytuje úvod do databáze první vývoj pomocí rozhraní Entity Framework. Nejprve lze provést zpětnou analýzu modelu z existující databáze. Modelu je uložen v souboru EDMX (s příponou edmx) a může se zobrazit a upravit v Návrháři Entity Framework. Třídy, které budete moct používat ve vaší aplikaci se automaticky vygenerují ze souboru EDMX.

## <a name="watch-the-video"></a>Podívejte se na video
Toto video obsahuje úvod do databáze první vývoj pomocí rozhraní Entity Framework. Nejprve lze provést zpětnou analýzu modelu z existující databáze. Modelu je uložen v souboru EDMX (s příponou edmx) a může se zobrazit a upravit v Návrháři Entity Framework. Třídy, které budete moct používat ve vaší aplikaci se automaticky vygenerují ze souboru EDMX.

**Přednášející:**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Předpoklady

Budete muset mít alespoň Visual Studio 2010 nebo Visual Studio 2012 nainstalovat k dokončení tohoto návodu.

Pokud používáte Visual Studio 2010, musíte také mít [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) nainstalované.

 

## <a name="1-create-an-existing-database"></a>1. Vytvořit existující databáze

Obvykle Pokud cílíte na existující databázi, které se už vytvořili, ale v tomto návodu budeme potřebovat vytvořit databázi pro přístup k.

Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:

-   Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.
-   Pokud používáte sadu Visual Studio 2012 pak vytvoříte [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) databáze.

 

Pojďme tedy vygenerovala databáze.

-   Otevřít Visual Studio
-   **Zobrazení –&gt; Průzkumníka serveru**
-   Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**
-   Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat jako zdroj dat Microsoft SQL Server

    ![Vyberte zdroj dat](~/ef6/media/selectdatasource.png)

-   Připojení k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali a zadejte **DatabaseFirst.Blogging** jako název databáze

    ![Připojení SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Připojení LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**

    ![Vytvoření dialogového okna databáze](~/ef6/media/createdatabasedialog.png)

-   Nové databáze se teď budou zobrazovat v Průzkumníku serveru, klikněte pravým tlačítkem myši na něj a vyberte **nový dotaz**
-   Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**

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

Pro zjednodušení budeme vytvářet základní Konzolová aplikace, která používá databázi první přístup k datům:

-   Otevřít Visual Studio
-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Windows** v levé nabídce a **konzolové aplikace**
-   Zadejte **DatabaseFirstSample** jako název
-   Vyberte **OK**

 

## <a name="3-reverse-engineer-model"></a>3. Zpětná analýza modelu

My budeme používat Entity Framework Designer, která je součástí sady Visual Studio, k vytvoření našeho modelu.

-   **Projekt –&gt; přidat novou položku...**
-   Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**
-   Zadejte **BloggingModel** jako název a klikněte na **OK**
-   Tím se spustí **Průvodce datovým modelem Entity**
-   Vyberte **Generovat z databáze** a klikněte na tlačítko **další**

    ![Krok 1 Průvodce](~/ef6/media/wizardstep1.png)

-   Vyberte připojení k databázi vytvořené v první části, zadejte **BloggingContext** jako název připojovacího řetězce a klikněte na tlačítko **další**

    ![Krok 2 Průvodce](~/ef6/media/wizardstep2.png)

-   Klikněte na zaškrtávací políčko vedle "Tables" k importu všech tabulek a klikněte na tlačítko 'Dokončit'

    ![Krok 3 Průvodce](~/ef6/media/wizardstep3.png)

 

Po dokončení procesu zpětné analýzy nový model je přidána do projektu a zpřístupnili k zobrazení v Návrháři Entity Framework. Soubor App.config má také byla přidána do projektu s podrobnostmi o připojení pro databázi.

![Počáteční modelu](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Další kroky v sadě Visual Studio 2010

Pokud pracujete v sadě Visual Studio 2010 existují některé další kroky, které je potřeba provést upgrade na nejnovější verzi rozhraní Entity Framework. Upgrade je důležité, protože poskytuje přístup k vylepšení plochy rozhraní API, který je jednodušší použít, a také nejnovější opravy chyb.

Nejprve, potřebujeme načíst nejnovější verzi rozhraní Entity Framework z NuGet.

-   **Project –&gt; spravovat balíčky NuGet... ** 
     *Pokud nemáte k dispozici **spravovat balíčky NuGet... ** možnost byste měli nainstalovat [nejnovější verze Nugetu](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Vyberte **Online** kartu
-   Vyberte **EntityFramework** balíčku
-   Klikněte na tlačítko **instalace**

Dále musíme Prohodit náš model se vygenerovat kód, který využívá rozhraní API DbContext, která byla zavedena v novějších verzích rozhraní Entity Framework.

-   Klikněte pravým tlačítkem na prázdné místo modelu v EF designeru a vyberte **přidat položku generování kódu...**
-   Vyberte **Online šablon** v levé nabídce a vyhledejte **DbContext**
-   Vyberte EF **5.x DbContext generátor pro jazyk C\#**, zadejte **BloggingModel** jako název a klikněte na **přidat**

    ![Šablona DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Čtení a zápis dat

Když teď máme modelu je čas ho používat pro přístup k nějaká data. Třídy budeme používat k přístupu k datům se automaticky generují založeného na souboru EDMX.

*Tento snímek obrazovky je z Visual Studio 2012, pokud používáte Visual Studio 2010 BloggingModel.tt a BloggingModel.Context.tt soubory přímo v rámci projektu budou místo vnořen v souladu s souboru EDMX.*

![Generované třídy DF](~/ef6/media/generatedclassesdf.png)

 

V souboru Program.cs implementujte metodu Main, jak je znázorněno níže. Tento kód vytvoří novou instanci třídy náš kontext a použije ho k vložení nového blogu. Pak používá dotaz LINQ k načtení všech blogy z databáze abecedně podle názvu.

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

Teď můžete aplikaci spustit a otestování.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a>5. Práce se změnami databáze

Nyní je čas při provádění těchto změn, musíme také aktualizovat náš model tak, aby odrážela tyto změny udělat nějaké změny do našich schéma databáze.

Prvním krokem je na něm provést nějaké změny schématu databáze. Chceme přidat tabulku uživatelů ve schématu.

-   Klikněte pravým tlačítkem na **DatabaseFirst.Blogging** databázi v Průzkumníku serveru a vyberte **nový dotaz**
-   Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Teď, když se aktualizuje schéma, je čas k aktualizaci modelu s těmito změnami.

-   Klikněte pravým tlačítkem na prázdné místo váš model v EF designeru a vyberte možnost "aktualizace modelu z databáze...", tím spustíte Průvodce aktualizací
-   Na kartě přidat Průvodce aktualizací zaškrtávací políčko vedle tabulky to znamená, že chceme přidat žádné nové tabulky ze schématu.
    *Na kartě aktualizace se zobrazí veškeré stávající tabulky v modelu, který bude sloužit k změny během aktualizace. Odstranit záložky zobrazit žádné tabulky, které byly odebrány ze schématu a také se odebere z modelu jako součást aktualizace. Informace o těchto dvou karet je automaticky rozpoznán a je k dispozici, pouze k informačním účelům jakékoli nastavení nejde změnit.*

    ![Aktualizovat Průvodce](~/ef6/media/refreshwizard.png)

-   Klikněte na tlačítko Dokončit v Průvodci aktualizacemi

 

Model je teď aktualizovaný zahrnout nové entity uživatele, který se mapuje na tabulku uživatelů, kterou jsme přidali do databáze.

![Aktualizace modelu](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Souhrn

V tomto návodu zvažovali jsme i první databáze vývoje, který nám umožnila vytvoření modelu v EF designeru založené na existující databázi. Potom jsme použili tohoto modelu číst a zapisovat data z databáze. Nakonec jsme aktualizovali tak, aby odrážely změny, které jsme provedli schéma databáze modelu.
