---
title: Definování DbSets-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419092"
---
# <a name="defining-dbsets"></a>Definování DbSets
Při vývoji pomocí pracovního postupu Code First definujete odvozenou DbContext, která představuje vaši relaci s databází a zpřístupňuje Negenerickými pro každý typ v modelu. Toto téma popisuje různé způsoby, jak můžete definovat vlastnosti Negenerickými.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext s vlastnostmi Negenerickými  

Běžný případ zobrazený v Code First příklady je mít DbContext s veřejnými automatickými Negenerickými vlastnostmi pro typy entit vašeho modelu. Příklad:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Při použití v režimu Code First Tato akce nakonfiguruje Blogy a příspěvky jako typy entit a také nakonfiguruje další typy, které jsou z nich dosažitelné. Kromě toho DbContext automaticky volá metodu setter pro každou z těchto vlastností pro nastavení instance příslušného Negenerickými.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext s vlastnostmi IDbSet  

Existují situace, například při vytváření návrhů nebo napodobenin, kde je užitečnější k deklaraci vlastností sady pomocí rozhraní. V takových případech lze rozhraní IDbSet použít místo Negenerickými. Příklad:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Tento kontext funguje naprosto stejným způsobem jako kontext, který používá třídu Negenerickými pro jeho vlastnosti set.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext s vlastnostmi sady jen pro čtení  

Pokud nechcete zveřejnit veřejné metody setter pro vlastnosti Negenerickými nebo IDbSet, můžete místo toho vytvořit vlastnosti jen pro čtení a vytvořit instance sady sami. Příklad:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

Všimněte si, že DbContext ukládá do mezipaměti instanci Negenerickými vrácenou z metody set, aby každá z těchto vlastností vrátila stejnou instanci pokaždé, když je volána.  

Zjišťování typů entit pro Code First funguje stejným způsobem jako u vlastností s veřejnými metodami getter a setter.  
