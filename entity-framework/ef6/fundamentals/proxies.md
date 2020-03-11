---
title: Práce s proxy servery – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419337"
---
# <a name="working-with-proxies"></a>Práce s proxy servery
Při vytváření instancí Entity Framework typů entit POCO často vytváří instance dynamicky generovaného odvozeného typu, který funguje jako proxy pro entitu. Tento proxy Přepisuje některé virtuální vlastnosti entity, aby mohl vkládat háky pro provádění akcí automaticky při získání k vlastnosti. Například tento mechanismus slouží k podpoře opožděného načítání vztahů. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

## <a name="disabling-proxy-creation"></a>Zákaz vytváření proxy serveru  

Někdy je užitečné zabránit Entity Framework vytváření proxy instancí. Například serializace instancí, které nejsou proxy, je výrazně snazší než serializace instancí proxy serveru. Vytvoření proxy serveru může být vypnuté zrušením příznaku ProxyCreationEnabled. Jedno místo, které lze provést, je v konstruktoru vašeho kontextu. Příklad:  

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

Všimněte si, že EF nevytvoří proxy pro typy, kde není nic pro to, aby mohl proxy dělat. To znamená, že se můžete také vyhnout PROXYm typům, které mají zapečetěné nebo nemají žádné virtuální vlastnosti.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Explicitní vytvoření instance proxy  

Instance proxy nebude vytvořena, pokud vytvoříte instanci entity pomocí operátoru new. Nemusí se jednat o problém, ale pokud potřebujete vytvořit proxy instanci (například tak, aby fungovalo opožděné načítání nebo sledování změn proxy), můžete tak učinit pomocí metody Create Negenerickými. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Obecnou verzi objektu Create lze použít, pokud chcete vytvořit instanci odvozeného typu entity. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Všimněte si, že metoda Create nepřidá do kontextu vytvořenou entitu ani ji nepřipojí.  

Všimněte si, že metoda Create vytvoří pouze instanci samotného typu entity, pokud vytvoření typu proxy pro entitu nemá žádnou hodnotu, protože by nedošlo k žádným chybám. Například pokud je typ entity zapečetěný nebo nemá žádné virtuální vlastnosti, pak vytvořit vytvoří instanci typu entity.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Získání skutečného typu entity z typu proxy  

Typy proxy mají názvy, které vypadají přibližně takto:  

System. data. entity. DynamicProxies. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Typ entity pro tento typ proxy serveru můžete najít pomocí metody GetObjectType z ObjectContext. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Všimněte si, že pokud typ předaný metodě GetObjectType je instancí typu entity, který není typem proxy serveru, typ entity se pořád vrátí. To znamená, že tuto metodu můžete vždy použít k získání skutečného typu entity bez jakékoli jiné kontroly, aby se zobrazilo, zda typ je typ proxy serveru nebo ne.  
