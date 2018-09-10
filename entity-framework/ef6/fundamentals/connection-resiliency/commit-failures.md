---
title: Zpracování selhání potvrzení transakce - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: f912777104c2e925122c05046d4d65660de8b8a8
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250853"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="f07b3-102">Zpracování selhání potvrzení transakce</span><span class="sxs-lookup"><span data-stu-id="f07b3-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="f07b3-103">**EF6.1 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="f07b3-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="f07b3-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="f07b3-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="f07b3-105">Jako součást 6.1 Představujeme novou funkci odolnosti proti chybám připojení pro EF: schopnost detekovat a automatického obnovení po selhání přechodné připojení ovlivnit potvrzení potvrzení transakcí.</span><span class="sxs-lookup"><span data-stu-id="f07b3-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="f07b3-106">Všechny podrobnosti scénáře jsou nejlépe popisuje tento blogový příspěvek [připojení k databázi SQL a problém idempotence](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="f07b3-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="f07b3-107">Stručně řečeno tento scénář je, že když je výjimka vyvolána během zápisu transakce, existují dva možné příčiny:</span><span class="sxs-lookup"><span data-stu-id="f07b3-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="f07b3-108">Potvrzení transakce nejde na serveru</span><span class="sxs-lookup"><span data-stu-id="f07b3-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="f07b3-109">Potvrzení transakce byla úspěšně dokončena na serveru, ale problém s připojením zabránila oznámením o úspěšné dosažení klienta</span><span class="sxs-lookup"><span data-stu-id="f07b3-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="f07b3-110">Při první situace nastane u uživatele nebo aplikace můžete operaci opakovat, ale pokud druhá situace nastane, mělo by se vyhnout opakovaných pokusů a aplikace může automaticky obnovit.</span><span class="sxs-lookup"><span data-stu-id="f07b3-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="f07b3-111">Na výzvu je, že kdybychom neměli možnost zjistit, co byl skutečné důvod nahlášení výjimky během zápisu, aplikace nejde zvolit, ta správná cesta akce.</span><span class="sxs-lookup"><span data-stu-id="f07b3-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="f07b3-112">Nová funkce v EF 6.1 umožňuje EF zkontrolujte s databází, pokud transakce byla úspěšná a transparentně trvat ta správná cesta akce.</span><span class="sxs-lookup"><span data-stu-id="f07b3-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="f07b3-113">Pomocí funkce</span><span class="sxs-lookup"><span data-stu-id="f07b3-113">Using the feature</span></span>  

<span data-ttu-id="f07b3-114">Chcete-li povolit tuto funkci musí obsahovat volání [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) v konstruktoru vaše **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="f07b3-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="f07b3-115">Pokud nejste obeznámeni s **DbConfiguration**, naleznete v tématu [kódu na základě konfigurace](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="f07b3-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="f07b3-116">Tuto funkci můžete použít v kombinaci s Automatické opakované pokusy, kterou jsme představili v EF6, které pomáhají v situaci, ve kterém ve skutečnosti zápis transakce se nezdařil na serveru, protože došlo k přechodné chybě:</span><span class="sxs-lookup"><span data-stu-id="f07b3-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

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

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="f07b3-117">Způsob sledování transakce</span><span class="sxs-lookup"><span data-stu-id="f07b3-117">How transactions are tracked</span></span>  

<span data-ttu-id="f07b3-118">Pokud je povolena funkce, EF automaticky přidá novou tabulku pro databázi s názvem **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="f07b3-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="f07b3-119">V této tabulce je vložen nový řádek, pokaždé, když transakce je vytvořen pomocí EF a tento řádek je zaškrtnuté políčko existenci, pokud během zápisu dojde k selhání transakce.</span><span class="sxs-lookup"><span data-stu-id="f07b3-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="f07b3-120">I když EF provede nezaručené vyřadit řádky z tabulky, když už nejsou potřeba, můžou růst v tabulce, pokud se aplikace ukončí předčasně a z tohoto důvodu možná bude nutné k vyprázdnění tabulek ručně v některých případech.</span><span class="sxs-lookup"><span data-stu-id="f07b3-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="f07b3-121">Jak řešit selhání potvrzení s předchozími verzemi</span><span class="sxs-lookup"><span data-stu-id="f07b3-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="f07b3-122">Před EF 6.1 není mechanismem pro zpracování selhání potvrzení do EF produktu.</span><span class="sxs-lookup"><span data-stu-id="f07b3-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="f07b3-123">K práci s touto situací, který lze použít k předchozím verzím EF6 několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="f07b3-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="f07b3-124">Možnost 1 - nedělat nic</span><span class="sxs-lookup"><span data-stu-id="f07b3-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="f07b3-125">Pravděpodobnost selhání připojení během zápisu transakce tak může být přijatelný pro vaše aplikace právě selhat, pokud dojde k tomuto stavu dochází.</span><span class="sxs-lookup"><span data-stu-id="f07b3-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="f07b3-126">Možnost 2 – databázi můžete obnovit stav</span><span class="sxs-lookup"><span data-stu-id="f07b3-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="f07b3-127">Zrušit aktuální kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="f07b3-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="f07b3-128">Vytvořit nový kontext databáze a obnovení stavu aplikace z databáze</span><span class="sxs-lookup"><span data-stu-id="f07b3-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="f07b3-129">Informujte uživatele, že poslední operaci možná nebyly úspěšně dokončeny</span><span class="sxs-lookup"><span data-stu-id="f07b3-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="f07b3-130">Možnost 3 - manuálně sledovat transakce</span><span class="sxs-lookup"><span data-stu-id="f07b3-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="f07b3-131">Přidáte tabulku – sledovat na databáze, která slouží ke sledování stavu transakce.</span><span class="sxs-lookup"><span data-stu-id="f07b3-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="f07b3-132">Vložte řádek do tabulky na začátku každé transakci.</span><span class="sxs-lookup"><span data-stu-id="f07b3-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="f07b3-133">Pokud se nepovede při potvrzení změn, zkontrolujte přítomnost odpovídající řádek v databázi.</span><span class="sxs-lookup"><span data-stu-id="f07b3-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="f07b3-134">Pokud se nachází na řádku, bude normálně pokračujte, protože transakce byla úspěšně zapsána</span><span class="sxs-lookup"><span data-stu-id="f07b3-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="f07b3-135">Pokud chybí na řádku, použijte strategie provádění aktuální operaci zopakovat.</span><span class="sxs-lookup"><span data-stu-id="f07b3-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="f07b3-136">Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, aby se zabránilo růst v tabulce.</span><span class="sxs-lookup"><span data-stu-id="f07b3-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="f07b3-137">[Tento příspěvek na blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) obsahuje ukázkový kód pro provedení to v SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="f07b3-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
