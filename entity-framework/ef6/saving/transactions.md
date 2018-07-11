---
title: Práce s transakcí - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
caps.latest.revision: 3
ms.openlocfilehash: 4238c88cc149458ed11b96a0bf9aaed9aac40b2d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949238"
---
# <a name="working-with-transactions"></a><span data-ttu-id="f63da-102">Práce s transakcí</span><span class="sxs-lookup"><span data-stu-id="f63da-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="f63da-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f63da-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f63da-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="f63da-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="f63da-105">Tento dokument popisuje použití transakcí v EF6, včetně vylepšení, které jsme přidali od EF5 usnadňují práci s transakcí.</span><span class="sxs-lookup"><span data-stu-id="f63da-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="f63da-106">EF nemá ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="f63da-106">What EF does by default</span></span>  

<span data-ttu-id="f63da-107">Ve všech verzích rozhraní Entity Framework při každém spuštění **SaveChanges()** vložit, aktualizovat nebo odstranit v databázi rozhraní framework, zabalit tuto operaci v transakci.</span><span class="sxs-lookup"><span data-stu-id="f63da-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="f63da-108">Tato transakce trvá pouze na dobu nutnou k provedení operace a pak je dokončí.</span><span class="sxs-lookup"><span data-stu-id="f63da-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="f63da-109">Při spouštění jiná taková operace se spustí novou transakci.</span><span class="sxs-lookup"><span data-stu-id="f63da-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="f63da-110">Počínaje EF6 **Database.ExecuteSqlCommand()** ve výchozím nastavení se zabalení příkazu v transakci Pokud jeden není již k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f63da-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="f63da-111">Existují přetížení této metody, které umožňují toto chování přepsat, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="f63da-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="f63da-112">Také v EF6 provádění uložené procedury, které jsou součástí modelu prostřednictvím rozhraní API, jako **ObjectContext.ExecuteFunction()** dělá to samé (s tím rozdílem, že výchozí chování nelze v tuto chvíli přepsat).</span><span class="sxs-lookup"><span data-stu-id="f63da-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="f63da-113">V obou případech je úroveň izolace transakce libovolnou úroveň izolace poskytovatele databáze bude považovat za jeho výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="f63da-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="f63da-114">Ve výchozím nastavení například na SQL serveru jde READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="f63da-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="f63da-115">Entity Framework nezalamuje dotazy v rámci transakce.</span><span class="sxs-lookup"><span data-stu-id="f63da-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="f63da-116">Toto výchozí chování je vhodná pro velké množství uživatelů a pokud to není nutné udělat nic jiného v EF6; stejně jako vždy stačí napište požadovaný kód.</span><span class="sxs-lookup"><span data-stu-id="f63da-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="f63da-117">Ale někteří uživatelé vyžadují větší kontrolu nad jejich transakcí – tento proces je popsán v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="f63da-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="f63da-118">Jak fungují rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f63da-118">How the APIs work</span></span>  

