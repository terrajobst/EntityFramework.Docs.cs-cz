---
title: Práce s hodnoty vlastností - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: a9b969950ec7dcfb86a2abc9c8bd6cc24899948c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998300"
---
# <a name="working-with-property-values"></a><span data-ttu-id="a9187-102">Práce s hodnotami vlastností</span><span class="sxs-lookup"><span data-stu-id="a9187-102">Working with property values</span></span>
<span data-ttu-id="a9187-103">Ve většině případů Entity Framework se postará o sledování stavu, původní hodnoty a aktuální hodnoty vlastností instance entity.</span><span class="sxs-lookup"><span data-stu-id="a9187-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="a9187-104">Však může být někdy – například odpojená řešení – Pokud chcete zobrazit nebo pracovat s informací, které EF má o vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a9187-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="a9187-105">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="a9187-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="a9187-106">Entity Framework uchovává informace o dvou hodnot pro každou vlastnost sledované entity.</span><span class="sxs-lookup"><span data-stu-id="a9187-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="a9187-107">Aktuální hodnota je aktuální hodnota vlastnosti v entitě, protože název naznačuje.</span><span class="sxs-lookup"><span data-stu-id="a9187-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="a9187-108">Původní hodnota je hodnota, která měla vlastnost entity se posílat dotaz z databáze nebo připojit ke kontextu.</span><span class="sxs-lookup"><span data-stu-id="a9187-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="a9187-109">Existují dva hlavní mechanismy pro práci s hodnotami vlastností:</span><span class="sxs-lookup"><span data-stu-id="a9187-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="a9187-110">Nejde získat hodnotu jedné vlastnosti způsobem silného typu pomocí metody vlastností.</span><span class="sxs-lookup"><span data-stu-id="a9187-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="a9187-111">Hodnoty všech vlastností entity lze načíst do objektu DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="a9187-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="a9187-112">DbPropertyValues pak funguje jako objekt slovníku jako umožňuje číst a nastavovat hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="a9187-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="a9187-113">Hodnoty v objektu DbPropertyValues lze nastavit z hodnot v jiném objektu DbPropertyValues nebo z hodnot v druhý objekt, jako je například jiná kopie entity nebo objekt pro přenos jednoduché dat (DTO).</span><span class="sxs-lookup"><span data-stu-id="a9187-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="a9187-114">Následující části popisují příklady použití obou výše uvedených mechanismů.</span><span class="sxs-lookup"><span data-stu-id="a9187-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="a9187-115">Získání a nastavení aktuální a původní hodnoty jednotlivých vlastností</span><span class="sxs-lookup"><span data-stu-id="a9187-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="a9187-116">Následující příklad ukazuje, jak můžete číst a poté nastavte novou hodnotu aktuální hodnota vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a9187-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="a9187-117">Pomocí původní hodnota vlastnosti namísto vlastnosti CurrentValue čtena nebo nastavena na původní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a9187-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="a9187-118">Všimněte si, že vrácená hodnota je zadán jako "object", když řetězec se používá k určení názvu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a9187-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="a9187-119">Pokud výraz lambda se používá na druhé straně silného typu vrácené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a9187-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="a9187-120">Nastavení hodnoty vlastnosti tímto způsobem bude pouze označí vlastnost upravit, pokud je nová hodnota se liší od původní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a9187-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="a9187-121">Pokud je hodnota vlastnosti nastavena tímto způsobem změn je automaticky rozpoznán i v případě, že AutoDetectChanges je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="a9187-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="a9187-122">Získání a nastavení aktuální hodnota nemapovaných vlastnosti</span><span class="sxs-lookup"><span data-stu-id="a9187-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="a9187-123">Aktuální hodnota vlastnosti, která není namapována na databázi lze také číst.</span><span class="sxs-lookup"><span data-stu-id="a9187-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="a9187-124">Příklad nenamapované vlastnost může být na vlastnost RssLink na blogu.</span><span class="sxs-lookup"><span data-stu-id="a9187-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="a9187-125">Tato hodnota může být vypočtena podle BlogId a proto nemusí být uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="a9187-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="a9187-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a9187-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

