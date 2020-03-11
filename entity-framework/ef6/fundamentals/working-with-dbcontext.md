---
title: Práce s DbContext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416320"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="47b25-102">Práce s DbContext</span><span class="sxs-lookup"><span data-stu-id="47b25-102">Working with DbContext</span></span>

<span data-ttu-id="47b25-103">Aby bylo možné použít Entity Framework k dotazování, vkládání, aktualizaci a odstraňování dat pomocí objektů .NET, je nejprve nutné [vytvořit model](~/ef6/modeling/index.md) , který mapuje entity a vztahy, které jsou definovány v modelu, na tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="47b25-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="47b25-104">Jakmile máte model, primární třída, se kterou komunikuje vaše aplikace, je `System.Data.Entity.DbContext` (často se označuje jako třída kontextu).</span><span class="sxs-lookup"><span data-stu-id="47b25-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="47b25-105">Pomocí DbContext přidruženého k modelu můžete:</span><span class="sxs-lookup"><span data-stu-id="47b25-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="47b25-106">Zápis a spouštění dotazů</span><span class="sxs-lookup"><span data-stu-id="47b25-106">Write and execute queries</span></span>   
- <span data-ttu-id="47b25-107">Vyhodnotit výsledků dotazu jako objekty entit</span><span class="sxs-lookup"><span data-stu-id="47b25-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="47b25-108">Sledování změn provedených u těchto objektů</span><span class="sxs-lookup"><span data-stu-id="47b25-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="47b25-109">Zachovat změny objektů zpátky v databázi</span><span class="sxs-lookup"><span data-stu-id="47b25-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="47b25-110">Svázání objektů v paměti s ovládacími prvky uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="47b25-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="47b25-111">Tato stránka obsahuje pokyny k tomu, jak spravovat třídu kontextu.</span><span class="sxs-lookup"><span data-stu-id="47b25-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="47b25-112">Definování odvozené třídy DbContext</span><span class="sxs-lookup"><span data-stu-id="47b25-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="47b25-113">Doporučeným způsobem, jak pracovat s kontextem, je definovat třídu, která je odvozena z DbContext a zpřístupňuje vlastnosti Negenerickými, které reprezentují kolekce zadaných entit v kontextu.</span><span class="sxs-lookup"><span data-stu-id="47b25-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="47b25-114">Pokud pracujete s návrhářem EF, bude pro vás generován kontext.</span><span class="sxs-lookup"><span data-stu-id="47b25-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="47b25-115">Pokud pracujete se Code First, budete obvykle psát kontext sami.</span><span class="sxs-lookup"><span data-stu-id="47b25-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="47b25-116">Jakmile budete mít kontext, měli byste dotazovat na, přidat (pomocí `Add` nebo `Attach` metody) nebo odebrat entity (pomocí `Remove`) v kontextu prostřednictvím těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="47b25-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="47b25-117">Přístup k vlastnosti `DbSet` u objektu Context představuje počáteční dotaz, který vrátí všechny entity zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="47b25-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="47b25-118">Všimněte si, že při pouhém přístupu k vlastnosti se dotaz nespustí.</span><span class="sxs-lookup"><span data-stu-id="47b25-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="47b25-119">Dotaz se spustí, když:</span><span class="sxs-lookup"><span data-stu-id="47b25-119">A query is executed when:</span></span>  

