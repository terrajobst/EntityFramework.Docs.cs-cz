---
title: Upgrade na Entity Framework 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 4395a9c117a6cf38e7fc08f11ee689d6fffa6fed
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419652"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="4fc83-102">Upgrade na Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4fc83-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="4fc83-103">V předchozích verzích EF byl kód rozdělen mezi základní knihovny (primárně System. data. entity. dll) dodávané jako součást .NET Framework a mimo jiné knihovny OOB (primárně EntityFramework. dll) dodávané do balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="4fc83-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="4fc83-104">EF6 přebírá kód ze základních knihoven a zahrnuje ho do knihoven OOB.</span><span class="sxs-lookup"><span data-stu-id="4fc83-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="4fc83-105">To bylo nutné, aby bylo možné, že EF má být otevřen open source a v případě, že bude moci vyvíjet jiný tempo od .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4fc83-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="4fc83-106">Důvodem je, že aplikace bude nutné znovu sestavit proti přesunutým typům.</span><span class="sxs-lookup"><span data-stu-id="4fc83-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="4fc83-107">To by mělo být jednoduché pro aplikace, které využívají DbContext jako dodávané v EF 4,1 a novějších.</span><span class="sxs-lookup"><span data-stu-id="4fc83-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="4fc83-108">Pro aplikace, které využívají ObjectContext, se vyžaduje trochu více práce, ale stále není těžké.</span><span class="sxs-lookup"><span data-stu-id="4fc83-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="4fc83-109">Tady je kontrolní seznam věcí, které je třeba provést při upgradu existující aplikace na EF6.</span><span class="sxs-lookup"><span data-stu-id="4fc83-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="4fc83-110">1. Nainstalujte balíček NuGet EF6.</span><span class="sxs-lookup"><span data-stu-id="4fc83-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="4fc83-111">Musíte upgradovat na nový modul runtime Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="4fc83-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="4fc83-112">Klikněte pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="4fc83-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="4fc83-113">Na kartě **online** vyberte **EntityFramework** a klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="4fc83-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="4fc83-114">Pokud byla nainstalována předchozí verze balíčku NuGet EntityFramework, bude upgradována na EF6.</span><span class="sxs-lookup"><span data-stu-id="4fc83-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="4fc83-115">Případně můžete z konzoly Správce balíčků spustit následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4fc83-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="4fc83-116">2. Ujistěte se, že jsou odebrány odkazy na sestavení System. data. entity. dll.</span><span class="sxs-lookup"><span data-stu-id="4fc83-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="4fc83-117">Instalace balíčku NuGet EF6 by měla automaticky odebrat všechny odkazy na System. data. entity z vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="4fc83-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="4fc83-118">3. Proměňte všechny modely EF Designer (EDMX), aby bylo možné použít kód EF 6. x.</span><span class="sxs-lookup"><span data-stu-id="4fc83-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="4fc83-119">Pokud máte v Návrháři EF vytvořené nějaké modely, budete muset aktualizovat šablony pro generování kódu, aby se generoval kód kompatibilní s EF6.</span><span class="sxs-lookup"><span data-stu-id="4fc83-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="4fc83-120">V současné době jsou k dispozici pouze šablony generátoru DbContext v EF 6. x pro Visual Studio 2012 a 2013.</span><span class="sxs-lookup"><span data-stu-id="4fc83-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="4fc83-121">Odstranit existující šablony pro generování kódu.</span><span class="sxs-lookup"><span data-stu-id="4fc83-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="4fc83-122">Tyto soubory budou obvykle pojmenovány **\<edmx_file_name\>. TT** a **\<edmx_file_name\>. Context.tt** a vnořovat do souboru EDMX v Průzkumník řešení.</span><span class="sxs-lookup"><span data-stu-id="4fc83-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="4fc83-123">Můžete vybrat šablony v Průzkumník řešení a stisknout klávesu **del** a odstranit je.</span><span class="sxs-lookup"><span data-stu-id="4fc83-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="4fc83-124">V projektech webů nebudou šablony vnořené do souboru EDMX, ale jsou uvedeny vedle sebe v Průzkumník řešení.</span><span class="sxs-lookup"><span data-stu-id="4fc83-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="4fc83-125">V projektech VB.NET budete muset povolit možnost Zobrazit všechny soubory, aby bylo možné zobrazit vnořené soubory šablon.</span><span class="sxs-lookup"><span data-stu-id="4fc83-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="4fc83-126">Přidejte příslušnou šablonu pro generování kódu EF 6. x.</span><span class="sxs-lookup"><span data-stu-id="4fc83-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="4fc83-127">Otevřete model v Návrháři EF, klikněte pravým tlačítkem na návrhovou plochu a vyberte **Přidat položku pro generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="4fc83-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="4fc83-128">Pokud používáte rozhraní API DbContext (doporučeno), bude na kartě **data** k dispozici **generátor EF 6. x DbContext** .</span><span class="sxs-lookup"><span data-stu-id="4fc83-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="4fc83-129">Pokud používáte Visual Studio 2012, budete muset nainstalovat nástroje EF 6, abyste měli tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="4fc83-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="4fc83-130">Podrobnosti najdete v tématu [získání Entity Framework](~/ef6/fundamentals/install.md) .</span><span class="sxs-lookup"><span data-stu-id="4fc83-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="4fc83-131">Pokud používáte rozhraní API ObjectContext, budete muset vybrat **online** kartu a vyhledat **generátor EF 6. x objektů EntityObject**.</span><span class="sxs-lookup"><span data-stu-id="4fc83-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="4fc83-132">Pokud jste v šablonách generování kódu použili jakékoli úpravy, budete je muset znovu použít na aktualizované šablony.</span><span class="sxs-lookup"><span data-stu-id="4fc83-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="4fc83-133">4. aktualizujte obory názvů pro všechny používané typy EF Core.</span><span class="sxs-lookup"><span data-stu-id="4fc83-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="4fc83-134">Obory názvů pro typy DbContext a Code First se nezměnily.</span><span class="sxs-lookup"><span data-stu-id="4fc83-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="4fc83-135">To znamená, že pro mnoho aplikací, které používají EF 4,1 nebo novější, nebudete muset cokoli měnit.</span><span class="sxs-lookup"><span data-stu-id="4fc83-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="4fc83-136">Typy, jako ObjectContext dříve v System. data. entity. dll byly přesunuty na nové obory názvů.</span><span class="sxs-lookup"><span data-stu-id="4fc83-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="4fc83-137">To znamená, že možná budete muset aktualizovat direktivy *using* nebo *Import* pro sestavení pro EF6.</span><span class="sxs-lookup"><span data-stu-id="4fc83-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="4fc83-138">Obecné pravidlo pro změny oboru názvů je, že jakýkoliv typ v System. data. \* je přesunut do System. data. entity. Core. \*.</span><span class="sxs-lookup"><span data-stu-id="4fc83-138">The general rule for namespace changes is that any type in System.Data.\* is moved to System.Data.Entity.Core.\*.</span></span> <span data-ttu-id="4fc83-139">Jinými slovy, stačí vložit **entity. Core.**</span><span class="sxs-lookup"><span data-stu-id="4fc83-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="4fc83-140">po System. data.</span><span class="sxs-lookup"><span data-stu-id="4fc83-140">after System.Data.</span></span> <span data-ttu-id="4fc83-141">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4fc83-141">For example:</span></span>

