---
title: Upgrade na rozhraní Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 711f1940080de27bd23cb8f641a5c7f2711dd65b
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182004"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="4d893-102">Upgrade na rozhraní Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4d893-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="4d893-103">V předchozích verzích EF kód byl rozdělen mezi základní knihovny (primárně knihovně System.Data.Entity.dll) dodán jako součást rozhraní .NET Framework a out-of-band (OOB) knihovny (primárně EntityFramework.dll) součástí balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="4d893-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="4d893-104">EF6 má kód ze základní knihovny a zahrnuje je do knihovny OOB.</span><span class="sxs-lookup"><span data-stu-id="4d893-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="4d893-105">To bylo nezbytné, aby se povolit EF má být provedeno opensourcový a, abyste mohli rozvíjet věci jinak rychle z rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4d893-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="4d893-106">Důsledkem tohoto je, že aplikace bude nutné znovu sestavit na jakou přesunutý.</span><span class="sxs-lookup"><span data-stu-id="4d893-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="4d893-107">To by měl být jednoduché pro aplikace, které využívají DbContext jako dodané v EF 4.1 a novějším.</span><span class="sxs-lookup"><span data-stu-id="4d893-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="4d893-108">O něco více práce je nutná pro aplikace, které využívají ObjectContext, ale stále není těžko provádět.</span><span class="sxs-lookup"><span data-stu-id="4d893-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="4d893-109">Tady je kontrolní seznam toho, co je třeba provést upgrade existující aplikaci do EF6.</span><span class="sxs-lookup"><span data-stu-id="4d893-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="4d893-110">1. Nainstalujte balíček EF6 NuGet</span><span class="sxs-lookup"><span data-stu-id="4d893-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="4d893-111">Budete muset upgradovat na nový modul runtime Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="4d893-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="4d893-112">Klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="4d893-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="4d893-113">V části **Online** kartě vyberte **EntityFramework** a klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="4d893-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="4d893-114">Pokud předchozí verze EntityFramework NuGet byl nainstalován balíček to bude ho upgradovat na EF6.</span><span class="sxs-lookup"><span data-stu-id="4d893-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="4d893-115">Alternativně můžete spustit následující příkaz z konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="4d893-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="4d893-116">2. Ujistěte se, že jsou odebrány odkazy na knihovně System.Data.Entity.dll sestavení</span><span class="sxs-lookup"><span data-stu-id="4d893-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="4d893-117">Instaluje se balíček EF6 NuGet by měl automaticky odebrání veškerých odkazů na System.Data.Entity z projektu za vás.</span><span class="sxs-lookup"><span data-stu-id="4d893-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="4d893-118">3. Zaměnit žádné modely EF návrháře (EDMX) Chcete-li použít generování kódu 6.x EF</span><span class="sxs-lookup"><span data-stu-id="4d893-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="4d893-119">Pokud máte jakékoli modelů vytvořených pomocí EF designeru, je potřeba aktualizovat šablony generování kódu pro generování kódu EF6 kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="4d893-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="4d893-120">Aktuálně nejsou k dispozici pro sadu Visual Studio 2012 a 2013 pouze EF 6.x DbContext generátor šablony.</span><span class="sxs-lookup"><span data-stu-id="4d893-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="4d893-121">Odstraňte existující šablony generování kódu.</span><span class="sxs-lookup"><span data-stu-id="4d893-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="4d893-122">Tyto soubory budou mít názvy obvykle  **\<edmx_file_name\>.tt** a  **\<edmx_file_name\>. Context.tt** a vnořit souboru edmx, v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="4d893-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="4d893-123">Můžete vybrat šablony v Průzkumníku řešení a stiskněte klávesu **Del** klíč k jejich odstranění.</span><span class="sxs-lookup"><span data-stu-id="4d893-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="4d893-124">V webových projektů šablony nesmí být vnořen v souladu s souboru edmx, ale uvedené spolu s ho v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="4d893-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="4d893-125">V projektech VB.NET je potřeba povolit "Zobrazit všechny soubory" bude moci zobrazit soubory vnořené šablony.</span><span class="sxs-lookup"><span data-stu-id="4d893-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="4d893-126">Přidejte příslušné šablony EF 6.x pro generování kódu.</span><span class="sxs-lookup"><span data-stu-id="4d893-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="4d893-127">Otevřete váš model v EF designeru klikněte pravým tlačítkem na návrhové ploše a vyberte **přidat položku generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="4d893-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="4d893-128">Pokud používáte rozhraní API DbContext (doporučeno) pak **EF 6.x DbContext generátor** bude k dispozici v části **Data** kartu.</span><span class="sxs-lookup"><span data-stu-id="4d893-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="4d893-129">Pokud používáte sadu Visual Studio 2012, musíte nainstalovat nástroje EF 6 na používají tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="4d893-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="4d893-130">Zobrazit [získat Entity Framework](~/ef6/fundamentals/install.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4d893-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="4d893-131">Pokud používáte rozhraní API pro objekt ObjectContext pak bude nutné vybrat **Online** kartu a vyhledejte **EF 6.x EntityObject generátor**.</span><span class="sxs-lookup"><span data-stu-id="4d893-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="4d893-132">Pokud jste použili všechna vlastní nastavení pro šablony generování kódu, budete muset znovu aplikovat na aktualizované šablony.</span><span class="sxs-lookup"><span data-stu-id="4d893-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="4d893-133">4. Obory názvů aktualizace pro všechny typy EF core používá</span><span class="sxs-lookup"><span data-stu-id="4d893-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="4d893-134">Obory názvů pro typy DbContext a Code First nezměnily.</span><span class="sxs-lookup"><span data-stu-id="4d893-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="4d893-135">To znamená pro mnoho aplikací, které používají EF 4.1 nebo novější nemusíte nic měnit.</span><span class="sxs-lookup"><span data-stu-id="4d893-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="4d893-136">Typy jako objekt ObjectContext, které byly dříve v knihovně System.Data.Entity.dll byly přesunuty do nové obory názvů.</span><span class="sxs-lookup"><span data-stu-id="4d893-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="4d893-137">To znamená, že je potřeba aktualizovat vaše *pomocí* nebo *Import* direktivy pro sestavení EF6.</span><span class="sxs-lookup"><span data-stu-id="4d893-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="4d893-138">Obecným pravidlem pro obor názvů změny je, že jakýkoli typ v System.Data.\* se přesune do System.Data.Entity.Core.\*.</span><span class="sxs-lookup"><span data-stu-id="4d893-138">The general rule for namespace changes is that any type in System.Data.\* is moved to System.Data.Entity.Core.\*.</span></span> <span data-ttu-id="4d893-139">Jinými slovy, stačí vložit **Entity.Core.**</span><span class="sxs-lookup"><span data-stu-id="4d893-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="4d893-140">Po System.Data.</span><span class="sxs-lookup"><span data-stu-id="4d893-140">after System.Data.</span></span> <span data-ttu-id="4d893-141">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4d893-141">For example:</span></span>