<span data-ttu-id="a9187-127">Aktuální hodnota můžete také nastavit, pokud vlastnost zpřístupní setter.</span><span class="sxs-lookup"><span data-stu-id="a9187-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="a9187-128">Čtení hodnot nemapovaných vlastnosti je užitečná při provádění ověření Entity Framework nenamapované vlastností.</span><span class="sxs-lookup"><span data-stu-id="a9187-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="a9187-129">Ze stejného důvodu může číst a nastavit pro vlastnosti entity, které nejsou aktuálně nesleduje podle kontextu aktuální hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a9187-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="a9187-130">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a9187-130">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="a9187-131">Všimněte si, že původní hodnoty nejsou k dispozici pro nemapovaných vlastnosti nebo vlastnosti entity, které nejsou sledován správou kontextu.</span><span class="sxs-lookup"><span data-stu-id="a9187-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="a9187-132">Kontroluje se, zda je vlastnost označena jako upravená</span><span class="sxs-lookup"><span data-stu-id="a9187-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="a9187-133">Následující příklad ukazuje, jak zkontrolovat, zda jednotlivé vlastnosti je označena jako upravená:</span><span class="sxs-lookup"><span data-stu-id="a9187-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="a9187-134">Hodnoty upravené vlastnosti jsou odesílány jako aktualizace do databáze, když je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="a9187-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="a9187-135">Označení jako upravená vlastnost</span><span class="sxs-lookup"><span data-stu-id="a9187-135">Marking a property as modified</span></span>  

<span data-ttu-id="a9187-136">Následující příklad ukazuje, jak vynutit individuální vlastnosti označit jako změny:</span><span class="sxs-lookup"><span data-stu-id="a9187-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="a9187-137">Označí vlastnost jako upravené vynutí aktualizaci se odesílají do databáze pro vlastnost při volání SaveChanges i v případě, že aktuální hodnota vlastnosti je stejný jako původní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a9187-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="a9187-138">Není aktuálně možné resetovat individuální vlastnosti nelze změnit poté, co byla označena jako upravená.</span><span class="sxs-lookup"><span data-stu-id="a9187-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="a9187-139">To je něco, co jsme chcete podporovat v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="a9187-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="a9187-140">Čtení databáze hodnoty všech vlastností entity, aktuální a původní</span><span class="sxs-lookup"><span data-stu-id="a9187-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="a9187-141">Následující příklad znázorňuje způsob čtení aktuálních hodnot, původní hodnoty a hodnoty ve skutečnosti v databázi pro všechny připojené vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="a9187-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

<span data-ttu-id="a9187-142">Aktuální hodnoty jsou hodnoty, které aktuálně obsahuje vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="a9187-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="a9187-143">Původní hodnoty jsou hodnoty, které byla přečtena z databáze, pokud byla dotazována entity.</span><span class="sxs-lookup"><span data-stu-id="a9187-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="a9187-144">Databáze hodnoty jsou hodnoty, jako jsou aktuálně uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="a9187-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="a9187-145">Získání hodnot v databázi je užitečné, když hodnoty v databázi mohl být změněn dotazu entity, jako je při souběžné upravit, pokud chcete databázi provedené jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="a9187-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="a9187-146">Aktuální a původní hodnoty nastavení z jiného objektu</span><span class="sxs-lookup"><span data-stu-id="a9187-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="a9187-147">Aktuální a původní hodnoty sledované entity je možné aktualizovat tak, že zkopírujete hodnoty z jiného objektu.</span><span class="sxs-lookup"><span data-stu-id="a9187-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="a9187-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a9187-148">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

<span data-ttu-id="a9187-149">Spuštění výše uvedený kód bude vytisknout:</span><span class="sxs-lookup"><span data-stu-id="a9187-149">Running the code above will print out:</span></span>  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="a9187-150">Tato technika se někdy používá při aktualizaci entity s hodnotami získanými z klienta v n vrstvou aplikaci nebo volání služby.</span><span class="sxs-lookup"><span data-stu-id="a9187-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="a9187-151">Všimněte si, že objekt použitý nemusí být stejného typu jako entity tak dlouho, dokud má vlastnosti, jejichž názvy odpovídají těm entity.</span><span class="sxs-lookup"><span data-stu-id="a9187-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="a9187-152">V předchozím příkladu instance BlogDTO slouží k aktualizaci původní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a9187-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="a9187-153">Všimněte si, že pouze vlastnosti, které jsou nastaveny na různé hodnoty při kopírování z jiného objektu se označí jako upravená.</span><span class="sxs-lookup"><span data-stu-id="a9187-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="a9187-154">Nastavení aktuální a původní hodnoty ze slovníku</span><span class="sxs-lookup"><span data-stu-id="a9187-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="a9187-155">Aktuální a původní hodnoty sledované entity je možné aktualizovat tak, že zkopírujete hodnoty ze slovníku nebo jiné datové struktury.</span><span class="sxs-lookup"><span data-stu-id="a9187-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="a9187-156">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a9187-156">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

