---
title: Zpracování konfliktů souběžnosti - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: f233af217287dd6bf35e5b7fea8e44974168b312
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997807"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="a5ea4-102">Zpracování konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="a5ea4-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="a5ea4-103">Optimistická souběžnost zahrnuje optimisticky pokusu o uložení vaší entity k databázi v naději, že nedošlo ke změně tamní data od entita byla načtena.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="a5ea4-104">Ukázalo se, která byla data změněna, je vyvolána výjimka, a musíte vyřešit konflikt, než se pokusíte uložit znovu.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="a5ea4-105">Toto téma popisuje způsob zpracování těchto výjimek v rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="a5ea4-106">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="a5ea4-107">Tento příspěvek není vhodné místo pro úplnou diskusi o optimistického řízení souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="a5ea4-108">Následující části se předpokládá základní znalost souběžnosti řešení a zobrazení schémat pro běžné úlohy.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="a5ea4-109">Ujistěte se, mnohé z těchto vzorců použití témata v [práce s hodnotami vlastností](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="a5ea4-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="a5ea4-110">Řešení potíží s souběžnosti, při použití nezávislé přidružení (kde cizí klíč není namapována na vlastnost ve vaší entitě) je mnohem obtížnější než při použití přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="a5ea4-111">Proto pokud se chystáte provést rozlišení souběžnosti v aplikaci se doporučuje vždy mapování cizí klíče do entity.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="a5ea4-112">Následující příklady předpokládají, že používáte přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="a5ea4-113">DbUpdateConcurrencyException je vyvolané SaveChanges zjištěném optimistického řízení souběžnosti výjimka při pokusu o uložení entity, která využívá přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="a5ea4-114">Řešení výjimek optimistického řízení souběžnosti s Reload (databáze služby wins)</span><span class="sxs-lookup"><span data-stu-id="a5ea4-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="a5ea4-115">Znovu načíst metodu je možné přepsat aktuální hodnoty entity s hodnotami v databázi.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="a5ea4-116">Entita potom obvykle dostane zpět uživateli v nějaké podobě a musí pokusit znovu proveďte své změny a znovu ho uložit.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="a5ea4-117">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a5ea4-117">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

<span data-ttu-id="a5ea4-118">Nastavení zarážky na volání SaveChanges a následně upravit entitu, která se ukládá v databázi pomocí jiného nástroje, jako jsou SQL Management Studio je dobrým způsobem, jak simulovat výjimky souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="a5ea4-119">Může také vložení řádku před SaveChanges aktualizace databáze přímo pomocí třídy SqlCommand.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="a5ea4-120">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a5ea4-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="a5ea4-121">Metoda položky na DbUpdateConcurrencyException vrátí instance DbEntityEntry pro entity, které se nepovedlo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="a5ea4-122">(Tato vlastnost aktuálně vždy vrátí jednu hodnotu pro problémy s souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="a5ea4-123">Může vrátit více hodnot pro výjimky obecné aktualizace.) Alternativu v některých situacích může být k získání položek pro všechny entity, které může být potřeba znovu načíst z databáze a znovu načtěte volání pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="a5ea4-124">Řešení výjimek optimistického řízení souběžnosti jako klient služby wins</span><span class="sxs-lookup"><span data-stu-id="a5ea4-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="a5ea4-125">V příkladu výše, který používá nové načtení se někdy označuje jako databáze wins nebo úložiště služby wins, protože hodnoty v této entitě jsou přepsány hodnoty z databáze.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="a5ea4-126">Někdy můžete chtít provést opak a přepsání hodnot v databázi s hodnotami aktuálně v dané entitě.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="a5ea4-127">To se někdy označuje jako klient wins a je možné díky získání aktuální hodnoty v databázi a nastavit je jako původní hodnoty pro entitu.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="a5ea4-128">(Viz [práce s hodnotami vlastností](~/ef6/saving/change-tracking/property-values.md) informace o aktuální a původní hodnoty.) Příklad:</span><span class="sxs-lookup"><span data-stu-id="a5ea4-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="a5ea4-129">Vlastní řešení výjimek optimistického řízení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="a5ea4-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="a5ea4-130">Někdy můžete chtít kombinací hodnot aktuálně v databázi s hodnotami aktuálně v dané entitě.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="a5ea4-131">To obvykle vyžaduje některé vlastní logiku nebo uživatelské interakce.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="a5ea4-132">Například mohou představovat formulář obsahující aktuálních hodnot, hodnot v databázi, uživateli a výchozí sadu přeložit hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="a5ea4-133">Uživatel by pak upravte přeložit hodnoty podle potřeby a bylo by tyto přeložit hodnoty, které se neuloží do databáze.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="a5ea4-134">To lze provést pomocí objektů DbPropertyValues vrácená CurrentValues a GetDatabaseValues na záznam entity.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="a5ea4-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a5ea4-135">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="a5ea4-136">Vlastní řešení pomocí objektů výjimek optimistického řízení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="a5ea4-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="a5ea4-137">Výše uvedený kód používá DbPropertyValues instance pro předávání kolem aktuálního, databáze a vyřešení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="a5ea4-138">V některých případech může být jednodušší použít instance typu entity pro tuto.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="a5ea4-139">To lze provést pomocí metody ToObject a SetValues DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="a5ea4-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="a5ea4-140">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a5ea4-140">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