- <span data-ttu-id="4d893-142">System.Data.EntityException = > System.Data. **Entity.Core**. EntityException</span><span class="sxs-lookup"><span data-stu-id="4d893-142">System.Data.EntityException => System.Data.**Entity.Core**.EntityException</span></span>  
- <span data-ttu-id="4d893-143">System.Data.Objects.ObjectContext = > System.Data. **Entity.Core**. Objects.ObjectContext</span><span class="sxs-lookup"><span data-stu-id="4d893-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core**.Objects.ObjectContext</span></span>  
- <span data-ttu-id="4d893-144">System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core**. Objects.DataClasses.RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="4d893-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core**.Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="4d893-145">Tyto typy jsou *Core* obory názvů vzhledem k tomu, že nejsou použity přímo pro většinu aplikací na základě DbContext.</span><span class="sxs-lookup"><span data-stu-id="4d893-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="4d893-146">Některé typy, které byly součástí knihovně System.Data.Entity.dll se stále používají často a přímo pro aplikace založené na kontext databáze a dosud byl přesunut do *Core* obory názvů.</span><span class="sxs-lookup"><span data-stu-id="4d893-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="4d893-147">Toto jsou:</span><span class="sxs-lookup"><span data-stu-id="4d893-147">These are:</span></span>

- <span data-ttu-id="4d893-148">System.Data.EntityState = > System.Data. **Entity**. EntityState</span><span class="sxs-lookup"><span data-stu-id="4d893-148">System.Data.EntityState => System.Data.**Entity**.EntityState</span></span>  
- <span data-ttu-id="4d893-149">System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="4d893-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="4d893-150">Tato třída se přejmenoval; Třída s názvem starého stále existuje a funguje, ale je nyní označeny jako zastaralé.</span><span class="sxs-lookup"><span data-stu-id="4d893-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="4d893-151">System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**</span><span class="sxs-lookup"><span data-stu-id="4d893-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="4d893-152">Tato třída se přejmenoval; Třída s názvem starého stále existuje a funguje, ale nyní označeny jako zastaralé.)</span><span class="sxs-lookup"><span data-stu-id="4d893-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="4d893-153">Prostorový třídy (například DbGeography, DbGeometry) byly přesunuty z System.Data.Spatial = > System.Data. **Entity**. Prostorový</span><span class="sxs-lookup"><span data-stu-id="4d893-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity**.Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="4d893-154">Některé typy v oboru názvů System.Data jsou System.Data.dll, který není EF sestavení.</span><span class="sxs-lookup"><span data-stu-id="4d893-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="4d893-155">Tyto typy nepřesunuli a tak zůstanou beze změny jejich obory názvů.</span><span class="sxs-lookup"><span data-stu-id="4d893-155">These types have not moved and so their namespaces remain unchanged.</span></span>
