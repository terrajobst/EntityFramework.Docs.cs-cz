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
# <a name="database-first"></a><span data-ttu-id="d4ab8-102">Database First</span><span class="sxs-lookup"><span data-stu-id="d4ab8-102">Database First</span></span>
<span data-ttu-id="d4ab8-103">Toto video a podrobný návod vám poskytnou Úvod do Database Firstho vývoje pomocí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="d4ab8-104">Database First umožňuje zpětně analyzovat model z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="d4ab8-105">Model je uložen v souboru EDMX (. edmx rozšíření) a lze jej zobrazit a upravit v Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="d4ab8-106">Třídy, ve kterých pracujete ve vaší aplikaci, jsou automaticky generovány ze souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="d4ab8-107">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="d4ab8-107">Watch the video</span></span>
<span data-ttu-id="d4ab8-108">Toto video představuje úvod do Database First vývoje pomocí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="d4ab8-109">Database First umožňuje zpětně analyzovat model z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="d4ab8-110">Model je uložen v souboru EDMX (. edmx rozšíření) a lze jej zobrazit a upravit v Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="d4ab8-111">Třídy, ve kterých pracujete ve vaší aplikaci, jsou automaticky generovány ze souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="d4ab8-112">**Prezentující**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="d4ab8-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="d4ab8-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="d4ab8-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d4ab8-114">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="d4ab8-114">Pre-Requisites</span></span>

