---
title: Zpracování chyb potvrzení transakce – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417368"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="afe3e-102">Zpracování chyb potvrzení transakce</span><span class="sxs-lookup"><span data-stu-id="afe3e-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="afe3e-103">**EF 6.1 a vyšší pouze** – funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="afe3e-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="afe3e-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="afe3e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="afe3e-105">V rámci 6,1 jsme zavedli novou funkci odolnosti připojení pro EF: schopnost detekovat a obnovovat automaticky v případě, že při přechodném selhání připojení dojde k ovlivnění potvrzení transakcí.</span><span class="sxs-lookup"><span data-stu-id="afe3e-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="afe3e-106">Úplné podrobnosti o tomto scénáři se nejlépe popisují v příspěvku blogu [SQL Database možnosti připojení a problému idempotence](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="afe3e-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="afe3e-107">Ve shrnutí je scénář, že když je vyvolána výjimka během potvrzení transakce, existují dvě možné příčiny:</span><span class="sxs-lookup"><span data-stu-id="afe3e-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="afe3e-108">Potvrzení transakce na serveru selhalo.</span><span class="sxs-lookup"><span data-stu-id="afe3e-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="afe3e-109">Potvrzení transakce na serveru bylo úspěšné, ale potíže s připojením zabránily klientovi v navázání oznámení o úspěšnosti.</span><span class="sxs-lookup"><span data-stu-id="afe3e-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="afe3e-110">Když se první situace stane aplikací nebo uživatel může operaci zopakovat, ale když dojde k druhé situaci, je třeba se vyhnout opakování a aplikace by se mohla automaticky obnovit.</span><span class="sxs-lookup"><span data-stu-id="afe3e-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="afe3e-111">Důvodem je to, že bez možnosti rozpoznat, co byl skutečným důvodem, že se během potvrzení nahlásila výjimka, aplikace nemůže zvolit správný průběh akce.</span><span class="sxs-lookup"><span data-stu-id="afe3e-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="afe3e-112">Nová funkce v EF 6,1 umožňuje EF pořídit databázi v případě úspěchu transakce a provedení správného postupu transparentního postupu.</span><span class="sxs-lookup"><span data-stu-id="afe3e-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="afe3e-113">Používání funkce</span><span class="sxs-lookup"><span data-stu-id="afe3e-113">Using the feature</span></span>  

<span data-ttu-id="afe3e-114">Aby bylo možné povolit funkci, kterou potřebujete, uveďte volání [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) v konstruktoru svého **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="afe3e-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="afe3e-115">Pokud neznáte **DbConfiguration**, přečtěte si téma [Konfigurace na základě kódu](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="afe3e-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="afe3e-116">Tato funkce se dá použít v kombinaci s automatickými pokusy, které jsme zavedli v EF6, což vám pomůže v situaci, kdy se transakci ve skutečnosti nepodařilo zapsat na serveru z důvodu přechodného selhání:</span><span class="sxs-lookup"><span data-stu-id="afe3e-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="afe3e-117">Jak jsou sledovány transakce</span><span class="sxs-lookup"><span data-stu-id="afe3e-117">How transactions are tracked</span></span>  

<span data-ttu-id="afe3e-118">Když je funkce povolená, EF automaticky přidá novou tabulku do databáze s názvem **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="afe3e-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="afe3e-119">V této tabulce je vložen nový řádek pokaždé, když je transakce vytvořena pomocí EF a tento řádek je kontrolován pro existenci, pokud dojde k selhání transakce během zápisu.</span><span class="sxs-lookup"><span data-stu-id="afe3e-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="afe3e-120">I když EF dovede k vyřazení řádků z tabulky, když už nejsou potřeba, může tabulka růst, jestli se aplikace ukončí předčasně a z tohoto důvodu možná budete muset tabulku v některých případech vyprázdnit ručně.</span><span class="sxs-lookup"><span data-stu-id="afe3e-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="afe3e-121">Postup zpracování selhání potvrzení s předchozími verzemi</span><span class="sxs-lookup"><span data-stu-id="afe3e-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="afe3e-122">Před EF 6,1 neexistuje mechanismus pro zpracování selhání potvrzení v produktu EF.</span><span class="sxs-lookup"><span data-stu-id="afe3e-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="afe3e-123">Existuje několik způsobů, jak v této situaci řešit, které je možné použít pro předchozí verze EF6:</span><span class="sxs-lookup"><span data-stu-id="afe3e-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="afe3e-124">Možnost 1 – nedělat nic</span><span class="sxs-lookup"><span data-stu-id="afe3e-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="afe3e-125">Pravděpodobnost selhání připojení během potvrzení transakce je nízká, takže může být přijatelné, aby vaše aplikace v případě, že k této situaci skutečně dojde, nedošlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="afe3e-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="afe3e-126">Možnost 2 – obnovení stavu pomocí databáze</span><span class="sxs-lookup"><span data-stu-id="afe3e-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="afe3e-127">Zahodit aktuální DbContext</span><span class="sxs-lookup"><span data-stu-id="afe3e-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="afe3e-128">Vytvořte novou DbContext a obnovte stav aplikace z databáze.</span><span class="sxs-lookup"><span data-stu-id="afe3e-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="afe3e-129">Informujte uživatele, že poslední operace nemusí být úspěšně dokončená.</span><span class="sxs-lookup"><span data-stu-id="afe3e-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="afe3e-130">Možnost 3 – ruční sledování transakce</span><span class="sxs-lookup"><span data-stu-id="afe3e-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="afe3e-131">Přidejte nesledovanou tabulku do databáze použité ke sledování stavu transakcí.</span><span class="sxs-lookup"><span data-stu-id="afe3e-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="afe3e-132">Vloží řádek do tabulky na začátku každé transakce.</span><span class="sxs-lookup"><span data-stu-id="afe3e-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="afe3e-133">Pokud během potvrzení dojde k chybě připojení, vyhledejte přítomnost odpovídajícího řádku v databázi.</span><span class="sxs-lookup"><span data-stu-id="afe3e-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="afe3e-134">Pokud je řádek k dispozici, pokračujte normálně, protože transakce byla úspěšně potvrzena.</span><span class="sxs-lookup"><span data-stu-id="afe3e-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="afe3e-135">Pokud řádek chybí, použijte k opakování aktuální operace strategii spouštění.</span><span class="sxs-lookup"><span data-stu-id="afe3e-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="afe3e-136">Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, abyste se vyhnuli nárůstu tabulky.</span><span class="sxs-lookup"><span data-stu-id="afe3e-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="afe3e-137">[Tento Blogový příspěvek](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) obsahuje vzorový kód pro dosažení tohoto SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="afe3e-137">[This blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
