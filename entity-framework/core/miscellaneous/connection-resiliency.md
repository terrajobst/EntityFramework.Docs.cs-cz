---
title: Odolnost připojení – EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: d5101d0622ddc2c90ddded16b9ec6cc4eb814c36
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283833"
---
# <a name="connection-resiliency"></a><span data-ttu-id="da089-102">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="da089-102">Connection Resiliency</span></span>

<span data-ttu-id="da089-103">Odolnost připojení automaticky opakuje neúspěšné databázových příkazů.</span><span class="sxs-lookup"><span data-stu-id="da089-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="da089-104">Funkci lze použít s libovolnou databází zadáním "strategie provádění", která zapouzdří logiku potřebnou pro zjišťování chyb a opakování příkazů.</span><span class="sxs-lookup"><span data-stu-id="da089-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="da089-105">EF Core poskytovatelé může poskytovat přizpůsobené jejich konkrétní databázi podmínky při selhání a zásady opakování optimální strategií provádění.</span><span class="sxs-lookup"><span data-stu-id="da089-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="da089-106">Jako příklad zprostředkovatele SQL Server obsahuje strategie provádění, která je specificky přizpůsobit, aby SQL Server (včetně SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="da089-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="da089-107">Zná typy výjimek, které umožňují opakovaný pokus a má účelné výchozí hodnoty pro maximální počet opakovaných pokusů, zpoždění mezi opakovanými pokusy apod.</span><span class="sxs-lookup"><span data-stu-id="da089-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="da089-108">Strategie provádění je zadaný při konfiguraci možnosti pro váš kontext.</span><span class="sxs-lookup"><span data-stu-id="da089-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="da089-109">Toto je obvykle v `OnConfiguring` metodu odvozené kontextu nebo v `Startup.cs` pro aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="da089-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="da089-110">Strategie provádění vlastní</span><span class="sxs-lookup"><span data-stu-id="da089-110">Custom execution strategy</span></span>

<span data-ttu-id="da089-111">Je mechanismus pro registraci strategie vlastní provádění vlastní, pokud chcete změnit výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="da089-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="da089-112">Strategie provádění a transakce</span><span class="sxs-lookup"><span data-stu-id="da089-112">Execution strategies and transactions</span></span>

