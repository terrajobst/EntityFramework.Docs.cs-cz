---
title: Odolnost připojení - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054556"
---
# <a name="connection-resiliency"></a><span data-ttu-id="25e02-102">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="25e02-102">Connection Resiliency</span></span>

<span data-ttu-id="25e02-103">Odolnost připojení automaticky opakuje příkazy databáze se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="25e02-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="25e02-104">Funkci lze použít s libovolnou databází zadáním "provádění strategie", který zapouzdřuje logice potřeba rozpoznala chyby a opakujte příkazy.</span><span class="sxs-lookup"><span data-stu-id="25e02-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="25e02-105">Hlavními zprostředkovateli EF může poskytovat strategie provádění přizpůsobené jejich podmínky při selhání konkrétní databázi a zásady optimální opakování.</span><span class="sxs-lookup"><span data-stu-id="25e02-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="25e02-106">Jako příklad obsahuje zprostředkovatele SQL Server provádění strategie, který je specificky přizpůsobit, aby SQL Server (včetně SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="25e02-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="25e02-107">Je vědět typů výjimek, které můžete zkusit znovu a má rozumný výchozí hodnoty pro maximální počet opakovaných pokusů, zpoždění mezi opakování atd.</span><span class="sxs-lookup"><span data-stu-id="25e02-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="25e02-108">Při konfiguraci možností pro váš kontext zadaná strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="25e02-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="25e02-109">To je obvykle v `OnConfiguring` metoda vaší odvozené kontextu, nebo v `Startup.cs` pro aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25e02-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="25e02-110">Strategie vlastního provádění</span><span class="sxs-lookup"><span data-stu-id="25e02-110">Custom execution strategy</span></span>

<span data-ttu-id="25e02-111">Je mechanismus k registraci strategie vlastní provádění vlastní, pokud chcete změnit výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="25e02-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="25e02-112">Strategie provádění a transakce</span><span class="sxs-lookup"><span data-stu-id="25e02-112">Execution strategies and transactions</span></span>

