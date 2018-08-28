---
title: Vlastní kód první konvence - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: 79450790c6d3c8ce7fad209e3946e81d3fad4b75
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995825"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="e8539-102">První vytváření vlastního kódu</span><span class="sxs-lookup"><span data-stu-id="e8539-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="e8539-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="e8539-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="e8539-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="e8539-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="e8539-105">Při použití Code First modelu se počítá od tříd pomocí sady konvence.</span><span class="sxs-lookup"><span data-stu-id="e8539-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="e8539-106">Výchozí hodnota [první konvence kódu](~/ef6/modeling/code-first/conventions/built-in.md) určit věci, třeba která vlastnost se stane primární klíč entity, názvu entity se mapuje na tabulku a jaké hodnot precision a scale desítkové sloupec má ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e8539-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="e8539-107">Někdy těchto výchozích konvencí nejsou ideální pro váš model, a je nutné je obejít tím, že nakonfigurujete jednotlivé entity pomocí anotacemi dat nebo rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="e8539-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="e8539-108">Vlastní první konvence kódu umožňují definovat vlastní zásady odvíjející, které poskytují výchozí konfigurační hodnoty pro váš model.</span><span class="sxs-lookup"><span data-stu-id="e8539-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="e8539-109">V tomto názorném postupu se podíváme na různé typy vlastní konvence a jak každý z nich vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e8539-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="e8539-110">Vytváření názvů založených na modelu</span><span class="sxs-lookup"><span data-stu-id="e8539-110">Model-Based Conventions</span></span>

<span data-ttu-id="e8539-111">Tato stránka popisuje DbModelBuilder rozhraní API pro vlastní konvence.</span><span class="sxs-lookup"><span data-stu-id="e8539-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="e8539-112">Toto rozhraní API by mělo být dostatečné pro většinu vlastní konvence vytváření.</span><span class="sxs-lookup"><span data-stu-id="e8539-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="e8539-113">Existuje ale také možnost vytvářet založené na modelu konvencí – konvence, které pracují s finálního modelu, jakmile se vytvoří – pro pokročilé scénáře zpracování.</span><span class="sxs-lookup"><span data-stu-id="e8539-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="e8539-114">Další informace najdete v tématu [založené na modelu konvencí](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="e8539-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="e8539-115">Náš Model</span><span class="sxs-lookup"><span data-stu-id="e8539-115">Our Model</span></span>

<span data-ttu-id="e8539-116">Začněme tím, že definování jednoduchého modelu, můžeme použít s naší konvence.</span><span class="sxs-lookup"><span data-stu-id="e8539-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="e8539-117">Přidejte následující třídy do projektu.</span><span class="sxs-lookup"><span data-stu-id="e8539-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="e8539-118">Úvod do vytváření vlastního</span><span class="sxs-lookup"><span data-stu-id="e8539-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="e8539-119">Napíšeme úmluvy, který konfiguruje všechny vlastnosti s názvem klíče se primární klíč pro daný typ entity.</span><span class="sxs-lookup"><span data-stu-id="e8539-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="e8539-120">Konvence jsou povolené na tvůrce modelu, který se dá dostat tak, že přepíšete OnModelCreating v kontextu.</span><span class="sxs-lookup"><span data-stu-id="e8539-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="e8539-121">Aktualizace třídy ProductContext následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e8539-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="e8539-122">Nyní, žádné vlastnosti v náš model s názvem klíče budou nakonfigurované jako primární klíč entity, ať jeho součástí.</span><span class="sxs-lookup"><span data-stu-id="e8539-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="e8539-123">Může také neposkytujeme naše konvence konkrétnější pomocí filtrování podle typu vlastnosti, které budeme konfigurace:</span><span class="sxs-lookup"><span data-stu-id="e8539-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="e8539-124">Tím se nakonfiguruje všechny vlastnosti volá klíčem k zajištění primární klíče jejich entity, ale pouze v případě, že se celé číslo.</span><span class="sxs-lookup"><span data-stu-id="e8539-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="e8539-125">Zajímavé funkce IsKey metody je, že je sčítání.</span><span class="sxs-lookup"><span data-stu-id="e8539-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="e8539-126">To znamená, že pokud zavoláte na více vlastností IsKey a se stanou součástí složený klíč.</span><span class="sxs-lookup"><span data-stu-id="e8539-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="e8539-127">Jeden výstrahou to je, že pokud zadáte více vlastností pro klíč musíte zadat také objednávky pro tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e8539-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="e8539-128">Toto lze provést zavoláním HasColumnOrder metoda podobná níže uvedenému příkladu:</span><span class="sxs-lookup"><span data-stu-id="e8539-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="e8539-129">Tento kód se nakonfiguruje typy v náš model má složený klíč skládající se z klíčový sloupec int a ve sloupci Název řetězce.</span><span class="sxs-lookup"><span data-stu-id="e8539-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="e8539-130">Pokud jsme v Návrháři zobrazení modelu by vypadalo takto:</span><span class="sxs-lookup"><span data-stu-id="e8539-130">If we view the model in the designer it would look like this:</span></span>

