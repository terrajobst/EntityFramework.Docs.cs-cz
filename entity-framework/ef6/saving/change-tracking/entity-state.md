---
title: Práce s entitami stavů – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419697"
---
# <a name="working-with-entity-states"></a>Práce s entitami stavů
V tomto tématu se dozvíte, jak přidat a připojit entity k kontextu a jak je Entity Framework zpracovávat během metody SaveChanges.
Entity Framework se postará o sledování stavu entit, když jsou připojeni k kontextu, ale v případě odpojených nebo N-vrstvých scénářů můžete v EF zjistit, v jakém stavu by měly být vaše entity.
Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

## <a name="entity-states-and-savechanges"></a>Stavy entit a metody SaveChanges

Entita může být v jednom z pěti stavů, jak je definováno výčtem EntityState. Tyto stavy jsou:  

- Přidáno: entita je sledována kontextem, ale ještě neexistuje v databázi.  
- Nezměněno: entita je sledována kontextem a existuje v databázi a její hodnoty vlastností se nezměnily z hodnot v databázi.  
- Změněno: entita je sledována kontextem a existuje v databázi a některé nebo všechny její hodnoty vlastností byly změněny.  
- Odstraněno: entita je sledována kontextem a existuje v databázi, ale byla označena pro odstranění z databáze při volání dalšího volání metody SaveChanges.  
- Odpojeno: entita není sledována kontextem.  

SaveChanges používá pro entity v různých stavech různé věci:  

- Nezměněné entity nejsou nijak ovlivněny pomocí metody SaveChanges. Aktualizace nejsou odesílány do databáze pro entity v nezměněném stavu.  
- Přidané entity jsou vloženy do databáze a pak se nezměnily, když se funkce SaveChanges vrátí.  
- Změněné entity jsou aktualizovány v databázi a pak se nezměnily, když se funkce SaveChanges vrátí.  
- Odstraněné entity se odstraní z databáze a pak se odpojí z kontextu.  

Následující příklady znázorňují způsoby, kterými lze změnit stav entity nebo grafu entity.  

## <a name="adding-a-new-entity-to-the-context"></a>Přidání nové entity do kontextu  

Do kontextu lze přidat novou entitu voláním metody Add v Negenerickými.
Tím se entita vloží do přidaného stavu, což znamená, že se při příštím volání metody SaveChanges vloží do databáze.
Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Dalším způsobem, jak přidat novou entitu do kontextu, je změnit její stav na přidáno. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Nakonec můžete přidat novou entitu do kontextu tím, že ji připojíte k jiné entitě, která je již sledována.
To může být přidání nové entity do navigační vlastnosti kolekce jiné entity nebo nastavením navigační vlastnosti odkazu jiné entity, která odkazuje na novou entitu. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Všimněte si, že pro všechny tyto příklady, pokud přidávaná entita obsahuje odkazy na jiné entity, které ještě nejsou sledovány, budou tyto nové entity přidány také do kontextu a při příštím volání metody SaveChanges budou vloženy do databáze.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Připojení existující entity k kontextu  

Pokud máte entitu, kterou víte, že již v databázi existuje, ale není aktuálně sledována kontextem, můžete určit, aby kontext sledoval entitu pomocí metody Attach v Negenerickými. Entita bude v kontextu v nezměněném stavu. Příklad:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Všimněte si, že v databázi nebudou provedeny žádné změny, pokud je volána metoda SaveChanges bez jakékoli jiné manipulace s připojenou entitou. Je to proto, že entita je ve stavu Unchanged.  

Dalším způsobem, jak připojit existující entitu k kontextu, je změnit svůj stav na nezměněný. Příklad:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Všimněte si, že v obou těchto příkladech platí, že pokud entita, která je připojena, odkazuje na jiné entity, které ještě nejsou sledovány, budou tyto nové entity také připojeny k tomuto kontextu v nezměněném stavu.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Připojení existující ale změněné entity k kontextu  

Máte-li entitu, kterou víte již v databázi existuje, ale na kterou mohly být provedeny změny, můžete určit, že má kontext připojit entitu a nastavit jeho stav na změněno.
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

Když změníte stav na změněno, všechny vlastnosti entity budou označeny jako upravené a všechny hodnoty vlastností budou odeslány do databáze při volání metody SaveChanges.  

Všimněte si, že pokud připojená entita obsahuje odkazy na jiné entity, které ještě nejsou sledovány, budou tyto nové entity připojeny k tomuto kontextu v nezměněném stavu – nebudou automaticky upraveny.
Pokud máte více entit, které je třeba označit jako změněné, měli byste nastavit stav pro každou z těchto entit jednotlivě.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Změna stavu sledované entity  

Stav entity, která je již sledována, můžete změnit nastavením vlastnosti stav na jejím vstupu. Příklad:  

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

Všimněte si, že volání metody Add nebo Attach pro entitu, která je již sledována, lze také použít ke změně stavu entity. Například volání metody attach pro entitu, která je aktuálně ve stavu přidáno, změní svůj stav na nezměněný.  

## <a name="insert-or-update-pattern"></a>Vložit nebo aktualizovat vzor  

Běžným vzorem pro některé aplikace je přidání entity jako nového (výsledkem vložení do databáze) nebo připojení entity jako existující a její označení jako upravené (výsledkem aktualizace databáze) v závislosti na hodnotě primárního klíče.
Například při použití databáze generované celočíselnými klíči je běžné zacházet se entitou s nulovým klíčem jako novou a entitou s nenulovým klíčem jako stávající.
Tento vzor lze dosáhnout nastavením stavu entity na základě kontroly hodnoty primárního klíče. Příklad:  

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

Všimněte si, že když změníte stav na změněno, všechny vlastnosti entity budou označeny jako upravené a všechny hodnoty vlastností budou odeslány do databáze při volání metody SaveChanges.  
