---
title: Práce s transakcemi – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306527"
---
# <a name="working-with-transactions"></a><span data-ttu-id="d1673-102">Práce s transakcemi</span><span class="sxs-lookup"><span data-stu-id="d1673-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="d1673-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d1673-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="d1673-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="d1673-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="d1673-105">Tento dokument popisuje použití transakcí v EF6, včetně vylepšení, která jsme přidali od EF5, aby bylo snadné pracovat s transakcemi.</span><span class="sxs-lookup"><span data-stu-id="d1673-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="d1673-106">Jaké má EF ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="d1673-106">What EF does by default</span></span>  

<span data-ttu-id="d1673-107">Ve všech verzích Entity Framework, kdykoli spustíte **SaveChanges ()** pro vložení, aktualizaci nebo odstranění databáze, rozhraní zalomí tuto operaci v transakci.</span><span class="sxs-lookup"><span data-stu-id="d1673-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="d1673-108">Tato transakce trvá pouze dostatečně dlouhou dobu pro provedení operace a poté dokončí.</span><span class="sxs-lookup"><span data-stu-id="d1673-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="d1673-109">Při provádění jiné takové operace se spustí nová transakce.</span><span class="sxs-lookup"><span data-stu-id="d1673-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="d1673-110">Počínaje příkazem EF6 **Database. ExecuteSqlCommand ()** se ve výchozím nastavení zabalí příkaz do transakce, pokud ještě nikdo neexistoval.</span><span class="sxs-lookup"><span data-stu-id="d1673-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="d1673-111">Existují přetížení této metody, které vám umožní toto chování přepsat, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="d1673-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="d1673-112">Také v EF6 provádění uložených procedur obsažených v modelu prostřednictvím rozhraní API, jako je **ObjectContext. ExecuteFunction ()** , je stejné (s tím rozdílem, že výchozí chování nelze v okamžiku přepsat).</span><span class="sxs-lookup"><span data-stu-id="d1673-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="d1673-113">V obou případech je úroveň izolace transakce libovolnou úrovní izolace, kterou poskytovatel databáze považuje za výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="d1673-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="d1673-114">Ve výchozím nastavení je to například u SQL Server se jedná o POTVRZENé čtení.</span><span class="sxs-lookup"><span data-stu-id="d1673-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="d1673-115">Entity Framework nebalí dotazy v transakci.</span><span class="sxs-lookup"><span data-stu-id="d1673-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="d1673-116">Tato výchozí funkce je vhodná pro velké množství uživatelů, a pokud není potřeba nic dělat jinak v EF6; Stačí napsat kód tak, jak jste to trvali.</span><span class="sxs-lookup"><span data-stu-id="d1673-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="d1673-117">Někteří uživatelé však vyžadují větší kontrolu nad svými transakcemi – to je popsáno v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="d1673-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="d1673-118">Jak fungují rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d1673-118">How the APIs work</span></span>  