![compositeKey](~/ef6/media/compositekey.png)

<span data-ttu-id="e8539-132">Další příklad konvence vlastnost je konfigurace všechny vlastnosti data a času v mé modelu mapovat na typ datetime2 v systému SQL Server namísto data a času.</span><span class="sxs-lookup"><span data-stu-id="e8539-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="e8539-133">Toho můžete dosáhnout s následujícími možnostmi:</span><span class="sxs-lookup"><span data-stu-id="e8539-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="e8539-134">Vytváření názvů tříd</span><span class="sxs-lookup"><span data-stu-id="e8539-134">Convention Classes</span></span>

<span data-ttu-id="e8539-135">Dalším způsobem definování konvence je použít k zapouzdření vaše konvence vytváření názvů tříd.</span><span class="sxs-lookup"><span data-stu-id="e8539-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="e8539-136">Při použití konvence třídy vytvořit typ, který dědí z třídy vytváření názvů v oboru názvů System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="e8539-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="e8539-137">Můžeme vytvořit třídu konvence s konvencí datetime2, který jsme vám ukázali výše následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e8539-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="e8539-138">Chcete-li zjistit EF používat tato konvence je přidat do kolekce konvence OnModelCreating, což bude v případě, že jste postupovali podle spolu s návodu vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="e8539-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="e8539-139">Jak je vidět, že můžeme přidat instanci naše konvence ke kolekci konvence.</span><span class="sxs-lookup"><span data-stu-id="e8539-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="e8539-140">Dědění z konvencí poskytuje pohodlný způsob seskupení a konvence sdílení napříč týmy nebo projekty.</span><span class="sxs-lookup"><span data-stu-id="e8539-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="e8539-141">Můžete například mít knihovny tříd pomocí společnou sadu konvence, že všechny vaše organizace projekty použití.</span><span class="sxs-lookup"><span data-stu-id="e8539-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="e8539-142">Vlastní atributy</span><span class="sxs-lookup"><span data-stu-id="e8539-142">Custom Attributes</span></span>

<span data-ttu-id="e8539-143">Další skvělé konvence slouží k povolení nové atributy pro použití při konfiguraci modelu.</span><span class="sxs-lookup"><span data-stu-id="e8539-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="e8539-144">Pro znázornění, Pojďme vytvořit atribut, který můžete použít k označení vlastnosti řetězce jako kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="e8539-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="e8539-145">Teď vytvoříme konvence použití tohoto atributu na náš model:</span><span class="sxs-lookup"><span data-stu-id="e8539-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="e8539-146">S touto konvencí jsme můžete přidat atribut NonUnicode k některé z našich vlastnosti řetězce, které znamená, že sloupce v databázi se uloží jako varchar místo nvarchar.</span><span class="sxs-lookup"><span data-stu-id="e8539-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="e8539-147">Jedna věc, kterou byste měli znát Tato konvence je, že když vložíte NonUnicode atribut na nic jiného než vlastnost řetězce a vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="e8539-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="e8539-148">Dělá to, protože nelze nakonfigurovat IsUnicode v jakéhokoli typu než řetězce.</span><span class="sxs-lookup"><span data-stu-id="e8539-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="e8539-149">Pokud k tomu dojde, pak můžete vytvořit vaše konvence konkrétnější, tak, aby odfiltruje cokoli, co není řetězec.</span><span class="sxs-lookup"><span data-stu-id="e8539-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="e8539-150">Při výše konvence se dá použít pro definování uživatelských atributů, které je jiné rozhraní API, které může být mnohem jednodušší, pokud chcete použít, zejména, pokud chcete použít vlastnosti z třídy atributů.</span><span class="sxs-lookup"><span data-stu-id="e8539-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="e8539-151">V tomto příkladu budeme aktualizovat naše atribut a změňte ji na atribut IsUnicode tak vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e8539-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="e8539-152">Jakmile budeme mít, jsme nastavili bool na naše atribut říct úmluvy Určuje, jestli vlastnost by měla být v kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="e8539-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="e8539-153">Můžeme to udělat v konvenci, která jsme již díky přístupu do ClrProperty třídu konfigurace takto:</span><span class="sxs-lookup"><span data-stu-id="e8539-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="e8539-154">Toto je docela jednoduché, ale není tak stručnější hodí pomocí Having metodu vytváření rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e8539-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="e8539-155">S metoda má parametr typu Func&lt;PropertyInfo, T&gt; která přijímá PropertyInfo stejný jako Where metody, ale očekává se vrátit objekt.</span><span class="sxs-lookup"><span data-stu-id="e8539-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="e8539-156">Pokud vrácený objekt má hodnotu null, pak vlastnost nenakonfigurují, což znamená, že můžete filtrovat vlastnosti s ní stejně jako Where, ale se liší, bude také zaznamenání vráceného objektu a předejte metodě konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e8539-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="e8539-157">Tento postup funguje takto:</span><span class="sxs-lookup"><span data-stu-id="e8539-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="e8539-158">Vlastní atributy nejsou pouze z důvodu použití Having metoda, je užitečné kdekoli, budete muset o něco, co se na filtrování při konfiguraci typy nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e8539-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="e8539-159">Konfigurace typů</span><span class="sxs-lookup"><span data-stu-id="e8539-159">Configuring Types</span></span>

