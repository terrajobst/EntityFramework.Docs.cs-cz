---
title: Dotazování a hledání entit – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417168"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="37fce-102">Dotazování a hledání entit</span><span class="sxs-lookup"><span data-stu-id="37fce-102">Querying and Finding Entities</span></span>
<span data-ttu-id="37fce-103">Toto téma popisuje různé způsoby, jak můžete dotazovat na data pomocí entity framework, včetně LINQ a Find metoda.</span><span class="sxs-lookup"><span data-stu-id="37fce-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="37fce-104">Techniky uvedené v tomto tématu platí stejně pro modely vytvořené pomocí Code First a EF Designer.</span><span class="sxs-lookup"><span data-stu-id="37fce-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="37fce-105">Hledání entit pomocí dotazu</span><span class="sxs-lookup"><span data-stu-id="37fce-105">Finding entities using a query</span></span>  

<span data-ttu-id="37fce-106">DbSet a IDbSet implementovat IQueryable a tak lze použít jako výchozí bod pro zápis dotazu LINQ proti databázi.</span><span class="sxs-lookup"><span data-stu-id="37fce-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="37fce-107">Toto není vhodné místo pro hloubkovou diskusi o LINQ, ale zde je několik jednoduchých příkladů:</span><span class="sxs-lookup"><span data-stu-id="37fce-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

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

<span data-ttu-id="37fce-108">Všimněte si, že DbSet a IDbSet vždy vytvářet dotazy proti databázi a bude vždy zahrnovat odezvu do databáze i v případě, že entity vrácené již existují v kontextu.</span><span class="sxs-lookup"><span data-stu-id="37fce-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="37fce-109">Dotaz je proveden proti databázi, pokud:</span><span class="sxs-lookup"><span data-stu-id="37fce-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="37fce-110">Je výčtu **foreach** (C#) nebo **for each** (Visual Basic) příkaz.</span><span class="sxs-lookup"><span data-stu-id="37fce-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="37fce-111">Je výčtu operace kolekce, jako je [například ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)nebo [ToList](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="37fce-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="37fce-112">Linq operátory jako [První](https://msdn.microsoft.com/library/bb291976) nebo [Any](https://msdn.microsoft.com/library/bb337697) jsou určeny v nejvzdálenější části dotazu.</span><span class="sxs-lookup"><span data-stu-id="37fce-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="37fce-113">Nazývají se následující metody: Metoda [rozšíření Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) na dbset, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)a Database.ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="37fce-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="37fce-114">Při vrácení výsledků z databáze jsou k kontextu připojeny objekty, které neexistují v kontextu.</span><span class="sxs-lookup"><span data-stu-id="37fce-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="37fce-115">Pokud je objekt již v kontextu, je vrácen existující objekt (aktuální a původní hodnoty vlastností objektu v položce **nejsou** přepsány hodnotami databáze).</span><span class="sxs-lookup"><span data-stu-id="37fce-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="37fce-116">Při provádění dotazu entity, které byly přidány do kontextu, ale dosud nebyly uloženy do databáze nejsou vráceny jako součást sady výsledků.</span><span class="sxs-lookup"><span data-stu-id="37fce-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="37fce-117">Chcete-li získat data, která je v kontextu, naleznete [v tématu místní data](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="37fce-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="37fce-118">Pokud dotaz vrátí žádné řádky z databáze, výsledkem bude prázdná kolekce, nikoli **null**.</span><span class="sxs-lookup"><span data-stu-id="37fce-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="37fce-119">Hledání entit pomocí primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="37fce-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="37fce-120">Find Metoda na DbSet používá hodnotu primárního klíče k pokusu o nalezení entity sledované kontextem.</span><span class="sxs-lookup"><span data-stu-id="37fce-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="37fce-121">Pokud entita není nalezena v kontextu, bude do databáze odeslán dotaz, aby se tam entita nalezla.</span><span class="sxs-lookup"><span data-stu-id="37fce-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="37fce-122">Null je vrácena, pokud entita není nalezena v kontextu nebo v databázi.</span><span class="sxs-lookup"><span data-stu-id="37fce-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="37fce-123">Hledání se liší od použití dotazu dvěma významnými způsoby:</span><span class="sxs-lookup"><span data-stu-id="37fce-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="37fce-124">Odezva do databáze bude provedena pouze v případě, že entita s daným klíčem není nalezena v kontextu.</span><span class="sxs-lookup"><span data-stu-id="37fce-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="37fce-125">Hledání vrátí entity, které jsou ve stavu Added.</span><span class="sxs-lookup"><span data-stu-id="37fce-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="37fce-126">To znamená Najít vrátí entity, které byly přidány do kontextu, ale dosud nebyly uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="37fce-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="37fce-127">Hledání entity podle primárního klíče</span><span class="sxs-lookup"><span data-stu-id="37fce-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="37fce-128">Následující kód ukazuje některá použití najít:</span><span class="sxs-lookup"><span data-stu-id="37fce-128">The following code shows some uses of Find:</span></span>  

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

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="37fce-129">Hledání entity pomocí složeného primárního klíče</span><span class="sxs-lookup"><span data-stu-id="37fce-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="37fce-130">Entity Framework umožňuje entity mít složené klíče – to je klíč, který se skládá z více než jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="37fce-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="37fce-131">Můžete mít například entitu Nastavení blogu, která představuje nastavení uživatelů pro konkrétní blog.</span><span class="sxs-lookup"><span data-stu-id="37fce-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="37fce-132">Vzhledem k tomu, že uživatel by měl pouze jeden BlogSettings pro každý blog, můžete se rozhodnout, že primární klíč BlogSettings je kombinací BlogId a uživatelského jména.</span><span class="sxs-lookup"><span data-stu-id="37fce-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="37fce-133">Následující kód se pokusí najít BlogSettings s BlogId = 3 a uživatelské jméno = "johndoe1987":</span><span class="sxs-lookup"><span data-stu-id="37fce-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="37fce-134">Všimněte si, že pokud máte složené klíče, musíte použít ColumnAttribute nebo fluent API k určení pořadí vlastností složeného klíče.</span><span class="sxs-lookup"><span data-stu-id="37fce-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="37fce-135">Volání Najít musí použít toto pořadí při zadávání hodnot, které tvoří klíč.</span><span class="sxs-lookup"><span data-stu-id="37fce-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
