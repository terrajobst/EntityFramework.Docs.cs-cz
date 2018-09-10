---
title: Database First – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: 93ae5729e487ed9be3972ac78d599dbea19ed458
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251086"
---
# <a name="database-first"></a><span data-ttu-id="0b66e-102">První databáze</span><span class="sxs-lookup"><span data-stu-id="0b66e-102">Database First</span></span>
<span data-ttu-id="0b66e-103">Tato videa a podrobný návod poskytuje úvod do databáze první vývoj pomocí rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b66e-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="0b66e-104">Nejprve lze provést zpětnou analýzu modelu z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="0b66e-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="0b66e-105">Modelu je uložen v souboru EDMX (s příponou edmx) a může se zobrazit a upravit v Návrháři Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b66e-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="0b66e-106">Třídy, které budete moct používat ve vaší aplikaci se automaticky vygenerují ze souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="0b66e-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="0b66e-107">Podívejte se na video</span><span class="sxs-lookup"><span data-stu-id="0b66e-107">Watch the video</span></span>
<span data-ttu-id="0b66e-108">Toto video obsahuje úvod do databáze první vývoj pomocí rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b66e-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="0b66e-109">Nejprve lze provést zpětnou analýzu modelu z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="0b66e-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="0b66e-110">Modelu je uložen v souboru EDMX (s příponou edmx) a může se zobrazit a upravit v Návrháři Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b66e-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="0b66e-111">Třídy, které budete moct používat ve vaší aplikaci se automaticky vygenerují ze souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="0b66e-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="0b66e-112">**Přednášející:**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="0b66e-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="0b66e-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="0b66e-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="0b66e-114">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="0b66e-114">Pre-Requisites</span></span>

<span data-ttu-id="0b66e-115">Budete muset mít alespoň Visual Studio 2010 nebo Visual Studio 2012 nainstalovat k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="0b66e-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="0b66e-116">Pokud používáte Visual Studio 2010, musíte také mít [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) nainstalované.</span><span class="sxs-lookup"><span data-stu-id="0b66e-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="0b66e-117">1. Vytvořit existující databáze</span><span class="sxs-lookup"><span data-stu-id="0b66e-117">1. Create an Existing Database</span></span>