- <span data-ttu-id="47b25-120">Je vyhodnoceno pomocí příkazu `foreach` (C#) nebo `For Each` (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="47b25-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="47b25-121">Je vyhodnocena operací shromažďování, například `ToArray`, `ToDictionary`nebo `ToList`.</span><span class="sxs-lookup"><span data-stu-id="47b25-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="47b25-122">V krajní části dotazu jsou uvedeny operátory LINQ, jako je například `First` nebo `Any`.</span><span class="sxs-lookup"><span data-stu-id="47b25-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="47b25-123">Je volána jedna z následujících metod: metoda rozšíření `Load`, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`a `DbSet<T>.Find`, pokud entita se zadaným klíčem nebyla v kontextu již načtena.</span><span class="sxs-lookup"><span data-stu-id="47b25-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="47b25-124">Životnost</span><span class="sxs-lookup"><span data-stu-id="47b25-124">Lifetime</span></span>  

<span data-ttu-id="47b25-125">Životnost kontextu začíná při vytvoření instance a končí, když je instance buď vyřazena, nebo uvolňována paměti.</span><span class="sxs-lookup"><span data-stu-id="47b25-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="47b25-126">Použijte k **použití** , pokud chcete, aby všechny prostředky, které kontext ovládá, byly odstraněny na konci bloku.</span><span class="sxs-lookup"><span data-stu-id="47b25-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="47b25-127">Při použití metody **using**kompilátor automaticky vytvoří blok try/finally a zavolá Dispose v bloku **finally** .</span><span class="sxs-lookup"><span data-stu-id="47b25-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="47b25-128">Tady jsou některé obecné pokyny pro rozhodování o životnosti kontextu:</span><span class="sxs-lookup"><span data-stu-id="47b25-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="47b25-129">Při práci s webovými aplikacemi použijte kontextovou instanci na požadavek.</span><span class="sxs-lookup"><span data-stu-id="47b25-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="47b25-130">Při práci s Windows Presentation Foundation (WPF) nebo model Windows Forms použijte kontextovou instanci na formulář.</span><span class="sxs-lookup"><span data-stu-id="47b25-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="47b25-131">To vám umožní používat funkce sledování změn, které poskytuje kontext.</span><span class="sxs-lookup"><span data-stu-id="47b25-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="47b25-132">Je-li instance kontextu vytvořena pomocí kontejneru pro vkládání závislostí, obvykle je odpovědností kontejneru, aby odstranil kontext.</span><span class="sxs-lookup"><span data-stu-id="47b25-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="47b25-133">Pokud je kontext vytvořen v kódu aplikace, nezapomeňte odstranit kontext, pokud již není požadován.</span><span class="sxs-lookup"><span data-stu-id="47b25-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="47b25-134">Při práci s dlouhodobě běžícím kontextem zvažte následující:</span><span class="sxs-lookup"><span data-stu-id="47b25-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="47b25-135">Při načítání více objektů a jejich odkazů do paměti se může velmi rychle zvýšit spotřeba paměti daného kontextu.</span><span class="sxs-lookup"><span data-stu-id="47b25-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="47b25-136">To může způsobit problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="47b25-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="47b25-137">Kontext není bezpečný pro přístup z více vláken, proto by neměl být sdílen napříč více vlákny, na kterých pracuje souběžně.</span><span class="sxs-lookup"><span data-stu-id="47b25-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="47b25-138">Pokud výjimka způsobí, že je kontext v neobnovitelné stavu, může dojít k ukončení celé aplikace.</span><span class="sxs-lookup"><span data-stu-id="47b25-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="47b25-139">Šance na to, aby se při potížích s souběžnou přenosem zvýšila velikost, se zvyšuje jako mezera mezi okamžikem, kdy se data dotazují a aktualizují.</span><span class="sxs-lookup"><span data-stu-id="47b25-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="47b25-140">Připojení</span><span class="sxs-lookup"><span data-stu-id="47b25-140">Connections</span></span>  

<span data-ttu-id="47b25-141">Ve výchozím nastavení kontext spravuje připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="47b25-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="47b25-142">Kontext v případě potřeby otevře a zavře připojení.</span><span class="sxs-lookup"><span data-stu-id="47b25-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="47b25-143">Například kontext otevírá připojení pro spuštění dotazu a pak ukončí připojení při zpracování všech sad výsledků.</span><span class="sxs-lookup"><span data-stu-id="47b25-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="47b25-144">Existují případy, kdy chcete mít větší kontrolu nad tím, kdy se připojení otevírá a zavírá.</span><span class="sxs-lookup"><span data-stu-id="47b25-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="47b25-145">Například při práci s SQL Server Compact se často doporučuje udržovat samostatné otevřené připojení k databázi po dobu života aplikace za účelem zvýšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="47b25-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="47b25-146">Tento proces můžete spravovat ručně pomocí vlastnosti `Connection`.</span><span class="sxs-lookup"><span data-stu-id="47b25-146">You can manage this process manually by using the `Connection` property.</span></span>  
