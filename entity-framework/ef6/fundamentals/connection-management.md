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
# <a name="connection-management"></a><span data-ttu-id="f94a4-102">Správa připojení</span><span class="sxs-lookup"><span data-stu-id="f94a4-102">Connection management</span></span>
<span data-ttu-id="f94a4-103">Tato stránka popisuje chování Entity Framework s ohledem na předávání připojení do kontextu a funkce **databáze. Connection. Open ()** API.</span><span class="sxs-lookup"><span data-stu-id="f94a4-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="f94a4-104">Předávání připojení do kontextu</span><span class="sxs-lookup"><span data-stu-id="f94a4-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="f94a4-105">Chování pro EF5 a starší verze</span><span class="sxs-lookup"><span data-stu-id="f94a4-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="f94a4-106">Existují dva konstruktory, které přijímají připojení:</span><span class="sxs-lookup"><span data-stu-id="f94a4-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="f94a4-107">Je možné je použít, ale musíte vyřešit několik omezení:</span><span class="sxs-lookup"><span data-stu-id="f94a4-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="f94a4-108">Pokud předáte otevřené připojení k některé z těchto kroků, při prvním pokusu o jeho použití se spustí příkaz InvalidOperationException, že nemůže znovu otevřít již otevřené připojení.</span><span class="sxs-lookup"><span data-stu-id="f94a4-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="f94a4-109">Příznak contextOwnsConnection je interpretován tak, zda by mělo být zdrojové připojení úložiště uvolněno při uvolnění kontextu.</span><span class="sxs-lookup"><span data-stu-id="f94a4-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="f94a4-110">Bez ohledu na toto nastavení je připojení úložiště vždy uzavřeno, když je kontext vyřazen.</span><span class="sxs-lookup"><span data-stu-id="f94a4-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="f94a4-111">Takže pokud máte více než jeden DbContext se stejným připojením, jako by se odstranila jakákoli kontextová připojení, bude připojení ukončeno (podobně, pokud jste nastavili kombinaci stávajícího ADO.NET připojení s DbContext, DbContext bude připojení vždycky po vyřazení trvale ukončit). .</span><span class="sxs-lookup"><span data-stu-id="f94a4-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="f94a4-112">Výše uvedené omezení můžete obejít tak, že předáte uzavřená připojení a spustíte jenom kód, který by ho otevřel po vytvoření všech kontextů:</span><span class="sxs-lookup"><span data-stu-id="f94a4-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="f94a4-113">Druhé omezení znamená, že musíte upustit od odstranění všech objektů DbContext, dokud nebudete připraveni na uzavření připojení.</span><span class="sxs-lookup"><span data-stu-id="f94a4-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="f94a4-114">Chování v EF6 a budoucích verzích</span><span class="sxs-lookup"><span data-stu-id="f94a4-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="f94a4-115">V EF6 a budoucích verzích má DbContext stejné dva konstruktory, ale již nevyžaduje, aby bylo připojení předané do konstruktoru uzavřeno při jeho přijetí.</span><span class="sxs-lookup"><span data-stu-id="f94a4-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="f94a4-116">To je teď možné:</span><span class="sxs-lookup"><span data-stu-id="f94a4-116">So this is now possible:</span></span>  

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

<span data-ttu-id="f94a4-117">Příznak contextOwnsConnection nyní také určuje, zda je připojení uzavřeno a odstraněno, když je DbContext vyřazeno.</span><span class="sxs-lookup"><span data-stu-id="f94a4-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="f94a4-118">Takže v předchozím příkladu není připojení uzavřeno, když je kontext vyřazen (řádek 32), protože by byl v předchozích verzích EF, ale místo toho, když je samotné připojení vyřazeno (řádek 40).</span><span class="sxs-lookup"><span data-stu-id="f94a4-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="f94a4-119">Samozřejmě je stále možné, aby DbContext mohl převzít kontrolu nad připojením (stačí nastavit contextOwnsConnection na hodnotu true nebo použít jeden z dalších konstruktorů), pokud to chcete.</span><span class="sxs-lookup"><span data-stu-id="f94a4-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="f94a4-120">Při použití transakcí s tímto novým modelem existují další okolnosti.</span><span class="sxs-lookup"><span data-stu-id="f94a4-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="f94a4-121">Podrobnosti najdete v tématu [práce s transakcemi](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="f94a4-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="f94a4-122">Database. Connection. Open ()</span><span class="sxs-lookup"><span data-stu-id="f94a4-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="f94a4-123">Chování pro EF5 a starší verze</span><span class="sxs-lookup"><span data-stu-id="f94a4-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="f94a4-124">V EF5 a dřívějších verzích je chyba, například **objekt ObjectContext. Connection. State** se neaktualizoval tak, aby odrážel skutečný stav základního připojení úložiště.</span><span class="sxs-lookup"><span data-stu-id="f94a4-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="f94a4-125">Například pokud jste provedli následující kód, můžete vrátit stav **Uzavřeno** , i když je ve skutečnosti **otevřené**základní připojení úložiště.</span><span class="sxs-lookup"><span data-stu-id="f94a4-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="f94a4-126">Samostatně, pokud otevřete připojení databáze voláním Database. Connection. Open (), bude otevřeno až do příštího spuštění dotazu nebo volání cokoli, co vyžaduje připojení k databázi (například SaveChanges ()), ale poté, co příslušné úložiště dostanou. připojení bude zavřeno.</span><span class="sxs-lookup"><span data-stu-id="f94a4-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="f94a4-127">Kontext pak znovu otevřete a znovu uzavřete připojení, kdykoli je potřeba jiná databázová operace:</span><span class="sxs-lookup"><span data-stu-id="f94a4-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="f94a4-128">Chování v EF6 a budoucích verzích</span><span class="sxs-lookup"><span data-stu-id="f94a4-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="f94a4-129">Pro EF6 a budoucí verze jsme převzali přístup, že pokud se volající kód rozhodne otevřít připojení pomocí volajícího kontextu. Database. Connection. Open () pak má dobrý důvod k tomu, že se tak předpokládá, že bude mít za to kontrolu nad otevřením a zavřením připojení a nebude už připojení automaticky ukončeno.</span><span class="sxs-lookup"><span data-stu-id="f94a4-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="f94a4-130">To může potenciálně vést k otevření připojení, která jsou otevřená po dlouhou dobu, takže se bude používat opatrně.</span><span class="sxs-lookup"><span data-stu-id="f94a4-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="f94a4-131">Také jsme aktualizovali kód tak, aby objekt ObjectContext. Connection. State nyní správně sledoval stav základního připojení.</span><span class="sxs-lookup"><span data-stu-id="f94a4-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
