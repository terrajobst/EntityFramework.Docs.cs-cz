---
title: Model First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: d429d5ea590b22c77f3f7f0bcfbd5dfc0a3e0049
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283872"
---
# <a name="model-first"></a><span data-ttu-id="c0091-102">První model</span><span class="sxs-lookup"><span data-stu-id="c0091-102">Model First</span></span>
<span data-ttu-id="c0091-103">Tato videa a podrobný návod poskytuje úvod do modelu první vývoj pomocí rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0091-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="c0091-104">Model nejprve umožňuje vytvořit nový model využívající Entity Framework Designer a pak vygenerovat schéma databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="c0091-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="c0091-105">Modelu je uložen v souboru EDMX (s příponou edmx) a může se zobrazit a upravit v Návrháři Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0091-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="c0091-106">Třídy, které budete moct používat ve vaší aplikaci se automaticky vygenerují ze souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="c0091-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="c0091-107">Podívejte se na video</span><span class="sxs-lookup"><span data-stu-id="c0091-107">Watch the video</span></span>
<span data-ttu-id="c0091-108">Tato videa a podrobný návod poskytuje úvod do modelu první vývoj pomocí rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0091-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="c0091-109">Model nejprve umožňuje vytvořit nový model využívající Entity Framework Designer a pak vygenerovat schéma databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="c0091-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="c0091-110">Modelu je uložen v souboru EDMX (s příponou edmx) a může se zobrazit a upravit v Návrháři Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0091-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="c0091-111">Třídy, které budete moct používat ve vaší aplikaci se automaticky vygenerují ze souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="c0091-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="c0091-112">**Přednášející:**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="c0091-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="c0091-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="c0091-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="c0091-114">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="c0091-114">Pre-Requisites</span></span>

<span data-ttu-id="c0091-115">Musíte mít Visual Studio 2010 nebo Visual Studio 2012 nainstalovat k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="c0091-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="c0091-116">Pokud používáte Visual Studio 2010, musíte také mít [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) nainstalované.</span><span class="sxs-lookup"><span data-stu-id="c0091-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="c0091-117">1. Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="c0091-117">1. Create the Application</span></span>

<span data-ttu-id="c0091-118">Pro zjednodušení budeme vytvořit základní konzolovou aplikaci, která používá pro přístup k datům modelu první:</span><span class="sxs-lookup"><span data-stu-id="c0091-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="c0091-119">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0091-119">Open Visual Studio</span></span>
-   <span data-ttu-id="c0091-120">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="c0091-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="c0091-121">Vyberte **Windows** v levé nabídce a **konzolové aplikace**</span><span class="sxs-lookup"><span data-stu-id="c0091-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="c0091-122">Zadejte **ModelFirstSample** jako název</span><span class="sxs-lookup"><span data-stu-id="c0091-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="c0091-123">Vyberte **OK**</span><span class="sxs-lookup"><span data-stu-id="c0091-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="c0091-124">2. Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="c0091-124">2. Create Model</span></span>

<span data-ttu-id="c0091-125">My budeme používat Entity Framework Designer, která je součástí sady Visual Studio, k vytvoření našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="c0091-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="c0091-126">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="c0091-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="c0091-127">Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="c0091-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="c0091-128">Zadejte **BloggingModel** jako název a klikněte na **OK**, spustí se Průvodce datovým modelem Entity</span><span class="sxs-lookup"><span data-stu-id="c0091-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="c0091-129">Vyberte **prázdný Model** a klikněte na tlačítko **dokončit**</span><span class="sxs-lookup"><span data-stu-id="c0091-129">Select **Empty Model** and click **Finish**</span></span>

    ![Vytvořit prázdný Model](~/ef6/media/createemptymodel.png)

