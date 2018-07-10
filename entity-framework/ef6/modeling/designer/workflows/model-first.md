---
title: Model First – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
caps.latest.revision: 3
ms.openlocfilehash: e7876776ed0dee764d5ced97b863a3580e02e20b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912676"
---
# <a name="model-first"></a>První model
Tato videa a podrobný návod poskytuje úvod do modelu první vývoj pomocí rozhraní Entity Framework. Model nejprve umožňuje vytvořit nový model využívající Entity Framework Designer a pak vygenerovat schéma databáze z modelu. Modelu je uložen v souboru EDMX (s příponou edmx) a může se zobrazit a upravit v Návrháři Entity Framework. Třídy, které budete moct používat ve vaší aplikaci se automaticky vygenerují ze souboru EDMX.

## <a name="watch-the-video"></a>Podívejte se na video
Tato videa a podrobný návod poskytuje úvod do modelu první vývoj pomocí rozhraní Entity Framework. Model nejprve umožňuje vytvořit nový model využívající Entity Framework Designer a pak vygenerovat schéma databáze z modelu. Modelu je uložen v souboru EDMX (s příponou edmx) a může se zobrazit a upravit v Návrháři Entity Framework. Třídy, které budete moct používat ve vaší aplikaci se automaticky vygenerují ze souboru EDMX.

**Přednášející:**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Předpoklady

Musíte mít Visual Studio 2010 nebo Visual Studio 2012 nainstalovat k dokončení tohoto návodu.

Pokud používáte Visual Studio 2010, musíte také mít [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) nainstalované.

## <a name="1-create-the-application"></a>1. Vytvoření aplikace

Pro zjednodušení budeme vytvořit základní konzolovou aplikaci, která používá pro přístup k datům modelu první:

-   Otevřít Visual Studio
-   **Soubor –&gt; nové –&gt; projektu...**
-   Vyberte **Windows** v levé nabídce a **konzolové aplikace**
-   Zadejte **ModelFirstSample** jako název
-   Vyberte **OK**

## <a name="2-create-model"></a>2. Vytvoření modelu

My budeme používat Entity Framework Designer, která je součástí sady Visual Studio, k vytvoření našeho modelu.

-   **Projekt –&gt; přidat novou položku...**
-   Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**
-   Zadejte **BloggingModel** jako název a klikněte na **OK**, spustí se Průvodce datovým modelem Entity
-   Vyberte **prázdný Model** a klikněte na tlačítko **dokončit**

    ![CreateEmptyModel](~/ef6/media/createemptymodel.png)

Entity Framework Designer se otevře s prázdnou modelu. Nyní můžeme začít přidání entity, vlastnosti a asociace do modelu.

-   Klikněte pravým tlačítkem na návrhové ploše a vyberte **vlastnosti**
-   V okně změnit vlastnosti **názvu kontejneru Entity** k **BloggingContext**
    *jde o název odvozené kontextu, který se vygeneruje pro vás kontextu představuje relaci s databází, což nám pro dotazování a ukládání dat*
-   Klikněte pravým tlačítkem na návrhové ploše a vyberte **přidat nový -&gt; Entity...**
-   Zadejte **blogu** jako název entity a **BlogId** název klíče a klikněte na **OK**

    ![AddBlogEntity](~/ef6/media/addblogentity.png)

-   Klikněte pravým tlačítkem na nové entity na návrhové ploše a vyberte **přidat nový -&gt; skalární vlastnost**, zadejte **název** jako název vlastnosti.
-   Opakujte tento postup pro přidání **Url** vlastnost.
-   Klikněte pravým tlačítkem na **adresy Url** vlastnost na návrhové ploše a vyberte **vlastnosti**, v okně změnit vlastnosti **Nullable** nastavení na **True** 
     *Díky tomu můžeme blogu uložte do databáze bez jeho přiřazení adresy Url*
-   Pomocí technik právě dozvědí, přidejte **příspěvek** entita s **PostId** klíčové vlastnosti
-   Přidat **Title** a **obsahu** Skalární vlastnosti, které chcete **příspěvek** entity

Když teď máme několik entit, je čas Přidat přidružení (nebo vztah) mezi nimi.

-   Klikněte pravým tlačítkem na návrhové ploše a vyberte **přidat nový -&gt; přidružení...**
-   Ujistěte se, jeden konec relace, přejděte na **blogu** s násobnost **jeden** a koncový bod pro **příspěvek** s násobnost **mnoho** 
     *To znamená, že má mnoho příspěvky blogu a příspěvek patří do jedné blogu*
-   Zkontrolujte **přidat vlastnosti cizího klíče na "Post" entitu** políčko je zaškrtnuté políčko a klikněte na tlačítko **OK**

    ![AddAssociationMF](~/ef6/media/addassociationmf.png)

Nyní je k dispozici jednoduchý model, který jsme Generovat z databáze a umožňuje číst a zapisovat data.

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Další kroky v sadě Visual Studio 2010

Pokud pracujete v sadě Visual Studio 2010 existují některé další kroky, které je potřeba provést upgrade na nejnovější verzi rozhraní Entity Framework. Upgrade je důležité, protože poskytuje přístup k vylepšení plochy rozhraní API, který je jednodušší použít, a také nejnovější opravy chyb.