<span data-ttu-id="a9187-157">Vlastnost OriginalValues místo vlastnost CurrentValues nastavit původní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a9187-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="a9187-158">Nastavení aktuální a původní hodnoty ze slovníku pomocí vlastnosti</span><span class="sxs-lookup"><span data-stu-id="a9187-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="a9187-159">O alternativu k použití CurrentValues nebo OriginalValues, jak je znázorněno výše se má používat metoda vlastnosti k nastavení hodnoty jednotlivých vlastností.</span><span class="sxs-lookup"><span data-stu-id="a9187-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="a9187-160">To může být vhodnější, pokud je nutné nastavit hodnoty komplexní vlastností.</span><span class="sxs-lookup"><span data-stu-id="a9187-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="a9187-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a9187-161">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

<span data-ttu-id="a9187-162">V příkladu výše komplexní vlastnosti jsou přistupovat pomocí tečkové názvy.</span><span class="sxs-lookup"><span data-stu-id="a9187-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="a9187-163">Další možnosti pro přístup ke komplexní vlastnosti zobrazí dva oddíly dále v tomto tématu, konkrétně o komplexní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a9187-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="a9187-164">Vytváří se Klonovaný objekt obsahující aktuální, původní nebo hodnot v databázi</span><span class="sxs-lookup"><span data-stu-id="a9187-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="a9187-165">DbPropertyValues objekt se vrátil ze CurrentValues OriginalValues, nebo je možné GetDatabaseValues naklonoval entitu.</span><span class="sxs-lookup"><span data-stu-id="a9187-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="a9187-166">Tenhle klon. bude obsahovat hodnoty vlastnosti z objektu DbPropertyValues použitá k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a9187-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="a9187-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a9187-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="a9187-168">Všimněte si, že objekt se vrátil není entita a není sledována podle kontextu.</span><span class="sxs-lookup"><span data-stu-id="a9187-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="a9187-169">Vrácený objekt také nemá žádné relace nastavena na jiné objekty.</span><span class="sxs-lookup"><span data-stu-id="a9187-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="a9187-170">Klonovaný objekt může být užitečné při řešení problémů souvisejících s souběžných aktualizací do databáze, zejména když uživatelského rozhraní, která zahrnuje datové vazby pro objekty určitého typu se používá.</span><span class="sxs-lookup"><span data-stu-id="a9187-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="a9187-171">Získání a nastavení aktuální a původní hodnoty komplexní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="a9187-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="a9187-172">Hodnota celého komplexní objekt může být pro čtení a nastavte vlastnost metodou stejně jako může být pro primitivní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a9187-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="a9187-173">Kromě toho můžete přejít ke čtení nebo nastavte vlastnosti tohoto objektu nebo vnořený objekt a komplexního objektu.</span><span class="sxs-lookup"><span data-stu-id="a9187-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="a9187-174">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="a9187-174">Here are some examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

<span data-ttu-id="a9187-175">Použijte vlastnost původní hodnota namísto CurrentValue vlastnosti pro získání nebo nastavení původní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a9187-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="a9187-176">Všimněte si, že vlastnost nebo metoda ComplexProperty umožňuje přístup ke komplexní vlastností.</span><span class="sxs-lookup"><span data-stu-id="a9187-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="a9187-177">Metodu ComplexProperty však musí použít, pokud chcete přejít na komplexní objekt s další vlastnost nebo ComplexProperty volá.</span><span class="sxs-lookup"><span data-stu-id="a9187-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="a9187-178">Použití DbPropertyValues pro přístup ke komplexní vlastnosti</span><span class="sxs-lookup"><span data-stu-id="a9187-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="a9187-179">Když používáte CurrentValues, OriginalValues nebo GetDatabaseValues zobrazíte všechny aktuální původní nebo hodnot v databázi pro entitu, hodnoty žádné složité vlastností se vrátí jako vnořené objekty DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="a9187-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="a9187-180">Tyto vnořené objekty může potom použít k získání hodnoty komplexního objektu.</span><span class="sxs-lookup"><span data-stu-id="a9187-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="a9187-181">Například následující metoda vytiskne hodnoty všech vlastností, včetně hodnot všech komplexní vlastnosti a komplexních vnořených vlastností.</span><span class="sxs-lookup"><span data-stu-id="a9187-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

<span data-ttu-id="a9187-182">Který vytiskne všechny aktuální hodnoty vlastností metody by byla volána takto:</span><span class="sxs-lookup"><span data-stu-id="a9187-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
