---
title: Odolnost připojení – EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 07646e6ead845c38537945a03367ac7f50784236
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416639"
---
# <a name="connection-resiliency"></a><span data-ttu-id="8b5c2-102">Odolnost připojení</span><span class="sxs-lookup"><span data-stu-id="8b5c2-102">Connection Resiliency</span></span>

<span data-ttu-id="8b5c2-103">Odolnost připojení automaticky opakuje neúspěšné příkazy databáze.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="8b5c2-104">Funkci lze použít s libovolnou databází poskytnutím "strategie provádění", která zapouzdřuje logiku potřebnou k detekci selhání a opakování příkazů.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="8b5c2-105">Poskytovatelé EF Core můžou poskytovat strategie spouštění přizpůsobené konkrétním stavům selhání databáze a optimálním zásadám opakování.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="8b5c2-106">Například poskytovatel SQL Server zahrnuje strategii provádění, která je určena speciálně pro SQL Server (včetně SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="8b5c2-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="8b5c2-107">Informace o typech výjimek, které se dají opakovat, a má rozumné výchozí hodnoty pro maximální počet opakování, zpoždění mezi opakovanými pokusy atd.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="8b5c2-108">Při konfiguraci možností pro váš kontext je určena strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="8b5c2-109">Obvykle se jedná o metodu `OnConfiguring` vašeho odvozeného kontextu:</span><span class="sxs-lookup"><span data-stu-id="8b5c2-109">This is typically in the `OnConfiguring` method of your derived context:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

<span data-ttu-id="8b5c2-110">nebo v `Startup.cs` pro aplikaci ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="8b5c2-110">or in `Startup.cs` for an ASP.NET Core application:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a><span data-ttu-id="8b5c2-111">Vlastní strategie provádění</span><span class="sxs-lookup"><span data-stu-id="8b5c2-111">Custom execution strategy</span></span>

<span data-ttu-id="8b5c2-112">Existuje mechanismus, jak zaregistrovat vlastní strategii spouštění, pokud chcete změnit některou z výchozích hodnot.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-112">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="8b5c2-113">Strategie provádění a transakce</span><span class="sxs-lookup"><span data-stu-id="8b5c2-113">Execution strategies and transactions</span></span>

<span data-ttu-id="8b5c2-114">Strategie provádění, která se automaticky opakuje při selhání, musí být schopná přehrát všechny operace v bloku opakování, který se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-114">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="8b5c2-115">Když jsou povoleny opakované pokusy, každá operace, kterou provedete prostřednictvím EF Core, se stal svou vlastní operací vyvolaly.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-115">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="8b5c2-116">To znamená, že každý dotaz a každé volání `SaveChanges()` se bude opakovat jako jednotka, pokud dojde k přechodnému selhání.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-116">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="8b5c2-117">Nicméně pokud váš kód inicializuje transakci pomocí `BeginTransaction()` definujete vlastní skupinu operací, které je třeba považovat za jednotku, a všechno, co se v transakci musí přehrát, dojde k selhání.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-117">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="8b5c2-118">Pokud se pokusíte provést tuto operaci při použití strategie provádění, obdržíte výjimku podobnou následující:</span><span class="sxs-lookup"><span data-stu-id="8b5c2-118">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="8b5c2-119">InvalidOperationException: nakonfigurovaná strategie provádění SqlServerRetryingExecutionStrategy nepodporuje transakce iniciované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-119">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="8b5c2-120">K provedení všech operací v transakci jako jednotky vyvolaly použijte strategii spuštění vrácenou funkcí DbContext. Database. CreateExecutionStrategy ().</span><span class="sxs-lookup"><span data-stu-id="8b5c2-120">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="8b5c2-121">Řešením je ručně vyvolat strategii spouštění s delegátem, který představuje všechno, co je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-121">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="8b5c2-122">Pokud dojde k přechodnému selhání, strategie provádění znovu vyvolá delegáta.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-122">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

<span data-ttu-id="8b5c2-123">Tento přístup lze také použít s ambientních transakcí.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-123">This approach can also be used with ambient transactions.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="8b5c2-124">Selhání potvrzení transakce a problém idempotence</span><span class="sxs-lookup"><span data-stu-id="8b5c2-124">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="8b5c2-125">Obecně platí, že pokud dojde k selhání připojení, aktuální transakce se vrátí zpět.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-125">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="8b5c2-126">Pokud je ale připojení vyřazeno, zatímco probíhá potvrzení transakce, výsledný stav transakce není známý.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-126">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> 

<span data-ttu-id="8b5c2-127">Ve výchozím nastavení strategie spouštění zopakuje operaci, jako kdyby byla transakce vrácena zpět, ale pokud to ale není, bude výsledkem výjimka, pokud je stav nového databáze nekompatibilní nebo může způsobit **poškození dat** , pokud se operace nespoléhá na určitý stav, například při vložení nového řádku s automaticky generovanými hodnotami klíče.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-127">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="8b5c2-128">Existuje několik způsobů, jak s nimi pracovat.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-128">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="8b5c2-129">Možnost 1 – do (téměř) nic</span><span class="sxs-lookup"><span data-stu-id="8b5c2-129">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="8b5c2-130">Pravděpodobnost selhání připojení během potvrzení transakce je nízká, takže může být přijatelné, aby vaše aplikace v případě, že k této situaci skutečně dojde, nedošlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-130">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="8b5c2-131">Je však nutné se vyhnout použití klíčů generovaných úložištěm, aby bylo zajištěno, že bude vyvolána výjimka namísto přidání duplicitního řádku.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-131">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="8b5c2-132">Zvažte použití hodnoty GUID generované klientem nebo generátoru hodnot na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-132">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="8b5c2-133">Možnost 2 – opětovné sestavení stavu aplikace</span><span class="sxs-lookup"><span data-stu-id="8b5c2-133">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="8b5c2-134">Zahodí aktuální `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-134">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="8b5c2-135">Vytvořte novou `DbContext` a obnovte stav aplikace z databáze.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-135">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="8b5c2-136">Informujte uživatele, že poslední operace nemusí být úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-136">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="8b5c2-137">Možnost 3 – Přidání ověření stavu</span><span class="sxs-lookup"><span data-stu-id="8b5c2-137">Option 3 - Add state verification</span></span>

<span data-ttu-id="8b5c2-138">Pro většinu operací, které mění stav databáze je možné přidat kód, který kontroluje, zda bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-138">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="8b5c2-139">EF poskytuje metodu rozšíření, která usnadňuje `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-139">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="8b5c2-140">Tato metoda začíná a potvrdí transakci a také přijímá funkci v parametru `verifySucceeded`, který je vyvolán, když dojde k přechodné chybě během zápisu transakce.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-140">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="8b5c2-141">Zde je `SaveChanges` vyvolána s `acceptAllChangesOnSuccess` nastavenou na `false`, aby nedošlo ke změně stavu `Blog` entity na `Unchanged`, pokud `SaveChanges` úspěšné.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-141">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="8b5c2-142">To umožňuje opakovat stejnou operaci, pokud se potvrzení nezdařilo a transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-142">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="8b5c2-143">Možnost 4 – ruční sledování transakce</span><span class="sxs-lookup"><span data-stu-id="8b5c2-143">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="8b5c2-144">Pokud potřebujete použít klíče generované úložištěm nebo potřebujete obecný způsob zpracování selhání potvrzení, které není závislé na operaci provedené jednotlivými transakce, může být přiřazeno ID, které je zaškrtnuto, když se potvrzení nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-144">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="8b5c2-145">Přidejte tabulku do databáze použité ke sledování stavu transakcí.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-145">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="8b5c2-146">Vloží řádek do tabulky na začátku každé transakce.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-146">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="8b5c2-147">Pokud během potvrzení dojde k chybě připojení, vyhledejte přítomnost odpovídajícího řádku v databázi.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-147">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="8b5c2-148">Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, abyste se vyhnuli nárůstu tabulky.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-148">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="8b5c2-149">Ujistěte se, že kontext použitý pro ověření má strategii spuštění definovanou v případě, že během ověřování dojde k chybě při potvrzení transakce.</span><span class="sxs-lookup"><span data-stu-id="8b5c2-149">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