<span data-ttu-id="d1673-119">Před EF6 Entity Framework nastaly při otevírání samotného databázového připojení (vyvolalo výjimku, pokud bylo předáno připojení, které již bylo otevřeno).</span><span class="sxs-lookup"><span data-stu-id="d1673-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="d1673-120">Vzhledem k tomu, že transakci lze spustit pouze v otevřeném připojení, což znamená, že jediný způsob, jakým může uživatel zabalit několik operací do jedné transakce, byl buď použit jako [objekt TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) , nebo použít vlastnost **ObjectContext. Connection** a spustit volání metody **Open ()** a **BeginTransaction ()** přímo na vráceném objektu **EntityConnection** .</span><span class="sxs-lookup"><span data-stu-id="d1673-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="d1673-121">Kromě toho volání rozhraní API, která kontaktovala databázi, by nebylo úspěšné, pokud jste spustili transakci na podkladovém připojení databáze sami.</span><span class="sxs-lookup"><span data-stu-id="d1673-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="d1673-122">Omezení pouze přijetí uzavřených připojení bylo v Entity Framework 6 odebráno.</span><span class="sxs-lookup"><span data-stu-id="d1673-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="d1673-123">Podrobnosti najdete v tématu [Správa připojení](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="d1673-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="d1673-124">Počínaje EF6 Framework nyní poskytuje:</span><span class="sxs-lookup"><span data-stu-id="d1673-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="d1673-125">**Database.BeginTransaction()** : Jednodušší způsob, jak uživatel spustit a dokončit transakce samotné v rámci existující DbContext – umožňuje kombinovat několik operací v rámci stejné transakce a tedy všechny potvrzené nebo všechny provedené zpátky jako jeden.</span><span class="sxs-lookup"><span data-stu-id="d1673-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="d1673-126">Umožňuje také uživateli snadněji zadat úroveň izolace transakce.</span><span class="sxs-lookup"><span data-stu-id="d1673-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="d1673-127">**Database. UseTransaction ()** : umožňuje DbContext použít transakci, která byla spuštěna mimo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d1673-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="d1673-128">Kombinování několika operací do jedné transakce v rámci stejného kontextu</span><span class="sxs-lookup"><span data-stu-id="d1673-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="d1673-129">**Database. BeginTransaction ()** má dvě přepsání – jedna, která přijímá explicitní [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) a jednu, která nepřijímá žádné argumenty a používá výchozí IsolationLevel z podkladového poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="d1673-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="d1673-130">Obě přepsání vrátí objekt **DbContextTransaction** , který poskytuje metody **potvrzení ()** a **vrácení zpět ()** , které provádějí potvrzení a vrácení zpět v podkladové transakci úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1673-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="d1673-131">**DbContextTransaction** má být uvolněna, jakmile bude potvrzena nebo vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="d1673-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="d1673-132">Jedním ze způsobů, jak toho dosáhnout, je **použití (...). {...}**</span><span class="sxs-lookup"><span data-stu-id="d1673-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="d1673-133">syntaxe, která bude automaticky volat **Dispose ()** při dokončení bloku using:</span><span class="sxs-lookup"><span data-stu-id="d1673-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    context.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'"
                        );

                    var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }

                    context.SaveChanges();

                    dbContextTransaction.Commit();
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="d1673-134">Zahájení transakce vyžaduje otevření základního připojení úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1673-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="d1673-135">Proto voláním Database. BeginTransaction () otevře připojení, pokud ještě není otevřené.</span><span class="sxs-lookup"><span data-stu-id="d1673-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="d1673-136">Pokud DbContextTransaction připojení otevřelo, zavře se, až se zavolá Dispose ().</span><span class="sxs-lookup"><span data-stu-id="d1673-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="d1673-137">Předání existující transakce do kontextu</span><span class="sxs-lookup"><span data-stu-id="d1673-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="d1673-138">Někdy byste chtěli být transakce, která je ještě širší v oboru a která zahrnuje operace ve stejné databázi, ale mimo EF zcela.</span><span class="sxs-lookup"><span data-stu-id="d1673-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="d1673-139">Aby to bylo možné, musíte otevřít připojení a spustit transakci sami, sdělit EF a) pro použití již otevřeného připojení k databázi a b) pro použití existující transakce na tomto připojení.</span><span class="sxs-lookup"><span data-stu-id="d1673-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="d1673-140">Chcete-li to provést, je nutné definovat a použít konstruktor pro třídu Context, která dědí z jednoho z konstruktorů DbContext, který přebírá i stávající parametr připojení a II) contextOwnsConnectionou logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d1673-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="d1673-141">Při volání v tomto scénáři musí být příznak contextOwnsConnection nastaven na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="d1673-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="d1673-142">To je důležité, protože informují Entity Framework, že by nezavřeli připojení, když s ním bude provedeno (například na řádku 4 níže):</span><span class="sxs-lookup"><span data-stu-id="d1673-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="d1673-143">Kromě toho je nutné spustit transakci sami sami (včetně IsolationLevel, pokud chcete zabránit výchozímu nastavení) a nechat Entity Framework, že již existuje existující transakce, která již začala na připojení (viz řádek 33 níže).</span><span class="sxs-lookup"><span data-stu-id="d1673-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="d1673-144">Pak můžete spouštět databázové operace buď přímo na samotném SqlConnection, nebo na DbContext.</span><span class="sxs-lookup"><span data-stu-id="d1673-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="d1673-145">Všechny tyto operace jsou spouštěny v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="d1673-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="d1673-146">Přinesete zodpovědnost za potvrzení nebo vrácení transakce a pro volání metody Dispose () na ni a také pro uzavření a zrušení připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="d1673-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="d1673-147">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d1673-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   var sqlCommand = new SqlCommand();
                   sqlCommand.Connection = conn;
                   sqlCommand.Transaction = sqlTxn;
                   sqlCommand.CommandText =
                       @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'";
                   sqlCommand.ExecuteNonQuery();

                   using (var context =  
                     new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        context.Database.UseTransaction(sqlTxn);

                        var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                       context.SaveChanges();
                    }

                    sqlTxn.Commit();
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="d1673-148">Vymazávání transakce</span><span class="sxs-lookup"><span data-stu-id="d1673-148">Clearing up the transaction</span></span>

