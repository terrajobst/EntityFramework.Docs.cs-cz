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
# <a name="model-first"></a><span data-ttu-id="7008f-102">Model First</span><span class="sxs-lookup"><span data-stu-id="7008f-102">Model First</span></span>
<span data-ttu-id="7008f-103">Toto video a podrobný návod vám poskytnou Úvod do Model Firstho vývoje pomocí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7008f-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="7008f-104">Model First umožňuje vytvořit nový model pomocí Entity Framework Designer a pak z modelu vygenerovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7008f-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="7008f-105">Model je uložen v souboru EDMX (. edmx rozšíření) a lze jej zobrazit a upravit v Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7008f-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="7008f-106">Třídy, ve kterých pracujete ve vaší aplikaci, jsou automaticky generovány ze souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="7008f-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="7008f-107">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="7008f-107">Watch the video</span></span>
<span data-ttu-id="7008f-108">Toto video a podrobný návod vám poskytnou Úvod do Model Firstho vývoje pomocí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7008f-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="7008f-109">Model First umožňuje vytvořit nový model pomocí Entity Framework Designer a pak z modelu vygenerovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7008f-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="7008f-110">Model je uložen v souboru EDMX (. edmx rozšíření) a lze jej zobrazit a upravit v Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7008f-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="7008f-111">Třídy, ve kterých pracujete ve vaší aplikaci, jsou automaticky generovány ze souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="7008f-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="7008f-112">**Prezentující**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="7008f-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="7008f-113">**Video**: [wmv](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="7008f-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="7008f-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7008f-114">Pre-Requisites</span></span>

<span data-ttu-id="7008f-115">Abyste mohli dokončit tento návod, budete muset mít nainstalovanou aplikaci Visual Studio 2010 nebo Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="7008f-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="7008f-116">Pokud používáte Visual Studio 2010, budete také muset nainstalovat [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="7008f-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="7008f-117">1. Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="7008f-117">1. Create the Application</span></span>

<span data-ttu-id="7008f-118">Aby se zajistilo něco jednoduchého, vytvoříme základní konzolovou aplikaci, která používá Model First k provádění přístupu k datům:</span><span class="sxs-lookup"><span data-stu-id="7008f-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="7008f-119">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7008f-119">Open Visual Studio</span></span>
-   <span data-ttu-id="7008f-120">**Soubor –&gt; projekt New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="7008f-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="7008f-121">V levé nabídce a v **konzolové aplikaci** vyberte **Windows** .</span><span class="sxs-lookup"><span data-stu-id="7008f-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="7008f-122">Jako název zadejte **ModelFirstSample** .</span><span class="sxs-lookup"><span data-stu-id="7008f-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="7008f-123">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="7008f-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="7008f-124">2. vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="7008f-124">2. Create Model</span></span>

<span data-ttu-id="7008f-125">Budeme používat Entity Framework Designer, který je součástí sady Visual Studio, a vytvořit náš model.</span><span class="sxs-lookup"><span data-stu-id="7008f-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="7008f-126">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="7008f-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="7008f-127">V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**</span><span class="sxs-lookup"><span data-stu-id="7008f-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="7008f-128">Jako název zadejte **BloggingModel** a klikněte na **OK**. tím se spustí Průvodce model EDM (Entity Data Model).</span><span class="sxs-lookup"><span data-stu-id="7008f-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="7008f-129">Vyberte **prázdný model** a klikněte na **Dokončit** .</span><span class="sxs-lookup"><span data-stu-id="7008f-129">Select **Empty Model** and click **Finish**</span></span>

    ![Vytvořit prázdný model](~/ef6/media/createemptymodel.png)

<span data-ttu-id="7008f-131">Entity Framework Designer se otevře s prázdným modelem.</span><span class="sxs-lookup"><span data-stu-id="7008f-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="7008f-132">Nyní můžeme do modelu začít přidávat entity, vlastnosti a přidružení.</span><span class="sxs-lookup"><span data-stu-id="7008f-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="7008f-133">Klikněte pravým tlačítkem na návrhovou plochu a vyberte **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="7008f-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="7008f-134">V okno Vlastnosti změňte **název kontejneru entity** na **BloggingContext**
    *je to název odvozeného kontextu, který bude vygenerován za vás, kontext představuje relaci s databází, což nám umožní dotazovat se na data a uložit* je.</span><span class="sxs-lookup"><span data-stu-id="7008f-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="7008f-135">Klikněte pravým tlačítkem na návrhovou plochu a vyberte **Přidat novou&gt; entitu...**</span><span class="sxs-lookup"><span data-stu-id="7008f-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="7008f-136">Jako název entity zadejte **blog** a jako název klíče **BlogId** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="7008f-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Přidat entitu blogu](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="7008f-138">V návrhové ploše klikněte pravým tlačítkem myši na novou entitu a vyberte **Přidat novou&gt; skalární vlastnost**, jako název vlastnosti zadejte **název** .</span><span class="sxs-lookup"><span data-stu-id="7008f-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="7008f-139">Opakujte tento postup, chcete-li přidat vlastnost **URL** .</span><span class="sxs-lookup"><span data-stu-id="7008f-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="7008f-140">Klikněte pravým tlačítkem na vlastnost **URL** na návrhové ploše a vyberte možnost **vlastnosti**. v části okno Vlastnosti změňte nastavení **Nullable** na **hodnotu true**
    *to nám umožní uložit blog do databáze bez přiřazení adresy URL* .</span><span class="sxs-lookup"><span data-stu-id="7008f-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="7008f-141">Pomocí technik, které jste právě seznámili, přidejte entitu **post** s vlastností klíče **PostId** .</span><span class="sxs-lookup"><span data-stu-id="7008f-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="7008f-142">Přidání skalárních vlastností **nadpisu** a **obsahu** k entitě **post**</span><span class="sxs-lookup"><span data-stu-id="7008f-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="7008f-143">Teď, když máme několik entit, je čas Přidat přidružení (nebo relaci) mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="7008f-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="7008f-144">Klikněte pravým tlačítkem na návrhovou plochu a vyberte **Přidat přidružení nové&gt;...**</span><span class="sxs-lookup"><span data-stu-id="7008f-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="7008f-145">Nastavte jeden konec bodu relace na **blog** s násobností **jednoho** a druhého koncového bodu pro **publikování** s násobností **mnoha**
    *to znamená, že blog obsahuje mnoho příspěvků a příspěvek patří do jednoho blogu* .</span><span class="sxs-lookup"><span data-stu-id="7008f-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="7008f-146">Ujistěte se, že je zaškrtnuté políčko **Přidat vlastnosti cizího klíče do pole "post" entity** , a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="7008f-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Přidat MF přidružení](~/ef6/media/addassociationmf.png)

