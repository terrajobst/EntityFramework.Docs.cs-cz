---
title: Odolnost připojení a logika opakování – EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824835"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="d325c-102">Odolnost připojení a logika opakování</span><span class="sxs-lookup"><span data-stu-id="d325c-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="d325c-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d325c-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="d325c-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="d325c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="d325c-105">Aplikace, které se připojují k databázovému serveru, byly vždycky zranitelné z důvodu selhání back-endu a nestability sítě.</span><span class="sxs-lookup"><span data-stu-id="d325c-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="d325c-106">V prostředí založeném na síti LAN, které pracuje na vyhrazených databázových serverech, jsou ale tyto chyby vzácná natolik, že navíc není často nutná logika, která by tyto chyby zpracovala.</span><span class="sxs-lookup"><span data-stu-id="d325c-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="d325c-107">S růstem cloudových databázových serverů, jako jsou Windows Azure SQL Database a připojení přes méně spolehlivé sítě, je teď častěji častější, než dojde k přerušením připojení.</span><span class="sxs-lookup"><span data-stu-id="d325c-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="d325c-108">To může být způsobeno obrannou linií technikami, které používají cloudové databáze k zajištění spravedlivého poskytování služeb, jako je omezování připojení nebo nestabilitě sítě, která způsobuje přerušované časové limity a další přechodné chyby.</span><span class="sxs-lookup"><span data-stu-id="d325c-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="d325c-109">Odolnost připojení označuje schopnost EF automaticky opakovat všechny příkazy, které selhaly kvůli přerušení připojení.</span><span class="sxs-lookup"><span data-stu-id="d325c-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="d325c-110">Strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d325c-110">Execution Strategies</span></span>  

<span data-ttu-id="d325c-111">Pokus o připojení se postará o implementaci rozhraní IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="d325c-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="d325c-112">Implementace IDbExecutionStrategy budou zodpovědné za přijetí operace a, pokud dojde k výjimce, určí, jestli je opakování vhodné, a zkuste to znovu, pokud je.</span><span class="sxs-lookup"><span data-stu-id="d325c-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="d325c-113">Existují čtyři strategie provádění dodávané s EF:</span><span class="sxs-lookup"><span data-stu-id="d325c-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="d325c-114">**DefaultExecutionStrategy**: Tato strategie provádění neopakuje žádné operace, je výchozí pro databáze jiné než SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d325c-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="d325c-115">**DefaultSqlExecutionStrategy**: Jedná se o interní strategii provádění, která se používá ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d325c-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="d325c-116">Tato strategie se vůbec neopakuje, ale zabalí všechny výjimky, které by mohly být přechodné, aby informovaly uživatele o tom, že by mohly chtít povolit odolnost připojení.</span><span class="sxs-lookup"><span data-stu-id="d325c-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="d325c-117">**DbExecutionStrategy**: Tato třída je vhodná jako základní třída pro jiné strategie provádění, včetně vašich vlastních.</span><span class="sxs-lookup"><span data-stu-id="d325c-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="d325c-118">Implementuje zásadu exponenciálního opakování, kde počáteční opakování proběhne s nulovým zpožděním a zpoždění se zvyšuje exponenciálně, dokud nebude dosaženo maximálního počtu opakování.</span><span class="sxs-lookup"><span data-stu-id="d325c-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="d325c-119">Tato třída má abstraktní metodu ShouldRetryOn, která může být implementována v odvozených strategiích provádění, aby bylo možné řídit, které výjimky by se měly opakovat.</span><span class="sxs-lookup"><span data-stu-id="d325c-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="d325c-120">**SqlAzureExecutionStrategy**: Tato strategie provádění dědí z DbExecutionStrategy a zopakuje pokus o výjimkách, u kterých je známo, že budou pravděpodobně přechodné při práci s Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d325c-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="d325c-121">Strategie provádění 2 a 4 jsou součástí poskytovatele SQL serveru, který je dodáván s EF, který je v sestavení EntityFramework. SqlServer a je navržen pro práci s SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d325c-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="d325c-122">Povolení strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d325c-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="d325c-123">Nejjednodušší způsob, jak říct EF, aby používal strategii provádění, je metoda SetExecutionStrategy třídy [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) :</span><span class="sxs-lookup"><span data-stu-id="d325c-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="d325c-124">Tento kód oznamuje EF, aby při připojování k SQL Server používal rozhraní SqlAzureExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="d325c-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="d325c-125">Konfigurace strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d325c-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="d325c-126">Konstruktor třídy SqlAzureExecutionStrategy může přijmout dva parametry, MaxRetryCount a MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="d325c-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="d325c-127">MaxRetry Count je maximální počet pokusů, kolikrát se strategie zopakuje.</span><span class="sxs-lookup"><span data-stu-id="d325c-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="d325c-128">MaxDelay je interval TimeSpan, který představuje maximální zpoždění mezi opakovanými pokusy, které bude používat strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="d325c-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="d325c-129">Pokud chcete nastavit maximální počet opakovaných pokusů na hodnotu 1 a maximální zpoždění na 30 sekund, proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="d325c-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execute the following:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