<span data-ttu-id="d1673-149">Můžete předat hodnotu null objektu Database. UseTransaction (), aby bylo jasné, že se Entity Framework aktuální transakce.</span><span class="sxs-lookup"><span data-stu-id="d1673-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="d1673-150">Pokud to uděláte, Entity Framework nebude ani po provedení této akce potvrdit ani vrátit zpátky existující transakci, takže používejte s péčí a jenom pokud jste si jisti, co chcete udělat.</span><span class="sxs-lookup"><span data-stu-id="d1673-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="d1673-151">Chyby v UseTransaction</span><span class="sxs-lookup"><span data-stu-id="d1673-151">Errors in UseTransaction</span></span>

<span data-ttu-id="d1673-152">Výjimka z databáze. UseTransaction () se zobrazí, Pokud předáte transakci v těchto případech:</span><span class="sxs-lookup"><span data-stu-id="d1673-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="d1673-153">Entity Framework už má existující transakci.</span><span class="sxs-lookup"><span data-stu-id="d1673-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="d1673-154">Entity Framework už v rámci objektu TransactionScope funguje.</span><span class="sxs-lookup"><span data-stu-id="d1673-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="d1673-155">Objekt připojení v předané transakci má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d1673-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="d1673-156">To znamená, že transakce není přidružena k připojení – obvykle se jedná o znaménko, že tato transakce již byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="d1673-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="d1673-157">Objekt připojení v předané transakci se neshoduje s připojením Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d1673-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="d1673-158">Používání transakcí s jinými funkcemi</span><span class="sxs-lookup"><span data-stu-id="d1673-158">Using transactions with other features</span></span>  

<span data-ttu-id="d1673-159">Tato část podrobně popisuje, jak výše uvedené transakce spolupracují:</span><span class="sxs-lookup"><span data-stu-id="d1673-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="d1673-160">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="d1673-160">Connection resiliency</span></span>  
- <span data-ttu-id="d1673-161">Asynchronní metody</span><span class="sxs-lookup"><span data-stu-id="d1673-161">Asynchronous methods</span></span>  
- <span data-ttu-id="d1673-162">Transakce TransactionScope</span><span class="sxs-lookup"><span data-stu-id="d1673-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="d1673-163">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="d1673-163">Connection Resiliency</span></span>  

