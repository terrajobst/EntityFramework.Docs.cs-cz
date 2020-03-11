---
title: Předem vygenerovaná zobrazení mapování – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419389"
---
# <a name="pre-generated-mapping-views"></a>Předem vygenerovaná zobrazení mapování
Předtím, než Entity Framework může spustit dotaz nebo uložit změny do zdroje dat, musí vygenerovat sadu zobrazení mapování pro přístup k databázi. Tato zobrazení mapování jsou sada příkazů Entity SQL, které představují databázi abstraktním způsobem, a jsou součástí metadat, která jsou ukládána do mezipaměti na jednu doménu aplikace. Pokud vytvoříte více instancí stejného kontextu ve stejné doméně aplikace, bude znovu použita mapování zobrazení z metadat v mezipaměti, nikoli znovu jejich generování. Vzhledem k tomu, že generování zobrazení mapování je významnou součástí celkových nákladů na provedení prvního dotazu, Entity Framework umožňuje předem generovat zobrazení mapování a zahrnout je do zkompilovaného projektu. Další informace najdete v tématu  [požadavky na výkon (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>Generování zobrazení mapování pomocí edice EF Power Tools Community

Nejjednodušší způsob, jak předběžně generovat zobrazení, je použití [edice EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition). Po instalaci nástrojů Power Tools budete mít možnost nabídky pro generování zobrazení, jak je uvedeno níže.

-   U modelů **Code First** klikněte pravým tlačítkem myši na soubor kódu, který obsahuje vaši třídu DbContext.
-   Pro modely **Návrháře EF** klikněte pravým tlačítkem na soubor EDMX.

![generování zobrazení](~/ef6/media/generateviews.png)

Až se proces dokončí, budete mít třídu, která bude podobná následujícímu vygenerovanému.

![vygenerovaná zobrazení](~/ef6/media/generatedviews.png)

Když teď spustíte aplikaci EF, použije se tato třída k načtení zobrazení podle potřeby. Pokud se váš model změní a znovu negenerujete tuto třídu, EF EF vyvolá výjimku.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Generování zobrazení mapování z kódu – EF6 a vyšší

Dalším způsobem, jak vygenerovat zobrazení, je použití rozhraní API, které poskytuje EF. Při použití této metody máte volnost v jejich serializaci, ale budete je muset také načíst sami.

> [!NOTE]
> **Jenom EF6** – rozhraní API uvedená v této části se zavedla v Entity Framework 6. Pokud používáte starší verzi, tyto informace se nevztahují.

### <a name="generating-views"></a>Generování zobrazení

Rozhraní API pro generování zobrazení jsou na třídě System. data. entity. Core. Mapping. StorageMappingItemCollection. Můžete načíst StorageMappingCollection pro kontext pomocí objektu MetadataWorkspace objektu ObjectContext. Pokud používáte novější rozhraní API DbContext, můžete k tomu získat přístup pomocí IObjectContextAdapter, jak je uvedené níže. v tomto kódu máme instanci odvozeného DbContextu s názvem dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Jakmile budete mít StorageMappingItemCollection, můžete získat přístup k metodám GenerateViews a ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

První metoda vytvoří slovník s položkou pro každé zobrazení v mapování kontejneru. Druhá metoda vypočítá hodnotu hash pro jedno mapování kontejneru a používá se za běhu k ověření, že se model nezměnil od chvíle, kdy byla zobrazení předem vygenerována. Přepsání těchto dvou metod jsou k dispozici pro složité scénáře zahrnující několik mapování kontejnerů.

Při generování zobrazení zavoláte metodu GenerateViews a pak vypíšete výsledné EntitySetBase a DbMappingView. Také budete muset uložit hodnotu hash generovanou metodou ComputeMappingHashValue.

### <a name="loading-views"></a>Načítání zobrazení

Aby bylo možné načíst zobrazení generovaná metodou GenerateViews, můžete pro EF poskytnout třídu, která dědí z abstraktní třídy DbMappingViewCache. DbMappingViewCache určuje dvě metody, které je nutné implementovat:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

Vlastnost MappingHashValue musí vracet hodnotu hash generovanou metodou ComputeMappingHashValue. Když EF bude vyžadovat pro zobrazení, nejprve vygeneruje a porovná hodnotu hash modelu s hodnotou hash vrácenou touto vlastností. Pokud se neshodují, EF EF vyvolá výjimku EntityCommandCompilationException.

Metoda GetView přijme EntitySetBase a musíte vrátit DbMappingVIew obsahující EntitySql, které bylo vytvořeno pro, které bylo přidruženo k dané EntitySetBase ve slovníku generovaném metodou GenerateViews. Pokud EF požádá o zobrazení, které nemáte, měla by GetView vracet hodnotu null.

Následuje extrakce z DbMappingViewCache, která je generována pomocí nástrojů Power Tools, jak je popsáno výše, v této části vidíte jeden způsob, jak uložit a načíst požadovaný EntitySql.

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

Chcete-li, aby EF používal vaše DbMappingViewCache, použijte DbMappingViewCacheTypeAttribute a určete tak kontext, pro který byl vytvořen. V následujícím kódu přiřadíme BlogContext ke třídě MyMappingViewCache.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

U složitějších scénářů je možné zadat mapování instancí zobrazení mapování zadáním objektu pro vytváření mezipaměti pro zobrazení mapování. To lze provést implementací abstraktní třídy System. data. entity. Infrastructure. MappingViews. DbMappingViewCacheFactory. Instance objektu pro vytváření mezipaměti zobrazení mapování, která se používá, se dá načíst nebo nastavit pomocí StorageMappingItemCollection. MappingViewCacheFactoryproperty.
