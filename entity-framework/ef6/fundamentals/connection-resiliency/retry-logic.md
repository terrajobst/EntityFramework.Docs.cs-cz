---
title: Odolnost proti chybám a zkuste to znovu připojení logic - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 09ebed18b43f864af36b6931f45638f3a3056229
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490802"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="d391a-102">Připojení logic odolnost proti chybám a zkuste to znovu</span><span class="sxs-lookup"><span data-stu-id="d391a-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="d391a-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d391a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="d391a-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="d391a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="d391a-105">Připojení k serveru pro databázi aplikace byly vždy snadno napadnutelný konce připojení z důvodu selhání back-end a nestability sítě.</span><span class="sxs-lookup"><span data-stu-id="d391a-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="d391a-106">Ale v prostředí sítě LAN, na základě pracovat se službou vyhrazené databázové servery tyto chyby jsou dostatečně vzácné, že další logiky, která by tyto chyby se často nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="d391a-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="d391a-107">S nástupem cloud na základě databázové servery, jako jsou Windows Azure SQL Database a připojení přes méně spolehlivé sítě, které je nyní běžné připojení konce řádků.</span><span class="sxs-lookup"><span data-stu-id="d391a-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="d391a-108">To může být způsobeno obranné techniky, které cloudové databáze pomocí zajistit dostupnost služby, například omezování využití připojení, nebo na nestabilitu v síti způsobují přerušované vypršení časových limitů a jiné přechodné chyby.</span><span class="sxs-lookup"><span data-stu-id="d391a-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="d391a-109">Odolnost připojení odkazuje na možnost pro EF, aby automaticky opakovala všechny příkazy, které selžou kvůli konce těchto připojení.</span><span class="sxs-lookup"><span data-stu-id="d391a-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="d391a-110">Strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d391a-110">Execution Strategies</span></span>  

<span data-ttu-id="d391a-111">Opakování připojení se postaral o implementaci rozhraní IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="d391a-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="d391a-112">Implementace IDbExecutionStrategy bude zodpovídat za přijímání operace a pokud dojde k výjimce, určíte, pokud je opakování pokusu vhodné a to zkusíte znovu, pokud se jedná.</span><span class="sxs-lookup"><span data-stu-id="d391a-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="d391a-113">Existují čtyři strategií provádění, které se dodávají s EF:</span><span class="sxs-lookup"><span data-stu-id="d391a-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="d391a-114">**DefaultExecutionStrategy**: Tato strategie provádění neopakuje žádné operace, je výchozí nastavení pro databáze než sql server.</span><span class="sxs-lookup"><span data-stu-id="d391a-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="d391a-115">**DefaultSqlExecutionStrategy**: Toto je strategie provádění vnitřní, který se používá ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d391a-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="d391a-116">Tato strategie neopakuje vůbec, ale sbalíte žádné výjimky, které by mohly být přechodné informovat uživatele, kteří chtějí povolit odolnost připojení.</span><span class="sxs-lookup"><span data-stu-id="d391a-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="d391a-117">**DbExecutionStrategy**: Tato třída je vhodná jako základní třída pro jiné strategie provádění, včetně vlastních vlastní značky.</span><span class="sxs-lookup"><span data-stu-id="d391a-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="d391a-118">Implementuje zásady opakování exponenciálního, kde počáteční opakování se stane s nulovou zpoždění a zpoždění zvyšuje exponenciálně až do dosažení maximální počet opakování.</span><span class="sxs-lookup"><span data-stu-id="d391a-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="d391a-119">Tato třída je abstraktní metody ShouldRetryOn, který se dá implementovat v odvozené spuštění strategie řízení výjimek, které je třeba opakovat.</span><span class="sxs-lookup"><span data-stu-id="d391a-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="d391a-120">**SqlAzureExecutionStrategy**: Tato strategie provádění dědí z DbExecutionStrategy a bude opakovat při výjimkách, které jsou známé jako pravděpodobně přechodné při práci s Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d391a-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="d391a-121">Strategie provádění 2 a 4 jsou součástí zprostředkovatele Sql Server, který se dodává s EF, který se nachází v sestavení EntityFramework.SqlServer a jsou navrženy pro práci se serverem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d391a-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="d391a-122">Povolení strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d391a-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="d391a-123">Nejjednodušší způsob, jak zjistit EF použití strategie provádění je s metodou SetExecutionStrategy [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) třídy:</span><span class="sxs-lookup"><span data-stu-id="d391a-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="d391a-124">Tento kód říká EF SqlAzureExecutionStrategy používat při připojování k serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d391a-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="d391a-125">Konfigurace strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d391a-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="d391a-126">Konstruktor SqlAzureExecutionStrategy může přijmout dva parametry, MaxRetryCount a MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="d391a-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="d391a-127">Počet MaxRetry je maximální počet opakování strategie.</span><span class="sxs-lookup"><span data-stu-id="d391a-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="d391a-128">MaxDelay je časový interval představující zpoždění mezi opakovanými pokusy, které se používají strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="d391a-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="d391a-129">Chcete-li nastavit maximální počet pokusů o 1 a Maximální zpoždění 30 sekund by execue následující:</span><span class="sxs-lookup"><span data-stu-id="d391a-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

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

