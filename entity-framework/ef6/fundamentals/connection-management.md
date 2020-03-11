---
title: Správa připojení – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417985"
---
# <a name="connection-management"></a>Správa připojení
Tato stránka popisuje chování Entity Framework s ohledem na předávání připojení do kontextu a funkce **databáze. Connection. Open ()** API.  

## <a name="passing-connections-to-the-context"></a>Předávání připojení do kontextu  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Chování pro EF5 a starší verze  

Existují dva konstruktory, které přijímají připojení:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Je možné je použít, ale musíte vyřešit několik omezení:  

1. Pokud předáte otevřené připojení k některé z těchto kroků, při prvním pokusu o jeho použití se spustí příkaz InvalidOperationException, že nemůže znovu otevřít již otevřené připojení.  
2. Příznak contextOwnsConnection je interpretován tak, zda by mělo být zdrojové připojení úložiště uvolněno při uvolnění kontextu. Bez ohledu na toto nastavení je připojení úložiště vždy uzavřeno, když je kontext vyřazen. Takže pokud máte více než jeden DbContext se stejným připojením, jako by se odstranila jakákoli kontextová připojení, bude připojení ukončeno (podobně, pokud jste nastavili kombinaci stávajícího ADO.NET připojení s DbContext, DbContext bude připojení vždycky po vyřazení trvale ukončit). .  

Výše uvedené omezení můžete obejít tak, že předáte uzavřená připojení a spustíte jenom kód, který by ho otevřel po vytvoření všech kontextů:  

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

Druhé omezení znamená, že musíte upustit od odstranění všech objektů DbContext, dokud nebudete připraveni na uzavření připojení.  

### <a name="behavior-in-ef6-and-future-versions"></a>Chování v EF6 a budoucích verzích  

V EF6 a budoucích verzích má DbContext stejné dva konstruktory, ale již nevyžaduje, aby bylo připojení předané do konstruktoru uzavřeno při jeho přijetí. To je teď možné:  

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

Příznak contextOwnsConnection nyní také určuje, zda je připojení uzavřeno a odstraněno, když je DbContext vyřazeno. Takže v předchozím příkladu není připojení uzavřeno, když je kontext vyřazen (řádek 32), protože by byl v předchozích verzích EF, ale místo toho, když je samotné připojení vyřazeno (řádek 40).  

Samozřejmě je stále možné, aby DbContext mohl převzít kontrolu nad připojením (stačí nastavit contextOwnsConnection na hodnotu true nebo použít jeden z dalších konstruktorů), pokud to chcete.  

> [!NOTE]
> Při použití transakcí s tímto novým modelem existují další okolnosti. Podrobnosti najdete v tématu [práce s transakcemi](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database. Connection. Open ()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Chování pro EF5 a starší verze  

V EF5 a dřívějších verzích je chyba, například **objekt ObjectContext. Connection. State** se neaktualizoval tak, aby odrážel skutečný stav základního připojení úložiště. Například pokud jste provedli následující kód, můžete vrátit stav **Uzavřeno** , i když je ve skutečnosti **otevřené**základní připojení úložiště.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Samostatně, pokud otevřete připojení databáze voláním Database. Connection. Open (), bude otevřeno až do příštího spuštění dotazu nebo volání cokoli, co vyžaduje připojení k databázi (například SaveChanges ()), ale poté, co příslušné úložiště dostanou. připojení bude zavřeno. Kontext pak znovu otevřete a znovu uzavřete připojení, kdykoli je potřeba jiná databázová operace:  

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

### <a name="behavior-in-ef6-and-future-versions"></a>Chování v EF6 a budoucích verzích  

Pro EF6 a budoucí verze jsme převzali přístup, že pokud se volající kód rozhodne otevřít připojení pomocí volajícího kontextu. Database. Connection. Open () pak má dobrý důvod k tomu, že se tak předpokládá, že bude mít za to kontrolu nad otevřením a zavřením připojení a nebude už připojení automaticky ukončeno.  

> [!NOTE]
> To může potenciálně vést k otevření připojení, která jsou otevřená po dlouhou dobu, takže se bude používat opatrně.  

Také jsme aktualizovali kód tak, aby objekt ObjectContext. Connection. State nyní správně sledoval stav základního připojení.  

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
