---
title: Vlastní konvence Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419225"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="229fa-102">Vlastní konvence Code First</span><span class="sxs-lookup"><span data-stu-id="229fa-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="229fa-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="229fa-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="229fa-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="229fa-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="229fa-105">Při použití Code First se model počítá z vašich tříd pomocí sady konvencí.</span><span class="sxs-lookup"><span data-stu-id="229fa-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="229fa-106">Výchozí [konvence Code First](~/ef6/modeling/code-first/conventions/built-in.md) určují věci, jako je například vlastnost, která se změní na primární klíč entity, název tabulky, na kterou se entita mapuje, a jaká přesnost a velikost sloupce desetinného čísla mají ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="229fa-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="229fa-107">Tyto výchozí konvence jsou někdy ideální pro váš model a vy je budete muset vyřešit tak, že nakonfigurujete mnoho jednotlivých entit pomocí datových poznámek nebo rozhraní API Fluent.</span><span class="sxs-lookup"><span data-stu-id="229fa-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="229fa-108">Vlastní konvence Code First umožňují definovat vlastní konvence, které pro váš model poskytují výchozí nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="229fa-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="229fa-109">V tomto návodu budeme prozkoumat různé typy vlastních konvencí a postup, jak je vytvořit.</span><span class="sxs-lookup"><span data-stu-id="229fa-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="229fa-110">Konvence založené na modelu</span><span class="sxs-lookup"><span data-stu-id="229fa-110">Model-Based Conventions</span></span>

<span data-ttu-id="229fa-111">Tato stránka pokrývá rozhraní DbModelBuilder API pro vlastní konvence.</span><span class="sxs-lookup"><span data-stu-id="229fa-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="229fa-112">Toto rozhraní API by mělo být dostačující pro vytváření většiny vlastních konvencí.</span><span class="sxs-lookup"><span data-stu-id="229fa-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="229fa-113">Existuje však také možnost vytvářet konvence založené na modelu, které zpracovávají konečný model po jeho vytvoření – pro zpracování pokročilých scénářů.</span><span class="sxs-lookup"><span data-stu-id="229fa-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="229fa-114">Další informace najdete v tématu [konvence založené na modelu](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="229fa-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="229fa-115">Náš model</span><span class="sxs-lookup"><span data-stu-id="229fa-115">Our Model</span></span>

<span data-ttu-id="229fa-116">Pojďme začít definováním jednoduchého modelu, který můžeme použít s našimi konvencemi.</span><span class="sxs-lookup"><span data-stu-id="229fa-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="229fa-117">Do projektu přidejte následující třídy.</span><span class="sxs-lookup"><span data-stu-id="229fa-117">Add the following classes to your project.</span></span>

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

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="229fa-118">Představujeme vlastní konvence</span><span class="sxs-lookup"><span data-stu-id="229fa-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="229fa-119">Pojďme napsat konvenci, která nakonfiguruje libovolnou vlastnost s názvem klíč na primární klíč pro svůj typ entity.</span><span class="sxs-lookup"><span data-stu-id="229fa-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="229fa-120">V Tvůrci modelů jsou povoleny konvence, které mohou být zpřístupněny přepsáním OnModelCreating v kontextu.</span><span class="sxs-lookup"><span data-stu-id="229fa-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="229fa-121">Aktualizujte třídu ProductContext následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="229fa-121">Update the ProductContext class as follows:</span></span>

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

