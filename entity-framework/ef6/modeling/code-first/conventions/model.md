---
title: Konvence založené na modelu – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419169"
---
# <a name="model-based-conventions"></a><span data-ttu-id="58b5e-102">Konvence založené na modelu</span><span class="sxs-lookup"><span data-stu-id="58b5e-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="58b5e-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="58b5e-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="58b5e-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="58b5e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="58b5e-105">Konvence založené na modelu jsou pokročilou metodou konfigurace modelu založenou na konvencích.</span><span class="sxs-lookup"><span data-stu-id="58b5e-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="58b5e-106">Pro většinu scénářů by se měla použít [rozhraní API pro vlastní konvence Code First v DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) .</span><span class="sxs-lookup"><span data-stu-id="58b5e-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="58b5e-107">Před použitím konvencí založených na modelech se doporučuje pochopit rozhraní DbModelBuilder API pro konvence.</span><span class="sxs-lookup"><span data-stu-id="58b5e-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="58b5e-108">Konvence založené na modelu umožňují vytvářet konvence, které ovlivňují vlastnosti a tabulky, které se nedají konfigurovat pomocí standardních konvencí.</span><span class="sxs-lookup"><span data-stu-id="58b5e-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="58b5e-109">Příklady jsou sloupce diskriminátorů v tabulce pro modely hierarchie a sloupce nezávislého přidružení.</span><span class="sxs-lookup"><span data-stu-id="58b5e-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="58b5e-110">Vytvoření konvence</span><span class="sxs-lookup"><span data-stu-id="58b5e-110">Creating a Convention</span></span>   

<span data-ttu-id="58b5e-111">Prvním krokem při vytváření konvence založené na modelu je výběr, kdy v kanálu je potřeba použít pro model konvenci.</span><span class="sxs-lookup"><span data-stu-id="58b5e-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="58b5e-112">Existují dva typy konvencí modelu, koncepční (C-Space) a úložiště (S-Space).</span><span class="sxs-lookup"><span data-stu-id="58b5e-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="58b5e-113">Konvence v jazyce C je aplikována na model, který aplikace sestaví, zatímco konvence S pro místo je použita na verzi modelu, která představuje databázi, a ovládací prvky, jako je například způsob pojmenování automaticky generovaných sloupců.</span><span class="sxs-lookup"><span data-stu-id="58b5e-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="58b5e-114">Modelová konvence je třída, která rozšiřuje buď IConceptualModelConvention nebo IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="58b5e-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="58b5e-115">Tato rozhraní obě přijímají obecný typ, který může být typu MetadataItem, který slouží k filtrování datového typu, na který se konvence vztahuje.</span><span class="sxs-lookup"><span data-stu-id="58b5e-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="58b5e-116">Přidání konvence</span><span class="sxs-lookup"><span data-stu-id="58b5e-116">Adding a Convention</span></span>   

<span data-ttu-id="58b5e-117">Konvence modelu jsou přidány stejným způsobem jako třídy regulárních konvencí.</span><span class="sxs-lookup"><span data-stu-id="58b5e-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="58b5e-118">V metodě **OnModelCreating** přidejte konvenci do seznamu konvencí pro model.</span><span class="sxs-lookup"><span data-stu-id="58b5e-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

