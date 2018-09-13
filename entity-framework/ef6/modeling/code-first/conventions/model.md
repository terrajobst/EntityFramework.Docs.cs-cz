---
title: Založené na modelu konvencí - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: fb79164f71cb3afff705a83f5078a13d043abca8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490932"
---
# <a name="model-based-conventions"></a><span data-ttu-id="a2a35-102">Vytváření názvů založených na modelu</span><span class="sxs-lookup"><span data-stu-id="a2a35-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="a2a35-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a2a35-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="a2a35-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="a2a35-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="a2a35-105">Model na základě konvence jsou rozšířený způsob konfigurace založené na konvenci modelu.</span><span class="sxs-lookup"><span data-stu-id="a2a35-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="a2a35-106">Pro většinu scénářů [vlastní kód prvního konvence rozhraní API na DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="a2a35-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="a2a35-107">Znalost DbModelBuilder rozhraní API pro vytváření názvů se doporučuje před použitím modelu na základě konvence.</span><span class="sxs-lookup"><span data-stu-id="a2a35-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="a2a35-108">Model na základě konvencí povolit vytvoření vliv na vlastnosti a tabulky, která nejdou konfigurovat pomocí standardní konvence vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="a2a35-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="a2a35-109">Tyto příklady diskriminátoru sloupců v tabulce na hierarchii modely a nezávislé přidružení sloupce.</span><span class="sxs-lookup"><span data-stu-id="a2a35-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="a2a35-110">Vytváření konvence</span><span class="sxs-lookup"><span data-stu-id="a2a35-110">Creating a Convention</span></span>   

<span data-ttu-id="a2a35-111">Prvním krokem při vytváření modelu na základě konvence je výběr, když v kanálu úmluvy musí být použité pro model.</span><span class="sxs-lookup"><span data-stu-id="a2a35-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="a2a35-112">Existují dva typy modelu konvencí, konceptuální (C + MEZERNÍK) a Store (S-místa).</span><span class="sxs-lookup"><span data-stu-id="a2a35-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="a2a35-113">Místo C konvence je použité pro model, který aplikace sestavena, že vytváření názvů S místo platí pro verze modelu, který představuje databázi a ovládací prvky věcí, například jak automaticky generované sloupce mají název.</span><span class="sxs-lookup"><span data-stu-id="a2a35-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="a2a35-114">Vytváření modelu je třída, která rozšiřuje IConceptualModelConvention nebo IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="a2a35-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="a2a35-115">Tato rozhraní, obě přijmout obecný typ, který může být zadejte MetadataItem, který se používá k filtrování podle úmluvy platí pro datového typu.</span><span class="sxs-lookup"><span data-stu-id="a2a35-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="a2a35-116">Přidání konvence</span><span class="sxs-lookup"><span data-stu-id="a2a35-116">Adding a Convention</span></span>   

<span data-ttu-id="a2a35-117">Stejným způsobem jako pravidelné vytváření třídy se přidají modelu konvencí.</span><span class="sxs-lookup"><span data-stu-id="a2a35-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="a2a35-118">V **OnModelCreating** metodu, přidejte tato konvence do seznamu konvence pro model.</span><span class="sxs-lookup"><span data-stu-id="a2a35-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="a2a35-119">Konvence lze také přidat ve vztahu k jiné konvence pomocí Conventions.AddBefore\< \> nebo Conventions.AddAfter\< \> metody.</span><span class="sxs-lookup"><span data-stu-id="a2a35-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="a2a35-120">Další informace o konvencích, pro které platí Entity Framework naleznete v části poznámky.</span><span class="sxs-lookup"><span data-stu-id="a2a35-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="a2a35-121">Příklad: Diskriminátoru modelu konvence</span><span class="sxs-lookup"><span data-stu-id="a2a35-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="a2a35-122">Přejmenování sloupců generovaných EF je příkladem něco, co není možné provádět pomocí jiné konvence rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a2a35-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="a2a35-123">To je naopak situace, kde je pomocí modelu konvencí je vaší jedinou možností.</span><span class="sxs-lookup"><span data-stu-id="a2a35-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="a2a35-124">Příklad toho, jak použít model podle úmluvy ke konfiguraci generované sloupce přizpůsobuje způsob, jakým jsou pojmenované sloupce diskriminátoru.</span><span class="sxs-lookup"><span data-stu-id="a2a35-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="a2a35-125">Níže je příklad jednoduchého modelu na základě úmluvy, který se přejmenuje každý sloupec v modelu s názvem "Diskriminátoru" na "EntityType".</span><span class="sxs-lookup"><span data-stu-id="a2a35-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="a2a35-126">To zahrnuje sloupce, že vývojář jednoduše pojmenované "Diskriminátoru".</span><span class="sxs-lookup"><span data-stu-id="a2a35-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="a2a35-127">Protože generovaný sloupec je sloupec "Diskriminátoru" to je potřeba spustit v prostoru S.</span><span class="sxs-lookup"><span data-stu-id="a2a35-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="a2a35-128">Příklad: Obecné IA přejmenování konvence</span><span class="sxs-lookup"><span data-stu-id="a2a35-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="a2a35-129">Další složitější příklad modelu na základě konvencí v akci, je nakonfigurovat tak, že jsou názvy nezávislé přidružení (IAs).</span><span class="sxs-lookup"><span data-stu-id="a2a35-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="a2a35-130">Toto je případě, že se dají použít, protože služba ověřování v Internetu jsou generovány pomocí EF a nejsou k dispozici v modelu, s přístupem k rozhraní API DbModelBuilder modelu konvencí.</span><span class="sxs-lookup"><span data-stu-id="a2a35-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="a2a35-131">Když EF generuje IA, vytvoří se sloupec s názvem EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="a2a35-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="a2a35-132">Například pro přidružení s názvem zákazníků s klíčový sloupec s názvem CustomerId vygeneruje sloupec s názvem Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="a2a35-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="a2a35-133">Následující pásky konvence "\_" znak mimo název sloupce, který je generován pro i a.</span><span class="sxs-lookup"><span data-stu-id="a2a35-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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
        for (int i = 0; i \< properties.Count; ++i)
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