<span data-ttu-id="229fa-122">Teď bude jakákoli vlastnost v našem modelu s názvem Key nakonfigurovaná jako primární klíč jakékoli entity jeho součásti.</span><span class="sxs-lookup"><span data-stu-id="229fa-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="229fa-123">Naše konvence můžeme také lépe specifikovat filtrováním typu vlastnosti, kterou chceme nakonfigurovat:</span><span class="sxs-lookup"><span data-stu-id="229fa-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="229fa-124">Tím se nakonfigurují všechny vlastnosti s názvem klíč jako primární klíč své entity, ale jenom v případě, že se jedná o celé číslo.</span><span class="sxs-lookup"><span data-stu-id="229fa-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="229fa-125">Zajímavou funkcí metody vlastností IsKey nastavenou je, že je aditivní.</span><span class="sxs-lookup"><span data-stu-id="229fa-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="229fa-126">To znamená, že pokud voláte vlastností IsKey nastavenou u více vlastností a všechny budou stávají součástí složeného klíče.</span><span class="sxs-lookup"><span data-stu-id="229fa-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="229fa-127">Tato výstraha je taková, že když pro klíč zadáte více vlastností, musíte zadat také pořadí těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="229fa-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="229fa-128">To lze provést voláním metody HasColumnOrder, například:</span><span class="sxs-lookup"><span data-stu-id="229fa-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="229fa-129">Tento kód nakonfiguruje typy v našem modelu tak, aby měly složený klíč tvořený sloupcem klíče int a sloupce názvu řetězce.</span><span class="sxs-lookup"><span data-stu-id="229fa-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="229fa-130">Pokud se model zobrazuje v návrháři, může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="229fa-130">If we view the model in the designer it would look like this:</span></span>

![složený klíč](~/ef6/media/compositekey.png)

<span data-ttu-id="229fa-132">Dalším příkladem konvence vlastností je nakonfigurovat všechny vlastnosti DateTime v modelu můj model tak, aby se místo hodnoty DateTime namapovaly na datetime2 typ v SQL Server.</span><span class="sxs-lookup"><span data-stu-id="229fa-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="229fa-133">Můžete to dosáhnout následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="229fa-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="229fa-134">Třídy konvence</span><span class="sxs-lookup"><span data-stu-id="229fa-134">Convention Classes</span></span>

<span data-ttu-id="229fa-135">Dalším způsobem definování konvencí je použití třídy konvence k zapouzdření vaší konvence.</span><span class="sxs-lookup"><span data-stu-id="229fa-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="229fa-136">Při použití třídy konvence pak vytvoříte typ, který dědí z třídy konvence v oboru názvů System. data. entity. ModelConfiguration. Convention.</span><span class="sxs-lookup"><span data-stu-id="229fa-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="229fa-137">Můžete vytvořit třídu konvence s konvencí datetime2, kterou jsme dříve zjistili pomocí následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="229fa-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

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

<span data-ttu-id="229fa-138">Chcete-li, aby EF používalo tuto konvenci, přidejte ji do kolekce konvence v OnModelCreating, kterou jste měli v případě, že budete postupovat podle pokynů společně s návodem, bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="229fa-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="229fa-139">Jak vidíte, přidáme instanci naší konvence do kolekce konvence.</span><span class="sxs-lookup"><span data-stu-id="229fa-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="229fa-140">Dědění z konvence nabízí pohodlný způsob seskupování a sdílení v rámci týmů nebo projektů.</span><span class="sxs-lookup"><span data-stu-id="229fa-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="229fa-141">Můžete například mít knihovnu tříd se společnou sadou konvencí, kterou používají všechny projekty vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="229fa-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="229fa-142">Vlastní atributy</span><span class="sxs-lookup"><span data-stu-id="229fa-142">Custom Attributes</span></span>

