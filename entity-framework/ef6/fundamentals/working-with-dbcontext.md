---
title: Práce s DbContext - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: f95f503c4e40e65503d5af0c1b686d0055728bfe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998143"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="a0696-102">Práce s DbContext</span><span class="sxs-lookup"><span data-stu-id="a0696-102">Working with DbContext</span></span>

<span data-ttu-id="a0696-103">Pokud chcete používat rozhraní Entity Framework k dotazování, vkládání, aktualizace a odstranění dat pomocí objektů .NET, musíte nejprve [vytvořit Model](~/ef6/modeling/index.md) který mapuje entit a vztahů, které jsou definovány v modelu s tabulkami v databázi.</span><span class="sxs-lookup"><span data-stu-id="a0696-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="a0696-104">Jakmile model aplikace komunikuje s hlavní třídou je `System.Data.Entity.DbContext` (často označované jako třídy kontextu).</span><span class="sxs-lookup"><span data-stu-id="a0696-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="a0696-105">Můžete použít DbContext přidružené k modelu:</span><span class="sxs-lookup"><span data-stu-id="a0696-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="a0696-106">Zápis a spouštění dotazů</span><span class="sxs-lookup"><span data-stu-id="a0696-106">Write and execute queries</span></span>   
- <span data-ttu-id="a0696-107">Sloučit výsledky dotazu jako objekty entity</span><span class="sxs-lookup"><span data-stu-id="a0696-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="a0696-108">Sledovat změny provedené na tyto objekty</span><span class="sxs-lookup"><span data-stu-id="a0696-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="a0696-109">Zachovat změny objektu zpět na databázi</span><span class="sxs-lookup"><span data-stu-id="a0696-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="a0696-110">Vytvoření vazby objektů v paměti pro ovládací prvky uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="a0696-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="a0696-111">Tato stránka poskytuje pokyny o tom, jak spravovat třídy kontextu.</span><span class="sxs-lookup"><span data-stu-id="a0696-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="a0696-112">Definování DbContext odvozené třídy</span><span class="sxs-lookup"><span data-stu-id="a0696-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="a0696-113">Doporučeným způsobem, jak pracovat s kontextem je definovat třídu, která je odvozena od položky DbContext a zpřístupní vlastnosti DbSet, které představují kolekce zadané entity v kontextu.</span><span class="sxs-lookup"><span data-stu-id="a0696-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="a0696-114">Pokud pracujete s EF designeru, vygeneruje se pro vás kontextu.</span><span class="sxs-lookup"><span data-stu-id="a0696-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="a0696-115">Pokud pracujete s Code First, obvykle napíšete kontextu sami.</span><span class="sxs-lookup"><span data-stu-id="a0696-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="a0696-116">Jakmile budete mít kontext, by pro dotazování, přidejte (pomocí `Add` nebo `Attach` metody) nebo odstranění (pomocí `Remove`) entity v kontextu prostřednictvím těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="a0696-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="a0696-117">Přístup k `DbSet` vlastnost u objektu context představují výchozí dotaz, který vrátí všechny entity zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="a0696-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="a0696-118">Mějte na paměti, stačí přístupem k vlastnosti nebude provádět dotaz.</span><span class="sxs-lookup"><span data-stu-id="a0696-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="a0696-119">Dotaz je proveden při:</span><span class="sxs-lookup"><span data-stu-id="a0696-119">A query is executed when:</span></span>  