<span data-ttu-id="0b66e-118">Obvykle Pokud cílíte na existující databázi, které se už vytvořili, ale v tomto návodu budeme potřebovat vytvořit databázi pro přístup k.</span><span class="sxs-lookup"><span data-stu-id="0b66e-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="0b66e-119">Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="0b66e-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="0b66e-120">Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="0b66e-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="0b66e-121">Pokud používáte sadu Visual Studio 2012 pak vytvoříte [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) databáze.</span><span class="sxs-lookup"><span data-stu-id="0b66e-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="0b66e-122">Pojďme tedy vygenerovala databáze.</span><span class="sxs-lookup"><span data-stu-id="0b66e-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="0b66e-123">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b66e-123">Open Visual Studio</span></span>
-   <span data-ttu-id="0b66e-124">**Zobrazení –&gt; Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="0b66e-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="0b66e-125">Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="0b66e-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="0b66e-126">Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat jako zdroj dat Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="0b66e-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Vyberte zdroj dat](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="0b66e-128">Připojení k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali a zadejte **DatabaseFirst.Blogging** jako název databáze</span><span class="sxs-lookup"><span data-stu-id="0b66e-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![Připojení SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Připojení LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="0b66e-131">Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**</span><span class="sxs-lookup"><span data-stu-id="0b66e-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Vytvoření dialogového okna databáze](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="0b66e-133">Nové databáze se teď budou zobrazovat v Průzkumníku serveru, klikněte pravým tlačítkem myši na něj a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="0b66e-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="0b66e-134">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="0b66e-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="0b66e-135">2. Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="0b66e-135">2. Create the Application</span></span>

<span data-ttu-id="0b66e-136">Pro zjednodušení budeme vytvářet základní Konzolová aplikace, která používá databázi první přístup k datům:</span><span class="sxs-lookup"><span data-stu-id="0b66e-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="0b66e-137">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b66e-137">Open Visual Studio</span></span>
-   <span data-ttu-id="0b66e-138">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="0b66e-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="0b66e-139">Vyberte **Windows** v levé nabídce a **konzolové aplikace**</span><span class="sxs-lookup"><span data-stu-id="0b66e-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="0b66e-140">Zadejte **DatabaseFirstSample** jako název</span><span class="sxs-lookup"><span data-stu-id="0b66e-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="0b66e-141">Vyberte **OK**</span><span class="sxs-lookup"><span data-stu-id="0b66e-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="0b66e-142">3. Zpětná analýza modelu</span><span class="sxs-lookup"><span data-stu-id="0b66e-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="0b66e-143">My budeme používat Entity Framework Designer, která je součástí sady Visual Studio, k vytvoření našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="0b66e-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="0b66e-144">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="0b66e-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="0b66e-145">Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="0b66e-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="0b66e-146">Zadejte **BloggingModel** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="0b66e-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="0b66e-147">Tím se spustí **Průvodce datovým modelem Entity**</span><span class="sxs-lookup"><span data-stu-id="0b66e-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="0b66e-148">Vyberte **Generovat z databáze** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="0b66e-148">Select **Generate from Database** and click **Next**</span></span>

    ![Krok 1 Průvodce](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="0b66e-150">Vyberte připojení k databázi vytvořené v první části, zadejte **BloggingContext** jako název připojovacího řetězce a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="0b66e-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Krok 2 Průvodce](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="0b66e-152">Klikněte na zaškrtávací políčko vedle "Tables" k importu všech tabulek a klikněte na tlačítko 'Dokončit'</span><span class="sxs-lookup"><span data-stu-id="0b66e-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Krok 3 Průvodce](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="0b66e-154">Po dokončení procesu zpětné analýzy nový model je přidána do projektu a zpřístupnili k zobrazení v Návrháři Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b66e-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="0b66e-155">Soubor App.config má také byla přidána do projektu s podrobnostmi o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="0b66e-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Počáteční modelu](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="0b66e-157">Další kroky v sadě Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="0b66e-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="0b66e-158">Pokud pracujete v sadě Visual Studio 2010 existují některé další kroky, které je potřeba provést upgrade na nejnovější verzi rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b66e-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="0b66e-159">Upgrade je důležité, protože poskytuje přístup k vylepšení plochy rozhraní API, který je jednodušší použít, a také nejnovější opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="0b66e-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="0b66e-160">Nejprve, potřebujeme načíst nejnovější verzi rozhraní Entity Framework z NuGet.</span><span class="sxs-lookup"><span data-stu-id="0b66e-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="0b66e-161">\*\*Project –&gt; spravovat balíčky NuGet... \*\* 
     \*Pokud nemáte k dispozici \**spravovat balíčky NuGet... \*\* možnost byste měli nainstalovat [nejnovější verze Nugetu](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="0b66e-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="0b66e-162">Vyberte **Online** kartu</span><span class="sxs-lookup"><span data-stu-id="0b66e-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="0b66e-163">Vyberte **EntityFramework** balíčku</span><span class="sxs-lookup"><span data-stu-id="0b66e-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="0b66e-164">Klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="0b66e-164">Click **Install**</span></span>

<span data-ttu-id="0b66e-165">Dále musíme Prohodit náš model se vygenerovat kód, který využívá rozhraní API DbContext, která byla zavedena v novějších verzích rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b66e-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="0b66e-166">Klikněte pravým tlačítkem na prázdné místo modelu v EF designeru a vyberte **přidat položku generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="0b66e-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="0b66e-167">Vyberte **Online šablon** v levé nabídce a vyhledejte **DbContext**</span><span class="sxs-lookup"><span data-stu-id="0b66e-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="0b66e-168">Vyberte EF **5.x DbContext generátor pro jazyk C\#**, zadejte **BloggingModel** jako název a klikněte na **přidat**</span><span class="sxs-lookup"><span data-stu-id="0b66e-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Šablona DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="0b66e-170">4. Čtení a zápis dat</span><span class="sxs-lookup"><span data-stu-id="0b66e-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="0b66e-171">Když teď máme modelu je čas ho používat pro přístup k nějaká data.</span><span class="sxs-lookup"><span data-stu-id="0b66e-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="0b66e-172">Třídy budeme používat k přístupu k datům se automaticky generují založeného na souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="0b66e-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="0b66e-173">*Tento snímek obrazovky je z Visual Studio 2012, pokud používáte Visual Studio 2010 BloggingModel.tt a BloggingModel.Context.tt soubory přímo v rámci projektu budou místo vnořen v souladu s souboru EDMX.*</span><span class="sxs-lookup"><span data-stu-id="0b66e-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Generované třídy DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="0b66e-175">V souboru Program.cs implementujte metodu Main, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="0b66e-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="0b66e-176">Tento kód vytvoří novou instanci třídy náš kontext a použije ho k vložení nového blogu.</span><span class="sxs-lookup"><span data-stu-id="0b66e-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="0b66e-177">Pak používá dotaz LINQ k načtení všech blogy z databáze abecedně podle názvu.</span><span class="sxs-lookup"><span data-stu-id="0b66e-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="0b66e-178">Teď můžete aplikaci spustit a otestování.</span><span class="sxs-lookup"><span data-stu-id="0b66e-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="0b66e-179">5. Práce se změnami databáze</span><span class="sxs-lookup"><span data-stu-id="0b66e-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="0b66e-180">Nyní je čas při provádění těchto změn, musíme také aktualizovat náš model tak, aby odrážela tyto změny udělat nějaké změny do našich schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="0b66e-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="0b66e-181">Prvním krokem je na něm provést nějaké změny schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="0b66e-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="0b66e-182">Chceme přidat tabulku uživatelů ve schématu.</span><span class="sxs-lookup"><span data-stu-id="0b66e-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="0b66e-183">Klikněte pravým tlačítkem na **DatabaseFirst.Blogging** databázi v Průzkumníku serveru a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="0b66e-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="0b66e-184">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="0b66e-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="0b66e-185">Teď, když se aktualizuje schéma, je čas k aktualizaci modelu s těmito změnami.</span><span class="sxs-lookup"><span data-stu-id="0b66e-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="0b66e-186">Klikněte pravým tlačítkem na prázdné místo váš model v EF designeru a vyberte možnost "aktualizace modelu z databáze...", tím spustíte Průvodce aktualizací</span><span class="sxs-lookup"><span data-stu-id="0b66e-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="0b66e-187">Na kartě přidat Průvodce aktualizací zaškrtávací políčko vedle tabulky to znamená, že chceme přidat žádné nové tabulky ze schématu.</span><span class="sxs-lookup"><span data-stu-id="0b66e-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="0b66e-188">*Na kartě aktualizace se zobrazí veškeré stávající tabulky v modelu, který bude sloužit k změny během aktualizace. Odstranit záložky zobrazit žádné tabulky, které byly odebrány ze schématu a také se odebere z modelu jako součást aktualizace. Informace o těchto dvou karet je automaticky rozpoznán a je k dispozici, pouze k informačním účelům jakékoli nastavení nejde změnit.*</span><span class="sxs-lookup"><span data-stu-id="0b66e-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Aktualizovat Průvodce](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="0b66e-190">Klikněte na tlačítko Dokončit v Průvodci aktualizacemi</span><span class="sxs-lookup"><span data-stu-id="0b66e-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="0b66e-191">Model je teď aktualizovaný zahrnout nové entity uživatele, který se mapuje na tabulku uživatelů, kterou jsme přidali do databáze.</span><span class="sxs-lookup"><span data-stu-id="0b66e-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Aktualizace modelu](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="0b66e-193">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0b66e-193">Summary</span></span>

<span data-ttu-id="0b66e-194">V tomto návodu zvažovali jsme i první databáze vývoje, který nám umožnila vytvoření modelu v EF designeru založené na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="0b66e-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="0b66e-195">Potom jsme použili tohoto modelu číst a zapisovat data z databáze.</span><span class="sxs-lookup"><span data-stu-id="0b66e-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="0b66e-196">Nakonec jsme aktualizovali tak, aby odrážely změny, které jsme provedli schéma databáze modelu.</span><span class="sxs-lookup"><span data-stu-id="0b66e-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
