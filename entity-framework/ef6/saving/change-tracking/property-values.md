---
title: Práce s hodnotami vlastností – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: d8a18182754980d79b71df3f227b30c4ce40366f
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182146"
---
# <a name="working-with-property-values"></a><span data-ttu-id="f79f6-102">Práce s hodnotami vlastností</span><span class="sxs-lookup"><span data-stu-id="f79f6-102">Working with property values</span></span>
<span data-ttu-id="f79f6-103">V rámci většiny Entity Framework se postará o sledování stavu, původních hodnot a aktuálních hodnot vlastností vašich instancí entit.</span><span class="sxs-lookup"><span data-stu-id="f79f6-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="f79f6-104">Mohou však existovat některé případy – například odpojené scénáře – kde chcete zobrazit nebo manipulovat s informacemi, které se týkají vlastností EF.</span><span class="sxs-lookup"><span data-stu-id="f79f6-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="f79f6-105">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="f79f6-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="f79f6-106">Entity Framework sleduje dvě hodnoty pro každou vlastnost sledované entity.</span><span class="sxs-lookup"><span data-stu-id="f79f6-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="f79f6-107">Aktuální hodnota je, jak název označuje aktuální hodnotu vlastnosti v entitě.</span><span class="sxs-lookup"><span data-stu-id="f79f6-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="f79f6-108">Původní hodnota je hodnota, kterou vlastnost měla, když byla entita dotazována z databáze nebo připojena k tomuto kontextu.</span><span class="sxs-lookup"><span data-stu-id="f79f6-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="f79f6-109">Existují dva obecné mechanismy pro práci s hodnotami vlastností:</span><span class="sxs-lookup"><span data-stu-id="f79f6-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="f79f6-110">Hodnota jedné vlastnosti může být získána způsobem silného typu pomocí metody Property.</span><span class="sxs-lookup"><span data-stu-id="f79f6-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="f79f6-111">Hodnoty všech vlastností entity lze číst do objektu DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="f79f6-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="f79f6-112">DbPropertyValues pak funguje jako objekt typu slovník, aby bylo možné hodnoty vlastností číst a nastavit.</span><span class="sxs-lookup"><span data-stu-id="f79f6-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="f79f6-113">Hodnoty v objektu DbPropertyValues lze nastavit z hodnot v jiném objektu DbPropertyValues nebo z hodnot v některém jiném objektu, například jiné kopii entity nebo objektu jednoduchého přenosu dat (DTO).</span><span class="sxs-lookup"><span data-stu-id="f79f6-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="f79f6-114">Následující části znázorňují příklady použití obou výše uvedených mechanismů.</span><span class="sxs-lookup"><span data-stu-id="f79f6-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="f79f6-115">Získání a nastavení aktuální nebo původní hodnoty jednotlivých vlastností</span><span class="sxs-lookup"><span data-stu-id="f79f6-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="f79f6-116">Níže uvedený příklad ukazuje, jak může být aktuální hodnota vlastnosti čtena a nastavena na novou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="f79f6-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="f79f6-117">Použijte vlastnost původní namísto vlastnosti CurrentValue ke čtení nebo nastavení původní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f79f6-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="f79f6-118">Všimněte si, že vrácená hodnota je zadána jako "Object", pokud je řetězec použit k zadání názvu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f79f6-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="f79f6-119">Na druhé straně vrácená hodnota je silného typu, pokud je použit výraz lambda.</span><span class="sxs-lookup"><span data-stu-id="f79f6-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="f79f6-120">Nastavením hodnoty vlastnosti se tato vlastnost označí jenom jako upravená, pokud se nová hodnota liší od původní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f79f6-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="f79f6-121">Když je nastavená hodnota vlastnosti tímto způsobem, je změna automaticky detekována i v případě, že je vypnutá funkce AutoDetectChanges.</span><span class="sxs-lookup"><span data-stu-id="f79f6-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="f79f6-122">Získání a nastavení aktuální hodnoty nemapované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f79f6-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="f79f6-123">Aktuální hodnota vlastnosti, která není mapována na databázi, může být také přečtena.</span><span class="sxs-lookup"><span data-stu-id="f79f6-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="f79f6-124">Příkladem nemapované vlastnosti může být vlastnost RssLink na blogu.</span><span class="sxs-lookup"><span data-stu-id="f79f6-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="f79f6-125">Tato hodnota se může vypočítat na základě BlogId, a proto se v databázi nemusí ukládat.</span><span class="sxs-lookup"><span data-stu-id="f79f6-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="f79f6-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f79f6-126">For example:</span></span>  

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

<span data-ttu-id="f79f6-127">Aktuální hodnotu lze nastavit také v případě, že vlastnost zpřístupňuje metodu setter.</span><span class="sxs-lookup"><span data-stu-id="f79f6-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="f79f6-128">Čtení hodnot nemapovaných vlastností je užitečné při provádění Entity Framework ověřování nemapovaných vlastností.</span><span class="sxs-lookup"><span data-stu-id="f79f6-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="f79f6-129">Pro stejný důvod může být pro vlastnosti entit, které aktuálně nejsou sledovány kontextem, načteny a nastavené aktuální hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f79f6-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="f79f6-130">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f79f6-130">For example:</span></span>  

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

