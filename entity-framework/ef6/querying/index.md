---
title: Dotazování a hledání entit – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417168"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="f6dd9-102">Dotazování a hledání entit</span><span class="sxs-lookup"><span data-stu-id="f6dd9-102">Querying and Finding Entities</span></span>
<span data-ttu-id="f6dd9-103">Toto téma popisuje různé způsoby, jak se můžete dotazovat na data pomocí Entity Framework, včetně LINQ a metody Find.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="f6dd9-104">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="f6dd9-105">Hledání entit pomocí dotazu</span><span class="sxs-lookup"><span data-stu-id="f6dd9-105">Finding entities using a query</span></span>  

<span data-ttu-id="f6dd9-106">Negenerickými a IDbSet implementují rozhraní IQueryable a dají se použít jako výchozí bod pro zápis dotazu LINQ na databázi.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="f6dd9-107">Nejedná se o příslušné místo pro podrobné diskuzi LINQ, ale tady je několik jednoduchých příkladů:</span><span class="sxs-lookup"><span data-stu-id="f6dd9-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

<span data-ttu-id="f6dd9-108">Všimněte si, že Negenerickými a IDbSet vždy vytvářejí dotazy na databázi a budou vždy zahrnovat zpáteční cestu k databázi i v případě, že vrácené entity již existují v kontextu.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="f6dd9-109">Dotaz je proveden na databázi v těchto případech:</span><span class="sxs-lookup"><span data-stu-id="f6dd9-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="f6dd9-110">Je vyhodnocen pomocí příkazu **foreach** (C#) nebo **for each** (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="f6dd9-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="f6dd9-111">Je vyhodnocena operací shromažďování, například [ToArray –](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)nebo [ToList –](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="f6dd9-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="f6dd9-112">V krajní části dotazu jsou zadány operátory LINQ, například [First](https://msdn.microsoft.com/library/bb291976) nebo [Any](https://msdn.microsoft.com/library/bb337697) .</span><span class="sxs-lookup"><span data-stu-id="f6dd9-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="f6dd9-113">Jsou volány následující metody: metoda rozšíření [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) na Negenerickými, [DbEntityEntry. reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)a Database. ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="f6dd9-114">Při vrácení výsledků z databáze jsou objekty, které v kontextu neexistují, připojeny k tomuto kontextu.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="f6dd9-115">Pokud objekt již v kontextu je, je vrácen stávající objekt (aktuální a původní hodnoty vlastností objektu v **položce nejsou** přepsány hodnotami databáze).</span><span class="sxs-lookup"><span data-stu-id="f6dd9-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="f6dd9-116">Když provedete dotaz, entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze, nebudou vráceny jako součást sady výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="f6dd9-117">Pokud chcete získat data, která jsou v kontextu, přečtěte si téma [místní data](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="f6dd9-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="f6dd9-118">Pokud dotaz nevrátí z databáze žádné řádky, bude výsledkem prázdná kolekce, nikoli **hodnota null**.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="f6dd9-119">Hledání entit pomocí primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="f6dd9-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="f6dd9-120">Metoda Find v Negenerickými používá k pokusu o nalezení entity sledované kontextem hodnotu primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="f6dd9-121">Pokud se entita v kontextu nenajde, odešle se dotaz do databáze, kde se nachází entita.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="f6dd9-122">Pokud entita nebyla nalezena v kontextu nebo v databázi, je vrácena hodnota null.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="f6dd9-123">Hledání se liší od použití dotazu dvěma významnými způsoby:</span><span class="sxs-lookup"><span data-stu-id="f6dd9-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="f6dd9-124">Zpáteční cesta k databázi bude provedena pouze v případě, že se entita s daným klíčem nenajde v kontextu.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="f6dd9-125">Hledání vrátí entity, které jsou ve stavu přidáno.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="f6dd9-126">To znamená, že příkaz find vrátí entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="f6dd9-127">Hledání entity podle primárního klíče</span><span class="sxs-lookup"><span data-stu-id="f6dd9-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="f6dd9-128">Následující kód ukazuje některé použití najít:</span><span class="sxs-lookup"><span data-stu-id="f6dd9-128">The following code shows some uses of Find:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="f6dd9-129">Hledání entity pomocí složeného primárního klíče</span><span class="sxs-lookup"><span data-stu-id="f6dd9-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="f6dd9-130">Entity Framework umožňuje vašim entitám mít složené klíče – což je klíč, který se skládá z více než jedné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="f6dd9-131">Můžete mít například entitu BlogSettings, která představuje nastavení uživatele pro konkrétní blog.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="f6dd9-132">Vzhledem k tomu, že uživatel bude mít pro každý blog jenom jednu BlogSettings, můžete zvolit, aby primární klíč BlogSettings kombinaci BlogId a username.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="f6dd9-133">Následující kód se pokusí najít BlogSettings s BlogId = 3 a username = "johndoe1987":</span><span class="sxs-lookup"><span data-stu-id="f6dd9-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="f6dd9-134">Všimněte si, že pokud máte složené klíče, potřebujete použít ColumnAttribute nebo rozhraní API Fluent k určení řazení vlastností složeného klíče.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="f6dd9-135">Volání metody Find musí používat toto pořadí při zadávání hodnot, které tvoří klíč.</span><span class="sxs-lookup"><span data-stu-id="f6dd9-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