<span data-ttu-id="f63da-119">Před EF6 Entity Framework insisted při otevření připojení k databázi samotného (došlo k výjimce byl předán připojení, které už je otevřený).</span><span class="sxs-lookup"><span data-stu-id="f63da-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="f63da-120">Protože transakce lze spustit pouze na otevření připojení, to znamená, že jediný způsob, jak uživatel zabalit několik operací do jedné transakce se má používat [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) nebo použijte  **ObjectContext.Connection** vlastnosti a volání start **Open()** a **BeginTransaction()** přímo na vrácený **EntityConnection** objekt.</span><span class="sxs-lookup"><span data-stu-id="f63da-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="f63da-121">Kromě toho volání rozhraní API, které kontaktovat databáze by selhat, pokud transakce byl spuštěn na základní připojení k databázi sami.</span><span class="sxs-lookup"><span data-stu-id="f63da-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="f63da-122">Omezení pouze přijímat připojení uzavřené byla odebrána v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f63da-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="f63da-123">Podrobnosti najdete v tématu [správu připojení](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="f63da-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="f63da-124">Počínaje EF6 rozhraní framework teď poskytuje:</span><span class="sxs-lookup"><span data-stu-id="f63da-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="f63da-125">**Database.BeginTransaction()** : jednodušší způsob pro uživatele ke spuštění a dokončení transakcí sami v rámci existující DbContext – povolení několika operací a nelze jej zkombinovat v rámci jedné transakce a proto všechny potvrzené nebo všechny Vrátí zpět jako jeden.</span><span class="sxs-lookup"><span data-stu-id="f63da-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="f63da-126">Také umožňuje uživateli snadněji určit úroveň izolace transakce.</span><span class="sxs-lookup"><span data-stu-id="f63da-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="f63da-127">**Database.UseTransaction()** : umožňuje používat transakce, která byla spuštěna mimo rozhraní Entity Framework uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="f63da-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="f63da-128">Kombinování několik operací do jedné transakce v rámci stejného kontextu</span><span class="sxs-lookup"><span data-stu-id="f63da-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="f63da-129">**Database.BeginTransaction()** má dva přepisy – znak, který se má explicitní [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) a druhou, která nepřijímá žádné argumenty a používá výchozí IsolationLevel z podkladového zprostředkovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="f63da-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="f63da-130">Vrátí obě přepsání **DbContextTransaction** objekt, který poskytuje **Commit()** a **Rollback()** metody, které provádějí potvrzení změn a vrácení zpět na příslušné úložiště transakce.</span><span class="sxs-lookup"><span data-stu-id="f63da-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="f63da-131">**DbContextTransaction** má být uvolněn, jakmile byla potvrzena nebo vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="f63da-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="f63da-132">Snadný způsob, jak dosáhnout jednoho se **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="f63da-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="f63da-133">syntaxe, která automaticky zavolá **Dispose()** při using blokovat dokončení:</span><span class="sxs-lookup"><span data-stu-id="f63da-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
                    try
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
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="f63da-134">Od transakce vyžaduje, že základní úložiště připojení je otevřeno.</span><span class="sxs-lookup"><span data-stu-id="f63da-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="f63da-135">Proto volání Database.BeginTransaction() otevře připojení, pokud již není otevřen.</span><span class="sxs-lookup"><span data-stu-id="f63da-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="f63da-136">Pokud DbContextTransaction otevřít připojení pak dojde k uzavření ho při volání Dispose().</span><span class="sxs-lookup"><span data-stu-id="f63da-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="f63da-137">Předání kontextu existující transakce</span><span class="sxs-lookup"><span data-stu-id="f63da-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="f63da-138">Někdy chcete transakce, které se ještě zvětšuje, v oboru a která zahrnuje operace ve stejné databázi, ale mimo EF úplně.</span><span class="sxs-lookup"><span data-stu-id="f63da-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="f63da-139">K tomu musíte otevřít připojení a spuštění transakce a pak dali pokyn EF a) pro použití připojení k databázi už otevřený a (b) můžete využít existující transakce na toto připojení.</span><span class="sxs-lookup"><span data-stu-id="f63da-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="f63da-140">K tomu musíte definovat a použijte konstruktor na vaší třídy kontextu, který dědí z jednoho z konstruktorů DbContext, což trvat i) existující parametr připojení a ii) na contextOwnsConnection logická.</span><span class="sxs-lookup"><span data-stu-id="f63da-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="f63da-141">Příznak contextOwnsConnection musí být nastavena na hodnotu NEPRAVDA, pokud je volána v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="f63da-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="f63da-142">Je to důležité proto informuje Entity Framework, že se po dokončení se s ním neměli zavírat připojení (například. Viz řádek 4 níže):</span><span class="sxs-lookup"><span data-stu-id="f63da-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="f63da-143">Kromě toho musí začínat transakce sami (včetně IsolationLevel, pokud chcete, aby se zabránilo výchozí nastavení) a umožní Entity Framework, že je již spuštěna na připojení k existující transakce (viz řádek 33 níže).</span><span class="sxs-lookup"><span data-stu-id="f63da-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="f63da-144">Potom můžete libovolně u provádění operací databáze přímo na objekt SqlConnection samotné nebo objekt dbcontext.</span><span class="sxs-lookup"><span data-stu-id="f63da-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="f63da-145">Všechny tyto operace jsou spuštěny v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="f63da-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="f63da-146">Můžete převzít odpovědnost pro potvrzení nebo vrácení transakce a volání Dispose() na ni, stejně jako pro zavření a rušení připojení databáze.</span><span class="sxs-lookup"><span data-stu-id="f63da-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="f63da-147">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f63da-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

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
                   try
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
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="f63da-148">Odstraňování transakce</span><span class="sxs-lookup"><span data-stu-id="f63da-148">Clearing up the transaction</span></span>

<span data-ttu-id="f63da-149">Můžete předat hodnotu null Database.UseTransaction() zrušte Entity Framework znalost aktuální transakce.</span><span class="sxs-lookup"><span data-stu-id="f63da-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="f63da-150">Entity Framework bude ani potvrzení ani vrácení stávajících transakcí při tomto, proto používejte opatrně a pouze v případě, že jste si jisti, to je, co chcete udělat.</span><span class="sxs-lookup"><span data-stu-id="f63da-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="f63da-151">Chyby v transakci</span><span class="sxs-lookup"><span data-stu-id="f63da-151">Errors in UseTransaction</span></span>

<span data-ttu-id="f63da-152">Zobrazí se výjimka z Database.UseTransaction() Pokud předáte transakce při:</span><span class="sxs-lookup"><span data-stu-id="f63da-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="f63da-153">Entity Framework již má existující transakce</span><span class="sxs-lookup"><span data-stu-id="f63da-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="f63da-154">Entity Framework je již zpracovávána v rámci objekt TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f63da-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="f63da-155">V transakci předaný objekt připojení má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="f63da-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="f63da-156">To znamená, že transakce není přidružena připojení – to je obvykle známkou toho, že této transakce již byla dokončena</span><span class="sxs-lookup"><span data-stu-id="f63da-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="f63da-157">Objekt připojení v předán transakce neodpovídá připojení rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f63da-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="f63da-158">Použití transakcí s dalšími funkcemi</span><span class="sxs-lookup"><span data-stu-id="f63da-158">Using transactions with other features</span></span>  