<span data-ttu-id="da089-113">Strategie provádění, která automaticky opakovat při selhání musí být schopný k přehrání každou operaci v bloku opakování, které se nedaří.</span><span class="sxs-lookup"><span data-stu-id="da089-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="da089-114">Při opakování jsou povolené, bude každá operace, které můžete provádět prostřednictvím EF Core vyvolaly provozu.</span><span class="sxs-lookup"><span data-stu-id="da089-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="da089-115">To znamená, že každý dotaz a každé volání `SaveChanges()` bude zopakován jako celek, pokud dojde k přechodnému selhání.</span><span class="sxs-lookup"><span data-stu-id="da089-115">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="da089-116">Nicméně pokud váš kód zahájí transakci pomocí `BeginTransaction()` definujete vlastní skupinu operací, které je potřeba za jednotku považuje a všechno, co je uvnitř transakce bude nutné je znovu přehrát musí dojít k selhání.</span><span class="sxs-lookup"><span data-stu-id="da089-116">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="da089-117">Při pokusu provést při použití strategie provádění obdržíte výjimku podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="da089-117">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="da089-118">InvalidOperationException: Strategie provádění nakonfigurované "SqlServerRetryingExecutionStrategy" nepodporuje transakce iniciované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="da089-118">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="da089-119">Použití strategie provádění vrácený "DbContext.Database.CreateExecutionStrategy()" provádět všechny operace v transakci jako vyvolaly jednotky.</span><span class="sxs-lookup"><span data-stu-id="da089-119">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="da089-120">Řešením je vyvolat s všechno, co představuje delegáta, který je nutné spustit ručně strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="da089-120">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="da089-121">Pokud dojde k přechodnému selhání, strategie provádění znovu vyvolá delegáta.</span><span class="sxs-lookup"><span data-stu-id="da089-121">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="da089-122">Chyba při potvrzení transakce a problém idempotence</span><span class="sxs-lookup"><span data-stu-id="da089-122">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="da089-123">Obecně platí Pokud dojde k selhání připojení aktuální transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="da089-123">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="da089-124">Nicméně pokud připojení se ukončí, když je transakce vrácení potvrzené výsledný stav transakce není znám.</span><span class="sxs-lookup"><span data-stu-id="da089-124">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="da089-125">Najdete v tomto [blogový příspěvek](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="da089-125">See this [blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="da089-126">Ve výchozím nastavení, strategie provádění opakování operace, jako když transakce byla vrácena zpět, ale pokud se nejedná o případ výsledkem bude výjimky Pokud nový stav databáze není kompatibilní nebo může vést k **poškození dat** Pokud operace není závislý na konkrétní stav, například při vkládání nový řádek s automaticky generované hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="da089-126">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="da089-127">Chcete-li to vyřešit několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="da089-127">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="da089-128">Možnost 1 - proveďte (téměř) nic</span><span class="sxs-lookup"><span data-stu-id="da089-128">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="da089-129">Pravděpodobnost selhání připojení během zápisu transakce tak může být přijatelný pro vaše aplikace právě selhat, pokud dojde k tomuto stavu dochází.</span><span class="sxs-lookup"><span data-stu-id="da089-129">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="da089-130">Musíte ale Vyhněte se použití klíče generované úložištěm, aby se zajistilo, že místo přidání duplicitní řádek je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="da089-130">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="da089-131">Zvažte použití klientem generovaná hodnota GUID nebo generátor hodnot na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="da089-131">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="da089-132">Možnost 2 - stavu opětovné sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="da089-132">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="da089-133">Zrušit aktuální `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="da089-133">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="da089-134">Vytvořte nový `DbContext` a obnovení stavu aplikace z databáze.</span><span class="sxs-lookup"><span data-stu-id="da089-134">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="da089-135">Informujte uživatele, že poslední operaci možná nebyly úspěšně dokončeny.</span><span class="sxs-lookup"><span data-stu-id="da089-135">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="da089-136">Možnost 3 – Přidání ověření stavu</span><span class="sxs-lookup"><span data-stu-id="da089-136">Option 3 - Add state verification</span></span>

<span data-ttu-id="da089-137">Pro většinu operací, které mění stav databáze je možné přidat kód, který kontroluje, zda byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="da089-137">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="da089-138">Poskytuje metody rozšíření pro lepší pochopení – EF `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="da089-138">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="da089-139">Tato metoda začíná a potvrzení transakce a přijímá také funkci `verifySucceeded` parametr, který je voláno, když dojde k přechodné chybě během zápisu transakce.</span><span class="sxs-lookup"><span data-stu-id="da089-139">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="da089-140">Tady `SaveChanges` vyvolání `acceptAllChangesOnSuccess` nastavena na `false` chcete vyhnout změně stavu `Blog` entitu, kterou chcete `Unchanged` Pokud `SaveChanges` proběhne úspěšně.</span><span class="sxs-lookup"><span data-stu-id="da089-140">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="da089-141">Díky tomu opakování stejné operace potvrzení nezdaří a transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="da089-141">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="da089-142">Možnost 4 - manuálně sledovat transakce</span><span class="sxs-lookup"><span data-stu-id="da089-142">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="da089-143">Pokud chcete použít klíče generované úložištěm nebo potřebujete obecný způsob zpracování selhání potvrzení změn, které nejsou závislé na operace prováděné každou transakci, může přiřadit ID, které je zaškrtnuté políčko, pokud se potvrzení nezdaří.</span><span class="sxs-lookup"><span data-stu-id="da089-143">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="da089-144">Přidáte tabulku do databáze, která slouží ke sledování stavu transakce.</span><span class="sxs-lookup"><span data-stu-id="da089-144">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="da089-145">Vložte řádek do tabulky na začátku každé transakci.</span><span class="sxs-lookup"><span data-stu-id="da089-145">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="da089-146">Pokud se nepovede při potvrzení změn, zkontrolujte přítomnost odpovídající řádek v databázi.</span><span class="sxs-lookup"><span data-stu-id="da089-146">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="da089-147">Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, aby se zabránilo růst v tabulce.</span><span class="sxs-lookup"><span data-stu-id="da089-147">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="da089-148">Ujistěte se, že má kontext použitý pro ověření strategie provádění definován tak, že připojení je pravděpodobně znovu během ověřování selže, pokud došlo k selhání během zápisu transakce.</span><span class="sxs-lookup"><span data-stu-id="da089-148">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