<span data-ttu-id="d4ab8-115">Abyste mohli dokončit tento návod, budete muset mít nainstalovanou aspoň Visual Studio 2010 nebo Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="d4ab8-116">Pokud používáte Visual Studio 2010, budete také muset nainstalovat [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="d4ab8-117">1. Vytvoření existující databáze</span><span class="sxs-lookup"><span data-stu-id="d4ab8-117">1. Create an Existing Database</span></span>

<span data-ttu-id="d4ab8-118">Když cílíte na existující databázi, bude už vytvořená, ale pro tento návod musíme pro přístup vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="d4ab8-119">Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="d4ab8-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="d4ab8-120">Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="d4ab8-121">Pokud používáte Visual Studio 2012, budete vytvářet databázi [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="d4ab8-122">Pojďme dopředu a vygenerovat databázi.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="d4ab8-123">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4ab8-123">Open Visual Studio</span></span>
-   <span data-ttu-id="d4ab8-124">**Zobrazení-&gt; Průzkumník serveru**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d4ab8-125">Klikněte pravým tlačítkem na **datová připojení – &gt; Přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="d4ab8-126">Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat Microsoft SQL Server jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="d4ab8-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Vybrat zdroj dat](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="d4ab8-128">Připojte se buď k LocalDB nebo SQL Express v závislosti na tom, který z nich jste nainstalovali, a jako název databáze zadejte **DatabaseFirst. blog.**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![Připojení SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Připojení LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="d4ab8-131">Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Dialogové okno vytvořit databázi](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="d4ab8-133">Nová databáze se nyní zobrazí v Průzkumník serveru, klikněte na ni pravým tlačítkem myši a vyberte **Nový dotaz** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="d4ab8-134">Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="d4ab8-135">2. Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="d4ab8-135">2. Create the Application</span></span>

<span data-ttu-id="d4ab8-136">Aby se zajistilo něco jednoduchého, vytvoříme základní konzolovou aplikaci, která používá Database First k provádění přístupu k datům:</span><span class="sxs-lookup"><span data-stu-id="d4ab8-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="d4ab8-137">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4ab8-137">Open Visual Studio</span></span>
-   <span data-ttu-id="d4ab8-138">**Soubor-&gt; nový-&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="d4ab8-139">V levé nabídce a v **konzolové aplikaci** vyberte **Windows** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="d4ab8-140">Jako název zadejte **DatabaseFirstSample** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="d4ab8-141">Vybrat **OK**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="d4ab8-142">3. Model zpětného analýz</span><span class="sxs-lookup"><span data-stu-id="d4ab8-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="d4ab8-143">Budeme používat Entity Framework Designer, který je součástí sady Visual Studio, a vytvořit náš model.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="d4ab8-144">**Projekt-&gt; Přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="d4ab8-145">V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="d4ab8-146">Jako název zadejte **BloggingModel** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="d4ab8-147">Spustí se **průvodce model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="d4ab8-148">Vyberte **Generovat z databáze** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-148">Select **Generate from Database** and click **Next**</span></span>

    ![Krok 1 Průvodce](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="d4ab8-150">Vyberte připojení k databázi, kterou jste vytvořili v první části, jako název připojovacího řetězce zadejte **BloggingContext** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Krok 2 Průvodce](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="d4ab8-152">Kliknutím na zaškrtávací políčko vedle tabulky naimportujete všechny tabulky a kliknete na Dokončit.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Krok 3 Průvodce](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="d4ab8-154">Po dokončení procesu zpětného zpracování se do projektu přidá nový model, který jste otevřeli, abyste mohli zobrazit Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="d4ab8-155">Do projektu byl také přidán soubor App. config s podrobnostmi o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Počáteční model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="d4ab8-157">Další kroky v aplikaci Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d4ab8-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="d4ab8-158">Pokud pracujete v aplikaci Visual Studio 2010, je nutné provést další kroky k upgradu na nejnovější verzi Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="d4ab8-159">Upgrade je důležitý, protože poskytuje přístup k vylepšenému povrchu rozhraní API, který je mnohem jednodušší, a také o nejnovějších opravách chyb.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="d4ab8-160">Nejdřív potřebujeme získat nejnovější verzi Entity Framework z NuGetu.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="d4ab8-161">**Projekt – &gt; spravovat balíčky NuGet...** 
    ,*Pokud nemáte balíčky pro **správu NuGet...** , měli byste nainstalovat [nejnovější verzi nugetu](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .*</span><span class="sxs-lookup"><span data-stu-id="d4ab8-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="d4ab8-162">Vyberte **online** kartu.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="d4ab8-163">Výběr balíčku **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="d4ab8-164">Klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-164">Click **Install**</span></span>

<span data-ttu-id="d4ab8-165">Dále je potřeba prohodit náš model a vygenerovat kód, který využívá rozhraní DbContext API, které bylo představeno v novějších verzích Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="d4ab8-166">V Návrháři EF klikněte pravým tlačítkem na prázdný bod modelu a vyberte **Přidat položku pro generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="d4ab8-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="d4ab8-167">V nabídce vlevo vyberte **online šablony** a vyhledejte **DbContext** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="d4ab8-168">Vyberte generátor EF **5. x DbContext pro jazyk C @ no__t-1**, jako název zadejte **BloggingModel** a klikněte na **Přidat** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Šablona DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="d4ab8-170">4. Čtení & zápisu dat</span><span class="sxs-lookup"><span data-stu-id="d4ab8-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="d4ab8-171">Teď, když máme model, je čas ho použít pro přístup k některým datům.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="d4ab8-172">Třídy, které použijeme pro přístup k datům, se pro vás automaticky generují na základě souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="d4ab8-173">*Tento snímek obrazovky je ze sady Visual Studio 2012, pokud používáte Visual Studio 2010 soubory BloggingModel.tt a BloggingModel.Context.tt budou přímo v rámci projektu, a ne jako vnořené do souboru EDMX.*</span><span class="sxs-lookup"><span data-stu-id="d4ab8-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Generované třídy DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="d4ab8-175">Implementujte metodu Main v Program.cs, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="d4ab8-176">Tento kód vytvoří novou instanci našeho kontextu a pak ho použije k vložení nového blogu.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="d4ab8-177">Pak použije dotaz LINQ k načtení všech blogů z databáze seřazené abecedně podle názvu.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="d4ab8-178">Nyní můžete spustit aplikaci a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-178">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="d4ab8-179">5. Práce se změnami databáze</span><span class="sxs-lookup"><span data-stu-id="d4ab8-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="d4ab8-180">Teď je čas udělat nějaké změny ve schématu databáze, když tyto změny provedeme, abychom tyto změny provedli, i když aktualizujeme náš model.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="d4ab8-181">Prvním krokem je udělat změny ve schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="d4ab8-182">Přidáváme do schématu tabulku uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="d4ab8-183">Klikněte pravým tlačítkem na databázi **DatabaseFirst. blogů** v Průzkumník serveru a vyberte **Nový dotaz** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="d4ab8-184">Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .</span><span class="sxs-lookup"><span data-stu-id="d4ab8-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="d4ab8-185">Teď, když je schéma aktualizované, je čas aktualizovat model pomocí těchto změn.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="d4ab8-186">Klikněte pravým tlačítkem myši na prázdný bod modelu v Návrháři EF a vyberte aktualizovat model z databáze.... tím se spustí Průvodce aktualizací.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="d4ab8-187">Na kartě Přidat v Průvodci aktualizací zaškrtněte políčko vedle pole tabulky, což znamená, že chceme do schématu přidat jakékoli nové tabulky.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="d4ab8-188">karta aktualizace *The zobrazuje všechny existující tabulky v modelu, u kterých budou během aktualizace zkontrolovány změny. Karty odstranit zobrazují všechny tabulky, které byly odebrány ze schématu a budou také odebrány z modelu jako součást aktualizace. Informace na těchto dvou kartách se automaticky zjišťují a jsou dostupné jenom pro informativní účely, nemůžete změnit žádné nastavení.*</span><span class="sxs-lookup"><span data-stu-id="d4ab8-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Průvodce aktualizací](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="d4ab8-190">V Průvodci aktualizací klikněte na Dokončit.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="d4ab8-191">Model se teď aktualizoval tak, aby zahrnoval novou entitu uživatele, která se mapuje na tabulku uživatelů, kterou jsme přidali do databáze.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Model se aktualizoval](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="d4ab8-193">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d4ab8-193">Summary</span></span>

<span data-ttu-id="d4ab8-194">V tomto návodu jsme se podívali na Database First vývoj, což nám umožnilo vytvořit model v Návrháři EF na základě existující databáze.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="d4ab8-195">Pak jsme tento model použili ke čtení a zápisu dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="d4ab8-196">Nakonec jsme model aktualizovali tak, aby odrážel změny schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="d4ab8-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