<span data-ttu-id="f63da-159">Tato část podrobně popisuje, jak výše uvedené transakce interakci s:</span><span class="sxs-lookup"><span data-stu-id="f63da-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="f63da-160">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="f63da-160">Connection resiliency</span></span>  
- <span data-ttu-id="f63da-161">Asynchronní metody</span><span class="sxs-lookup"><span data-stu-id="f63da-161">Asynchronous methods</span></span>  
- <span data-ttu-id="f63da-162">Transakce TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f63da-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="f63da-163">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="f63da-163">Connection Resiliency</span></span>  

<span data-ttu-id="f63da-164">Novou funkci odolnosti proti chybám připojení nefunguje s uživatelem iniciované transakce.</span><span class="sxs-lookup"><span data-stu-id="f63da-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="f63da-165">Podrobnosti najdete v tématu [omezení strategie opakování pokusu o spuštění](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span><span class="sxs-lookup"><span data-stu-id="f63da-165">For details, see [Limitations with Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="f63da-166">Asynchronní programování</span><span class="sxs-lookup"><span data-stu-id="f63da-166">Asynchronous Programming</span></span>  

<span data-ttu-id="f63da-167">Přístup uvedených v předchozích částech potřebuje žádné další možnosti nebo nastavení pro práci s [asynchronního dotazu a uložit metody](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="f63da-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="f63da-168">Ale mějte na paměti, že v závislosti na tom, co dělat v rámci asynchronní metody může být výsledkem dlouhotrvajících transakcí – které pak může způsobit zablokování nebo blokování, což je vhodná pro výkon celkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f63da-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="f63da-169">Transakce TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f63da-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="f63da-170">Před EF6 doporučený způsob poskytnutí větší rozsah transakce byla použití objektu TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="f63da-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="f63da-171">Připojení SqlConnection a Entity Framework jak použije okolí transakce TransactionScope a proto se měly potvrdit společně.</span><span class="sxs-lookup"><span data-stu-id="f63da-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="f63da-172">Od verze rozhraní .NET 4.5.1 objekt TransactionScope byl aktualizován na také pracovat prostřednictvím použití asynchronních metod [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) výčtu:</span><span class="sxs-lookup"><span data-stu-id="f63da-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="f63da-173">Stále existují určitá omezení přístupu TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="f63da-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="f63da-174">Vyžaduje .NET 4.5.1 nebo novější pro práci s asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="f63da-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="f63da-175">Ve scénářích cloudové jej nelze použít, pokud jste si jisti, budete mít jeden a pouze jeden připojení (cloudové scénáře nepodporují distribuované transakce).</span><span class="sxs-lookup"><span data-stu-id="f63da-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="f63da-176">Ji nelze kombinovat s přístupem Database.UseTransaction() z předchozí části.</span><span class="sxs-lookup"><span data-stu-id="f63da-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="f63da-177">Výjimky vyvolá-li vydávat žádné DDL a nepovolili distribuované transakce ve službě MSDTC.</span><span class="sxs-lookup"><span data-stu-id="f63da-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="f63da-178">Výhody TransactionScope přístupu:</span><span class="sxs-lookup"><span data-stu-id="f63da-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="f63da-179">Se automaticky upgraduje místní transakce na distribuovanou transakci Pokud provedete víc než jedno připojení k dané databázi nebo připojení k jedné databáze v kombinaci s připojení k jiné databázi v rámci jedné transakce (Poznámka: je zapotřebí Služba MSDTC službu nakonfigurované tak, aby distribuovaných transakcí, aby to fungovalo).</span><span class="sxs-lookup"><span data-stu-id="f63da-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="f63da-180">Usnadnění psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="f63da-180">Ease of coding.</span></span> <span data-ttu-id="f63da-181">Pokud dáváte přednost transakci okolí a zpracovávány implicitně na pozadí, spíše než explicitně v části ovládací prvek pak TransactionScope přístup může vyhovovat je lepší.</span><span class="sxs-lookup"><span data-stu-id="f63da-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="f63da-182">Stručně řečeno, nové Database.BeginTransaction() a rozhraní API Database.UseTransaction() výše přístup objekt TransactionScope byl již není nezbytné pro většinu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f63da-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="f63da-183">Pokud budete nadále používat TransactionScope pak mějte na paměti z výše uvedených omezení.</span><span class="sxs-lookup"><span data-stu-id="f63da-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="f63da-184">Doporučujeme použít přístup popsaných v předchozích částech místo, kde je to možné.</span><span class="sxs-lookup"><span data-stu-id="f63da-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