- <span data-ttu-id="a0696-120">Ve výčtu `foreach` (C#) nebo `For Each` – příkaz (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="a0696-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="a0696-121">Kolekce operací je uveden jako `ToArray`, `ToDictionary`, nebo `ToList`.</span><span class="sxs-lookup"><span data-stu-id="a0696-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="a0696-122">Operátory LINQ, jako například `First` nebo `Any` jsou určené v nejkrajnější část dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0696-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="a0696-123">Jeden z následujících metod volá: `Load` rozšiřující metoda `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, a `DbSet<T>.Find`, pokud zadaný klíč nebyl nalezen již načteny v kontextu.</span><span class="sxs-lookup"><span data-stu-id="a0696-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="a0696-124">Doba platnosti</span><span class="sxs-lookup"><span data-stu-id="a0696-124">Lifetime</span></span>  

<span data-ttu-id="a0696-125">Doba platnosti kontextu začíná, když instance je vytvořena a končí, když je odstraněn nebo uvolňování instance.</span><span class="sxs-lookup"><span data-stu-id="a0696-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="a0696-126">Použití **pomocí** Pokud chcete, aby všechny prostředky, které řídí kontextu má být na konci bloku.</span><span class="sxs-lookup"><span data-stu-id="a0696-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="a0696-127">Při použití **pomocí**, kompilátor automaticky vytvoří blok try/finally a volá funkci dispose v **nakonec** bloku.</span><span class="sxs-lookup"><span data-stu-id="a0696-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="a0696-128">Tady jsou některé obecné pokyny při rozhodování o životnost kontextu:</span><span class="sxs-lookup"><span data-stu-id="a0696-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="a0696-129">Při práci s webovými aplikacemi, používala kontextu instance pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="a0696-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="a0696-130">Při práci s Windows Presentation Foundation (WPF) nebo formulářů Windows, pomocí místní instance na formulář.</span><span class="sxs-lookup"><span data-stu-id="a0696-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="a0696-131">Díky tomu můžete použít funkci řešení change tracking poskytuje daného kontextu.</span><span class="sxs-lookup"><span data-stu-id="a0696-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="a0696-132">Pokud tato instance kontextu vytvoří kontejner vkládání závislostí, je obvykle odpovědnost kontejneru k uvolnění kontextu.</span><span class="sxs-lookup"><span data-stu-id="a0696-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="a0696-133">Pokud kontextu se vytvoří v kódu aplikace, nezapomeňte k uvolnění kontextu, pokud se už nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="a0696-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="a0696-134">Při práci s kontextem dlouhotrvající zvažte následující:</span><span class="sxs-lookup"><span data-stu-id="a0696-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="a0696-135">Jak načíst další objekty a jejich odkazů do paměti, může rychle zvýšit spotřebu paměti kontextu.</span><span class="sxs-lookup"><span data-stu-id="a0696-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="a0696-136">To může způsobit problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="a0696-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="a0696-137">Kontext není bezpečná pro vlákno, proto by neměl být sdíleny napříč více vláken současně provádějící práce na něm.</span><span class="sxs-lookup"><span data-stu-id="a0696-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="a0696-138">Pokud výjimka způsobí, že se kontext, který má být ve stavu Neopravitelná, může ukončit celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0696-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="a0696-139">Pravděpodobnost, že narazíte na problémy s souběžnosti zvýšení nárůstu mezery mezi dobou, kdy se data dotazovat a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="a0696-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="a0696-140">Připojení</span><span class="sxs-lookup"><span data-stu-id="a0696-140">Connections</span></span>  

<span data-ttu-id="a0696-141">Ve výchozím kontextu spravuje připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="a0696-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="a0696-142">Kontext se otevře a zavře připojení podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="a0696-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="a0696-143">Například kontextu otevře připojení k provedení dotazu a potom jej zavře připojení, mají po zpracování všech sadách výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="a0696-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="a0696-144">Existují případy, kdy chcete mít větší kontrolu nad po připojení k otevření a zavření.</span><span class="sxs-lookup"><span data-stu-id="a0696-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="a0696-145">Například při práci s SQL serverem Compact, často doporučujeme udržovat samostatnou otevřít připojení k databázi po dobu životnosti aplikace pro zvýšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="a0696-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="a0696-146">Tento proces můžete spravovat ručně pomocí `Connection` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a0696-146">You can manage this process manually by using the `Connection` property.</span></span>  
