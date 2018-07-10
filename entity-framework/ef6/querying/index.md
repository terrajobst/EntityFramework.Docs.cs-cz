---
title: Dotazování a hledání entit - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
caps.latest.revision: 3
ms.openlocfilehash: f0319e97d8ca8cfc9c90dac51d2ecbe7a29c1929
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912715"
---
# <a name="querying-and-finding-entities"></a>Dotazování a hledání entit
Toto téma popisuje různé způsoby, můžete zadat dotaz na data pomocí Entity Frameworku, včetně LINQ a metodu Find. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

## <a name="finding-entities-using-a-query"></a>Hledání pomocí dotazu entity  

DbSet a IDbSet implementující rozhraní IQueryable a proto může sloužit jako výchozí bod pro psaní dotazu LINQ na databázi. Toto není vhodné místo pro podrobnou diskuzi o LINQ, ale tady je několik jednoduchých příkladů:  

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

Všimněte si, že DbSet IDbSet vždy vytvořit dotazy na databázi a bude vždy zahrnovat odezvy databáze, i v případě, že entity, které vrátil již existují v kontextu. Dotaz je proveden v databázi při:  

- Ve výčtu **foreach** (C#) nebo **pro každou** – příkaz (Visual Basic).  
- Kolekce operací je uveden jako [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), nebo ToList[zadat popis odkazu](https://msdn.microsoft.com/library/bb342261).  
- Operátory LINQ, jako například [první](https://msdn.microsoft.com/library/bb291976) nebo [jakékoli](https://msdn.microsoft.com/library/bb337697) jsou určené v nejkrajnější část dotazu.  
- Tyto metody jsou volány: [zatížení](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) rozšiřující metody na DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)a Database.ExecuteSqlCommand.  

Když výsledky jsou vráceny z databáze, jsou objekty, které neexistují v rámci připojit ke kontextu. Pokud objekt je již v kontextu, je vrácen existující objekt (jsou aktuální a původní hodnoty vlastností objektu v položce **není** přepsání hodnot v databázi).  

Při provádění dotazu entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze, nebudou zobrazeny jako součást sady výsledků dotazu. Chcete-li získat data, která je v kontextu, najdete v článku [místních dat](~/ef6/querying/local-data.md).  

Pokud dotaz vrací žádné řádky z databáze, výsledkem bude prázdnou kolekci, spíše než **null**.  

## <a name="finding-entities-using-primary-keys"></a>Vyhledávání entit pomocí primárních klíčů  

Metoda hledání na DbSet používá hodnotu primárního klíče pokusí se najít entitu sledován pomocí funkce kontextu. Pokud subjektem nebyl nalezen v kontextu se být dotaz odesílat do databáze se najít entitu existuje. Pokud subjektem nebyl nalezen v kontextu nebo v databázi, je vrácena hodnota Null.  

Hledání se liší od použití dotazu významné dvěma způsoby:  

- Cesty k databázi se provádí pouze v případě entita s daným klíčem nebyl nalezen v kontextu.  
- Hledání vrátí entity, které jsou ve stavu Added. To je, najít, vrátí entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze.  
### <a name="finding-an-entity-by-primary-key"></a>Vyhledávání podle primárního klíče entity  

Následující kód ukazuje některá použití hledání:  

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

### <a name="finding-an-entity-by-composite-primary-key"></a>Vyhledávání podle složený primární klíč entity  

Entity Framework umožňuje vaší entity, které mají složených klíčů – to je klíč, který se skládá z více než jednu vlastnost. Například můžete mít BlogSettings entity, která představuje nastavení uživatele pro konkrétní blogu. Vzhledem k tomu, že uživatel bude mít vždy jen jeden BlogSettings pro každý blog, který můžete zvolili, primární klíč BlogSettings kombinací BlogId a uživatelské jméno. Následující kód se pokusí najít BlogSettings s BlogId = 3 a uživatelské jméno = "johndoe1987":  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Mějte na paměti, když máte složených klíčů budete muset použít ColumnAttribute nebo rozhraní fluent API Pokud chcete zadat řazení pro vlastnosti složený klíč. Volání hledání musíte použít toto pořadí při zadávání hodnot, které tvoří klíč.  
