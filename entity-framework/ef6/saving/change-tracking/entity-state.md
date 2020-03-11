---
title: Práce s entitami stavů – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419697"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="c1e88-102">Práce s entitami stavů</span><span class="sxs-lookup"><span data-stu-id="c1e88-102">Working with entity states</span></span>
<span data-ttu-id="c1e88-103">V tomto tématu se dozvíte, jak přidat a připojit entity k kontextu a jak je Entity Framework zpracovávat během metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c1e88-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="c1e88-104">Entity Framework se postará o sledování stavu entit, když jsou připojeni k kontextu, ale v případě odpojených nebo N-vrstvých scénářů můžete v EF zjistit, v jakém stavu by měly být vaše entity.</span><span class="sxs-lookup"><span data-stu-id="c1e88-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="c1e88-105">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="c1e88-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="c1e88-106">Stavy entit a metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="c1e88-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="c1e88-107">Entita může být v jednom z pěti stavů, jak je definováno výčtem EntityState.</span><span class="sxs-lookup"><span data-stu-id="c1e88-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="c1e88-108">Tyto stavy jsou:</span><span class="sxs-lookup"><span data-stu-id="c1e88-108">These states are:</span></span>  

- <span data-ttu-id="c1e88-109">Přidáno: entita je sledována kontextem, ale ještě neexistuje v databázi.</span><span class="sxs-lookup"><span data-stu-id="c1e88-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="c1e88-110">Nezměněno: entita je sledována kontextem a existuje v databázi a její hodnoty vlastností se nezměnily z hodnot v databázi.</span><span class="sxs-lookup"><span data-stu-id="c1e88-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="c1e88-111">Změněno: entita je sledována kontextem a existuje v databázi a některé nebo všechny její hodnoty vlastností byly změněny.</span><span class="sxs-lookup"><span data-stu-id="c1e88-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="c1e88-112">Odstraněno: entita je sledována kontextem a existuje v databázi, ale byla označena pro odstranění z databáze při volání dalšího volání metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c1e88-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="c1e88-113">Odpojeno: entita není sledována kontextem.</span><span class="sxs-lookup"><span data-stu-id="c1e88-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="c1e88-114">SaveChanges používá pro entity v různých stavech různé věci:</span><span class="sxs-lookup"><span data-stu-id="c1e88-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="c1e88-115">Nezměněné entity nejsou nijak ovlivněny pomocí metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c1e88-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="c1e88-116">Aktualizace nejsou odesílány do databáze pro entity v nezměněném stavu.</span><span class="sxs-lookup"><span data-stu-id="c1e88-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="c1e88-117">Přidané entity jsou vloženy do databáze a pak se nezměnily, když se funkce SaveChanges vrátí.</span><span class="sxs-lookup"><span data-stu-id="c1e88-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="c1e88-118">Změněné entity jsou aktualizovány v databázi a pak se nezměnily, když se funkce SaveChanges vrátí.</span><span class="sxs-lookup"><span data-stu-id="c1e88-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="c1e88-119">Odstraněné entity se odstraní z databáze a pak se odpojí z kontextu.</span><span class="sxs-lookup"><span data-stu-id="c1e88-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="c1e88-120">Následující příklady znázorňují způsoby, kterými lze změnit stav entity nebo grafu entity.</span><span class="sxs-lookup"><span data-stu-id="c1e88-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="c1e88-121">Přidání nové entity do kontextu</span><span class="sxs-lookup"><span data-stu-id="c1e88-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="c1e88-122">Do kontextu lze přidat novou entitu voláním metody Add v Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="c1e88-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="c1e88-123">Tím se entita vloží do přidaného stavu, což znamená, že se při příštím volání metody SaveChanges vloží do databáze.</span><span class="sxs-lookup"><span data-stu-id="c1e88-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="c1e88-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e88-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="c1e88-125">Dalším způsobem, jak přidat novou entitu do kontextu, je změnit její stav na přidáno.</span><span class="sxs-lookup"><span data-stu-id="c1e88-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="c1e88-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e88-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="c1e88-127">Nakonec můžete přidat novou entitu do kontextu tím, že ji připojíte k jiné entitě, která je již sledována.</span><span class="sxs-lookup"><span data-stu-id="c1e88-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="c1e88-128">To může být přidání nové entity do navigační vlastnosti kolekce jiné entity nebo nastavením navigační vlastnosti odkazu jiné entity, která odkazuje na novou entitu.</span><span class="sxs-lookup"><span data-stu-id="c1e88-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="c1e88-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e88-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="c1e88-130">Všimněte si, že pro všechny tyto příklady, pokud přidávaná entita obsahuje odkazy na jiné entity, které ještě nejsou sledovány, budou tyto nové entity přidány také do kontextu a při příštím volání metody SaveChanges budou vloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="c1e88-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="c1e88-131">Připojení existující entity k kontextu</span><span class="sxs-lookup"><span data-stu-id="c1e88-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="c1e88-132">Pokud máte entitu, kterou víte, že již v databázi existuje, ale není aktuálně sledována kontextem, můžete určit, aby kontext sledoval entitu pomocí metody Attach v Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="c1e88-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="c1e88-133">Entita bude v kontextu v nezměněném stavu.</span><span class="sxs-lookup"><span data-stu-id="c1e88-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="c1e88-134">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e88-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="c1e88-135">Všimněte si, že v databázi nebudou provedeny žádné změny, pokud je volána metoda SaveChanges bez jakékoli jiné manipulace s připojenou entitou.</span><span class="sxs-lookup"><span data-stu-id="c1e88-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="c1e88-136">Je to proto, že entita je ve stavu Unchanged.</span><span class="sxs-lookup"><span data-stu-id="c1e88-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="c1e88-137">Dalším způsobem, jak připojit existující entitu k kontextu, je změnit svůj stav na nezměněný.</span><span class="sxs-lookup"><span data-stu-id="c1e88-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="c1e88-138">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e88-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="c1e88-139">Všimněte si, že v obou těchto příkladech platí, že pokud entita, která je připojena, odkazuje na jiné entity, které ještě nejsou sledovány, budou tyto nové entity také připojeny k tomuto kontextu v nezměněném stavu.</span><span class="sxs-lookup"><span data-stu-id="c1e88-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="c1e88-140">Připojení existující ale změněné entity k kontextu</span><span class="sxs-lookup"><span data-stu-id="c1e88-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="c1e88-141">Máte-li entitu, kterou víte již v databázi existuje, ale na kterou mohly být provedeny změny, můžete určit, že má kontext připojit entitu a nastavit jeho stav na změněno.</span><span class="sxs-lookup"><span data-stu-id="c1e88-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="c1e88-142">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e88-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="c1e88-143">Když změníte stav na změněno, všechny vlastnosti entity budou označeny jako upravené a všechny hodnoty vlastností budou odeslány do databáze při volání metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c1e88-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="c1e88-144">Všimněte si, že pokud připojená entita obsahuje odkazy na jiné entity, které ještě nejsou sledovány, budou tyto nové entity připojeny k tomuto kontextu v nezměněném stavu – nebudou automaticky upraveny.</span><span class="sxs-lookup"><span data-stu-id="c1e88-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="c1e88-145">Pokud máte více entit, které je třeba označit jako změněné, měli byste nastavit stav pro každou z těchto entit jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="c1e88-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="c1e88-146">Změna stavu sledované entity</span><span class="sxs-lookup"><span data-stu-id="c1e88-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="c1e88-147">Stav entity, která je již sledována, můžete změnit nastavením vlastnosti stav na jejím vstupu.</span><span class="sxs-lookup"><span data-stu-id="c1e88-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="c1e88-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e88-148">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="c1e88-149">Všimněte si, že volání metody Add nebo Attach pro entitu, která je již sledována, lze také použít ke změně stavu entity.</span><span class="sxs-lookup"><span data-stu-id="c1e88-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="c1e88-150">Například volání metody attach pro entitu, která je aktuálně ve stavu přidáno, změní svůj stav na nezměněný.</span><span class="sxs-lookup"><span data-stu-id="c1e88-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="c1e88-151">Vložit nebo aktualizovat vzor</span><span class="sxs-lookup"><span data-stu-id="c1e88-151">Insert or update pattern</span></span>  

<span data-ttu-id="c1e88-152">Běžným vzorem pro některé aplikace je přidání entity jako nového (výsledkem vložení do databáze) nebo připojení entity jako existující a její označení jako upravené (výsledkem aktualizace databáze) v závislosti na hodnotě primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="c1e88-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="c1e88-153">Například při použití databáze generované celočíselnými klíči je běžné zacházet se entitou s nulovým klíčem jako novou a entitou s nenulovým klíčem jako stávající.</span><span class="sxs-lookup"><span data-stu-id="c1e88-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="c1e88-154">Tento vzor lze dosáhnout nastavením stavu entity na základě kontroly hodnoty primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="c1e88-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="c1e88-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c1e88-155">For example:</span></span>  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

<span data-ttu-id="c1e88-156">Všimněte si, že když změníte stav na změněno, všechny vlastnosti entity budou označeny jako upravené a všechny hodnoty vlastností budou odeslány do databáze při volání metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c1e88-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