<span data-ttu-id="f79f6-131">Všimněte si, že původní hodnoty nejsou k dispozici pro nemapované vlastnosti nebo pro vlastnosti entit, které nejsou sledovány kontextem.</span><span class="sxs-lookup"><span data-stu-id="f79f6-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="f79f6-132">Kontroluje se, jestli je vlastnost označená jako upravená.</span><span class="sxs-lookup"><span data-stu-id="f79f6-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="f79f6-133">Následující příklad ukazuje, jak ověřit, zda je individuální vlastnost označena jako upravená:</span><span class="sxs-lookup"><span data-stu-id="f79f6-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="f79f6-134">Hodnoty změněných vlastností jsou odesílány jako aktualizace databáze při volání metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f79f6-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="f79f6-135">Označení vlastnosti jako upravené</span><span class="sxs-lookup"><span data-stu-id="f79f6-135">Marking a property as modified</span></span>  

<span data-ttu-id="f79f6-136">Následující příklad ukazuje, jak vynutit, aby byla jednotlivá vlastnost označena jako upravená:</span><span class="sxs-lookup"><span data-stu-id="f79f6-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="f79f6-137">Označení vlastnosti jako změněné vynutí, aby byla aktualizace odeslána do databáze pro vlastnost při volání metody SaveChanges i v případě, že aktuální hodnota vlastnosti je shodná s původní hodnotou.</span><span class="sxs-lookup"><span data-stu-id="f79f6-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="f79f6-138">V současné době není možné resetovat jednotlivé vlastnosti, aby se po označení jako změněné nezměnily.</span><span class="sxs-lookup"><span data-stu-id="f79f6-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="f79f6-139">To je něco, co plánujeme podporovat v budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="f79f6-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="f79f6-140">Čtení aktuálních, původních a databázových hodnot pro všechny vlastnosti entity</span><span class="sxs-lookup"><span data-stu-id="f79f6-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="f79f6-141">Následující příklad ukazuje, jak číst aktuální hodnoty, původní hodnoty a hodnoty ve skutečnosti v databázi pro všechny mapované vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="f79f6-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

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

<span data-ttu-id="f79f6-142">Aktuální hodnoty jsou hodnoty, které vlastnosti entity aktuálně obsahují.</span><span class="sxs-lookup"><span data-stu-id="f79f6-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="f79f6-143">Původní hodnoty jsou hodnoty, které byly načteny z databáze při dotazu na entitu.</span><span class="sxs-lookup"><span data-stu-id="f79f6-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="f79f6-144">Hodnoty databáze jsou hodnoty, které jsou aktuálně uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="f79f6-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="f79f6-145">Získávání hodnot databáze je užitečné v případě, že se hodnoty v databázi změnily od doby, kdy byla entita dotazována, jako by byla souběžná úprava databáze provedena jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="f79f6-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="f79f6-146">Nastavení aktuálních nebo původních hodnot z jiného objektu</span><span class="sxs-lookup"><span data-stu-id="f79f6-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="f79f6-147">Aktuální nebo původní hodnoty sledované entity lze aktualizovat kopírováním hodnot z jiného objektu.</span><span class="sxs-lookup"><span data-stu-id="f79f6-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="f79f6-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f79f6-148">For example:</span></span>  

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

<span data-ttu-id="f79f6-149">Po spuštění výše uvedeného kódu se vytiskne:</span><span class="sxs-lookup"><span data-stu-id="f79f6-149">Running the code above will print out:</span></span>  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="f79f6-150">Tato technika se někdy používá při aktualizaci entity s hodnotami získanými z volání služby nebo klienta v n-vrstvé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f79f6-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="f79f6-151">Všimněte si, že použitý objekt nemusí být stejného typu jako entita, pokud má vlastnosti, jejichž názvy odpovídají názvům entity.</span><span class="sxs-lookup"><span data-stu-id="f79f6-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="f79f6-152">V předchozím příkladu se k aktualizaci původních hodnot používá instance BlogDTO.</span><span class="sxs-lookup"><span data-stu-id="f79f6-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="f79f6-153">Všimněte si, že pouze vlastnosti, které jsou nastaveny na jiné hodnoty při kopírování z jiného objektu, budou označeny jako upravené.</span><span class="sxs-lookup"><span data-stu-id="f79f6-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="f79f6-154">Nastavení aktuálních nebo původních hodnot ze slovníku</span><span class="sxs-lookup"><span data-stu-id="f79f6-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="f79f6-155">Aktuální nebo původní hodnoty sledované entity lze aktualizovat kopírováním hodnot ze slovníku nebo jiné struktury dat.</span><span class="sxs-lookup"><span data-stu-id="f79f6-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="f79f6-156">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f79f6-156">For example:</span></span>  

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