<span data-ttu-id="e8539-160">Zatím byly všechny naše konvence pro vlastnosti, ale existuje jiné oblasti vytváření rozhraní API pro konfiguraci typy ve vašem modelu.</span><span class="sxs-lookup"><span data-stu-id="e8539-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="e8539-161">Možnosti jsou podobné zásady, které jsme zatím viděli, ale možnosti konfigurace uvnitř bude na entitu namísto vlastnosti úrovně.</span><span class="sxs-lookup"><span data-stu-id="e8539-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="e8539-162">Jednou z věcí, které mohou být velmi užitečné pro typ úrovně konvence je změna zásady vytváření tabulky mapování na stávajícím schématu, která se liší od výchozí EF nebo vytvořit novou databázi pomocí jiné zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="e8539-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="e8539-163">K tomu potřeba nejdřív metodu, která může přijmout TypeInfo pro typ v náš model a vrátit, co by měl být název tabulky pro daný typ:</span><span class="sxs-lookup"><span data-stu-id="e8539-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="e8539-164">Tato metoda přebírá typ a vrátí řetězec, který používá malé místo CamelCase podtržítky.</span><span class="sxs-lookup"><span data-stu-id="e8539-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="e8539-165">V našem modelu to znamená, že třída ProductCategory budou zmapována do tabulky nazvané produktu\_místo oddělovaly kategorie.</span><span class="sxs-lookup"><span data-stu-id="e8539-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="e8539-166">Jakmile budeme mít metody říkáme mu v konvenci takto:</span><span class="sxs-lookup"><span data-stu-id="e8539-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="e8539-167">Tato konvence nakonfiguruje všechny typy v náš model mapovat na název tabulky, která je vrácena z našich GetTableName metody.</span><span class="sxs-lookup"><span data-stu-id="e8539-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="e8539-168">Tato konvence je ekvivalentem k volání metody ToTable pro každou entitu v modelu s použitím rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="e8539-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="e8539-169">Poznámka o tomto je, že při volání bude trvat ToTable EF, řetězec, který zadáte jako název tabulky přesné, bez jakékoli pluralizace, kterou by obvykle provádět při určování názvů tabulek.</span><span class="sxs-lookup"><span data-stu-id="e8539-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="e8539-170">To je důvod, proč je název tabulky z našich konvence produkt\_kategorie místo produktu\_kategorií.</span><span class="sxs-lookup"><span data-stu-id="e8539-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="e8539-171">Můžeme vyřešit, v našem konvence tím, že zavoláte na službu pluralizace sami.</span><span class="sxs-lookup"><span data-stu-id="e8539-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="e8539-172">V následujícím kódu budeme používat [řešení závislostí](~/ef6/fundamentals/configuring/dependency-resolution.md) funkce přidá EF6 načtení pluralizace služby, které byste použili EF a pluralize naše název tabulky.</span><span class="sxs-lookup"><span data-stu-id="e8539-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="e8539-173">Obecné verzi GetService je rozšiřující metodu v oboru názvů System.Data.Entity.Infrastructure.DependencyResolution, budete muset přidat, pomocí příkazu pro váš kontext, aby bylo možné ho použít.</span><span class="sxs-lookup"><span data-stu-id="e8539-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="e8539-174">ToTable a dědičnost</span><span class="sxs-lookup"><span data-stu-id="e8539-174">ToTable and Inheritance</span></span>

