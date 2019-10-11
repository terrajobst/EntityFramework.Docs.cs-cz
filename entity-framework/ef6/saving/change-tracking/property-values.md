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
# <a name="working-with-property-values"></a>Práce s hodnotami vlastností
V rámci většiny Entity Framework se postará o sledování stavu, původních hodnot a aktuálních hodnot vlastností vašich instancí entit. Mohou však existovat některé případy – například odpojené scénáře – kde chcete zobrazit nebo manipulovat s informacemi, které se týkají vlastností EF. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

Entity Framework sleduje dvě hodnoty pro každou vlastnost sledované entity. Aktuální hodnota je, jak název označuje aktuální hodnotu vlastnosti v entitě. Původní hodnota je hodnota, kterou vlastnost měla, když byla entita dotazována z databáze nebo připojena k tomuto kontextu.  

Existují dva obecné mechanismy pro práci s hodnotami vlastností:  

- Hodnota jedné vlastnosti může být získána způsobem silného typu pomocí metody Property.  
- Hodnoty všech vlastností entity lze číst do objektu DbPropertyValues. DbPropertyValues pak funguje jako objekt typu slovník, aby bylo možné hodnoty vlastností číst a nastavit. Hodnoty v objektu DbPropertyValues lze nastavit z hodnot v jiném objektu DbPropertyValues nebo z hodnot v některém jiném objektu, například jiné kopii entity nebo objektu jednoduchého přenosu dat (DTO).  

Následující části znázorňují příklady použití obou výše uvedených mechanismů.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Získání a nastavení aktuální nebo původní hodnoty jednotlivých vlastností  

Níže uvedený příklad ukazuje, jak může být aktuální hodnota vlastnosti čtena a nastavena na novou hodnotu:  

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

Použijte vlastnost původní namísto vlastnosti CurrentValue ke čtení nebo nastavení původní hodnoty.  

Všimněte si, že vrácená hodnota je zadána jako "Object", pokud je řetězec použit k zadání názvu vlastnosti. Na druhé straně vrácená hodnota je silného typu, pokud je použit výraz lambda.  

Nastavením hodnoty vlastnosti se tato vlastnost označí jenom jako upravená, pokud se nová hodnota liší od původní hodnoty.  

Když je nastavená hodnota vlastnosti tímto způsobem, je změna automaticky detekována i v případě, že je vypnutá funkce AutoDetectChanges.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Získání a nastavení aktuální hodnoty nemapované vlastnosti  

Aktuální hodnota vlastnosti, která není mapována na databázi, může být také přečtena. Příkladem nemapované vlastnosti může být vlastnost RssLink na blogu. Tato hodnota se může vypočítat na základě BlogId, a proto se v databázi nemusí ukládat. Příklad:  

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

Aktuální hodnotu lze nastavit také v případě, že vlastnost zpřístupňuje metodu setter.  

Čtení hodnot nemapovaných vlastností je užitečné při provádění Entity Framework ověřování nemapovaných vlastností. Pro stejný důvod může být pro vlastnosti entit, které aktuálně nejsou sledovány kontextem, načteny a nastavené aktuální hodnoty. Příklad:  

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

Všimněte si, že původní hodnoty nejsou k dispozici pro nemapované vlastnosti nebo pro vlastnosti entit, které nejsou sledovány kontextem.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Kontroluje se, jestli je vlastnost označená jako upravená.  

Následující příklad ukazuje, jak ověřit, zda je individuální vlastnost označena jako upravená:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Hodnoty změněných vlastností jsou odesílány jako aktualizace databáze při volání metody SaveChanges.  

##  <a name="marking-a-property-as-modified"></a>Označení vlastnosti jako upravené  

Následující příklad ukazuje, jak vynutit, aby byla jednotlivá vlastnost označena jako upravená:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Označení vlastnosti jako změněné vynutí, aby byla aktualizace odeslána do databáze pro vlastnost při volání metody SaveChanges i v případě, že aktuální hodnota vlastnosti je shodná s původní hodnotou.  

V současné době není možné resetovat jednotlivé vlastnosti, aby se po označení jako změněné nezměnily. To je něco, co plánujeme podporovat v budoucí verzi.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Čtení aktuálních, původních a databázových hodnot pro všechny vlastnosti entity  

Následující příklad ukazuje, jak číst aktuální hodnoty, původní hodnoty a hodnoty ve skutečnosti v databázi pro všechny mapované vlastnosti entity.  

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

