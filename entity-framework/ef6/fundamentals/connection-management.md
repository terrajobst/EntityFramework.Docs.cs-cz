---
title: Správa připojení - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489333"
---
# <a name="connection-management"></a><span data-ttu-id="25c59-102">Správa připojení</span><span class="sxs-lookup"><span data-stu-id="25c59-102">Connection management</span></span>
<span data-ttu-id="25c59-103">Tato stránka popisuje chování Entity Framework s ohledem na předávání připojení do kontextu a funkce **Database.Connection.Open()** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="25c59-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="25c59-104">Předávání připojení ke kontextu</span><span class="sxs-lookup"><span data-stu-id="25c59-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="25c59-105">Chování EF5 a starší verze</span><span class="sxs-lookup"><span data-stu-id="25c59-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="25c59-106">Existují dva konstruktory, které přijímají připojení:</span><span class="sxs-lookup"><span data-stu-id="25c59-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="25c59-107">Je možné použít, ale budete muset vyřešit několik omezení:</span><span class="sxs-lookup"><span data-stu-id="25c59-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="25c59-108">Pokud předáte otevřené připojení na jeden z následujících pak při prvním rozhraní se pokusí použít, pokud je vyvolána výjimka InvalidOperationException říká ji nelze znovu otevřít již otevřeného připojení.</span><span class="sxs-lookup"><span data-stu-id="25c59-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="25c59-109">Příznak contextOwnsConnection je interpretována jako jestli nadřízené připojení úložiště by mělo být uvolněno při uvolnění kontextu.</span><span class="sxs-lookup"><span data-stu-id="25c59-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="25c59-110">Ale bez ohledu na to, že nastavení připojení úložiště je vždy uzavřen, při uvolnění kontextu.</span><span class="sxs-lookup"><span data-stu-id="25c59-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="25c59-111">Takže pokud máte více než jeden DbContext pomocí stejného připojení uvolnění podle kontextu, je nejprve připojení zavře (podobně Pokud jste kombinovat existující připojení ADO.NET s DbContext, DbContext vždy připojení zavře při jeho uvolnění) .</span><span class="sxs-lookup"><span data-stu-id="25c59-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="25c59-112">Je možné obejít omezení na první nad úspěšným ukončeném připojení a pouze provádění kódu, který by se otevřely se po všech kontextech byly vytvořeny:</span><span class="sxs-lookup"><span data-stu-id="25c59-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="25c59-113">Druhé omezení jenom znamená, že potřebujete nepoužívejte disposing některý z objektů DbContext, dokud nebudete připraveni pro připojení bude uzavřen.</span><span class="sxs-lookup"><span data-stu-id="25c59-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="25c59-114">Chování v budoucích verzích a EF6</span><span class="sxs-lookup"><span data-stu-id="25c59-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="25c59-115">V budoucích verzích a EF6 uvolněn objekt DbContext má stejné dva konstruktory, ale už nevyžaduje, aby předaný konstruktoru připojení ukončeno při obdržení.</span><span class="sxs-lookup"><span data-stu-id="25c59-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="25c59-116">Tohle je teď možné:</span><span class="sxs-lookup"><span data-stu-id="25c59-116">So this is now possible:</span></span>  

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

<span data-ttu-id="25c59-117">Také příznak contextOwnsConnection teď řídí, zda připojení je uzavřen i uvolněno při uvolnění uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="25c59-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="25c59-118">Takže ve výše uvedeném příkladu připojení není ukončeno v kontextu uvolněn (řádek 32), jako by byl v předchozích verzích EF, ale místo toho, když je uvolněn samotné připojení (40 řádků).</span><span class="sxs-lookup"><span data-stu-id="25c59-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="25c59-119">Samozřejmě je stále možné pro objekt DbContext převzít kontrolu nad připojení (pouze sada contextOwnsConnection hodnotu true, nebo použijte jednu z dalších konstruktory) Pokud tedy chcete.</span><span class="sxs-lookup"><span data-stu-id="25c59-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="25c59-120">Při použití transakcí se tento nový model se několik dalších důležitých informací.</span><span class="sxs-lookup"><span data-stu-id="25c59-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="25c59-121">Podrobnosti najdete v tématu [práce s transakcí](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="25c59-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="25c59-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="25c59-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="25c59-123">Chování EF5 a starší verze</span><span class="sxs-lookup"><span data-stu-id="25c59-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="25c59-124">V dřívějších verzích a EF5 je chyba tak, aby **ObjectContext.Connection.State** nebyla aktualizována tak, aby odrážely o skutečném stavu nadřízené připojení úložiště.</span><span class="sxs-lookup"><span data-stu-id="25c59-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="25c59-125">Například pokud jste spustili následující kód je může být vrácen stav **uzavřeno** i v případě, že ve skutečnosti základní ukládání připojení je **otevřít**.</span><span class="sxs-lookup"><span data-stu-id="25c59-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="25c59-126">Samostatně, je-li otevřít připojení k databázi pomocí volání Database.Connection.Open() bude otevřen až při příštím spuštění dotazu, nebo volat nic, která vyžaduje připojení k databázi (například SaveChanges()) ale za základní ukládání, připojení bude ukončeno.</span><span class="sxs-lookup"><span data-stu-id="25c59-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="25c59-127">Kontext se pak znovu otevřete a znovu ukončete připojení pokaždé, když jiná operace databáze se vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="25c59-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="25c59-128">Chování v budoucích verzích a EF6</span><span class="sxs-lookup"><span data-stu-id="25c59-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="25c59-129">EF6 a budoucí verze přesměrovali jsme si přístup, pokud volající kód zvolí možnost otevření připojení pomocí volání kontextu. Database.Connection.Open() pak má dobrý důvod pro to a rozhraní bude předpokládat, že chce kontrolu nad otevření a zavření připojení a se už připojení automaticky zavře.</span><span class="sxs-lookup"><span data-stu-id="25c59-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="25c59-130">To může potenciálně vést k připojení, které jsou otevřené pro dlouho proto používejte opatrně.</span><span class="sxs-lookup"><span data-stu-id="25c59-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="25c59-131">Také jsme aktualizovali kód tak, aby ObjectContext.Connection.State nyní uchovává informace o stavu nadřízené připojení správně.</span><span class="sxs-lookup"><span data-stu-id="25c59-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
