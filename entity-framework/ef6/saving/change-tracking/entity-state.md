---
title: Práce s stavy entity - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
caps.latest.revision: 3
ms.openlocfilehash: e27d70dd7a6a63c2b225e9cf4fe7ac8cc280d4ca
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912055"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="cf886-102">Práce s stavy entity</span><span class="sxs-lookup"><span data-stu-id="cf886-102">Working with entity states</span></span>
<span data-ttu-id="cf886-103">Toto téma zahrnuje přidání a připojit entity do kontextu a jak Entity Framework zpracovává během SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="cf886-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="cf886-104">Sledování stavu entity, pokud jsou připojeny k kontextu, ale v odpojeném nebo N-vrstvá scénáře můžete nechat EF vědět, jednotlivých stavech entity musí být v postará Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cf886-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="cf886-105">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="cf886-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="cf886-106">Stavy entity a SaveChanges</span><span class="sxs-lookup"><span data-stu-id="cf886-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="cf886-107">Entita může být v jednom z pěti stavů fronty definovaných výčtem EntityState.</span><span class="sxs-lookup"><span data-stu-id="cf886-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="cf886-108">Tyto stavy jsou:</span><span class="sxs-lookup"><span data-stu-id="cf886-108">These states are:</span></span>  

- <span data-ttu-id="cf886-109">Přidání: Entita sledován správou kontextu, ale dosud neexistuje v databázi</span><span class="sxs-lookup"><span data-stu-id="cf886-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="cf886-110">Beze změny: Entita sledován správou kontextu a existuje v databázi a její hodnoty vlastností se neliší od hodnot v databázi</span><span class="sxs-lookup"><span data-stu-id="cf886-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="cf886-111">Upravené: Entita sledován správou kontextu a v databázi existuje a byly změněny některé nebo všechny příslušné hodnoty vlastnosti</span><span class="sxs-lookup"><span data-stu-id="cf886-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="cf886-112">Odstraněné: Entita sledován správou kontextu existuje v databázi, ale byl označen k odstranění v databázi při dalším je volána metoda SaveChanges</span><span class="sxs-lookup"><span data-stu-id="cf886-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="cf886-113">Odpojit: entity není sledována podle kontextu</span><span class="sxs-lookup"><span data-stu-id="cf886-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="cf886-114">SaveChanges provádí různé věci pro entity v různých stavech:</span><span class="sxs-lookup"><span data-stu-id="cf886-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="cf886-115">Beze změny entity nejsou přistupovala SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="cf886-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="cf886-116">Aktualizace se neodešlou do databáze pro entity v nezměněném stavu.</span><span class="sxs-lookup"><span data-stu-id="cf886-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="cf886-117">Přidání entity jsou vloženy do databáze a pak budou beze změn při SaveChanges vrátí.</span><span class="sxs-lookup"><span data-stu-id="cf886-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="cf886-118">Změny entity jsou aktualizována v databázi a pak budou beze změn při SaveChanges vrátí.</span><span class="sxs-lookup"><span data-stu-id="cf886-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="cf886-119">Odstraněné entity jsou odstraněna z databáze a potom se odpojí z kontextu.</span><span class="sxs-lookup"><span data-stu-id="cf886-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="cf886-120">Následující příklady ukazují způsoby, ve kterém lze změnit stav entity nebo grafu entity.</span><span class="sxs-lookup"><span data-stu-id="cf886-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="cf886-121">Přidání nové entity v kontextu</span><span class="sxs-lookup"><span data-stu-id="cf886-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="cf886-122">Nová entita lze přidat do kontextu pomocí volání metody Add u DbSet.</span><span class="sxs-lookup"><span data-stu-id="cf886-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="cf886-123">To umístí entity do stavu Added, což znamená, že ho bude vložen do databáze při příštím, která je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="cf886-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="cf886-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cf886-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="cf886-125">Dalším způsobem, jak přidat nové entity v kontextu je změna stavu na přidání.</span><span class="sxs-lookup"><span data-stu-id="cf886-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="cf886-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cf886-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="cf886-127">Nakonec můžete přidat nové entity v kontextu podle zapojení až jinou entitu, která je již sledován.</span><span class="sxs-lookup"><span data-stu-id="cf886-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="cf886-128">To může být tak, že přidáte nové entity pro navigační vlastnost kolekce z druhé entity nebo tak, že nastavíte vlastnost navigace odkaz z druhé entity tak, aby odkazoval na nové entity.</span><span class="sxs-lookup"><span data-stu-id="cf886-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="cf886-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cf886-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="cf886-130">Všimněte si, že u všech těchto příkladech, pokud má entita, přidává odkazy na jiné entity, které nejsou ještě sledovat pak tyto nové entity se také zařadí do kontextu a budou při příštím, která je volána metoda SaveChanges vložena do databáze.</span><span class="sxs-lookup"><span data-stu-id="cf886-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="cf886-131">Připojení existující entity v kontextu</span><span class="sxs-lookup"><span data-stu-id="cf886-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="cf886-132">Pokud máte v databázi, ale které se aktuálně nesleduje podle kontextu můžete v kontextu ke sledování entity pomocí metody připojit na DbSet existuje entita, která už znáte.</span><span class="sxs-lookup"><span data-stu-id="cf886-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="cf886-133">Entita bude v nezměněném stavu v kontextu.</span><span class="sxs-lookup"><span data-stu-id="cf886-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="cf886-134">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cf886-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="cf886-135">Všimněte si, že žádné změny provedené do databáze, pokud SaveChanges je volána bez provádění jakékoli manipulaci s připojenými entity.</span><span class="sxs-lookup"><span data-stu-id="cf886-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="cf886-136">Je to proto, že entita je v nezměněném stavu.</span><span class="sxs-lookup"><span data-stu-id="cf886-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="cf886-137">Dalším způsobem, jak připojit stávající entity v kontextu je změnit její stav Unchanged.</span><span class="sxs-lookup"><span data-stu-id="cf886-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="cf886-138">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cf886-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="cf886-139">Všimněte si, že pro oba tyto příklady Pokud entita připojovaný obsahuje odkazy na jiné entity, které ještě nejsou sledovány pak tyto nové entity se taky připojit ke kontextu v nezměněném stavu.</span><span class="sxs-lookup"><span data-stu-id="cf886-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="cf886-140">Připojení existující ale upravená entita v kontextu</span><span class="sxs-lookup"><span data-stu-id="cf886-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="cf886-141">Pokud máte entity, o kterém víte už existuje v databázi, ale které může byly provedeny změny můžete v kontextu připojte entitu a nastavit jeho stav Modified.</span><span class="sxs-lookup"><span data-stu-id="cf886-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="cf886-142">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cf886-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="cf886-143">Při změně stavu na změněné všechny vlastnosti entity se označí jako upravená a hodnoty všech vlastností se odešlou do databáze, když je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="cf886-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="cf886-144">Všimněte si, že pokud entita připojovaný obsahuje odkazy na jiné entity, které ještě nejsou sledovány, pak tyto nové entity se připojit ke kontextu v nezměněném stavu – nebude automaticky se stane změněno.</span><span class="sxs-lookup"><span data-stu-id="cf886-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="cf886-145">Pokud máte více entit, které je třeba označit změněné byste měli nastavit stav zvlášť pro každý z těchto entit.</span><span class="sxs-lookup"><span data-stu-id="cf886-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="cf886-146">Změna stavu sledovaného entity</span><span class="sxs-lookup"><span data-stu-id="cf886-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="cf886-147">Můžete změnit stav entity, která je již sledován nastavením vlastnosti stavu na vstupu.</span><span class="sxs-lookup"><span data-stu-id="cf886-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="cf886-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cf886-148">For example:</span></span>  

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