<span data-ttu-id="d391a-130">SqlAzureExecutionStrategy bude opakovat okamžitě překročil poprvé, dojde k přechodnému selhání, ale bude nadále zpoždění mezi opakováními, dokud se buď maximální limit opakování nebo celkové doby narazí na maximální zpoždění.</span><span class="sxs-lookup"><span data-stu-id="d391a-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="d391a-131">Strategie provádění bude opakovat jenom omezený počet výjimek, které jsou obvykle tansient, bude i nadále potřebujete zpracovávat jiné chyby, jakož i RetryLimitExceeded výjimku pro případ, kdy chyba není přechodná nebo trvá příliš dlouho řešení samotný.</span><span class="sxs-lookup"><span data-stu-id="d391a-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="d391a-132">Existují některé známé omezení při použití strategie opakování spuštění:</span><span class="sxs-lookup"><span data-stu-id="d391a-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="d391a-133">Streamování dotazů se nepodporují.</span><span class="sxs-lookup"><span data-stu-id="d391a-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="d391a-134">Ve výchozím nastavení EF6 a novější verze bude ve vyrovnávací paměti výsledky dotazu a nikoli jejich streamování.</span><span class="sxs-lookup"><span data-stu-id="d391a-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="d391a-135">Pokud chcete mít výsledky streamování můžete použít metodu AsStreaming změnit LINQ dotaz entity pro streamování.</span><span class="sxs-lookup"><span data-stu-id="d391a-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="d391a-136">Streamování se nepodporuje při registraci opakuje strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="d391a-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="d391a-137">Toto omezení existuje, protože připojení může vyřadit rozúčtují přes výsledky se vrací.</span><span class="sxs-lookup"><span data-stu-id="d391a-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="d391a-138">V tomto případě je potřeba znovu spustit celý dotaz EF, ale nemá žádné spolehlivě zjistit, jaké výsledky již byly vráceny (dat mohl být změněn počáteční dotaz byla odeslána, výsledky mohou vrátit v jiném pořadí, výsledky nemusí být jedinečný identifikátor atd.).</span><span class="sxs-lookup"><span data-stu-id="d391a-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="d391a-139">Uživatelem iniciované transakce nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="d391a-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="d391a-140">Pokud jste nakonfigurovali strategie provádění, jehož výsledkem opakovaných pokusů, narazíte na určitá omezení týkající použití transakcí.</span><span class="sxs-lookup"><span data-stu-id="d391a-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="d391a-141">Ve výchozím nastavení provede EF žádné aktualizace databáze v rámci transakce.</span><span class="sxs-lookup"><span data-stu-id="d391a-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="d391a-142">Nemusíte dělat nic, aby tuto možnost povolte, EF vždy to dělá automaticky.</span><span class="sxs-lookup"><span data-stu-id="d391a-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="d391a-143">Například v následujícím kódu metoda SaveChanges se provádí automaticky v rámci transakce.</span><span class="sxs-lookup"><span data-stu-id="d391a-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="d391a-144">Pokud SaveChanges po vložení mezi novou lokalitou a transakce bude vrácena zpět a žádné změny k databázi selhat.</span><span class="sxs-lookup"><span data-stu-id="d391a-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="d391a-145">Kontext je také zbývá ve stavu umožňujícím uloží změny do volat pokus o použití změn.</span><span class="sxs-lookup"><span data-stu-id="d391a-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="d391a-146">Pokud nepoužíváte opakuje strategie provádění v rámci jedné transakce můžete zalomit více operací.</span><span class="sxs-lookup"><span data-stu-id="d391a-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="d391a-147">Následující kód například zabalí dvě SaveChanges volání v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="d391a-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="d391a-148">Pokud se nezdaří libovolné části buď operaci pak žádná ze změn, jsou použity.</span><span class="sxs-lookup"><span data-stu-id="d391a-148">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="d391a-149">To není podporováno při použití opakuje strategie provádění, protože není si vědom jakékoli předchozí operace a způsob opakování je EF.</span><span class="sxs-lookup"><span data-stu-id="d391a-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="d391a-150">Například pokud se druhý SaveChanges nezdařilo pak EF už má požadované informace pro první volání SaveChanges zopakovat.</span><span class="sxs-lookup"><span data-stu-id="d391a-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="workaround-suspend-execution-strategy"></a><span data-ttu-id="d391a-151">Alternativní řešení: Pozastavení strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d391a-151">Workaround: Suspend Execution Strategy</span></span>  