<span data-ttu-id="25e02-113">Provádění strategii, která automaticky opakovat při selhání musí být schopni přehrání jednotlivých operací v bloku opakování, který selže.</span><span class="sxs-lookup"><span data-stu-id="25e02-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="25e02-114">Pokud jsou povolené opakování, každou operaci provádět prostřednictvím EF základní stane vlastní nevyřeší operaci, tj. každý dotaz a každé volání `SaveChanges()` bude zopakován jako jednotku, pokud dojde k přechodné chybě.</span><span class="sxs-lookup"><span data-stu-id="25e02-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="25e02-115">Ale pokud váš kód zahájí transakce pomocí `BeginTransaction()` definujete vlastní skupiny operací, které musí být považovány za jednotku, tj. všechno uvnitř transakce muset být přehrání musí dojít k chybě.</span><span class="sxs-lookup"><span data-stu-id="25e02-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="25e02-116">Zobrazí se výjimku takto a pokusíte se to udělat, když používá provádění strategie.</span><span class="sxs-lookup"><span data-stu-id="25e02-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="25e02-117">InvalidOperationException: V nakonfigurované strategii provádění 'SqlServerRetryingExecutionStrategy' nepodporuje transakce inicializované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="25e02-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="25e02-118">Použijte strategii provádění vrácený 'DbContext.Database.CreateExecutionStrategy()' provádět všechny operace v rámci transakce jako nevyřeší jednotku.</span><span class="sxs-lookup"><span data-stu-id="25e02-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="25e02-119">Řešení je ručně vyvolání strategie provádění s představující všechno delegáta, který je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="25e02-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="25e02-120">Pokud dojde k přechodné chybě, bude strategii provádění znovu vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="25e02-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="25e02-121">Chyba při potvrzení transakce a idempotenci problému</span><span class="sxs-lookup"><span data-stu-id="25e02-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="25e02-122">Obecně platí když dojde k selhání připojení aktuální transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="25e02-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="25e02-123">Ale pokud připojení se ukončí při transakce vrácení potvrdí výsledná stav transakce neznámý.</span><span class="sxs-lookup"><span data-stu-id="25e02-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="25e02-124">Toto [příspěvku na blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="25e02-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="25e02-125">Ve výchozím nastavení, strategii provádění bude opakovat operaci jako kdyby byla transakce vrácena zpět, ale pokud se nejedná o tento případ tato akce způsobí výjimku Pokud nový stav databáze není kompatibilní, nebo může vést k **poškození dat** Pokud operace není závislý na konkrétní stav, například při vkládání nový řádek s automaticky generované hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="25e02-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="25e02-126">Existuje několik způsobů, jak nakládat s to.</span><span class="sxs-lookup"><span data-stu-id="25e02-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="25e02-127">Možnost 1 - proveďte (téměř) nic</span><span class="sxs-lookup"><span data-stu-id="25e02-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="25e02-128">Pravděpodobnost selhání připojení během zápisu transakce je nízký, může být přijatelné pro vaši aplikaci právě nezdaří, pokud k tomuto stavu ve skutečnosti dochází.</span><span class="sxs-lookup"><span data-stu-id="25e02-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="25e02-129">Ale budete muset Vyhněte se použití klíče generované úložištěm, aby se zajistilo, že místo přidávání duplicitní řádek je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="25e02-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="25e02-130">Zvažte použití klientem generovaná hodnota GUID nebo generátor hodnoty na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="25e02-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="25e02-131">Možnost 2 – stav aplikace opětovné sestavení</span><span class="sxs-lookup"><span data-stu-id="25e02-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="25e02-132">Zrušit aktuální `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="25e02-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="25e02-133">Vytvořte novou `DbContext` a obnovení stavu aplikace z databáze.</span><span class="sxs-lookup"><span data-stu-id="25e02-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="25e02-134">Informujte uživatele, že poslední operace nemusí mít byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="25e02-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="25e02-135">Možnost 3 – Přidání ověření stavu</span><span class="sxs-lookup"><span data-stu-id="25e02-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="25e02-136">Pro většinu operací, které změní stav databáze je možné přidat kód, který kontroluje, zda byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="25e02-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="25e02-137">EF poskytuje metody rozšíření pro zjednodušení - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="25e02-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="25e02-138">Tato metoda začne a potvrdí transakce a také přijímá funkce při `verifySucceeded` parametru, která je volána, když dojde k přechodné chybě. během zápisu transakce.</span><span class="sxs-lookup"><span data-stu-id="25e02-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="25e02-139">Zde `SaveChanges` vyvolání `acceptAllChangesOnSuccess` nastavena na `false` chcete vyhnout změně stavu `Blog` entity, která má `Unchanged` Pokud `SaveChanges` úspěšné.</span><span class="sxs-lookup"><span data-stu-id="25e02-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="25e02-140">To umožňuje opakovat stejné operace, pokud se potvrzení nezdaří a transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="25e02-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="25e02-141">Možnost 4 - ručně sledovat transakce</span><span class="sxs-lookup"><span data-stu-id="25e02-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="25e02-142">Pokud potřebujete použít klíče generované úložištěm nebo potřebujete obecné tak zpracovávat chyby potvrzení, že nezávisí na operaci provést, může být přiřazen ID, které je zaškrtnuto, pokud se potvrzení nezdaří každou transakci.</span><span class="sxs-lookup"><span data-stu-id="25e02-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="25e02-143">Přidání tabulky do databáze použít ke sledování stavu transakce.</span><span class="sxs-lookup"><span data-stu-id="25e02-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="25e02-144">Vložte řádek do tabulky na začátku každou transakci.</span><span class="sxs-lookup"><span data-stu-id="25e02-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="25e02-145">Pokud se nepovede připojit během potvrzení, zkontrolujte přítomnost odpovídající řádek v databázi.</span><span class="sxs-lookup"><span data-stu-id="25e02-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="25e02-146">Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, aby se zabránilo růst v tabulce.</span><span class="sxs-lookup"><span data-stu-id="25e02-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="25e02-147">Zajistěte, aby kontextu použít pro ověření strategie provádění definován jako je připojení, pravděpodobně selžou znovu během ověřování, pokud se nezdařilo během zápisu transakce.</span><span class="sxs-lookup"><span data-stu-id="25e02-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