<span data-ttu-id="229fa-143">Dalším velkým používáním konvencí je povolit použití nových atributů při konfiguraci modelu.</span><span class="sxs-lookup"><span data-stu-id="229fa-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="229fa-144">Pro ilustraci si vytvoříme atribut, který můžeme použít k označení vlastností řetězce jako jiné než Unicode.</span><span class="sxs-lookup"><span data-stu-id="229fa-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="229fa-145">Nyní vytvoříme konvenci pro použití tohoto atributu pro náš model:</span><span class="sxs-lookup"><span data-stu-id="229fa-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="229fa-146">V této úmluvě můžeme přidat atribut nepodporující sadu Unicode do žádné z našich vlastností řetězce, což znamená, že sloupec v databázi bude uložen jako varchar namísto typu nvarchar.</span><span class="sxs-lookup"><span data-stu-id="229fa-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="229fa-147">Jednou z věcí k této konvenci je, že pokud zadáte atribut nepodporující kódování Unicode na jinou hodnotu než vlastnost řetězce, vyvolá se výjimka.</span><span class="sxs-lookup"><span data-stu-id="229fa-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="229fa-148">To je způsobeno tím, že nemůžete nakonfigurovat kódování UTF pro jakýkoliv typ jiný než řetězec.</span><span class="sxs-lookup"><span data-stu-id="229fa-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="229fa-149">Pokud k tomu dojde, můžete nastavit konkrétní konvenci tak, aby odfiltroval cokoli, co není řetězec.</span><span class="sxs-lookup"><span data-stu-id="229fa-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="229fa-150">I když výše uvedená konvence funguje pro definování vlastních atributů, existuje jiné rozhraní API, které může být mnohem jednodušší, zejména v případě, že chcete použít vlastnosti z třídy atributů.</span><span class="sxs-lookup"><span data-stu-id="229fa-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="229fa-151">V tomto příkladu budeme aktualizovat náš atribut a změnit ho na atribut typu s kódováním Unicode, aby vypadal takto:</span><span class="sxs-lookup"><span data-stu-id="229fa-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

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

<span data-ttu-id="229fa-152">Jakmile to máme, můžeme pro náš atribut nastavit bool, která určuje, jestli má být vlastnost nastavená na Unicode.</span><span class="sxs-lookup"><span data-stu-id="229fa-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="229fa-153">To jsme mohli udělat v konvenci, kterou už máme přístup k ClrProperty třídy konfigurace, jako je tato:</span><span class="sxs-lookup"><span data-stu-id="229fa-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="229fa-154">To je dostatečně snadné, ale existuje stručnější způsob, jak toho dosáhnout pomocí metody rozhraní API pro konvence.</span><span class="sxs-lookup"><span data-stu-id="229fa-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="229fa-155">Metoda having má parametr typu Func&lt;PropertyInfo, T&gt;, který přijímá PropertyInfo stejné jako metoda Where, ale očekává se, že vrátí objekt.</span><span class="sxs-lookup"><span data-stu-id="229fa-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="229fa-156">Pokud je vrácený objekt null, vlastnost nebude nakonfigurována, což znamená, že můžete vyfiltrovat vlastnosti, které mají, stejně jako v případě, že se ale liší v tom, že bude také zachytit vrácený objekt a předat ho metodě configure.</span><span class="sxs-lookup"><span data-stu-id="229fa-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="229fa-157">To funguje podobně jako u následujících:</span><span class="sxs-lookup"><span data-stu-id="229fa-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="229fa-158">Vlastní atributy nejsou jediným důvodem, proč použít metodu having, je užitečné všude, kde je třeba mít důvod na něco, co filtrujete při konfiguraci typů nebo vlastností.</span><span class="sxs-lookup"><span data-stu-id="229fa-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="229fa-159">Konfigurace typů</span><span class="sxs-lookup"><span data-stu-id="229fa-159">Configuring Types</span></span>

