---
title: Dotazování a hledání entit - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489788"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="06077-102">Dotazování a hledání entit</span><span class="sxs-lookup"><span data-stu-id="06077-102">Querying and Finding Entities</span></span>
<span data-ttu-id="06077-103">Toto téma popisuje různé způsoby, můžete zadat dotaz na data pomocí Entity Frameworku, včetně LINQ a metodu Find.</span><span class="sxs-lookup"><span data-stu-id="06077-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="06077-104">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="06077-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="06077-105">Hledání pomocí dotazu entity</span><span class="sxs-lookup"><span data-stu-id="06077-105">Finding entities using a query</span></span>  

<span data-ttu-id="06077-106">DbSet a IDbSet implementující rozhraní IQueryable a proto může sloužit jako výchozí bod pro psaní dotazu LINQ na databázi.</span><span class="sxs-lookup"><span data-stu-id="06077-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="06077-107">Toto není vhodné místo pro podrobnou diskuzi o LINQ, ale tady je několik jednoduchých příkladů:</span><span class="sxs-lookup"><span data-stu-id="06077-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

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

<span data-ttu-id="06077-108">Všimněte si, že DbSet IDbSet vždy vytvořit dotazy na databázi a bude vždy zahrnovat odezvy databáze, i v případě, že entity, které vrátil již existují v kontextu.</span><span class="sxs-lookup"><span data-stu-id="06077-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="06077-109">Dotaz je proveden v databázi při:</span><span class="sxs-lookup"><span data-stu-id="06077-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="06077-110">Ve výčtu **foreach** (C#) nebo **pro každou** – příkaz (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="06077-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="06077-111">Kolekce operací je uveden jako [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), nebo [ToList](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="06077-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="06077-112">Operátory LINQ, jako například [první](https://msdn.microsoft.com/library/bb291976) nebo [jakékoli](https://msdn.microsoft.com/library/bb337697) jsou určené v nejkrajnější část dotazu.</span><span class="sxs-lookup"><span data-stu-id="06077-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="06077-113">Tyto metody jsou volány: [zatížení](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) rozšiřující metody na DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)a Database.ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="06077-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="06077-114">Když výsledky jsou vráceny z databáze, jsou objekty, které neexistují v rámci připojit ke kontextu.</span><span class="sxs-lookup"><span data-stu-id="06077-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="06077-115">Pokud objekt je již v kontextu, je vrácen existující objekt (jsou aktuální a původní hodnoty vlastností objektu v položce **není** přepsání hodnot v databázi).</span><span class="sxs-lookup"><span data-stu-id="06077-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="06077-116">Při provádění dotazu entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze, nebudou zobrazeny jako součást sady výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="06077-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="06077-117">Chcete-li získat data, která je v kontextu, najdete v článku [místních dat](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="06077-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="06077-118">Pokud dotaz vrací žádné řádky z databáze, výsledkem bude prázdnou kolekci, spíše než **null**.</span><span class="sxs-lookup"><span data-stu-id="06077-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="06077-119">Vyhledávání entit pomocí primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="06077-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="06077-120">Metoda hledání na DbSet používá hodnotu primárního klíče pokusí se najít entitu sledován pomocí funkce kontextu.</span><span class="sxs-lookup"><span data-stu-id="06077-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="06077-121">Pokud subjektem nebyl nalezen v kontextu se být dotaz odesílat do databáze se najít entitu existuje.</span><span class="sxs-lookup"><span data-stu-id="06077-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="06077-122">Pokud subjektem nebyl nalezen v kontextu nebo v databázi, je vrácena hodnota Null.</span><span class="sxs-lookup"><span data-stu-id="06077-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="06077-123">Hledání se liší od použití dotazu významné dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="06077-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="06077-124">Cesty k databázi se provádí pouze v případě entita s daným klíčem nebyl nalezen v kontextu.</span><span class="sxs-lookup"><span data-stu-id="06077-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="06077-125">Hledání vrátí entity, které jsou ve stavu Added.</span><span class="sxs-lookup"><span data-stu-id="06077-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="06077-126">To je, najít, vrátí entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="06077-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="06077-127">Vyhledávání podle primárního klíče entity</span><span class="sxs-lookup"><span data-stu-id="06077-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="06077-128">Následující kód ukazuje některá použití hledání:</span><span class="sxs-lookup"><span data-stu-id="06077-128">The following code shows some uses of Find:</span></span>  

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

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="06077-129">Vyhledávání podle složený primární klíč entity</span><span class="sxs-lookup"><span data-stu-id="06077-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="06077-130">Entity Framework umožňuje vaší entity, které mají složených klíčů – to je klíč, který se skládá z více než jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="06077-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="06077-131">Například můžete mít BlogSettings entity, která představuje nastavení uživatele pro konkrétní blogu.</span><span class="sxs-lookup"><span data-stu-id="06077-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="06077-132">Vzhledem k tomu, že uživatel bude mít vždy jen jeden BlogSettings pro každý blog, který můžete zvolili, primární klíč BlogSettings kombinací BlogId a uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="06077-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="06077-133">Následující kód se pokusí najít BlogSettings s BlogId = 3 a uživatelské jméno = "johndoe1987":</span><span class="sxs-lookup"><span data-stu-id="06077-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="06077-134">Mějte na paměti, když máte složených klíčů budete muset použít ColumnAttribute nebo rozhraní fluent API Pokud chcete zadat řazení pro vlastnosti složený klíč.</span><span class="sxs-lookup"><span data-stu-id="06077-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="06077-135">Volání hledání musíte použít toto pořadí při zadávání hodnot, které tvoří klíč.</span><span class="sxs-lookup"><span data-stu-id="06077-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
