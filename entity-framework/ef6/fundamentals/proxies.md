---
title: Práce s proxy - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
caps.latest.revision: 3
ms.openlocfilehash: 4632e246d28a3cd53dabe5ac76e44f4538739abc
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912064"
---
# <a name="working-with-proxies"></a>Práce s proxy servery
Při vytváření instancí typů entit POCO, Entity Framework často vytváří instance dynamicky generované odvozeného typu, který funguje jako proxy pro entitu. Tento proxy server přepíše některé virtuální vlastnosti entity, chcete-li vložit háky pro provádění akcí automaticky při přístupu k vlastnosti. Tento mechanismus je například použít pro podporu opožděné načtení vztahů. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

## <a name="disabling-proxy-creation"></a>Zakazuje vytváření proxy serveru  

Někdy je užitečné pro Entity Framework zabránit ve vytváření instancí serveru proxy. Například serializaci instancí bez proxy serveru je mnohem jednodušší než serializaci instancí proxy serveru. Vytváření proxy server vypnete tím, že zrušíte ProxyCreationEnabled příznak. V konstruktoru kontext je pohromadě, můžete to udělat. Příklad:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Všimněte si, že EF nevytvoří proxy pro typy tam, kde není nic pro proxy server provést. To znamená, že se také můžete vyhnout proxy servery tak, že typy, které jsou zapečetěné a/nebo mít žádné virtuální vlastnosti.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Explicitní vytvoření instance serveru proxy  

Instance serveru proxy se nevytvoří, pokud vytvoříte instanci entity pomocí operátoru new. Nemusí se jednat o problém, ale pokud budete muset vytvořit instanci proxy serveru (například tak, aby opožděné načtení nebo proxy server sledování změn bude fungovat) pak můžete tak učinit pomocí metody Create DbSet. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Obecné verzi vytvořit lze použít, pokud chcete vytvořit instanci typu odvozené entity. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Všimněte si, že metody Create přidat nebo připojit ke kontextu vytvořené entity.  

Všimněte si, že metody Create pouze vytvoří instance samotného typu entity při vytváření typ proxy entity by nemají žádnou hodnotu, protože nic nedělali. Například pokud typ entity je zapečetěná a/nebo nemá žádné virtuální vlastnosti pak vytvořit pouze vytvoří instanci typu entity.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Získávání skutečné entity typu z typu proxy  

Proxy server typů mají názvy, které vypadat přibližně takto:  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Najít typ entity pro tento typ proxy serveru pomocí metody Metoda GetObjectType z objektu ObjectContext. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Upozorňujeme, že pokud typ předaný Metoda GetObjectType, vrátí se stále instance typu entity, která není typu proxy potom je typ entity. To znamená, že použijete tuto metodu můžete vždy získat typ skutečné entity bez jakékoli další kontroly a zjistěte, jestli typ je typ proxy serveru, nebo ne.  