- <span data-ttu-id="4fc83-142">System. data. EntityException = > System. data. **Entita. Core** EntityException</span><span class="sxs-lookup"><span data-stu-id="4fc83-142">System.Data.EntityException => System.Data.**Entity.Core**.EntityException</span></span>  
- <span data-ttu-id="4fc83-143">System. data. Objects. ObjectContext = > System. data. **Entita. Core** Objects. ObjectContext</span><span class="sxs-lookup"><span data-stu-id="4fc83-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core**.Objects.ObjectContext</span></span>  
- <span data-ttu-id="4fc83-144">System. data. Objects. DataClasses. RelationshipManager = > System. data. **Entita. Core** Objects. DataClasses. RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="4fc83-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core**.Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="4fc83-145">Tyto typy jsou v *základních* oborech názvů, protože nejsou používány přímo pro většinu aplikací založených na DbContext.</span><span class="sxs-lookup"><span data-stu-id="4fc83-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="4fc83-146">Některé typy, které byly součástí System. data. entity. dll, jsou stále používány často a přímo pro aplikace založené na DbContext, a proto nebyly přesunuty do *základních* oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="4fc83-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="4fc83-147">Jsou to:</span><span class="sxs-lookup"><span data-stu-id="4fc83-147">These are:</span></span>

- <span data-ttu-id="4fc83-148">System. data. EntityState = > System. data. **Entita**. EntityState</span><span class="sxs-lookup"><span data-stu-id="4fc83-148">System.Data.EntityState => System.Data.**Entity**.EntityState</span></span>  
- <span data-ttu-id="4fc83-149">System. data. Objects. DataClasses. EdmFunctionAttribute = > System. data. **Entita. DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="4fc83-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="4fc83-150">Tato třída byla přejmenována; Třída se starým názvem stále existuje a funguje, ale nyní je označena jako zastaralá.</span><span class="sxs-lookup"><span data-stu-id="4fc83-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="4fc83-151">System. data. Objects. EntityFunctions = > System. data. **Entita. DbFunctions**</span><span class="sxs-lookup"><span data-stu-id="4fc83-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="4fc83-152">Tato třída byla přejmenována; Třída se starým názvem stále existuje a funguje, ale nyní je označena jako zastaralá.)</span><span class="sxs-lookup"><span data-stu-id="4fc83-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="4fc83-153">Prostorové třídy (například DbGeography, DbGeometry) se přesunuly z System. data. prostor = > System. data. **Entita**. Klepání</span><span class="sxs-lookup"><span data-stu-id="4fc83-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity**.Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="4fc83-154">Některé typy v oboru názvů System. data jsou v System. data. dll, což není sestavení EF.</span><span class="sxs-lookup"><span data-stu-id="4fc83-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="4fc83-155">Tyto typy se nepřesunuly, takže jejich obory názvů zůstanou beze změny.</span><span class="sxs-lookup"><span data-stu-id="4fc83-155">These types have not moved and so their namespaces remain unchanged.</span></span>
