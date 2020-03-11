---
title: Zpracování konfliktů souběžnosti – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419690"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="47d39-102">Zpracování konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="47d39-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="47d39-103">Optimistická souběžnost zahrnuje optimistický pokus o uložení vaší entity do databáze v případě, že data, která se od načtení entity nezměnila.</span><span class="sxs-lookup"><span data-stu-id="47d39-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="47d39-104">Pokud dojde k tomu, že se data změnila, je vyvolána výjimka a před dalším pokusem o uložení je nutné konflikt vyřešit.</span><span class="sxs-lookup"><span data-stu-id="47d39-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="47d39-105">Toto téma popisuje, jak zpracovat takové výjimky v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="47d39-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="47d39-106">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="47d39-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="47d39-107">Tento příspěvek není vhodným místem pro úplnou diskuzi o optimistické souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="47d39-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="47d39-108">V následujících částech se předpokládá několik znalostí o řešení souběžnosti a zobrazení vzorů pro běžné úlohy.</span><span class="sxs-lookup"><span data-stu-id="47d39-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="47d39-109">Mnohé z těchto vzorů využívají témata popsaná v tématu [práce s hodnotami vlastností](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="47d39-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="47d39-110">Řešení problémů souběžnosti při používání nezávislých přidružení (kde cizí klíč není namapovaný na vlastnost ve vaší entitě) je mnohem obtížnější než při použití přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="47d39-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="47d39-111">Proto pokud v aplikaci provedete překlad souběžnosti, doporučujeme, abyste vždy namapovali cizí klíče do svých entit.</span><span class="sxs-lookup"><span data-stu-id="47d39-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="47d39-112">Všechny níže uvedené příklady předpokládají, že používáte přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="47d39-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="47d39-113">Rozhraní SaveChanges vyvolá DbUpdateConcurrencyException při zjištění výjimky optimistického zpracování při pokusu o uložení entity, která používá přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="47d39-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="47d39-114">Řešení optimistických výjimek souběžnosti s opětovným načtením (databáze WINS)</span><span class="sxs-lookup"><span data-stu-id="47d39-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="47d39-115">Metodu reload lze použít k přepsání aktuálních hodnot entity o hodnoty nyní v databázi.</span><span class="sxs-lookup"><span data-stu-id="47d39-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="47d39-116">Entita se pak většinou vrátí uživateli v nějakém formuláři a musí se pokusit znovu provést změny a znovu je uložit.</span><span class="sxs-lookup"><span data-stu-id="47d39-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="47d39-117">Příklad:</span><span class="sxs-lookup"><span data-stu-id="47d39-117">For example:</span></span>  

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

<span data-ttu-id="47d39-118">Dobrým způsobem, jak simulovat výjimku souběžnosti, je nastavit zarážku na volání metody SaveChanges a pak upravit entitu, která je ukládána v databázi pomocí jiného nástroje, jako je například SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="47d39-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="47d39-119">Můžete také vložit řádek před metodou SaveChanges a aktualizovat databázi přímo pomocí metody SqlCommand.</span><span class="sxs-lookup"><span data-stu-id="47d39-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="47d39-120">Příklad:</span><span class="sxs-lookup"><span data-stu-id="47d39-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="47d39-121">Metoda Entries v DbUpdateConcurrencyException vrací instance DbEntityEntry pro entity, které se nepodařilo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="47d39-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="47d39-122">(Tato vlastnost v současné době vždy vrací jedinou hodnotu pro problémy souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="47d39-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="47d39-123">Může vrátit více hodnot pro obecné výjimky aktualizace.) Alternativou v některých situacích může být získání položek pro všechny entity, které mohou být nutné z databáze znovu načteny, a volání pro každý z nich.</span><span class="sxs-lookup"><span data-stu-id="47d39-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="47d39-124">Řešení optimistických výjimek souběžnosti jako služby WINS klienta</span><span class="sxs-lookup"><span data-stu-id="47d39-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="47d39-125">Výše uvedený příklad, který používá opětovné načtení, se někdy nazývá databáze WINS nebo ukládá službu WINS, protože hodnoty v entitě jsou přepsány hodnotami z databáze.</span><span class="sxs-lookup"><span data-stu-id="47d39-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="47d39-126">Někdy můžete chtít provést opak a přepsat hodnoty v databázi hodnotami, které jsou aktuálně v entitě.</span><span class="sxs-lookup"><span data-stu-id="47d39-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="47d39-127">Tato situace se někdy označuje jako klient WINS a je možné ji provést získáním aktuálních hodnot databáze a jejich nastavením jako původní hodnoty pro entitu.</span><span class="sxs-lookup"><span data-stu-id="47d39-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="47d39-128">(Informace o aktuálních a původních hodnotách naleznete v tématu [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) .) Například:</span><span class="sxs-lookup"><span data-stu-id="47d39-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="47d39-129">Vlastní rozlišení optimistických výjimek souběžnosti</span><span class="sxs-lookup"><span data-stu-id="47d39-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="47d39-130">Někdy můžete chtít kombinovat hodnoty, které jsou aktuálně v databázi, s hodnotami, které jsou aktuálně v entitě.</span><span class="sxs-lookup"><span data-stu-id="47d39-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="47d39-131">To obvykle vyžaduje určitou vlastní logiku nebo interakci s uživatelem.</span><span class="sxs-lookup"><span data-stu-id="47d39-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="47d39-132">Například můžete zobrazit formulář pro uživatele obsahující aktuální hodnoty, hodnoty v databázi a výchozí sadu vyřešených hodnot.</span><span class="sxs-lookup"><span data-stu-id="47d39-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="47d39-133">Uživatel pak podle potřeby upraví vyřešené hodnoty a bude to vyřešené hodnoty, které se uloží do databáze.</span><span class="sxs-lookup"><span data-stu-id="47d39-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="47d39-134">To se dá udělat pomocí objektů DbPropertyValues vrácených z CurrentValues a GetDatabaseValues v záznamu entity.</span><span class="sxs-lookup"><span data-stu-id="47d39-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="47d39-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="47d39-135">For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="47d39-136">Vlastní rozlišení optimistické výjimky souběžnosti pomocí objektů</span><span class="sxs-lookup"><span data-stu-id="47d39-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="47d39-137">Výše uvedený kód používá instance DbPropertyValues pro předávání aktuálních, databázových a vyřešených hodnot.</span><span class="sxs-lookup"><span data-stu-id="47d39-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="47d39-138">V některých případech může být pro tuto situaci snazší použít instance typu entity.</span><span class="sxs-lookup"><span data-stu-id="47d39-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="47d39-139">To lze provést pomocí metod ToObject a SetValues DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="47d39-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="47d39-140">Příklad:</span><span class="sxs-lookup"><span data-stu-id="47d39-140">For example:</span></span>  

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
