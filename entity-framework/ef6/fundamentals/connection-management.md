---
title: Správa připojení - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 9588ef85435d3c0218defdc098f1e7150fb7ef72
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949045"
---
# <a name="connection-management"></a>Správa připojení
Tato stránka popisuje chování Entity Framework s ohledem na předávání připojení do kontextu a funkce **Database.Connection.Open()** rozhraní API.  

## <a name="passing-connections-to-the-context"></a>Předávání připojení ke kontextu  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Chování EF5 a starší verze  

Existují dva konstruktory, které přijímají připojení:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Je možné použít, ale budete muset vyřešit několik omezení:  

1. Pokud předáte otevřené připojení na jeden z následujících pak při prvním rozhraní se pokusí použít, pokud je vyvolána výjimka InvalidOperationException říká ji nelze znovu otevřít již otevřeného připojení.  
2. Příznak contextOwnsConnection je interpretována jako jestli nadřízené připojení úložiště by mělo být uvolněno při uvolnění kontextu. Ale bez ohledu na to, že nastavení připojení úložiště je vždy uzavřen, při uvolnění kontextu. Takže pokud máte více než jeden DbContext pomocí stejného připojení uvolnění podle kontextu, je nejprve připojení zavře (podobně Pokud jste kombinovat existující připojení ADO.NET s DbContext, DbContext vždy připojení zavře při jeho uvolnění) .  

Je možné obejít omezení na první nad úspěšným ukončeném připojení a pouze provádění kódu, který by se otevřely se po všech kontextech byly vytvořeny:  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

Druhé omezení jenom znamená, že potřebujete nepoužívejte disposing některý z objektů DbContext, dokud nebudete připraveni pro připojení bude uzavřen.  

### <a name="behavior-in-ef6-and-future-versions"></a>Chování v budoucích verzích a EF6  

V budoucích verzích a EF6 uvolněn objekt DbContext má stejné dva konstruktory, ale už nevyžaduje, aby předaný konstruktoru připojení ukončeno při obdržení. Tohle je teď možné:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

Také příznak contextOwnsConnection teď řídí, zda připojení je uzavřen i uvolněno při uvolnění uvolněn objekt DbContext. Takže ve výše uvedeném příkladu připojení není ukončeno v kontextu uvolněn (řádek 32), jako by byl v předchozích verzích EF, ale místo toho, když je uvolněn samotné připojení (40 řádků).  

Samozřejmě je stále možné pro objekt DbContext převzít kontrolu nad připojení (pouze sada contextOwnsConnection hodnotu true, nebo použijte jednu z dalších konstruktory) Pokud tedy chcete.  

> [!NOTE]
> Při použití transakcí se tento nový model se několik dalších důležitých informací. Podrobnosti najdete v tématu [práce s transakcí](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Chování EF5 a starší verze  

V dřívějších verzích a EF5 je chyba tak, aby **ObjectContext.Connection.State** nebyla aktualizována tak, aby odrážely o skutečném stavu nadřízené připojení úložiště. Například pokud jste spustili následující kód je může být vrácen stav **uzavřeno** i v případě, že ve skutečnosti základní ukládání připojení je **otevřít**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Samostatně, je-li otevřít připojení k databázi pomocí volání Database.Connection.Open() bude otevřen až při příštím spuštění dotazu, nebo volat nic, která vyžaduje připojení k databázi (například SaveChanges()) ale za základní ukládání, připojení bude ukončeno. Kontext se pak znovu otevřete a znovu ukončete připojení pokaždé, když jiná operace databáze se vyžaduje:  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a>Chování v budoucích verzích a EF6  

EF6 a budoucí verze přesměrovali jsme si přístup, pokud volající kód zvolí možnost otevření připojení pomocí volání kontextu. Database.Connection.Open() pak má dobrý důvod pro to a rozhraní bude předpokládat, že chce kontrolu nad otevření a zavření připojení a se už připojení automaticky zavře.  

> [!NOTE]
> To může potenciálně vést k připojení, které jsou otevřené pro dlouho proto používejte opatrně.  

Také jsme aktualizovali kód tak, aby ObjectContext.Connection.State nyní uchovává informace o stavu nadřízené připojení správně.  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