<span data-ttu-id="cf886-149">Všimněte si, že volání přidat nebo připojit pro entitu, která je již sledován je také možné změny stavu entity.</span><span class="sxs-lookup"><span data-stu-id="cf886-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="cf886-150">Například volání připojení pro entitu, která je aktuálně ve stavu Added se změní jeho stav Unchanged.</span><span class="sxs-lookup"><span data-stu-id="cf886-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="cf886-151">Vložit nebo aktualizovat vzor</span><span class="sxs-lookup"><span data-stu-id="cf886-151">Insert or update pattern</span></span>  

<span data-ttu-id="cf886-152">Běžným vzorem pro některé aplikace je přidání entity jako nový (výsledkem vložení databáze) nebo připojit entitu jako existující a označte ji jako změněnou (výsledkem je aktualizace databáze) v závislosti na hodnotě primární klíč.</span><span class="sxs-lookup"><span data-stu-id="cf886-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="cf886-153">Například při použití primárních klíčů databáze generované celé číslo je běžné zacházet se žádný klíč jako nové a nenulové klíč jako existující.</span><span class="sxs-lookup"><span data-stu-id="cf886-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="cf886-154">Tento vzor lze dosáhnout nastavením stavu entity podle kontrolu hodnoty primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="cf886-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="cf886-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cf886-155">For example:</span></span>  

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

<span data-ttu-id="cf886-156">Všimněte si, že při změně stavu na změněné všechny vlastnosti entity se označí jako upravená a hodnoty všech vlastností se odešlou do databáze, když je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="cf886-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