Nejprve, potřebujeme načíst nejnovější verzi rozhraní Entity Framework z NuGet.

-   **Project –&gt; spravovat balíčky NuGet... ** 
     *Pokud nemáte k dispozici **spravovat balíčky NuGet... ** možnost byste měli nainstalovat [nejnovější verze Nugetu](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Vyberte **Online** kartu
-   Vyberte **EntityFramework** balíčku
-   Klikněte na tlačítko **instalace**

Dále musíme Prohodit náš model se vygenerovat kód, který využívá rozhraní API DbContext, která byla zavedena v novějších verzích rozhraní Entity Framework.

-   Klikněte pravým tlačítkem na prázdné místo modelu v EF designeru a vyberte **přidat položku generování kódu...**
-   Vyberte **Online šablon** v levé nabídce a vyhledejte **DbContext**
-   Vyberte EF **5.x DbContext generátor pro jazyk C\#**, zadejte **BloggingModel** jako název a klikněte na **přidat**

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Generování databáze

Zadaný náš model, Entity Framework schéma databáze, které vám umožní nám k ukládání a načítání dat pomocí modelu vypočítat.

Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:

-   Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.
-   Pokud používáte sadu Visual Studio 2012 pak vytvoříte [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) databáze.

Pojďme tedy vygenerovala databáze.

-   Klikněte pravým tlačítkem na návrhové ploše a vyberte **generovat databáze z modelu...**
-   Klikněte na tlačítko **nové připojení...** a určete LocalDB nebo SQL Express, v závislosti na tom, kterou verzi sady Visual Studio používáte, zadejte **ModelFirst.Blogging** jako název databáze.

    ![LocalDBConnectionMF](~/ef6/media/localdbconnectionmf.png)

    ![SqlExpressConnectionMF](~/ef6/media/sqlexpressconnectionmf.png)

-   Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**
-   Vyberte **Další** a Entity Framework Designer vypočítá skript k vytvoření schématu databáze
-   Jakmile skript se zobrazí, klikněte na tlačítko **Dokončit** a budou přidány do projektu a otevřít skript
-   Klikněte pravým tlačítkem na skript a vyberte **Execute**, zobrazí výzva k zadání databáze připojit, zadejte LocalDB nebo SQL Server Express, v závislosti na tom, kterou verzi sady Visual Studio používáte

## <a name="4-reading--writing-data"></a>4. Čtení a zápis dat

Když teď máme modelu je čas ho používat pro přístup k nějaká data. Třídy budeme používat k přístupu k datům se automaticky generují založeného na souboru EDMX.

*Tento snímek obrazovky je z Visual Studio 2012, pokud používáte Visual Studio 2010 BloggingModel.tt a BloggingModel.Context.tt soubory přímo v rámci projektu budou místo vnořen v souladu s souboru EDMX.*

![GeneratedClasses](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. Práce se změnami modelu

Nyní je čas na něm provést nějaké změny náš model, když jsme provedení těchto změn je také potřeba aktualizovat schéma databáze.

Začneme tak, že přidáte nové entity uživatele pro náš model.

-   Přidat nový **uživatele** název entity s **uživatelské jméno** jako název klíče a **řetězec** jako typ vlastnosti klíče

    ![AddUserEntity](~/ef6/media/adduserentity.png)

-   Klikněte pravým tlačítkem na **uživatelské jméno** vlastnost na návrhové ploše a vyberte **vlastnosti**, změnit ve vlastnostech okna **MaxLength** nastavení **50 ** 
     *To omezuje data ukládaná ve službě uživatelské jméno do 50 znaků.*
-   Přidat **DisplayName** skalární vlastnost **uživatele** entity

Teď máme aktualizovaného modelu a jsme připraveni na aktualizaci databáze tak, aby vyhovovaly náš nový typ entity uživatele.

-   Klikněte pravým tlačítkem na návrhové ploše a vyberte **generovat databáze z modelu...** , Vypočítá skriptu znovu vytvořit schéma založené na aktualizovaného modelu entity Framework.
-   Klikněte na tlačítko **dokončit**
-   Zobrazí se upozornění o přepsání existujícího skriptu jazyka DDL a části mapování a úložiště modelu, klikněte na tlačítko **Ano** pro obě tato upozornění
-   Aktualizovaný skript k vytvoření databáze SQL je pro vás otevřen.  
    *Skript, který je generován vyřadit všechny existující tabulky a pak znovu vytvořte schéma úplně od začátku. To může fungovat pro místní vývoj, ale není přijatelné pro doručením (push) změny do databáze, která již byla nasazena. Pokud potřebujete a publikujte změny do databáze, která již byla nasazena, je potřeba upravit skript nebo použijte nástroj pro porovnání schématu pro výpočet skript migrace.*
-   Klikněte pravým tlačítkem na skript a vyberte **Execute**, zobrazí výzva k zadání databáze připojit, zadejte LocalDB nebo SQL Server Express, v závislosti na tom, kterou verzi sady Visual Studio používáte

## <a name="summary"></a>Souhrn

V tomto návodu zvažovali jsme i první Model vývoje, který nám umožnila vytvoření modelu v EF designeru a potom generovat databáze z tohoto modelu. Potom jsme použili model číst a zapisovat data z databáze. Nakonec se aktualizace modelu a poté znovu vytvořit schéma databáze tak, aby odpovídala modelu.
