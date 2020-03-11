---
title: Dotazování a hledání entit – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417168"
---
# <a name="querying-and-finding-entities"></a>Dotazování a hledání entit
Toto téma popisuje různé způsoby, jak se můžete dotazovat na data pomocí Entity Framework, včetně LINQ a metody Find. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

## <a name="finding-entities-using-a-query"></a>Hledání entit pomocí dotazu  

Negenerickými a IDbSet implementují rozhraní IQueryable a dají se použít jako výchozí bod pro zápis dotazu LINQ na databázi. Nejedná se o příslušné místo pro podrobné diskuzi LINQ, ale tady je několik jednoduchých příkladů:  

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

Všimněte si, že Negenerickými a IDbSet vždy vytvářejí dotazy na databázi a budou vždy zahrnovat zpáteční cestu k databázi i v případě, že vrácené entity již existují v kontextu. Dotaz je proveden na databázi v těchto případech:  

- Je vyhodnocen pomocí příkazu **foreach** (C#) nebo **for each** (Visual Basic).  
- Je vyhodnocena operací shromažďování, například [ToArray –](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)nebo [ToList –](https://msdn.microsoft.com/library/bb342261).  
- V krajní části dotazu jsou zadány operátory LINQ, například [First](https://msdn.microsoft.com/library/bb291976) nebo [Any](https://msdn.microsoft.com/library/bb337697) .  
- Jsou volány následující metody: metoda rozšíření [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) na Negenerickými, [DbEntityEntry. reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)a Database. ExecuteSqlCommand.  

Při vrácení výsledků z databáze jsou objekty, které v kontextu neexistují, připojeny k tomuto kontextu. Pokud objekt již v kontextu je, je vrácen stávající objekt (aktuální a původní hodnoty vlastností objektu v **položce nejsou** přepsány hodnotami databáze).  

Když provedete dotaz, entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze, nebudou vráceny jako součást sady výsledků dotazu. Pokud chcete získat data, která jsou v kontextu, přečtěte si téma [místní data](~/ef6/querying/local-data.md).  

Pokud dotaz nevrátí z databáze žádné řádky, bude výsledkem prázdná kolekce, nikoli **hodnota null**.  

## <a name="finding-entities-using-primary-keys"></a>Hledání entit pomocí primárních klíčů  

Metoda Find v Negenerickými používá k pokusu o nalezení entity sledované kontextem hodnotu primárního klíče. Pokud se entita v kontextu nenajde, odešle se dotaz do databáze, kde se nachází entita. Pokud entita nebyla nalezena v kontextu nebo v databázi, je vrácena hodnota null.  

Hledání se liší od použití dotazu dvěma významnými způsoby:  

- Zpáteční cesta k databázi bude provedena pouze v případě, že se entita s daným klíčem nenajde v kontextu.  
- Hledání vrátí entity, které jsou ve stavu přidáno. To znamená, že příkaz find vrátí entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze.  
### <a name="finding-an-entity-by-primary-key"></a>Hledání entity podle primárního klíče  

Následující kód ukazuje některé použití najít:  

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

Entity Framework umožňuje vašim entitám mít složené klíče – což je klíč, který se skládá z více než jedné vlastnosti. Můžete mít například entitu BlogSettings, která představuje nastavení uživatele pro konkrétní blog. Vzhledem k tomu, že uživatel bude mít pro každý blog jenom jednu BlogSettings, můžete zvolit, aby primární klíč BlogSettings kombinaci BlogId a username. Následující kód se pokusí najít BlogSettings s BlogId = 3 a username = "johndoe1987":  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Všimněte si, že pokud máte složené klíče, potřebujete použít ColumnAttribute nebo rozhraní API Fluent k určení řazení vlastností složeného klíče. Volání metody Find musí používat toto pořadí při zadávání hodnot, které tvoří klíč.  