<span data-ttu-id="d1673-164">Nová funkce odolnosti připojení nefunguje s transakcemi iniciované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="d1673-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="d1673-165">Podrobnosti najdete v tématu [opakování strategií provádění](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="d1673-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="d1673-166">Asynchronní programování</span><span class="sxs-lookup"><span data-stu-id="d1673-166">Asynchronous Programming</span></span>  

<span data-ttu-id="d1673-167">Přístup, který je popsaný v předchozích částech, nepotřebuje žádné další možnosti ani nastavení pro práci [s asynchronními dotazy a](~/ef6/fundamentals/async.md
)metodami úspory.</span><span class="sxs-lookup"><span data-stu-id="d1673-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="d1673-168">Mějte ale na paměti, že v závislosti na tom, co v rámci asynchronních metod provedete, to může způsobit dlouhotrvající transakce – což může způsobit zablokování nebo blokování, které je pro výkon celé aplikace špatné.</span><span class="sxs-lookup"><span data-stu-id="d1673-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="d1673-169">Transakce TransactionScope</span><span class="sxs-lookup"><span data-stu-id="d1673-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="d1673-170">Před EF6 doporučeným způsobem poskytnutí větších transakcí rozsahu bylo použití objektu TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="d1673-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

<span data-ttu-id="d1673-171">SqlConnection a Entity Framework by používaly ambientní transakci TransactionScope a měla by se tedy považovat za společně.</span><span class="sxs-lookup"><span data-stu-id="d1673-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="d1673-172">Počínaje rozhraním .NET 4.5.1 TransactionScope bylo aktualizováno, aby fungovalo také s asynchronními metodami prostřednictvím použití výčtu [nastavením TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) :</span><span class="sxs-lookup"><span data-stu-id="d1673-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

<span data-ttu-id="d1673-173">Existují i určitá omezení přístupu k objektu TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="d1673-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="d1673-174">Pro práci s asynchronními metodami vyžaduje rozhraní .NET 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d1673-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="d1673-175">Nedá se použít v cloudových scénářích, pokud si nejste jistí, že máte jenom jedno připojení (cloudové scénáře nepodporují distribuované transakce).</span><span class="sxs-lookup"><span data-stu-id="d1673-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="d1673-176">Nejde kombinovat s přístupem k databázi. UseTransaction () v předchozích částech.</span><span class="sxs-lookup"><span data-stu-id="d1673-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="d1673-177">Vyvolá výjimky, pokud vydáte skript DDL a nepovolíte distribuované transakce prostřednictvím služby MSDTC.</span><span class="sxs-lookup"><span data-stu-id="d1673-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="d1673-178">Výhody přístupu s objektem TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="d1673-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="d1673-179">Pokud provedete více než jedno připojení k dané databázi nebo zkombinujete připojení k jedné databázi s připojením k jiné databázi v rámci stejné transakce, bude automaticky upgradována místní transakce na distribuovanou transakci (Poznámka: musíte mít Služba MSDTC nakonfigurovaná tak, aby umožňovala fungování distribuovaných transakcí.</span><span class="sxs-lookup"><span data-stu-id="d1673-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="d1673-180">Snadné kódování.</span><span class="sxs-lookup"><span data-stu-id="d1673-180">Ease of coding.</span></span> <span data-ttu-id="d1673-181">Pokud upřednostňujete, aby transakce byla ambientní a řešena implicitně na pozadí místo explicitního řízení, může přístup k objektům TransactionScope lépe vyhovovat.</span><span class="sxs-lookup"><span data-stu-id="d1673-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="d1673-182">V souhrnu s novými rozhraními API Database. BeginTransaction () a Database. UseTransaction () výše už není přístup k objektům TransactionScope nutný pro většinu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d1673-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="d1673-183">Pokud budete pokračovat v používání objektu TransactionScope, pamatujte na výše uvedená omezení.</span><span class="sxs-lookup"><span data-stu-id="d1673-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="d1673-184">Místo toho doporučujeme použít postup popsaný v předchozích částech.</span><span class="sxs-lookup"><span data-stu-id="d1673-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