<span data-ttu-id="229fa-160">Všechny naše konvence byly proto pro vlastnosti, ale existuje další oblast rozhraní API konvence pro konfiguraci typů v modelu.</span><span class="sxs-lookup"><span data-stu-id="229fa-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="229fa-161">Prostředí je podobné konvencím, které jsme doposud viděli, ale možnosti v části konfigurovat budou na entitě namísto úrovně vlastností.</span><span class="sxs-lookup"><span data-stu-id="229fa-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="229fa-162">Jedna z věcí, ke kterým se konvence na úrovni typů může skutečně hodit, je změna zásady vytváření názvů tabulek, buď k namapování na existující schéma, které se liší od výchozího nastavení EF, nebo k vytvoření nové databáze s různými konvencemi vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="229fa-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="229fa-163">Abychom to mohli udělat, nejdřív potřebujeme metodu, která může přijmout volání TypeInfo pro typ v našem modelu a vracet, jak by měl být název tabulky tohoto typu:</span><span class="sxs-lookup"><span data-stu-id="229fa-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="229fa-164">Tato metoda přebírá typ a vrátí řetězec, který používá malá písmena s podtržítky namísto CamelCase.</span><span class="sxs-lookup"><span data-stu-id="229fa-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="229fa-165">V našem modelu to znamená, že třída ProductCategory bude namapována na tabulku s názvem produkt\_kategorie místo na ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="229fa-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="229fa-166">Až tuto metodu máme, můžeme ji volat v konvenci, jako je tato:</span><span class="sxs-lookup"><span data-stu-id="229fa-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="229fa-167">Tato konvence nakonfiguruje všechny typy v našem modelu pro mapování na název tabulky, který je vrácen z naší metody GetTable.</span><span class="sxs-lookup"><span data-stu-id="229fa-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="229fa-168">Tato konvence je ekvivalentem volání metody ToTable pro každou entitu v modelu pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="229fa-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="229fa-169">Všimněte si, že když zavoláte ToTable EF, převezme řetězec, který zadáte jako přesný název tabulky, bez jakékoli plurality, která by obvykle při určování názvů tabulek.</span><span class="sxs-lookup"><span data-stu-id="229fa-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="229fa-170">Důvodem je, že název tabulky z naší konvence je produkt\_kategorie místo kategorií produktů\_.</span><span class="sxs-lookup"><span data-stu-id="229fa-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="229fa-171">Můžeme to vyřešit v naší úmluvě tím, že zavoláte dodržovali služby pro pluralitování.</span><span class="sxs-lookup"><span data-stu-id="229fa-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="229fa-172">V následujícím kódu budeme používat funkci [překladu závislosti](~/ef6/fundamentals/configuring/dependency-resolution.md) přidanou v EF6 k načtení služby pro pojmenování, kterou použil EF, a doplnit jednotné našeho názvu tabulky.</span><span class="sxs-lookup"><span data-stu-id="229fa-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

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
> <span data-ttu-id="229fa-173">Obecná verze GetService je metoda rozšíření v oboru názvů System. data. entity. Infrastructure. DependencyResolution, budete muset přidat příkaz using do svého kontextu, aby ho bylo možné použít.</span><span class="sxs-lookup"><span data-stu-id="229fa-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="229fa-174">ToTable a dědičnost</span><span class="sxs-lookup"><span data-stu-id="229fa-174">ToTable and Inheritance</span></span>

<span data-ttu-id="229fa-175">Dalším důležitým aspektem ToTable je, že pokud explicitně namapujete typ na danou tabulku, můžete změnit strategii mapování, kterou EF bude používat.</span><span class="sxs-lookup"><span data-stu-id="229fa-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="229fa-176">Pokud zavoláte ToTable pro každý typ v hierarchii dědičnosti, dáte název typu jako název tabulky, jako jsme výše, a pak změníte výchozí strategii mapování tabulky na typ (TPT) na typ Table-per-Type ().</span><span class="sxs-lookup"><span data-stu-id="229fa-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="229fa-177">Nejlepším způsobem, jak to popsat, je whith konkrétní příklad:</span><span class="sxs-lookup"><span data-stu-id="229fa-177">The best way to describe this is whith a concrete example:</span></span>

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

<span data-ttu-id="229fa-178">Ve výchozím nastavení jsou zaměstnanci i manažer namapováni na stejnou tabulku (zaměstnanci) v databázi.</span><span class="sxs-lookup"><span data-stu-id="229fa-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="229fa-179">Tabulka bude obsahovat zaměstnance i manažery se sloupcem diskriminátoru, který vám sdělí, jaký typ instance je uložený v jednotlivých řádcích.</span><span class="sxs-lookup"><span data-stu-id="229fa-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="229fa-180">Toto je mapování typu TPH, protože pro hierarchii existuje jedna tabulka.</span><span class="sxs-lookup"><span data-stu-id="229fa-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="229fa-181">Nicméně pokud voláte ToTable na obou Classe, pak každý typ bude mapován na vlastní tabulku, označovanou také jako TPT, protože každý typ má svou vlastní tabulku.</span><span class="sxs-lookup"><span data-stu-id="229fa-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="229fa-182">Výše uvedený kód se namapuje na strukturu tabulky, která vypadá nějak takto:</span><span class="sxs-lookup"><span data-stu-id="229fa-182">The code above will map to a table structure that looks like the following:</span></span>

