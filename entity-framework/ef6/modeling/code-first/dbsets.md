---
title: Definování DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 8a495c6ce74d9a346a84b0e10fb28395f4dce07b
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067490"
---
# <a name="defining-dbsets"></a>Definování DbSets
Při vývoji se kód prvního pracovního postupu můžete definovat odvozené DbContext, který reprezentuje relaci s databází a zveřejňuje DbSet pro každý typ v modelu. Toto téma popisuje různé způsoby, jak můžete definovat vlastnosti DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>Kontext databáze s vlastnostmi DbSet  

Běžné Code First příkladů je DbContext s veřejné vlastnosti automatické DbSet pro typy entity z modelu. Příklad:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Při použití v režimu Code First, bude to jako typy entit, jakož i jiné typy, které jsou dostupné z těchto konfigurací konfigurace blogů a příspěvky. Kromě toho DbContext automaticky zavolá metodu setter pro každou z těchto vlastností lze nastavit instanci odpovídající DbSet.  

## <a name="dbcontext-with-idbset-properties"></a>Kontext databáze s vlastnostmi IDbSet  

Existují situace, jako je například při vytváření mocks nebo napodobeniny, kde je další užitečné k deklaraci vlastnosti sady pomocí rozhraní. V takových případech IDbSet rozhraní může být zastoupen DbSet. Příklad:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Tento kontext funguje přesně stejným způsobem jako kontext, který se používá třídu DbSet sady vlastností.  

## <a name="dbcontext-with-read-only-set-properties"></a>Nastavit vlastnosti DbContext s jen pro čtení  

Pokud nechcete, aby k vystavení veřejné metody setter pro DbSet nebo IDbSet vlastností můžete místo toho vytvořit vlastnosti jen pro čtení a vytvoření instancí sady sami. Příklad:  

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

Všimněte si, že DbContext ukládá do mezipaměti instance DbSet vrátil z metody Set tak, aby každá z těchto vlastností vrátí stejnou instanci pokaždé, když je volána.  

Zjišťování typů entit pro Code First lze použít stejně jako jako to udělá za vlastnosti veřejné metody getter a setter.  