<span data-ttu-id="c0091-131">Entity Framework Designer se otevře s prázdnou modelu.</span><span class="sxs-lookup"><span data-stu-id="c0091-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="c0091-132">Nyní můžeme začít přidání entity, vlastnosti a asociace do modelu.</span><span class="sxs-lookup"><span data-stu-id="c0091-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="c0091-133">Klikněte pravým tlačítkem na návrhové ploše a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="c0091-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="c0091-134">V okně změnit vlastnosti **názvu kontejneru Entity** k **BloggingContext**
    *jde o název odvozené kontextu, který se vygeneruje pro vás kontextu představuje relaci s databází, což nám pro dotazování a ukládání dat*</span><span class="sxs-lookup"><span data-stu-id="c0091-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="c0091-135">Klikněte pravým tlačítkem na návrhové ploše a vyberte **přidat nový -&gt; Entity...**</span><span class="sxs-lookup"><span data-stu-id="c0091-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="c0091-136">Zadejte **blogu** jako název entity a **BlogId** název klíče a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="c0091-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Přidání Entity blogu](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="c0091-138">Klikněte pravým tlačítkem na nové entity na návrhové ploše a vyberte **přidat nový -&gt; skalární vlastnost**, zadejte **název** jako název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c0091-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="c0091-139">Opakujte tento postup pro přidání **Url** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c0091-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="c0091-140">Klikněte pravým tlačítkem na **adresy Url** vlastnost na návrhové ploše a vyberte **vlastnosti**, v okně změnit vlastnosti **Nullable** nastavení na **True** 
     *Díky tomu můžeme blogu uložte do databáze bez jeho přiřazení adresy Url*</span><span class="sxs-lookup"><span data-stu-id="c0091-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="c0091-141">Pomocí technik právě dozvědí, přidejte **příspěvek** entita s **PostId** klíčové vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c0091-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="c0091-142">Přidat **Title** a **obsahu** Skalární vlastnosti, které chcete **příspěvek** entity</span><span class="sxs-lookup"><span data-stu-id="c0091-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="c0091-143">Když teď máme několik entit, je čas Přidat přidružení (nebo vztah) mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="c0091-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="c0091-144">Klikněte pravým tlačítkem na návrhové ploše a vyberte **přidat nový -&gt; přidružení...**</span><span class="sxs-lookup"><span data-stu-id="c0091-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="c0091-145">Ujistěte se, jeden konec relace, přejděte na **blogu** s násobnost **jeden** a koncový bod pro **příspěvek** s násobnost **mnoho** 
     *To znamená, že má mnoho příspěvky blogu a příspěvek patří do jedné blogu*</span><span class="sxs-lookup"><span data-stu-id="c0091-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="c0091-146">Zkontrolujte **přidat vlastnosti cizího klíče na "Post" entitu** políčko je zaškrtnuté políčko a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="c0091-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Přidat přidružení MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="c0091-148">Nyní je k dispozici jednoduchý model, který jsme Generovat z databáze a umožňuje číst a zapisovat data.</span><span class="sxs-lookup"><span data-stu-id="c0091-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Počáteční modelu](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="c0091-150">Další kroky v sadě Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="c0091-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="c0091-151">Pokud pracujete v sadě Visual Studio 2010 existují některé další kroky, které je potřeba provést upgrade na nejnovější verzi rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0091-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="c0091-152">Upgrade je důležité, protože poskytuje přístup k vylepšení plochy rozhraní API, který je jednodušší použít, a také nejnovější opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="c0091-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="c0091-153">Nejprve, potřebujeme načíst nejnovější verzi rozhraní Entity Framework z NuGet.</span><span class="sxs-lookup"><span data-stu-id="c0091-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="c0091-154">\*\*Project –&gt; spravovat balíčky NuGet... \*\* 
     \*Pokud nemáte k dispozici \**spravovat balíčky NuGet... \*\* možnost byste měli nainstalovat [nejnovější verze Nugetu](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="c0091-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="c0091-155">Vyberte **Online** kartu</span><span class="sxs-lookup"><span data-stu-id="c0091-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="c0091-156">Vyberte **EntityFramework** balíčku</span><span class="sxs-lookup"><span data-stu-id="c0091-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="c0091-157">Klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="c0091-157">Click **Install**</span></span>

<span data-ttu-id="c0091-158">Dále musíme Prohodit náš model se vygenerovat kód, který využívá rozhraní API DbContext, která byla zavedena v novějších verzích rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0091-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="c0091-159">Klikněte pravým tlačítkem na prázdné místo modelu v EF designeru a vyberte **přidat položku generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="c0091-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="c0091-160">Vyberte **Online šablon** v levé nabídce a vyhledejte **DbContext**</span><span class="sxs-lookup"><span data-stu-id="c0091-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="c0091-161">Vyberte EF **5.x DbContext generátor pro jazyk C\#**, zadejte **BloggingModel** jako název a klikněte na **přidat**</span><span class="sxs-lookup"><span data-stu-id="c0091-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Šablona DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="c0091-163">3. Generování databáze</span><span class="sxs-lookup"><span data-stu-id="c0091-163">3. Generating the Database</span></span>

<span data-ttu-id="c0091-164">Zadaný náš model, Entity Framework schéma databáze, které vám umožní nám k ukládání a načítání dat pomocí modelu vypočítat.</span><span class="sxs-lookup"><span data-stu-id="c0091-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="c0091-165">Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="c0091-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="c0091-166">Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="c0091-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="c0091-167">Pokud používáte sadu Visual Studio 2012 pak vytvoříte [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) databáze.</span><span class="sxs-lookup"><span data-stu-id="c0091-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="c0091-168">Pojďme tedy vygenerovala databáze.</span><span class="sxs-lookup"><span data-stu-id="c0091-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="c0091-169">Klikněte pravým tlačítkem na návrhové ploše a vyberte **generovat databáze z modelu...**</span><span class="sxs-lookup"><span data-stu-id="c0091-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="c0091-170">Klikněte na tlačítko **nové připojení...**</span><span class="sxs-lookup"><span data-stu-id="c0091-170">Click **New Connection…**</span></span> <span data-ttu-id="c0091-171">a určete LocalDB nebo SQL Express, v závislosti na tom, kterou verzi sady Visual Studio používáte, zadejte **ModelFirst.Blogging** jako název databáze.</span><span class="sxs-lookup"><span data-stu-id="c0091-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Připojení LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Připojení SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="c0091-174">Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**</span><span class="sxs-lookup"><span data-stu-id="c0091-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="c0091-175">Vyberte **Další** a Entity Framework Designer vypočítá skript k vytvoření schématu databáze</span><span class="sxs-lookup"><span data-stu-id="c0091-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="c0091-176">Jakmile skript se zobrazí, klikněte na tlačítko **Dokončit** a budou přidány do projektu a otevřít skript</span><span class="sxs-lookup"><span data-stu-id="c0091-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="c0091-177">Klikněte pravým tlačítkem na skript a vyberte **Execute**, zobrazí výzva k zadání databáze připojit, zadejte LocalDB nebo SQL Server Express, v závislosti na tom, kterou verzi sady Visual Studio používáte</span><span class="sxs-lookup"><span data-stu-id="c0091-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="c0091-178">4. Čtení a zápis dat</span><span class="sxs-lookup"><span data-stu-id="c0091-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="c0091-179">Když teď máme modelu je čas ho používat pro přístup k nějaká data.</span><span class="sxs-lookup"><span data-stu-id="c0091-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="c0091-180">Třídy budeme používat k přístupu k datům se automaticky generují založeného na souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="c0091-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="c0091-181">*Tento snímek obrazovky je z Visual Studio 2012, pokud používáte Visual Studio 2010 BloggingModel.tt a BloggingModel.Context.tt soubory přímo v rámci projektu budou místo vnořen v souladu s souboru EDMX.*</span><span class="sxs-lookup"><span data-stu-id="c0091-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Generované třídy](~/ef6/media/generatedclasses.png)

<span data-ttu-id="c0091-183">V souboru Program.cs implementujte metodu Main, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="c0091-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="c0091-184">Tento kód vytvoří novou instanci třídy náš kontext a použije ho k vložení nového blogu.</span><span class="sxs-lookup"><span data-stu-id="c0091-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="c0091-185">Pak používá dotaz LINQ k načtení všech blogy z databáze abecedně podle názvu.</span><span class="sxs-lookup"><span data-stu-id="c0091-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="c0091-186">Teď můžete aplikaci spustit a otestování.</span><span class="sxs-lookup"><span data-stu-id="c0091-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="c0091-187">5. Práce se změnami modelu</span><span class="sxs-lookup"><span data-stu-id="c0091-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="c0091-188">Nyní je čas na něm provést nějaké změny náš model, když jsme provedení těchto změn je také potřeba aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="c0091-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="c0091-189">Začneme tak, že přidáte nové entity uživatele pro náš model.</span><span class="sxs-lookup"><span data-stu-id="c0091-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="c0091-190">Přidat nový **uživatele** název entity s **uživatelské jméno** jako název klíče a **řetězec** jako typ vlastnosti klíče</span><span class="sxs-lookup"><span data-stu-id="c0091-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Přidat entitu uživatele](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="c0091-192">Klikněte pravým tlačítkem na **uživatelské jméno** vlastnost na návrhové ploše a vyberte **vlastnosti**, změnit ve vlastnostech okna **MaxLength** nastavení \*\*50 \*\* 
     *To omezuje data ukládaná ve službě uživatelské jméno do 50 znaků.*</span><span class="sxs-lookup"><span data-stu-id="c0091-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="c0091-193">Přidat **DisplayName** skalární vlastnost **uživatele** entity</span><span class="sxs-lookup"><span data-stu-id="c0091-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="c0091-194">Teď máme aktualizovaného modelu a jsme připraveni na aktualizaci databáze tak, aby vyhovovaly náš nový typ entity uživatele.</span><span class="sxs-lookup"><span data-stu-id="c0091-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="c0091-195">Klikněte pravým tlačítkem na návrhové ploše a vyberte **generovat databáze z modelu...** , Vypočítá skriptu znovu vytvořit schéma založené na aktualizovaného modelu entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0091-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="c0091-196">Klikněte na tlačítko **dokončit**</span><span class="sxs-lookup"><span data-stu-id="c0091-196">Click **Finish**</span></span>
-   <span data-ttu-id="c0091-197">Zobrazí se upozornění o přepsání existujícího skriptu jazyka DDL a části mapování a úložiště modelu, klikněte na tlačítko **Ano** pro obě tato upozornění</span><span class="sxs-lookup"><span data-stu-id="c0091-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="c0091-198">Aktualizovaný skript k vytvoření databáze SQL je pro vás otevřen.</span><span class="sxs-lookup"><span data-stu-id="c0091-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="c0091-199">*Skript, který je generován vyřadit všechny existující tabulky a pak znovu vytvořte schéma úplně od začátku. To může fungovat pro místní vývoj, ale není přijatelné pro doručením (push) změny do databáze, která již byla nasazena. Pokud potřebujete a publikujte změny do databáze, která již byla nasazena, je potřeba upravit skript nebo použijte nástroj pro porovnání schématu pro výpočet skript migrace.*</span><span class="sxs-lookup"><span data-stu-id="c0091-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="c0091-200">Klikněte pravým tlačítkem na skript a vyberte **Execute**, zobrazí výzva k zadání databáze připojit, zadejte LocalDB nebo SQL Server Express, v závislosti na tom, kterou verzi sady Visual Studio používáte</span><span class="sxs-lookup"><span data-stu-id="c0091-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="c0091-201">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c0091-201">Summary</span></span>

<span data-ttu-id="c0091-202">V tomto návodu zvažovali jsme i první Model vývoje, který nám umožnila vytvoření modelu v EF designeru a potom generovat databáze z tohoto modelu.</span><span class="sxs-lookup"><span data-stu-id="c0091-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="c0091-203">Potom jsme použili model číst a zapisovat data z databáze.</span><span class="sxs-lookup"><span data-stu-id="c0091-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="c0091-204">Nakonec se aktualizace modelu a poté znovu vytvořit schéma databáze tak, aby odpovídala modelu.</span><span class="sxs-lookup"><span data-stu-id="c0091-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