![Příklad TPT](~/ef6/media/tptexample.jpg)

<span data-ttu-id="229fa-184">Můžete tomu předejít a zachovat výchozí mapování TPH několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="229fa-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="229fa-185">Pro každý typ v hierarchii zavolejte ToTable se stejným názvem tabulky.</span><span class="sxs-lookup"><span data-stu-id="229fa-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="229fa-186">Volejte ToTable jenom pro základní třídu hierarchie, v našem příkladu, který by byl zaměstnanec.</span><span class="sxs-lookup"><span data-stu-id="229fa-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="229fa-187">Pořadí spouštění</span><span class="sxs-lookup"><span data-stu-id="229fa-187">Execution Order</span></span>

<span data-ttu-id="229fa-188">Konvence fungují jako poslední způsob služby WINS, stejně jako rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="229fa-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="229fa-189">To znamená, že pokud píšete dvě konvence, které nakonfigurují stejnou možnost stejné vlastnosti, pak poslední z nich spustí službu WINS.</span><span class="sxs-lookup"><span data-stu-id="229fa-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="229fa-190">Například v kódu níže je maximální délka všech řetězců nastavená na 500, ale my nakonfigurujeme všechny vlastnosti s názvem název v modelu tak, aby měly maximální délku 250.</span><span class="sxs-lookup"><span data-stu-id="229fa-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="229fa-191">Vzhledem k tomu, že konvence, která má nastavit maximální délku na 250, je po jedné, která nastaví všechny řetězce na 500, všechny vlastnosti s názvem v našem modelu budou mít hodnotu MaxLength 250, zatímco jakékoli jiné řetězce, například popisy, by byly 500.</span><span class="sxs-lookup"><span data-stu-id="229fa-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="229fa-192">Použití konvencí tímto způsobem znamená, že můžete zadat obecnou konvenci pro typy nebo vlastnosti v modelu a pak je overide pro podmnožiny, které jsou odlišné.</span><span class="sxs-lookup"><span data-stu-id="229fa-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="229fa-193">Rozhraní Fluent API a datové poznámky lze použít také k přepsání konvence v určitých případech.</span><span class="sxs-lookup"><span data-stu-id="229fa-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="229fa-194">Pokud jsme v našem příkladu používali rozhraní Fluent API k nastavení maximální délky vlastnosti, můžeme ji vložit před nebo po této konvenci, protože konkrétnější rozhraní API Fluent se podaří v obecnější konvenci konfigurace.</span><span class="sxs-lookup"><span data-stu-id="229fa-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="229fa-195">Předdefinované konvence</span><span class="sxs-lookup"><span data-stu-id="229fa-195">Built-in Conventions</span></span>

<span data-ttu-id="229fa-196">Vzhledem k tomu, že vlastní konvence můžou být ovlivněné výchozími konvencemi Code First, může být užitečné přidat konvence, které se spustí před nebo po jiné úmluvě.</span><span class="sxs-lookup"><span data-stu-id="229fa-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="229fa-197">K tomu můžete použít metody AddBefore a AddAfter kolekce konvence na odvozeném DbContext.</span><span class="sxs-lookup"><span data-stu-id="229fa-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="229fa-198">Následující kód by přidal třídu konvence, kterou jsme vytvořili dříve, aby se spustila před integrovanou konvencí zjišťování klíčů.</span><span class="sxs-lookup"><span data-stu-id="229fa-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="229fa-199">Tato možnost se bude přecházet z největšího počtu použití při přidávání konvencí, které je třeba spustit před nebo po předdefinovaných konvencích. seznam integrovaných konvencí najdete tady: [System. data. entity. ModelConfiguration. Conventions obor názvů](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="229fa-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="229fa-200">Můžete také odebrat konvence, které nechcete použít pro váš model.</span><span class="sxs-lookup"><span data-stu-id="229fa-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="229fa-201">Chcete-li odebrat konvenci, použijte metodu Remove.</span><span class="sxs-lookup"><span data-stu-id="229fa-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="229fa-202">Tady je příklad odebrání PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="229fa-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
