---
title: Zobrazení mapování předem generovaného - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: c2ad7125122c04af238e8fdd07da2c6c308a2756
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250761"
---
# <a name="pre-generated-mapping-views"></a>Zobrazení předem generovaného mapování
Entity Framework mohli spustit dotaz nebo uložit změny do zdroje dat, musíte vygenerovat sadu zobrazení mapování pro přístup k databázi. Tato mapování zobrazení jsou sady Entity SQL příkazu, který abstraktní jak reprezentaci databáze a část metadata, která se uloží do mezipaměti pro doménu aplikace. Pokud vytvoříte více instancí stejného kontextu ve stejné doméně aplikace, bude znovu použít zobrazení mapování ze metadata uložená v mezipaměti místo jejich obnovení. Protože generování zobrazení mapování je podstatnou část celkové náklady na provedení první dotaz, Entity Framework umožňuje předběžně generovat zobrazení mapování a zahrnout je do kompilované projektu. Další informace najdete v tématu [důležité informace o výkonu (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools"></a>Generuje se mapování zobrazení s EF Power Tools

Nejjednodušší způsob, jak předem vygenerovat zobrazení je použít [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d). Jakmile budete mít nainstalované nástroje Power budete mít možnost nabídky generovat zobrazení, jak je uvedeno níže.

-   Pro **Code First** modelů klikněte pravým tlačítkem na soubor kódu, který obsahuje vaše třídy DbContext.
-   Pro **EF designeru** modelů klikněte pravým tlačítkem na soubor EDMX.

![Generování zobrazení](~/ef6/media/generateviews.png)

Po dokončení procesu budete mít podobný následujícímu generované třídy

![vygenerovaných zobrazení](~/ef6/media/generatedviews.png)

Nyní při spuštění aplikace EF bude tato třída slouží k načtení zobrazení podle potřeby. Pokud se změny modelu a nejsou znovu generovány Tato třída EF vyvolá výjimku.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Generování zobrazení mapování z kódu – ef6 nebo novější

Použití rozhraní API, která poskytuje EF je další způsob, jak vygenerovat zobrazení. Při použití této metody budete moci svobodně k serializaci zobrazení, ale potřebujete, ale je také potřeba načíst zobrazení sami.

> [!NOTE]
> **EF6 a vyšší pouze** – API, které jsou uvedené v této části byly zavedeny v Entity Framework 6. Pokud používáte starší verzi tyto informace se nevztahují.

### <a name="generating-views"></a>Generování zobrazení

Rozhraní API pro generování zobrazení jsou ve třídě System.Data.Entity.Core.Mapping.StorageMappingItemCollection. StorageMappingCollection pro kontext můžete načíst pomocí objektu MetadataWorkspace objektu ObjectContext. Pokud používáte rozhraní API novější kontext databáze. poté budete mít přístup s použitím IObjectContextAdapter podobná níže uvedenému příkladu, v tomto kódu máme instance vaší odvozené DbContext volá dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Jakmile budete mít objekt StorageMappingItemCollection můžete získat přístup k metodám GenerateViews a ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

První metoda vytvoří slovník s položkou pro každé zobrazení v mapování kontejnerů. Druhá metoda vypočítá hodnotu hash pro mapování jedné kontejnerů a je použita v době běhu k ověření, protože zobrazení byly předem generovaného nedošlo ke změně modelu. Přepsání dvě metody jsou k dispozici pro komplexní scénáře zahrnující více mapování v kontejneru.

Při generování zobrazení budete volat metodu GenerateViews a pak zapíše výsledný EntitySetBase a DbMappingView. Musíte také uložit hodnotu hash pro generované metody ComputeMappingHashValue.

### <a name="loading-views"></a>Načítání zobrazení

Aby bylo možné načíst vzhled zobrazení vygenerovaných sadou GenerateViews metodu, zadáte EF s třídou, která dědí z abstraktní třídy DbMappingViewCache. DbMappingViewCache určuje dvě metody, které je nutné implementovat:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

Vlastnost MappingHashValue musí vracet-the-hash generovaných ComputeMappingHashValue metodou. Když je EF tak má dotaz pro zobrazení nejprve generovat a srovnání hodnoty hash modelu pomocí hodnot hash, kterou tato vlastnost vrátí. Pokud shodné nejsou EF vyvolá výjimku EntityCommandCompilationException.

Metoda GetView přijme EntitySetBase a je zapotřebí vrátit DbMappingVIew obsahující EntitySql, který byl vygenerován pro, který byl přidružen daný EntitySetBase ve slovníku vygenerovaná metoda GenerateViews. Pokud EF požádá o zobrazení, které nemají pak GetView by měl vrátit hodnotu null.

Toto je výpis z DbMappingViewCache, který je generován pomocí nástroje pro výkon, jak je popsáno výše, v ní můžeme vidět jeden způsob, jak ukládat a načítat EntitySql vyžaduje.

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

Pokud chcete, aby pomocí EF použijte vaše DbMappingViewCache přidáte DbMappingViewCacheTypeAttribute, kontext, který byl vytvořen pro zadání. V následujícím kódu BlogContext přidružené k MyMappingViewCache třídy.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Pro složitější scénáře lze zadat mapování zobrazit mezipaměti instance tak, že určíte objekt mapování zobrazení mezipaměti. To můžete udělat pomocí implementace abstraktní třídy System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory. Instance objektu pro vytváření mezipaměti zobrazení mapování, která se používá můžete načíst nebo nastavit pomocí StorageMappingItemCollection.MappingViewCacheFactoryproperty.
