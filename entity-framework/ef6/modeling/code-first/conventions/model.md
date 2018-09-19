---
title: Založené na modelu konvencí - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: 80b722730b4ca6c9d00a8611b6c9027e8bc9fe61
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283704"
---
# <a name="model-based-conventions"></a>Vytváření názvů založených na modelu
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Model na základě konvence jsou rozšířený způsob konfigurace založené na konvenci modelu. Pro většinu scénářů [vlastní kód prvního konvence rozhraní API na DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) by měla sloužit. Znalost DbModelBuilder rozhraní API pro vytváření názvů se doporučuje před použitím modelu na základě konvence.  

Model na základě konvencí povolit vytvoření vliv na vlastnosti a tabulky, která nejdou konfigurovat pomocí standardní konvence vytváření názvů. Tyto příklady diskriminátoru sloupců v tabulce na hierarchii modely a nezávislé přidružení sloupce.  

## <a name="creating-a-convention"></a>Vytváření konvence   

Prvním krokem při vytváření modelu na základě konvence je výběr, když v kanálu úmluvy musí být použité pro model. Existují dva typy modelu konvencí, konceptuální (C + MEZERNÍK) a Store (S-místa). Místo C konvence je použité pro model, který aplikace sestavena, že vytváření názvů S místo platí pro verze modelu, který představuje databázi a ovládací prvky věcí, například jak automaticky generované sloupce mají název.  

Vytváření modelu je třída, která rozšiřuje IConceptualModelConvention nebo IStoreModelConvention.  Tato rozhraní, obě přijmout obecný typ, který může být zadejte MetadataItem, který se používá k filtrování podle úmluvy platí pro datového typu.  

## <a name="adding-a-convention"></a>Přidání konvence   

Stejným způsobem jako pravidelné vytváření třídy se přidají modelu konvencí. V **OnModelCreating** metodu, přidejte tato konvence do seznamu konvence pro model.  

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

Konvence lze také přidat ve vztahu k jiné konvence pomocí Conventions.AddBefore\< \> nebo Conventions.AddAfter\< \> metody. Další informace o konvencích, pro které platí Entity Framework naleznete v části poznámky.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Příklad: Diskriminátoru modelu konvence  

Přejmenování sloupců generovaných EF je příkladem něco, co není možné provádět pomocí jiné konvence rozhraní API.  To je naopak situace, kde je pomocí modelu konvencí je vaší jedinou možností.  

Příklad toho, jak použít model podle úmluvy ke konfiguraci generované sloupce přizpůsobuje způsob, jakým jsou pojmenované sloupce diskriminátoru.  Níže je příklad jednoduchého modelu na základě úmluvy, který se přejmenuje každý sloupec v modelu s názvem "Diskriminátoru" na "EntityType".  To zahrnuje sloupce, že vývojář jednoduše pojmenované "Diskriminátoru". Protože generovaný sloupec je sloupec "Diskriminátoru" to je potřeba spustit v prostoru S.  

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

## <a name="example-general-ia-renaming-convention"></a>Příklad: Obecné IA přejmenování konvence   

Další složitější příklad modelu na základě konvencí v akci, je nakonfigurovat tak, že jsou názvy nezávislé přidružení (IAs).  Toto je případě, že se dají použít, protože služba ověřování v Internetu jsou generovány pomocí EF a nejsou k dispozici v modelu, s přístupem k rozhraní API DbModelBuilder modelu konvencí.  

Když EF generuje IA, vytvoří se sloupec s názvem EntityType_KeyName. Například pro přidružení s názvem zákazníků s klíčový sloupec s názvem CustomerId vygeneruje sloupec s názvem Customer_CustomerId. Následující pásky konvence "\_" znak mimo název sloupce, který je generován pro i a.  

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

## <a name="extending-existing-conventions"></a>Rozšíření stávající konvence   

Pokud potřebujete napsat konvence, která se podobá konvence Entity Framework již platí pro váš model je vždy rozšířit této konvenci, abyste ho nemuseli znovu od začátku jeho přepsání.  Příkladem je nahraďte stávající Id odpovídající konvenci s vlastní.   Dodatečná výhoda pro přepsání klíče konvence je, že bude zavolána přepsané metody pouze v případě, že neexistuje žádný klíč nerozpoznal nebo explicitně nakonfigurovat. Seznam konvence, která používá Entity Framework je k dispozici tady: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

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

Pak potřebujeme přidat naše nová konvence před existující klíče konvence. Po přidání CustomKeyDiscoveryConvention, můžeme odebrat IdKeyDiscoveryConvention.  Pokud jsme neměli odebrat existující IdKeyDiscoveryConvention Tato konvence by stále přednost konvenci zjišťování Id vzhledem k tomu, že je spuštěn jako první, ale v případě, pokud je nalezena žádná vlastnost "klíče", "id" konvence poběží.  Toto chování vidíme, protože každý konvence vidí modelu, která jsou aktualizovány předchozí konvence (nikoli na provoz na něm nezávisle a všechny je zkopírovat dohromady) tak, aby Pokud například předchozí konvence aktualizovat název sloupce tak, aby odpovídaly něco z zajímavé pro váš vlastní konvence (pokud dříve nebyla název zájmu) pak bude platit pro tento sloupec.  

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

## <a name="notes"></a>Poznámky  

Seznam smluv, které jsou aktuálně použité rozhraním Entity Framework je k dispozici v dokumentaci MSDN zde: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Tento seznam se načítají přímo z našeho zdrojového kódu.  Zdrojový kód pro Entity Framework 6 je k dispozici na [Githubu](https://github.com/aspnet/entityframework6/) a řadu konvencemi použitými rozhraním Entity Framework jsou dobré počáteční body pro vlastního modelu na základě konvence.  