<span data-ttu-id="d391a-152">Jedním z možných řešení je dočasně pozastavit opakuje strategie provádění pro část kódu, který potřebuje uživatel inicioval transakce.</span><span class="sxs-lookup"><span data-stu-id="d391a-152">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="d391a-153">Nejjednodušší způsob, jak to provést, je přidání SuspendExecutionStrategy příznak, který do vašeho kódu na základě konfigurace třídy a změňte lambda strategie provádění pro vrácení výchozí strategie provádění (bez retying), když je příznak nastaven.</span><span class="sxs-lookup"><span data-stu-id="d391a-153">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="d391a-154">Všimněte si, že používáme CallContext pro ukládání hodnoty horní příznak.</span><span class="sxs-lookup"><span data-stu-id="d391a-154">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="d391a-155">Poskytuje podobné funkce jako úložiště thread local, ale je bezpečné používat s asynchronní kód – včetně asynchronního dotazu a uložit s Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d391a-155">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="d391a-156">Nyní jsme můžete pozastavení strategie provádění pro části kódu, který používá transakce iniciované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="d391a-156">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a><span data-ttu-id="d391a-157">Alternativní řešení: Volání ručně strategie provádění</span><span class="sxs-lookup"><span data-stu-id="d391a-157">Workaround: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="d391a-158">Další možností je ručně pomocí strategie provádění a přiřaďte mu celá sada logiky pro spuštění, tak, že je všechno, co opakujte Pokud jedna operace selže.</span><span class="sxs-lookup"><span data-stu-id="d391a-158">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="d391a-159">Stále potřebujeme k pozastavení strategie provádění - technikou uvedeno výše - tak, aby všechny kontexty použít uvnitř blok opakovatelného kódu nebude pokoušet o opakování.</span><span class="sxs-lookup"><span data-stu-id="d391a-159">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="d391a-160">Všimněte si, že všechny kontexty by měla být vytvořena v rámci bloku kódu se na opakování pokusu.</span><span class="sxs-lookup"><span data-stu-id="d391a-160">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="d391a-161">Tím se zajistí, že jsme začínají s do čistého stavu pro každou. Zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="d391a-161">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

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

MyConfiguration.SuspendExecutionStrategy = false;
```  