<span data-ttu-id="f79f6-157">Pro nastavení původních hodnot použijte vlastnost OriginalValues namísto vlastnosti CurrentValues.</span><span class="sxs-lookup"><span data-stu-id="f79f6-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="f79f6-158">Nastavení aktuálních nebo původních hodnot ze slovníku pomocí vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f79f6-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="f79f6-159">Alternativou k použití CurrentValues nebo OriginalValues, jak je znázorněno výše, je použít metodu Property k nastavení hodnoty jednotlivých vlastností.</span><span class="sxs-lookup"><span data-stu-id="f79f6-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="f79f6-160">To může být vhodnější, pokud potřebujete nastavit hodnoty komplexních vlastností.</span><span class="sxs-lookup"><span data-stu-id="f79f6-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="f79f6-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f79f6-161">For example:</span></span>  

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

<span data-ttu-id="f79f6-162">V předchozím příkladu jsou k dispozici tyto vlastnosti pomocí teček s názvy.</span><span class="sxs-lookup"><span data-stu-id="f79f6-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="f79f6-163">Další způsoby přístupu ke složitým vlastnostem najdete v těchto dvou částech dále v tomto tématu, konkrétně o složitých vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="f79f6-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="f79f6-164">Vytvoření klonovaného objektu obsahujícího aktuální, původní nebo databázové hodnoty</span><span class="sxs-lookup"><span data-stu-id="f79f6-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="f79f6-165">Objekt DbPropertyValues vrácený z CurrentValues, OriginalValues nebo GetDatabaseValues lze použít k vytvoření klonu entity.</span><span class="sxs-lookup"><span data-stu-id="f79f6-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="f79f6-166">Tento klon bude obsahovat hodnoty vlastností z objektu DbPropertyValues používaného k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="f79f6-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="f79f6-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f79f6-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="f79f6-168">Všimněte si, že vrácený objekt není entita a není sledován kontextem.</span><span class="sxs-lookup"><span data-stu-id="f79f6-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="f79f6-169">Vrácený objekt také nemá žádné nastavené relace na jiné objekty.</span><span class="sxs-lookup"><span data-stu-id="f79f6-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="f79f6-170">Klonovaný objekt může být užitečný pro řešení problémů souvisejících s souběžnými aktualizacemi databáze, zejména v případě, že je použito uživatelské rozhraní, které zahrnuje datovou vazbu s objekty určitého typu.</span><span class="sxs-lookup"><span data-stu-id="f79f6-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="f79f6-171">Získávání a nastavování aktuálních nebo původních hodnot komplexních vlastností</span><span class="sxs-lookup"><span data-stu-id="f79f6-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="f79f6-172">Hodnota celého komplexního objektu může být čtena a nastavena pomocí metody vlastnosti stejně, jako by mohla být pro primitivní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f79f6-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="f79f6-173">Kromě toho můžete přejít k podrobnostem o složitém objektu a číst nebo nastavovat vlastnosti daného objektu nebo dokonce i vnořený objekt.</span><span class="sxs-lookup"><span data-stu-id="f79f6-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="f79f6-174">Následuje několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="f79f6-174">Here are some examples:</span></span>  

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

<span data-ttu-id="f79f6-175">K získání nebo nastavení původní hodnoty použijte vlastnost původní namísto vlastnosti CurrentValue.</span><span class="sxs-lookup"><span data-stu-id="f79f6-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="f79f6-176">Všimněte si, že vlastnost nebo metoda ComplexProperty lze použít pro přístup ke komplexní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f79f6-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="f79f6-177">Metoda ComplexProperty však musí být použita, pokud chcete přejít k podrobnostem objektu Complex pomocí dalších vlastností nebo volání ComplexProperty.</span><span class="sxs-lookup"><span data-stu-id="f79f6-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="f79f6-178">Přístup k složitým vlastnostem pomocí DbPropertyValues</span><span class="sxs-lookup"><span data-stu-id="f79f6-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="f79f6-179">Když použijete CurrentValues, OriginalValues nebo GetDatabaseValues k získání všech hodnot aktuální, původní nebo databáze pro entitu, hodnoty všech komplexních vlastností se vrátí jako vnořené objekty DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="f79f6-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="f79f6-180">Tyto vnořené objekty lze potom použít k získání hodnot komplexního objektu.</span><span class="sxs-lookup"><span data-stu-id="f79f6-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="f79f6-181">Následující metoda například vytiskne hodnoty všech vlastností, včetně hodnot všech komplexních vlastností a vnořených komplexních vlastností.</span><span class="sxs-lookup"><span data-stu-id="f79f6-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

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

<span data-ttu-id="f79f6-182">Chcete-li vytisknout všechny aktuální hodnoty vlastností, metoda bude volána takto:</span><span class="sxs-lookup"><span data-stu-id="f79f6-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
