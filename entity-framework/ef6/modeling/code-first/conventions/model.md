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
# <a name="model-based-conventions"></a>Konvence založené na modelu
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Konvence založené na modelu jsou pokročilou metodou konfigurace modelu založenou na konvencích. Pro většinu scénářů by se měla použít [rozhraní API pro vlastní konvence Code First v DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) . Před použitím konvencí založených na modelech se doporučuje pochopit rozhraní DbModelBuilder API pro konvence.  

Konvence založené na modelu umožňují vytvářet konvence, které ovlivňují vlastnosti a tabulky, které se nedají konfigurovat pomocí standardních konvencí. Příklady jsou sloupce diskriminátorů v tabulce pro modely hierarchie a sloupce nezávislého přidružení.  

## <a name="creating-a-convention"></a>Vytvoření konvence   

Prvním krokem při vytváření konvence založené na modelu je výběr, kdy v kanálu je potřeba použít pro model konvenci. Existují dva typy konvencí modelu, koncepční (C-Space) a úložiště (S-Space). Konvence v jazyce C je aplikována na model, který aplikace sestaví, zatímco konvence S pro místo je použita na verzi modelu, která představuje databázi, a ovládací prvky, jako je například způsob pojmenování automaticky generovaných sloupců.  

Modelová konvence je třída, která rozšiřuje buď IConceptualModelConvention nebo IStoreModelConvention.  Tato rozhraní obě přijímají obecný typ, který může být typu MetadataItem, který slouží k filtrování datového typu, na který se konvence vztahuje.  

## <a name="adding-a-convention"></a>Přidání konvence   

Konvence modelu jsou přidány stejným způsobem jako třídy regulárních konvencí. V metodě **OnModelCreating** přidejte konvenci do seznamu konvencí pro model.  

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

Konvenci je také možné přidat ve vztahu k jiné konvenci pomocí konvence. AddBefore\<\> nebo konvence. AddAfter\<\> metody. Další informace o konvencích, které Entity Framework použít, najdete v části poznámky.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Příklad: konvence modelu diskriminátoru  

Přejmenování sloupců generovaných EF je příkladem něčeho, co nemůžete dělat s jinými rozhraními API pro konvence.  Jedná se o situaci, kdy použití konvence modelu je jedinou možností.  

Příklad použití konvence založené na modelu ke konfiguraci vygenerovaných sloupců je přizpůsobením způsobu, jakým jsou pojmenovány sloupce diskriminátoru.  Níže je uveden příklad jednoduché konvence založené na modelu, která přejmenuje každý sloupec v modelu s názvem "diskriminátor" na "EntityType".  To zahrnuje sloupce, které vývojář jednoduše nazývá "diskriminátor". Vzhledem k tomu, že sloupec "diskriminátor" je vygenerovaný sloupec, musí být spuštěn v prostoru S.  

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

## <a name="example-general-ia-renaming-convention"></a>Příklad: konvence přejmenování obecné IA   

Dalším složitějším příkladem konvencí založených na modelu v akci je Konfigurace způsobu, jakým jsou pojmenovány nezávislé asociace (IAs).  Jedná se o situaci, kdy se používají konvence modelu, protože služba ověřování v Internetu je vygenerovaná v rámci modelu, ke kterému má přístup rozhraní DbModelBuilder API.  

Když EF generuje výjimku IA, vytvoří sloupec s názvem EntityType_KeyName. Například pro přidružení s názvem zákazník se sloupcem klíče s názvem CustomerId by vygeneroval sloupec s názvem Customer_CustomerId. Následující konvence obchází znak "\_" mimo název sloupce, který je generován pro platformu IA.  

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

## <a name="extending-existing-conventions"></a>Rozšíření existujících konvencí   

Pokud potřebujete napsat konvenci, která je podobná jedné z konvencí, kterou Entity Framework už u vašeho modelu použít, můžete tuto úmluvu vždycky rozšíříte, abyste se vyhnuli nutnosti jejich přepisu od začátku.  Příkladem je nahrazení existující konvence pro porovnání ID vlastním.   Přidaná výhoda pro přepsání klíčové konvence je, že přepsaná metoda se volá jenom v případě, že už není zjištěný ani explicitně nakonfigurovaný klíč. Seznam konvencí, které používá Entity Framework, je k dispozici zde: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

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

Pak musíme přidat naši novou konvenci před existující klíčovou smlouvou. Až přidáme CustomKeyDiscoveryConvention, můžeme IdKeyDiscoveryConvention odebrat.  Pokud jsme neodebrali stávající IdKeyDiscoveryConvention, Tato konvence by stále měla přednost před konvencí zjišťování ID, protože se spustí jako první, ale v případě, že se nenajde žádná vlastnost Key, se spustí konvence "ID".  Toto chování se zobrazuje, protože každá konvence prochází model aktualizovaný předchozí konvencí (místo toho, aby se nezávisle nepracoval), takže pokud například předchozí konvence aktualizovala název sloupce tak, aby odpovídala nějakému z Zajímá vás vaše vlastní konvence (Pokud před tím, než je název nezajímavá), použije se pro tento sloupec.  

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

## <a name="notes"></a>Poznámky:  

Seznam konvencí, které aktuálně používá Entity Framework, je k dispozici v dokumentaci MSDN: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Tento seznam je načítán přímo z našeho zdrojového kódu.  Zdrojový kód pro Entity Framework 6 je k dispozici na [GitHubu](https://github.com/aspnet/entityframework6/) a mnohé z konvencí používaných v Entity Framework jsou dobré počáteční body pro vlastní konvence založené na modelu.  
