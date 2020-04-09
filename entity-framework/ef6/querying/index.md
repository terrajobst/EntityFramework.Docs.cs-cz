---
title: Dotazování a hledání entit – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417168"
---
# <a name="querying-and-finding-entities"></a>Dotazování a hledání entit
Toto téma popisuje různé způsoby, jak můžete dotazovat na data pomocí entity framework, včetně LINQ a Find metoda. Techniky uvedené v tomto tématu platí stejně pro modely vytvořené pomocí Code First a EF Designer.  

## <a name="finding-entities-using-a-query"></a>Hledání entit pomocí dotazu  

DbSet a IDbSet implementovat IQueryable a tak lze použít jako výchozí bod pro zápis dotazu LINQ proti databázi. Toto není vhodné místo pro hloubkovou diskusi o LINQ, ale zde je několik jednoduchých příkladů:  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

Všimněte si, že DbSet a IDbSet vždy vytvářet dotazy proti databázi a bude vždy zahrnovat odezvu do databáze i v případě, že entity vrácené již existují v kontextu. Dotaz je proveden proti databázi, pokud:  

- Je výčtu **foreach** (C#) nebo **for each** (Visual Basic) příkaz.  
- Je výčtu operace kolekce, jako je [například ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)nebo [ToList](https://msdn.microsoft.com/library/bb342261).  
- Linq operátory jako [První](https://msdn.microsoft.com/library/bb291976) nebo [Any](https://msdn.microsoft.com/library/bb337697) jsou určeny v nejvzdálenější části dotazu.  
- Nazývají se následující metody: Metoda [rozšíření Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) na dbset, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)a Database.ExecuteSqlCommand.  

Při vrácení výsledků z databáze jsou k kontextu připojeny objekty, které neexistují v kontextu. Pokud je objekt již v kontextu, je vrácen existující objekt (aktuální a původní hodnoty vlastností objektu v položce **nejsou** přepsány hodnotami databáze).  

Při provádění dotazu entity, které byly přidány do kontextu, ale dosud nebyly uloženy do databáze nejsou vráceny jako součást sady výsledků. Chcete-li získat data, která je v kontextu, naleznete [v tématu místní data](~/ef6/querying/local-data.md).  

Pokud dotaz vrátí žádné řádky z databáze, výsledkem bude prázdná kolekce, nikoli **null**.  

## <a name="finding-entities-using-primary-keys"></a>Hledání entit pomocí primárních klíčů  

Find Metoda na DbSet používá hodnotu primárního klíče k pokusu o nalezení entity sledované kontextem. Pokud entita není nalezena v kontextu, bude do databáze odeslán dotaz, aby se tam entita nalezla. Null je vrácena, pokud entita není nalezena v kontextu nebo v databázi.  

Hledání se liší od použití dotazu dvěma významnými způsoby:  

- Odezva do databáze bude provedena pouze v případě, že entita s daným klíčem není nalezena v kontextu.  
- Hledání vrátí entity, které jsou ve stavu Added. To znamená Najít vrátí entity, které byly přidány do kontextu, ale dosud nebyly uloženy do databáze.  
### <a name="finding-an-entity-by-primary-key"></a>Hledání entity podle primárního klíče  

Následující kód ukazuje některá použití najít:  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a>Hledání entity pomocí složeného primárního klíče  

Entity Framework umožňuje entity mít složené klíče – to je klíč, který se skládá z více než jednu vlastnost. Můžete mít například entitu Nastavení blogu, která představuje nastavení uživatelů pro konkrétní blog. Vzhledem k tomu, že uživatel by měl pouze jeden BlogSettings pro každý blog, můžete se rozhodnout, že primární klíč BlogSettings je kombinací BlogId a uživatelského jména. Následující kód se pokusí najít BlogSettings s BlogId = 3 a uživatelské jméno = "johndoe1987":  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Všimněte si, že pokud máte složené klíče, musíte použít ColumnAttribute nebo fluent API k určení pořadí vlastností složeného klíče. Volání Najít musí použít toto pořadí při zadávání hodnot, které tvoří klíč.  