<span data-ttu-id="d325c-130">SqlAzureExecutionStrategy se zopakuje okamžitě při prvním výskytu přechodného selhání, ale během každého opakování bude trvat déle, dokud nedosáhnete maximálního počtu opakování, nebo celkovým časem, který uplyne na maximálním zpoždění.</span><span class="sxs-lookup"><span data-stu-id="d325c-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="d325c-131">Strategie provádění budou opakovat pouze omezený počet výjimek, které jsou obvykle přechodné, stále budete muset zpracovat další chyby a zachytit výjimku RetryLimitExceeded pro případ, že chyba není přechodná nebo je příliš dlouhá, aby ji bylo možné vyřešit. využít.</span><span class="sxs-lookup"><span data-stu-id="d325c-131">The execution strategies will only retry a limited number of exceptions that are usually transient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="d325c-132">Při použití strategie opakování pokusu je potřeba mít několik známých omezení:</span><span class="sxs-lookup"><span data-stu-id="d325c-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="d325c-133">Dotazy streamování nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="d325c-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="d325c-134">Ve výchozím nastavení EF6 a novější verze vyřadí výsledky dotazu do vyrovnávací paměti místo jejich streamování.</span><span class="sxs-lookup"><span data-stu-id="d325c-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="d325c-135">Pokud chcete mít výsledky streamování výsledků, můžete pomocí metody AsStreaming změnit dotaz LINQ to Entities na streamování.</span><span class="sxs-lookup"><span data-stu-id="d325c-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="d325c-136">Streamování není podporováno, pokud je zaregistrována strategie spuštění opakování.</span><span class="sxs-lookup"><span data-stu-id="d325c-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="d325c-137">Toto omezení existuje, protože připojení by mohlo vyřadit část způsobem prostřednictvím vrácených výsledků.</span><span class="sxs-lookup"><span data-stu-id="d325c-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="d325c-138">V takovém případě musí EF znovu spustit celý dotaz, ale nemá žádný spolehlivý způsob, jak zjistit, které výsledky již byly vráceny (data se pravděpodobně od odeslání počátečního dotazu změnila, výsledky mohou být vráceny v jiném pořadí, výsledky nemusí mít jedinečný identifikátor). atd.).</span><span class="sxs-lookup"><span data-stu-id="d325c-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="d325c-139">Uživatelem iniciované transakce nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="d325c-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="d325c-140">Pokud jste nakonfigurovali strategii provádění, která má za následek opakování, existují určitá omezení týkající se použití transakcí.</span><span class="sxs-lookup"><span data-stu-id="d325c-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="d325c-141">Ve výchozím nastavení bude EF provádět všechny aktualizace databáze v rámci transakce.</span><span class="sxs-lookup"><span data-stu-id="d325c-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="d325c-142">Nemusíte nic dělat, abyste ho mohli povolit, EF to vždycky dělá automaticky.</span><span class="sxs-lookup"><span data-stu-id="d325c-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="d325c-143">Například v následujícím kódu je kód SaveChanges automaticky proveden v rámci transakce.</span><span class="sxs-lookup"><span data-stu-id="d325c-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="d325c-144">Pokud nebylo po vložení jedné z nových lokalit provedeno selhání metody SaveChanges, transakce by se vrátila zpět a v databázi se nepoužily žádné změny.</span><span class="sxs-lookup"><span data-stu-id="d325c-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="d325c-145">Kontext je také ponechán ve stavu, který umožňuje, aby bylo volání metody SaveChanges znovu voláno pro opakované použití změn.</span><span class="sxs-lookup"><span data-stu-id="d325c-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="d325c-146">Pokud nepoužíváte strategii spuštění opakování, můžete v jedné transakci zabalit více operací.</span><span class="sxs-lookup"><span data-stu-id="d325c-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="d325c-147">Například následující kód zalomí dvě volání metody SaveChanges v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="d325c-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="d325c-148">Pokud některé z obou operací selžou, nepoužije se žádná ze změn.</span><span class="sxs-lookup"><span data-stu-id="d325c-148">If any part of either operation fails then none of the changes are applied.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

<span data-ttu-id="d325c-149">Tato funkce není podporována, pokud používáte strategii opakovaného spuštění, protože EF neznáte žádné předchozí operace a postup opakování.</span><span class="sxs-lookup"><span data-stu-id="d325c-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="d325c-150">Například pokud druhý příkaz SaveChanges neproběhne úspěšně, EF již neobsahuje požadované informace pro opakování prvního volání metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="d325c-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="solution-manually-call-execution-strategy"></a><span data-ttu-id="d325c-151">Řešení: ruční volání strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d325c-151">Solution: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="d325c-152">Řešením je ruční použití strategie spouštění a přidělení celé sady logiky, která se má spustit, aby se mohla opakovat vše, pokud jedna z operací neproběhne úspěšně.</span><span class="sxs-lookup"><span data-stu-id="d325c-152">The solution is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="d325c-153">Pokud je spuštěná strategie spouštění odvozená z DbExecutionStrategy, pozastaví se implicitní prováděcí strategie používané v SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="d325c-153">When an execution strategy derived from DbExecutionStrategy is running it will suspend the implicit execution strategy used in SaveChanges.</span></span>  

<span data-ttu-id="d325c-154">Všimněte si, že všechny kontexty by měly být vytvořeny v rámci bloku kódu, který se má opakovat.</span><span class="sxs-lookup"><span data-stu-id="d325c-154">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="d325c-155">Tím se zajistí, že pro každý pokus začneme s čistým stavem.</span><span class="sxs-lookup"><span data-stu-id="d325c-155">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });
```  