## <a name="extending-existing-conventions"></a><span data-ttu-id="a2a35-134">Rozšíření stávající konvence</span><span class="sxs-lookup"><span data-stu-id="a2a35-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="a2a35-135">Pokud potřebujete napsat konvence, která se podobá konvence Entity Framework již platí pro váš model je vždy rozšířit této konvenci, abyste ho nemuseli znovu od začátku jeho přepsání.</span><span class="sxs-lookup"><span data-stu-id="a2a35-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="a2a35-136">Příkladem je nahraďte stávající Id odpovídající konvenci s vlastní.</span><span class="sxs-lookup"><span data-stu-id="a2a35-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="a2a35-137">Dodatečná výhoda pro přepsání klíče konvence je, že bude zavolána přepsané metody pouze v případě, že neexistuje žádný klíč nerozpoznal nebo explicitně nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="a2a35-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="a2a35-138">Seznam konvence, která používá Entity Framework je k dispozici tady: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2a35-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="a2a35-139">Pak potřebujeme přidat naše nová konvence před existující klíče konvence.</span><span class="sxs-lookup"><span data-stu-id="a2a35-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="a2a35-140">Po přidání CustomKeyDiscoveryConvention, můžeme odebrat IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="a2a35-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="a2a35-141">Pokud jsme neměli odebrat existující IdKeyDiscoveryConvention Tato konvence by stále přednost konvenci zjišťování Id vzhledem k tomu, že je spuštěn jako první, ale v případě, pokud je nalezena žádná vlastnost "klíče", "id" konvence poběží.</span><span class="sxs-lookup"><span data-stu-id="a2a35-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="a2a35-142">Toto chování vidíme, protože každý konvence vidí modelu, která jsou aktualizovány předchozí konvence (nikoli na provoz na něm nezávisle a všechny je zkopírovat dohromady) tak, aby Pokud například předchozí konvence aktualizovat název sloupce tak, aby odpovídaly něco z zajímavé pro váš vlastní konvence (pokud dříve nebyla název zájmu) pak bude platit pro tento sloupec.</span><span class="sxs-lookup"><span data-stu-id="a2a35-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="a2a35-143">Poznámky</span><span class="sxs-lookup"><span data-stu-id="a2a35-143">Notes</span></span>  

<span data-ttu-id="a2a35-144">Seznam smluv, které jsou aktuálně použité rozhraním Entity Framework je k dispozici v dokumentaci MSDN zde: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2a35-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="a2a35-145">Tento seznam se načítají přímo z našeho zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="a2a35-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="a2a35-146">Zdrojový kód pro Entity Framework 6 je k dispozici na [Githubu](https://github.com/aspnet/entityframework6/) a řadu konvencemi použitými rozhraním Entity Framework jsou dobré počáteční body pro vlastního modelu na základě konvence.</span><span class="sxs-lookup"><span data-stu-id="a2a35-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
