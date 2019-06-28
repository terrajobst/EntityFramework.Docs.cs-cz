---
title: Práce s hodnoty vlastností - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: afde503bb4ed15fcf83a57053541cd5da8c89835
ms.sourcegitcommit: 50521b4a2f71139e6a7210a69ac73da582ef46cf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/27/2019
ms.locfileid: "67416673"
---
# <a name="working-with-property-values"></a>Práce s hodnotami vlastností
Ve většině případů Entity Framework se postará o sledování stavu, původní hodnoty a aktuální hodnoty vlastností instance entity. Však může být někdy – například odpojená řešení – Pokud chcete zobrazit nebo pracovat s informací, které EF má o vlastnosti. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

Entity Framework uchovává informace o dvou hodnot pro každou vlastnost sledované entity. Aktuální hodnota je aktuální hodnota vlastnosti v entitě, protože název naznačuje. Původní hodnota je hodnota, která měla vlastnost entity se posílat dotaz z databáze nebo připojit ke kontextu.  

Existují dva hlavní mechanismy pro práci s hodnotami vlastností:  

- Nejde získat hodnotu jedné vlastnosti způsobem silného typu pomocí metody vlastností.  
- Hodnoty všech vlastností entity lze načíst do objektu DbPropertyValues. DbPropertyValues pak funguje jako objekt slovníku jako umožňuje číst a nastavovat hodnoty vlastností. Hodnoty v objektu DbPropertyValues lze nastavit z hodnot v jiném objektu DbPropertyValues nebo z hodnot v druhý objekt, jako je například jiná kopie entity nebo objekt pro přenos jednoduché dat (DTO).  

Následující části popisují příklady použití obou výše uvedených mechanismů.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Získání a nastavení aktuální a původní hodnoty jednotlivých vlastností  

Následující příklad ukazuje, jak můžete číst a poté nastavte novou hodnotu aktuální hodnota vlastnosti:  

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

Pomocí původní hodnota vlastnosti namísto vlastnosti CurrentValue čtena nebo nastavena na původní hodnotu.  

Všimněte si, že vrácená hodnota je zadán jako "object", když řetězec se používá k určení názvu vlastnosti. Pokud výraz lambda se používá na druhé straně silného typu vrácené hodnoty.  

Nastavení hodnoty vlastnosti tímto způsobem bude pouze označí vlastnost upravit, pokud je nová hodnota se liší od původní hodnoty.  

Pokud je hodnota vlastnosti nastavena tímto způsobem změn je automaticky rozpoznán i v případě, že AutoDetectChanges je vypnutý.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Získání a nastavení aktuální hodnota nemapovaných vlastnosti  

Aktuální hodnota vlastnosti, která není namapována na databázi lze také číst. Příklad nenamapované vlastnost může být na vlastnost RssLink na blogu. Tato hodnota může být vypočtena podle BlogId a proto nemusí být uloženy v databázi. Příklad:  

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

Aktuální hodnota můžete také nastavit, pokud vlastnost zpřístupní setter.  

Čtení hodnot nemapovaných vlastnosti je užitečná při provádění ověření Entity Framework nenamapované vlastností. Ze stejného důvodu může číst a nastavit pro vlastnosti entity, které nejsou aktuálně nesleduje podle kontextu aktuální hodnoty. Příklad:  

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

Všimněte si, že původní hodnoty nejsou k dispozici pro nemapovaných vlastnosti nebo vlastnosti entity, které nejsou sledován správou kontextu.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Kontroluje se, zda je vlastnost označena jako upravená  

Následující příklad ukazuje, jak zkontrolovat, zda jednotlivé vlastnosti je označena jako upravená:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Hodnoty upravené vlastnosti jsou odesílány jako aktualizace do databáze, když je volána metoda SaveChanges.  

##  <a name="marking-a-property-as-modified"></a>Označení jako upravená vlastnost  

Následující příklad ukazuje, jak vynutit individuální vlastnosti označit jako změny:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Označí vlastnost jako upravené vynutí aktualizaci se odesílají do databáze pro vlastnost při volání SaveChanges i v případě, že aktuální hodnota vlastnosti je stejný jako původní hodnotu.  

Není aktuálně možné resetovat individuální vlastnosti nelze změnit poté, co byla označena jako upravená. To je něco, co jsme chcete podporovat v budoucí verzi.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Čtení databáze hodnoty všech vlastností entity, aktuální a původní  

Následující příklad znázorňuje způsob čtení aktuálních hodnot, původní hodnoty a hodnoty ve skutečnosti v databázi pro všechny připojené vlastnosti entity.  

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

