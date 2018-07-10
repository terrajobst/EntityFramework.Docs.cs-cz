---
title: Práce s stavy entity - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
caps.latest.revision: 3
ms.openlocfilehash: e27d70dd7a6a63c2b225e9cf4fe7ac8cc280d4ca
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912055"
---
# <a name="working-with-entity-states"></a>Práce s stavy entity
Toto téma zahrnuje přidání a připojit entity do kontextu a jak Entity Framework zpracovává během SaveChanges.
Sledování stavu entity, pokud jsou připojeny k kontextu, ale v odpojeném nebo N-vrstvá scénáře můžete nechat EF vědět, jednotlivých stavech entity musí být v postará Entity Framework.
Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

## <a name="entity-states-and-savechanges"></a>Stavy entity a SaveChanges

Entita může být v jednom z pěti stavů fronty definovaných výčtem EntityState. Tyto stavy jsou:  

- Přidání: Entita sledován správou kontextu, ale dosud neexistuje v databázi  
- Beze změny: Entita sledován správou kontextu a existuje v databázi a její hodnoty vlastností se neliší od hodnot v databázi  
- Upravené: Entita sledován správou kontextu a v databázi existuje a byly změněny některé nebo všechny příslušné hodnoty vlastnosti  
- Odstraněné: Entita sledován správou kontextu existuje v databázi, ale byl označen k odstranění v databázi při dalším je volána metoda SaveChanges  
- Odpojit: entity není sledována podle kontextu  

SaveChanges provádí různé věci pro entity v různých stavech:  

- Beze změny entity nejsou přistupovala SaveChanges. Aktualizace se neodešlou do databáze pro entity v nezměněném stavu.  
- Přidání entity jsou vloženy do databáze a pak budou beze změn při SaveChanges vrátí.  
- Změny entity jsou aktualizována v databázi a pak budou beze změn při SaveChanges vrátí.  
- Odstraněné entity jsou odstraněna z databáze a potom se odpojí z kontextu.  

Následující příklady ukazují způsoby, ve kterém lze změnit stav entity nebo grafu entity.  

## <a name="adding-a-new-entity-to-the-context"></a>Přidání nové entity v kontextu  

Nová entita lze přidat do kontextu pomocí volání metody Add u DbSet.
To umístí entity do stavu Added, což znamená, že ho bude vložen do databáze při příštím, která je volána metoda SaveChanges.
Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Dalším způsobem, jak přidat nové entity v kontextu je změna stavu na přidání. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Nakonec můžete přidat nové entity v kontextu podle zapojení až jinou entitu, která je již sledován.
To může být tak, že přidáte nové entity pro navigační vlastnost kolekce z druhé entity nebo tak, že nastavíte vlastnost navigace odkaz z druhé entity tak, aby odkazoval na nové entity. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Všimněte si, že u všech těchto příkladech, pokud má entita, přidává odkazy na jiné entity, které nejsou ještě sledovat pak tyto nové entity se také zařadí do kontextu a budou při příštím, která je volána metoda SaveChanges vložena do databáze.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Připojení existující entity v kontextu  

Pokud máte v databázi, ale které se aktuálně nesleduje podle kontextu můžete v kontextu ke sledování entity pomocí metody připojit na DbSet existuje entita, která už znáte. Entita bude v nezměněném stavu v kontextu. Příklad:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Všimněte si, že žádné změny provedené do databáze, pokud SaveChanges je volána bez provádění jakékoli manipulaci s připojenými entity. Je to proto, že entita je v nezměněném stavu.  

Dalším způsobem, jak připojit stávající entity v kontextu je změnit její stav Unchanged. Příklad:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Všimněte si, že pro oba tyto příklady Pokud entita připojovaný obsahuje odkazy na jiné entity, které ještě nejsou sledovány pak tyto nové entity se taky připojit ke kontextu v nezměněném stavu.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Připojení existující ale upravená entita v kontextu  

Pokud máte entity, o kterém víte už existuje v databázi, ale které může byly provedeny změny můžete v kontextu připojte entitu a nastavit jeho stav Modified.
Příklad:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Při změně stavu na změněné všechny vlastnosti entity se označí jako upravená a hodnoty všech vlastností se odešlou do databáze, když je volána metoda SaveChanges.  

Všimněte si, že pokud entita připojovaný obsahuje odkazy na jiné entity, které ještě nejsou sledovány, pak tyto nové entity se připojit ke kontextu v nezměněném stavu – nebude automaticky se stane změněno.
Pokud máte více entit, které je třeba označit změněné byste měli nastavit stav zvlášť pro každý z těchto entit.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Změna stavu sledovaného entity  

Můžete změnit stav entity, která je již sledován nastavením vlastnosti stavu na vstupu. Příklad:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Všimněte si, že volání přidat nebo připojit pro entitu, která je již sledován je také možné změny stavu entity. Například volání připojení pro entitu, která je aktuálně ve stavu Added se změní jeho stav Unchanged.  

## <a name="insert-or-update-pattern"></a>Vložit nebo aktualizovat vzor  

Běžným vzorem pro některé aplikace je přidání entity jako nový (výsledkem vložení databáze) nebo připojit entitu jako existující a označte ji jako změněnou (výsledkem je aktualizace databáze) v závislosti na hodnotě primární klíč.
Například při použití primárních klíčů databáze generované celé číslo je běžné zacházet se žádný klíč jako nové a nenulové klíč jako existující.
Tento vzor lze dosáhnout nastavením stavu entity podle kontrolu hodnoty primárního klíče. Příklad:  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

Všimněte si, že při změně stavu na změněné všechny vlastnosti entity se označí jako upravená a hodnoty všech vlastností se odešlou do databáze, když je volána metoda SaveChanges.  