<span data-ttu-id="e8539-175">Další důležitý aspekt ToTable je, že pokud namapujete typ explicitně do dané tabulky, je možné změnit mapování strategie, kterou bude používat EF.</span><span class="sxs-lookup"><span data-stu-id="e8539-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="e8539-176">Při volání ToTable pro každý typ v hierarchii dědičnosti, předá název typu jako název tabulky, jak jsme to udělali výše, pak změníte výchozí strategie mapování na hierarchii tabulky (TPH) na typ tabulky (TPT).</span><span class="sxs-lookup"><span data-stu-id="e8539-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="e8539-177">Nejlepší způsob, jak to popisují je whith konkrétní příklad:</span><span class="sxs-lookup"><span data-stu-id="e8539-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="e8539-178">Ve výchozím nastavení zaměstnance a správce jsou namapovány na stejnou tabulku (zaměstnanci) v databázi.</span><span class="sxs-lookup"><span data-stu-id="e8539-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="e8539-179">V tabulce bude obsahovat zaměstnancům i sloupec diskriminátoru, která vám dá vědět, jaký typ instance je uložen v jednotlivých řádcích.</span><span class="sxs-lookup"><span data-stu-id="e8539-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="e8539-180">Toto je TPH mapování je jedné tabulky v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="e8539-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="e8539-181">Ale při volání ToTable na obou clase pak každý typ bude místo toho mapovat na svou vlastní tabulku, označované také jako TPT vzhledem k tomu, že každý typ má svou vlastní tabulku.</span><span class="sxs-lookup"><span data-stu-id="e8539-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="e8539-182">Výše uvedený kód se namapuje do struktury tabulky, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e8539-182">The code above will map to a table structure that looks like the following:</span></span>

![tptExample](~/ef6/media/tptexample.jpg)

<span data-ttu-id="e8539-184">Můžete-li tomu zabránit a udržovat TPH výchozí mapování několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="e8539-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="e8539-185">Volání ToTable se stejným názvem tabulky pro každý typ v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="e8539-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="e8539-186">ToTable volejte pouze pro základní třídy v hierarchii, v našem příkladu, která by byla zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="e8539-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="e8539-187">Pořadí provádění</span><span class="sxs-lookup"><span data-stu-id="e8539-187">Execution Order</span></span>

<span data-ttu-id="e8539-188">Konvence pracovat v posledním způsobem wins, stejná jako rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="e8539-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="e8539-189">Co to znamená, že je, že pokud napíšete dvě vytváření názvů, které se stejným nastavením možnosti této vlastnosti a pak poslední z nich ke spuštění služby wins.</span><span class="sxs-lookup"><span data-stu-id="e8539-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="e8539-190">Například v následujícím kódu maximální délka všechny řetězce nastavená na 500, ale potom nakonfigurujeme všechny vlastnosti název v modelu, který má mít maximální délku 250.</span><span class="sxs-lookup"><span data-stu-id="e8539-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="e8539-191">Protože konvence pro nastavení maximální délky na 250 je po ten, který nastaví všechny řetězce na 500, všechny vlastnosti v našich modelu jako název budou mít MaxLength 250 při libovolné řetězce, jako jsou popisy, 500.</span><span class="sxs-lookup"><span data-stu-id="e8539-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="e8539-192">Pomocí konvencí tímto způsobem znamená, že můžete zadat obecné konvence pro typy nebo vlastnosti v modelu a poté toto pro podmnožiny, které se liší.</span><span class="sxs-lookup"><span data-stu-id="e8539-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="e8539-193">Rozhraní Fluent API a anotacemi dat lze použít také k přepsání konvence ve zvláštních případech.</span><span class="sxs-lookup"><span data-stu-id="e8539-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="e8539-194">V našem příkladu rozhraní Fluent API měli použili k nastavení maximální délka vlastnosti pak jsme může mít ji umístit před nebo po konvence, protože konkrétnější rozhraní Fluent API vyhraje přes obecnější konvence konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e8539-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="e8539-195">Integrované konvence</span><span class="sxs-lookup"><span data-stu-id="e8539-195">Built-in Conventions</span></span>

<span data-ttu-id="e8539-196">Protože konvence vlastní mohou být ovlivněny výchozích konvencí Code First, může být užitečné pro přidání konvence pro spuštění před nebo po jiném konvence.</span><span class="sxs-lookup"><span data-stu-id="e8539-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="e8539-197">K tomu můžete použít metody AddBefore a AddAfter konvence kolekce na odvozené DbContext.</span><span class="sxs-lookup"><span data-stu-id="e8539-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="e8539-198">Následující kód přidejte třídu konvence jsme vytvořili dříve, tak, aby se spustí před integrovaná v klíčových zjišťování konvence.</span><span class="sxs-lookup"><span data-stu-id="e8539-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="e8539-199">To bude nejvíc použití při přidávání vytváření názvů, které je potřeba spustit před nebo po integrované konvence, seznam integrované vytváření najdete tady: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="e8539-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="e8539-200">Můžete také odebrat vytváření názvů, které nechcete použít pro váš model.</span><span class="sxs-lookup"><span data-stu-id="e8539-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="e8539-201">K odebrání konvence, použijte metodu odebrat.</span><span class="sxs-lookup"><span data-stu-id="e8539-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="e8539-202">Tady je příklad odebrání PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="e8539-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