<span data-ttu-id="58b5e-119">Konvenci je také možné přidat ve vztahu k jiné konvenci pomocí konvence. AddBefore\<\> nebo konvence. AddAfter\<\> metody.</span><span class="sxs-lookup"><span data-stu-id="58b5e-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="58b5e-120">Další informace o konvencích, které Entity Framework použít, najdete v části poznámky.</span><span class="sxs-lookup"><span data-stu-id="58b5e-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="58b5e-121">Příklad: konvence modelu diskriminátoru</span><span class="sxs-lookup"><span data-stu-id="58b5e-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="58b5e-122">Přejmenování sloupců generovaných EF je příkladem něčeho, co nemůžete dělat s jinými rozhraními API pro konvence.</span><span class="sxs-lookup"><span data-stu-id="58b5e-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="58b5e-123">Jedná se o situaci, kdy použití konvence modelu je jedinou možností.</span><span class="sxs-lookup"><span data-stu-id="58b5e-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="58b5e-124">Příklad použití konvence založené na modelu ke konfiguraci vygenerovaných sloupců je přizpůsobením způsobu, jakým jsou pojmenovány sloupce diskriminátoru.</span><span class="sxs-lookup"><span data-stu-id="58b5e-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="58b5e-125">Níže je uveden příklad jednoduché konvence založené na modelu, která přejmenuje každý sloupec v modelu s názvem "diskriminátor" na "EntityType".</span><span class="sxs-lookup"><span data-stu-id="58b5e-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="58b5e-126">To zahrnuje sloupce, které vývojář jednoduše nazývá "diskriminátor".</span><span class="sxs-lookup"><span data-stu-id="58b5e-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="58b5e-127">Vzhledem k tomu, že sloupec "diskriminátor" je vygenerovaný sloupec, musí být spuštěn v prostoru S.</span><span class="sxs-lookup"><span data-stu-id="58b5e-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="58b5e-128">Příklad: konvence přejmenování obecné IA</span><span class="sxs-lookup"><span data-stu-id="58b5e-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="58b5e-129">Dalším složitějším příkladem konvencí založených na modelu v akci je Konfigurace způsobu, jakým jsou pojmenovány nezávislé asociace (IAs).</span><span class="sxs-lookup"><span data-stu-id="58b5e-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="58b5e-130">Jedná se o situaci, kdy se používají konvence modelu, protože služba ověřování v Internetu je vygenerovaná v rámci modelu, ke kterému má přístup rozhraní DbModelBuilder API.</span><span class="sxs-lookup"><span data-stu-id="58b5e-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="58b5e-131">Když EF generuje výjimku IA, vytvoří sloupec s názvem EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="58b5e-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="58b5e-132">Například pro přidružení s názvem zákazník se sloupcem klíče s názvem CustomerId by vygeneroval sloupec s názvem Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="58b5e-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="58b5e-133">Následující konvence obchází znak "\_" mimo název sloupce, který je generován pro platformu IA.</span><span class="sxs-lookup"><span data-stu-id="58b5e-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i < properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a><span data-ttu-id="58b5e-134">Rozšíření existujících konvencí</span><span class="sxs-lookup"><span data-stu-id="58b5e-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="58b5e-135">Pokud potřebujete napsat konvenci, která je podobná jedné z konvencí, kterou Entity Framework už u vašeho modelu použít, můžete tuto úmluvu vždycky rozšíříte, abyste se vyhnuli nutnosti jejich přepisu od začátku.</span><span class="sxs-lookup"><span data-stu-id="58b5e-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="58b5e-136">Příkladem je nahrazení existující konvence pro porovnání ID vlastním.</span><span class="sxs-lookup"><span data-stu-id="58b5e-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="58b5e-137">Přidaná výhoda pro přepsání klíčové konvence je, že přepsaná metoda se volá jenom v případě, že už není zjištěný ani explicitně nakonfigurovaný klíč.</span><span class="sxs-lookup"><span data-stu-id="58b5e-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="58b5e-138">Seznam konvencí, které používá Entity Framework, je k dispozici zde: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="58b5e-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

<span data-ttu-id="58b5e-139">Pak musíme přidat naši novou konvenci před existující klíčovou smlouvou.</span><span class="sxs-lookup"><span data-stu-id="58b5e-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="58b5e-140">Až přidáme CustomKeyDiscoveryConvention, můžeme IdKeyDiscoveryConvention odebrat.</span><span class="sxs-lookup"><span data-stu-id="58b5e-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="58b5e-141">Pokud jsme neodebrali stávající IdKeyDiscoveryConvention, Tato konvence by stále měla přednost před konvencí zjišťování ID, protože se spustí jako první, ale v případě, že se nenajde žádná vlastnost Key, se spustí konvence "ID".</span><span class="sxs-lookup"><span data-stu-id="58b5e-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="58b5e-142">Toto chování se zobrazuje, protože každá konvence prochází model aktualizovaný předchozí konvencí (místo toho, aby se nezávisle nepracoval), takže pokud například předchozí konvence aktualizovala název sloupce tak, aby odpovídala nějakému z Zajímá vás vaše vlastní konvence (Pokud před tím, než je název nezajímavá), použije se pro tento sloupec.</span><span class="sxs-lookup"><span data-stu-id="58b5e-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a><span data-ttu-id="58b5e-143">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="58b5e-143">Notes</span></span>  

<span data-ttu-id="58b5e-144">Seznam konvencí, které aktuálně používá Entity Framework, je k dispozici v dokumentaci MSDN: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="58b5e-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="58b5e-145">Tento seznam je načítán přímo z našeho zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="58b5e-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="58b5e-146">Zdrojový kód pro Entity Framework 6 je k dispozici na [GitHubu](https://github.com/aspnet/entityframework6/) a mnohé z konvencí používaných v Entity Framework jsou dobré počáteční body pro vlastní konvence založené na modelu.</span><span class="sxs-lookup"><span data-stu-id="58b5e-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