Aktuální hodnoty jsou hodnoty, které aktuálně obsahuje vlastnosti entity. Původní hodnoty jsou hodnoty, které byla přečtena z databáze, pokud byla dotazována entity. Databáze hodnoty jsou hodnoty, jako jsou aktuálně uloženy v databázi. Získání hodnot v databázi je užitečné, když hodnoty v databázi mohl být změněn dotazu entity, jako je při souběžné upravit, pokud chcete databázi provedené jiným uživatelem.  

## <a name="setting-current-or-original-values-from-another-object"></a>Aktuální a původní hodnoty nastavení z jiného objektu  

Aktuální a původní hodnoty sledované entity je možné aktualizovat tak, že zkopírujete hodnoty z jiného objektu. Příklad:  

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

Spuštění výše uvedený kód bude vytisknout:  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Tato technika se někdy používá při aktualizaci entity s hodnotami získanými z klienta v n vrstvou aplikaci nebo volání služby. Všimněte si, že objekt použitý nemusí být stejného typu jako entity tak dlouho, dokud má vlastnosti, jejichž názvy odpovídají těm entity. V předchozím příkladu instance BlogDTO slouží k aktualizaci původní hodnoty.  

Všimněte si, že pouze vlastnosti, které jsou nastaveny na různé hodnoty při kopírování z jiného objektu se označí jako upravená.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Nastavení aktuální a původní hodnoty ze slovníku  

Aktuální a původní hodnoty sledované entity je možné aktualizovat tak, že zkopírujete hodnoty ze slovníku nebo jiné datové struktury. Příklad:  

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

Vlastnost OriginalValues místo vlastnost CurrentValues nastavit původní hodnoty.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Nastavení aktuální a původní hodnoty ze slovníku pomocí vlastnosti  

O alternativu k použití CurrentValues nebo OriginalValues, jak je znázorněno výše se má používat metoda vlastnosti k nastavení hodnoty jednotlivých vlastností. To může být vhodnější, pokud je nutné nastavit hodnoty komplexní vlastností. Příklad:  

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

V příkladu výše komplexní vlastnosti jsou přistupovat pomocí tečkové názvy. Další možnosti pro přístup ke komplexní vlastnosti zobrazí dva oddíly dále v tomto tématu, konkrétně o komplexní vlastnosti.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Vytváří se Klonovaný objekt obsahující aktuální, původní nebo hodnot v databázi  

DbPropertyValues objekt se vrátil ze CurrentValues OriginalValues, nebo je možné GetDatabaseValues naklonoval entitu. Tenhle klon. bude obsahovat hodnoty vlastnosti z objektu DbPropertyValues použitá k jeho vytvoření. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Všimněte si, že objekt se vrátil není entita a není sledována podle kontextu. Vrácený objekt také nemá žádné relace nastavena na jiné objekty.  

Klonovaný objekt může být užitečné při řešení problémů souvisejících s souběžných aktualizací do databáze, zejména když uživatelského rozhraní, která zahrnuje datové vazby pro objekty určitého typu se používá.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Získání a nastavení aktuální a původní hodnoty komplexní vlastnosti  

Hodnota celého komplexní objekt může být pro čtení a nastavte vlastnost metodou stejně jako může být pro primitivní vlastnost. Kromě toho můžete přejít ke čtení nebo nastavte vlastnosti tohoto objektu nebo vnořený objekt a komplexního objektu. Následuje několik příkladů:  

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

Použijte vlastnost původní hodnota namísto CurrentValue vlastnosti pro získání nebo nastavení původní hodnotu.  

Všimněte si, že vlastnost nebo metoda ComplexProperty umožňuje přístup ke komplexní vlastností. Metodu ComplexProperty však musí použít, pokud chcete přejít na komplexní objekt s další vlastnost nebo ComplexProperty volá.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Použití DbPropertyValues pro přístup ke komplexní vlastnosti  

Když používáte CurrentValues, OriginalValues nebo GetDatabaseValues zobrazíte všechny aktuální původní nebo hodnot v databázi pro entitu, hodnoty žádné složité vlastností se vrátí jako vnořené objekty DbPropertyValues. Tyto vnořené objekty může potom použít k získání hodnoty komplexního objektu. Například následující metoda vytiskne hodnoty všech vlastností, včetně hodnot všech komplexní vlastnosti a komplexních vnořených vlastností.  

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

Který vytiskne všechny aktuální hodnoty vlastností metody by byla volána takto:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