Aktuální hodnoty jsou hodnoty, které vlastnosti entity aktuálně obsahují. Původní hodnoty jsou hodnoty, které byly načteny z databáze při dotazu na entitu. Hodnoty databáze jsou hodnoty, které jsou aktuálně uloženy v databázi. Získávání hodnot databáze je užitečné v případě, že se hodnoty v databázi změnily od doby, kdy byla entita dotazována, jako by byla souběžná úprava databáze provedena jiným uživatelem.  

## <a name="setting-current-or-original-values-from-another-object"></a>Nastavení aktuálních nebo původních hodnot z jiného objektu  

Aktuální nebo původní hodnoty sledované entity lze aktualizovat kopírováním hodnot z jiného objektu. Příklad:  

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

Po spuštění výše uvedeného kódu se vytiskne:  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Tato technika se někdy používá při aktualizaci entity s hodnotami získanými z volání služby nebo klienta v n-vrstvé aplikaci. Všimněte si, že použitý objekt nemusí být stejného typu jako entita, pokud má vlastnosti, jejichž názvy odpovídají názvům entity. V předchozím příkladu se k aktualizaci původních hodnot používá instance BlogDTO.  

Všimněte si, že pouze vlastnosti, které jsou nastaveny na jiné hodnoty při kopírování z jiného objektu, budou označeny jako upravené.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Nastavení aktuálních nebo původních hodnot ze slovníku  

Aktuální nebo původní hodnoty sledované entity lze aktualizovat kopírováním hodnot ze slovníku nebo jiné struktury dat. Příklad:  

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

Pro nastavení původních hodnot použijte vlastnost OriginalValues namísto vlastnosti CurrentValues.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Nastavení aktuálních nebo původních hodnot ze slovníku pomocí vlastnosti  

Alternativou k použití CurrentValues nebo OriginalValues, jak je znázorněno výše, je použít metodu Property k nastavení hodnoty jednotlivých vlastností. To může být vhodnější, pokud potřebujete nastavit hodnoty komplexních vlastností. Příklad:  

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

V předchozím příkladu jsou k dispozici tyto vlastnosti pomocí teček s názvy. Další způsoby přístupu ke složitým vlastnostem najdete v těchto dvou částech dále v tomto tématu, konkrétně o složitých vlastnostech.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Vytvoření klonovaného objektu obsahujícího aktuální, původní nebo databázové hodnoty  

Objekt DbPropertyValues vrácený z CurrentValues, OriginalValues nebo GetDatabaseValues lze použít k vytvoření klonu entity. Tento klon bude obsahovat hodnoty vlastností z objektu DbPropertyValues používaného k jeho vytvoření. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Všimněte si, že vrácený objekt není entita a není sledován kontextem. Vrácený objekt také nemá žádné nastavené relace na jiné objekty.  

Klonovaný objekt může být užitečný pro řešení problémů souvisejících s souběžnými aktualizacemi databáze, zejména v případě, že je použito uživatelské rozhraní, které zahrnuje datovou vazbu s objekty určitého typu.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Získávání a nastavování aktuálních nebo původních hodnot komplexních vlastností  

Hodnota celého komplexního objektu může být čtena a nastavena pomocí metody vlastnosti stejně, jako by mohla být pro primitivní vlastnost. Kromě toho můžete přejít k podrobnostem o složitém objektu a číst nebo nastavovat vlastnosti daného objektu nebo dokonce i vnořený objekt. Následuje několik příkladů:  

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

K získání nebo nastavení původní hodnoty použijte vlastnost původní namísto vlastnosti CurrentValue.  

Všimněte si, že vlastnost nebo metoda ComplexProperty lze použít pro přístup ke komplexní vlastnosti. Metoda ComplexProperty však musí být použita, pokud chcete přejít k podrobnostem objektu Complex pomocí dalších vlastností nebo volání ComplexProperty.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Přístup k složitým vlastnostem pomocí DbPropertyValues  

Když použijete CurrentValues, OriginalValues nebo GetDatabaseValues k získání všech hodnot aktuální, původní nebo databáze pro entitu, hodnoty všech komplexních vlastností se vrátí jako vnořené objekty DbPropertyValues. Tyto vnořené objekty lze potom použít k získání hodnot komplexního objektu. Následující metoda například vytiskne hodnoty všech vlastností, včetně hodnot všech komplexních vlastností a vnořených komplexních vlastností.  

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

Chcete-li vytisknout všechny aktuální hodnoty vlastností, metoda bude volána takto:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