<span data-ttu-id="7008f-148">Teď máme jednoduchý model, který umožňuje vygenerovat databázi a použít ji ke čtení a zápisu dat.</span><span class="sxs-lookup"><span data-stu-id="7008f-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Počáteční model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="7008f-150">Další kroky v aplikaci Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="7008f-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="7008f-151">Pokud pracujete v aplikaci Visual Studio 2010, je nutné provést další kroky k upgradu na nejnovější verzi Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7008f-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="7008f-152">Upgrade je důležitý, protože poskytuje přístup k vylepšenému povrchu rozhraní API, který je mnohem jednodušší, a také o nejnovějších opravách chyb.</span><span class="sxs-lookup"><span data-stu-id="7008f-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="7008f-153">Nejdřív potřebujeme získat nejnovější verzi Entity Framework z NuGetu.</span><span class="sxs-lookup"><span data-stu-id="7008f-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="7008f-154">**Projekt –&gt; spravovat balíčky NuGet...** 
    , *jestli nemáte balíčky pro **správu NuGet...** , měli byste nainstalovat [nejnovější verzi nugetu](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .*</span><span class="sxs-lookup"><span data-stu-id="7008f-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="7008f-155">Vyberte **online** kartu.</span><span class="sxs-lookup"><span data-stu-id="7008f-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="7008f-156">Výběr balíčku **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="7008f-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="7008f-157">Klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="7008f-157">Click **Install**</span></span>

<span data-ttu-id="7008f-158">Dále je potřeba prohodit náš model a vygenerovat kód, který využívá rozhraní DbContext API, které bylo představeno v novějších verzích Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7008f-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="7008f-159">V Návrháři EF klikněte pravým tlačítkem na prázdný bod modelu a vyberte **Přidat položku pro generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="7008f-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="7008f-160">V nabídce vlevo vyberte **online šablony** a vyhledejte **DbContext** .</span><span class="sxs-lookup"><span data-stu-id="7008f-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="7008f-161">Vyberte generátor EF **5. x DbContext pro jazyk C\#** , jako název zadejte **BloggingModel** a klikněte na **Přidat** .</span><span class="sxs-lookup"><span data-stu-id="7008f-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Šablona DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="7008f-163">3. vygenerování databáze</span><span class="sxs-lookup"><span data-stu-id="7008f-163">3. Generating the Database</span></span>

<span data-ttu-id="7008f-164">Vzhledem k našemu modelu Entity Framework může vypočítat schéma databáze, které nám umožní ukládat a načítat data pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="7008f-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="7008f-165">Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="7008f-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="7008f-166">Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="7008f-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="7008f-167">Pokud používáte Visual Studio 2012, budete vytvářet databázi [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="7008f-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="7008f-168">Pojďme dopředu a vygenerovat databázi.</span><span class="sxs-lookup"><span data-stu-id="7008f-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="7008f-169">Klikněte pravým tlačítkem na návrhovou plochu a vyberte **Generovat databázi z modelu...**</span><span class="sxs-lookup"><span data-stu-id="7008f-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="7008f-170">Klikněte na **nové připojení...**</span><span class="sxs-lookup"><span data-stu-id="7008f-170">Click **New Connection…**</span></span> <span data-ttu-id="7008f-171">a zadejte buď LocalDB nebo SQL Express, v závislosti na verzi sady Visual Studio, kterou používáte, jako název databáze zadejte **ModelFirst. blog** .</span><span class="sxs-lookup"><span data-stu-id="7008f-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDB propojení MF](~/ef6/media/localdbconnectionmf.png)

    ![MF propojení SQL Express](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="7008f-174">Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .</span><span class="sxs-lookup"><span data-stu-id="7008f-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="7008f-175">Vyberte **Další** a Entity Framework Designer vypočítá skript pro vytvoření schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="7008f-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="7008f-176">Po zobrazení skriptu klikněte na tlačítko **Dokončit** a skript se přidá do projektu a otevře se.</span><span class="sxs-lookup"><span data-stu-id="7008f-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="7008f-177">Klikněte pravým tlačítkem na skript a vyberte **Spustit**, budete vyzváni k zadání databáze, ke které se chcete připojit, a zadáním LocalDB nebo SQL Server Express v závislosti na tom, kterou verzi sady Visual Studio používáte.</span><span class="sxs-lookup"><span data-stu-id="7008f-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="7008f-178">4. čtení & zápisu dat</span><span class="sxs-lookup"><span data-stu-id="7008f-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="7008f-179">Teď, když máme model, je čas ho použít pro přístup k některým datům.</span><span class="sxs-lookup"><span data-stu-id="7008f-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="7008f-180">Třídy, které použijeme pro přístup k datům, se pro vás automaticky generují na základě souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="7008f-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="7008f-181">*Tento snímek obrazovky je ze sady Visual Studio 2012, pokud používáte Visual Studio 2010 soubory BloggingModel.tt a BloggingModel.Context.tt budou přímo v rámci projektu, a ne jako vnořené do souboru EDMX.*</span><span class="sxs-lookup"><span data-stu-id="7008f-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Generované třídy](~/ef6/media/generatedclasses.png)

<span data-ttu-id="7008f-183">Implementujte metodu Main v Program.cs, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="7008f-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="7008f-184">Tento kód vytvoří novou instanci našeho kontextu a pak ho použije k vložení nového blogu.</span><span class="sxs-lookup"><span data-stu-id="7008f-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="7008f-185">Pak použije dotaz LINQ k načtení všech blogů z databáze seřazené abecedně podle názvu.</span><span class="sxs-lookup"><span data-stu-id="7008f-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="7008f-186">Nyní můžete spustit aplikaci a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="7008f-186">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="7008f-187">5. zvládnutí změn modelu</span><span class="sxs-lookup"><span data-stu-id="7008f-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="7008f-188">Teď je čas udělat v našem modelu nějaké změny. když tyto změny provedeme, musíme také aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="7008f-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="7008f-189">Začneme přidáním nové entity uživatele do našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="7008f-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="7008f-190">Přidejte nový název entity **uživatele** s **uživatelským** jménem jako název klíče a **řetězec** jako typ vlastnosti pro klíč.</span><span class="sxs-lookup"><span data-stu-id="7008f-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Přidat entitu uživatele](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="7008f-192">Na návrhové ploše klikněte pravým tlačítkem na vlastnost **username** a vyberte **vlastnosti**. v okno Vlastnosti změňte nastavení **MaxLength** na **50**
    *tím se omezí data, která se dají ukládat v uživatelském jménu na 50 znaků* .</span><span class="sxs-lookup"><span data-stu-id="7008f-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="7008f-193">Přidání skalární vlastnosti **DisplayName** do entity **uživatele**</span><span class="sxs-lookup"><span data-stu-id="7008f-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="7008f-194">Teď máme aktualizovaný model a teď jsme připraveni aktualizovat databázi tak, aby vyhovovala našemu novému typu entity uživatele.</span><span class="sxs-lookup"><span data-stu-id="7008f-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="7008f-195">Klikněte pravým tlačítkem myši na návrhovou plochu a vyberte možnost **Generovat databázi z modelu...** , Entity Framework vypočítá skript pro opětovné vytvoření schématu na základě aktualizovaného modelu.</span><span class="sxs-lookup"><span data-stu-id="7008f-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="7008f-196">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="7008f-196">Click **Finish**</span></span>
-   <span data-ttu-id="7008f-197">Můžete obdržet upozornění týkající se přepsání stávajícího skriptu DDL a částí mapování a úložiště v modelu, pro obě tato upozornění klikněte na **Ano** .</span><span class="sxs-lookup"><span data-stu-id="7008f-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="7008f-198">Otevře se aktualizovaný skript SQL pro vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="7008f-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="7008f-199">*Skript, který je vygenerován, odstraní všechny existující tabulky a znovu vytvoří schéma od začátku. To může fungovat pro místní vývoj, ale není životaschopná pro doručování změn do databáze, která už je nasazená. Pokud potřebujete publikovat změny v databázi, která již byla nasazena, budete muset skript upravit nebo použít nástroj porovnání schématu pro výpočet migračního skriptu.*</span><span class="sxs-lookup"><span data-stu-id="7008f-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="7008f-200">Klikněte pravým tlačítkem na skript a vyberte **Spustit**, budete vyzváni k zadání databáze, ke které se chcete připojit, a zadáním LocalDB nebo SQL Server Express v závislosti na tom, kterou verzi sady Visual Studio používáte.</span><span class="sxs-lookup"><span data-stu-id="7008f-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="7008f-201">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7008f-201">Summary</span></span>

<span data-ttu-id="7008f-202">V tomto návodu jsme se podívali na Model First vývoj, což nám umožnilo vytvořit model v Návrháři EF a pak z tohoto modelu vygenerovat databázi.</span><span class="sxs-lookup"><span data-stu-id="7008f-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="7008f-203">Pak jsme model používali ke čtení a zápisu dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="7008f-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="7008f-204">Nakonec jsme model aktualizovali a pak znovu vytvořili schéma databáze, aby odpovídalo modelu.</span><span class="sxs-lookup"><span data-stu-id="7008f-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
