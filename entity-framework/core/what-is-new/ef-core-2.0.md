---
title: Co je nového v EF základní 2.0 - EF jádra
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 02d0b6fe2956e819e08e08c9a0658008abd36c34
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
ms.locfileid: "29680162"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="489a6-102">Nové funkce v EF základní 2.0</span><span class="sxs-lookup"><span data-stu-id="489a6-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="489a6-103">Rozhraní .NET standard 2.0</span><span class="sxs-lookup"><span data-stu-id="489a6-103">.NET Standard 2.0</span></span>
<span data-ttu-id="489a6-104">Základní EF teď cílí standardní 2.0 rozhraní .NET, která znamená, že můžete pracovat s .NET Core 2.0, .NET Framework 4.6.1 a další knihovny, které implementují rozhraní .NET 2.0 standardní.</span><span class="sxs-lookup"><span data-stu-id="489a6-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="489a6-105">V tématu [podporované implementace rozhraní .NET](../platforms/index.md) další podrobnosti o co je podporováno.</span><span class="sxs-lookup"><span data-stu-id="489a6-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="489a6-106">Modelování</span><span class="sxs-lookup"><span data-stu-id="489a6-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="489a6-107">Rozdělení tabulky</span><span class="sxs-lookup"><span data-stu-id="489a6-107">Table splitting</span></span>

<span data-ttu-id="489a6-108">Nyní je možné namapovat minimálně dva typy entit na stejnou tabulku, kde budou sdílet primární klíče sloupce a každý řádek odpovídá položce dvě nebo více entit.</span><span class="sxs-lookup"><span data-stu-id="489a6-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="489a6-109">Chcete-li tabulka rozdělení identifikační vztah (kde vlastnosti cizího klíče formuláři primární klíč) nutné nakonfigurovat mezi všemi typy entit sdílení v tabulce:</span><span class="sxs-lookup"><span data-stu-id="489a6-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="489a6-110">Vlastní typy</span><span class="sxs-lookup"><span data-stu-id="489a6-110">Owned types</span></span>

<span data-ttu-id="489a6-111">Typ entity ve vlastnictví můžete sdílet stejný typ CLR s jiným typem vlastní entity, ale vzhledem k tomu, že ho nelze identifikovat právě typ CLR musí být navigaci k němu z jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="489a6-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="489a6-112">Entita obsahující definující navigační je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="489a6-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="489a6-113">Při dotazování vlastník, budou ve výchozím nastavení zahrnuty vlastní typy.</span><span class="sxs-lookup"><span data-stu-id="489a6-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="489a6-114">Podle konvence se vytvoří stínové primární klíč pro vlastní typ a bude být namapované na stejné tabulce jako vlastník pomocí rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="489a6-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="489a6-115">To umožňuje použití vlastní typy podobně jako na tom, jak komplexní typy, které se používají v EF6:</span><span class="sxs-lookup"><span data-stu-id="489a6-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```
<span data-ttu-id="489a6-116">Pro čtení [části na typy entit ve vlastnictví](xref:core/modeling/owned-entities) Další informace o této funkci.</span><span class="sxs-lookup"><span data-stu-id="489a6-116">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="489a6-117">Dotaz na úrovni modelu filtry</span><span class="sxs-lookup"><span data-stu-id="489a6-117">Model-level query filters</span></span>

<span data-ttu-id="489a6-118">Základní EF 2.0 obsahuje novou funkci říkáme filtry dotazu na úrovni modelu.</span><span class="sxs-lookup"><span data-stu-id="489a6-118">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="489a6-119">Tato funkce umožňuje predikáty dotaz LINQ (logický výraz obvykle předaný operátor dotazu LINQ kde) aby byla definována přímo na typy entit v modelu metadat (obvykle v OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="489a6-119">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="489a6-120">Tyto filtry jsou automaticky použita pro všechny dotazy LINQ zahrnující tyto typy entit, včetně typů entit odkazované nepřímo, jako prostřednictvím zahrnout nebo přímé navigační vlastnost odkazů.</span><span class="sxs-lookup"><span data-stu-id="489a6-120">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="489a6-121">Některé běžné aplikace této funkce jsou:</span><span class="sxs-lookup"><span data-stu-id="489a6-121">Some common applications of this feature are:</span></span>

- <span data-ttu-id="489a6-122">Soft odstranit – typy entit definuje vlastnost IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="489a6-122">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="489a6-123">Víceklientský – typ Entity definuje vlastnost TenantId.</span><span class="sxs-lookup"><span data-stu-id="489a6-123">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="489a6-124">Zde je jednoduchý příklad ukázka funkce pro dva scénáře uvedené výše:</span><span class="sxs-lookup"><span data-stu-id="489a6-124">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId );
    }
}
```
<span data-ttu-id="489a6-125">Jsme definovali filtr na úrovni modelu, který implementuje víceklientský a konfigurace soft odstranění pro instance ```Post``` typ Entity.</span><span class="sxs-lookup"><span data-stu-id="489a6-125">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="489a6-126">Všimněte si použití úrovně vlastnost instance DbContext: ```TenantId```.</span><span class="sxs-lookup"><span data-stu-id="489a6-126">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="489a6-127">Filtry na úrovni modelu použije hodnotu z instance správný kontext.</span><span class="sxs-lookup"><span data-stu-id="489a6-127">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="489a6-128">Například ten, který je provádění dotazu.</span><span class="sxs-lookup"><span data-stu-id="489a6-128">I.e. the one that is executing the query.</span></span>

<span data-ttu-id="489a6-129">U jednotlivých dotazů LINQ pomocí operátoru IgnoreQueryFilters() může být zakázáno filtry.</span><span class="sxs-lookup"><span data-stu-id="489a6-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="489a6-130">Omezení</span><span class="sxs-lookup"><span data-stu-id="489a6-130">Limitations</span></span>

- <span data-ttu-id="489a6-131">Navigační odkazy nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="489a6-131">Navigation references are not allowed.</span></span> <span data-ttu-id="489a6-132">Tuto funkci lze přidat podle zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="489a6-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="489a6-133">Filtry lze definovat pouze v kořenovém adresáři typ Entity v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="489a6-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="489a6-134">Mapování skalární funkce databáze</span><span class="sxs-lookup"><span data-stu-id="489a6-134">Database scalar function mapping</span></span>

<span data-ttu-id="489a6-135">Základní EF 2.0 zahrnuje důležitým příspěvkem z [Paul Middleton](https://github.com/pmiddleton) která umožňuje mapování databáze skalární funkce metodě zástupných procedur tak, aby bylo možné použít v dotazech LINQ a převedeny na SQL.</span><span class="sxs-lookup"><span data-stu-id="489a6-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="489a6-136">Zde je stručný popis, jak můžete použít funkci:</span><span class="sxs-lookup"><span data-stu-id="489a6-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="489a6-137">Deklarovat statickou metodu na vaše `DbContext` a její přidání poznámek ke `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="489a6-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new Exception();
    }
}
```

<span data-ttu-id="489a6-138">Metody jako to jsou automaticky registruje.</span><span class="sxs-lookup"><span data-stu-id="489a6-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="489a6-139">Po registraci můžete volání metody v dotazu LINQ převedeny na volání funkcí v systému SQL:</span><span class="sxs-lookup"><span data-stu-id="489a6-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="489a6-140">Všimněte si, několik akcí:</span><span class="sxs-lookup"><span data-stu-id="489a6-140">A few things to note:</span></span>

- <span data-ttu-id="489a6-141">Podle konvence, které název metody, je použita jako název funkce (v tomto případě funkce definované uživatelem) při generování SQL, ale můžete přepsat název a schéma během registrace – metoda</span><span class="sxs-lookup"><span data-stu-id="489a6-141">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="489a6-142">Aktuálně jsou podporovány pouze skalární funkce</span><span class="sxs-lookup"><span data-stu-id="489a6-142">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="489a6-143">Je nutné vytvořit funkci namapované v databázi, například základní EF migrace nebude postará o její vytvoření</span><span class="sxs-lookup"><span data-stu-id="489a6-143">You must create the mapped function in the database, e.g. EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="489a6-144">Samostatný typ konfigurace pro kód první</span><span class="sxs-lookup"><span data-stu-id="489a6-144">Self-contained type configuration for code first</span></span>

<span data-ttu-id="489a6-145">V EF6 bylo možné zapouzdření kód první konfiguraci určitý typ entity odvozené z *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="489a6-145">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="489a6-146">V rámci EF základní 2.0 jsme zpět přináší tento vzor:</span><span class="sxs-lookup"><span data-stu-id="489a6-146">In EF Core 2.0 we are bringing this pattern back:</span></span>

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
  public void Configure(EntityTypeBuilder<Customer> builder)
  {
     builder.HasKey(c => c.AlternateKey);
     builder.Property(c => c.Name).HasMaxLength(200);
   }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a><span data-ttu-id="489a6-147">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="489a6-147">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="489a6-148">Sdružování DbContext</span><span class="sxs-lookup"><span data-stu-id="489a6-148">DbContext pooling</span></span>

<span data-ttu-id="489a6-149">Základní vzor pro používání jádra EF v aplikaci ASP.NET Core obvykle zahrnuje registrace vlastního typu DbContext do systému vkládání závislostí a později získání instance tohoto typu prostřednictvím parametrů konstruktor v seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="489a6-149">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="489a6-150">To znamená, že se pro každý požadavky vytvoří novou instanci třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="489a6-150">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="489a6-151">Ve verzi 2.0 Představujeme nový způsob, jak zaregistrovat vlastní typy DbContext v vkládání závislostí, které transparentně představuje fond opakovaně použitelné instance DbContext.</span><span class="sxs-lookup"><span data-stu-id="489a6-151">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="489a6-152">Pokud chcete použít, sdružování DbContext, použijte ```AddDbContextPool``` místo ```AddDbContext``` během registraci služby:</span><span class="sxs-lookup"><span data-stu-id="489a6-152">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="489a6-153">Pokud tato metoda se používá, v době DbContext instance je vyžádala řadič že jsme se nejdřív zkontrolujte, jestli instance k dispozici ve fondu.</span><span class="sxs-lookup"><span data-stu-id="489a6-153">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="489a6-154">Po zpracování žádosti dokončí, je resetovat jakýkoli stav v instanci a instance se vrátí do fondu.</span><span class="sxs-lookup"><span data-stu-id="489a6-154">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="489a6-155">Toto je koncepčně podobá jak sdružování připojení funguje ve zprostředkovatelích ADO.NET a nabízí výhodu v podobě ukládání některé náklady na inicializaci instance DbContext.</span><span class="sxs-lookup"><span data-stu-id="489a6-155">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="489a6-156">Omezení</span><span class="sxs-lookup"><span data-stu-id="489a6-156">Limitations</span></span>

<span data-ttu-id="489a6-157">Metoda new zavádí několik omezení co můžete udělat v ```OnConfiguring()``` metoda DbContext.</span><span class="sxs-lookup"><span data-stu-id="489a6-157">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="489a6-158">Vyhněte se používání sdružování DbContext, pokud chcete zachovat vlastní stavu (např. privátním polím) odvozené třídy DbContext, který by neměl být sdílení napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="489a6-158">Avoid using DbContext Pooling if you maintain your own state (e.g. private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="489a6-159">Základní EF pouze obnoví stav, který je vědět před přidáním DbContext instance do fondu.</span><span class="sxs-lookup"><span data-stu-id="489a6-159">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="489a6-160">Explicitně kompilované dotazy</span><span class="sxs-lookup"><span data-stu-id="489a6-160">Explicitly compiled queries</span></span>

<span data-ttu-id="489a6-161">Toto je druhý souhlas výkonu funkce určená k poskytování výhody ve scénářích velkého rozsahu.</span><span class="sxs-lookup"><span data-stu-id="489a6-161">This is the second opt-in performance features designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="489a6-162">Ruční nebo explicitně kompilovaném dotazu byly k dispozici v předchozích verzích EF a také v technologii LINQ to SQL k aplikacím pro ukládání do mezipaměti překlad dotazů, takže může být pouze jednou počítaný a provést mnohokrát rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="489a6-162">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="489a6-163">I když obecně možné EF základní automaticky zkompilovat a dotazy založené na hash reprezentace výrazy dotazů do mezipaměti, tento mechanismus lze použít k získání malé výkonnější vynecháte výpočet hodnoty hash a mezipaměti vyhledávání, což aplikace pro používání již kompilovaném dotazu prostřednictvím vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="489a6-163">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a><span data-ttu-id="489a6-164">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="489a6-164">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="489a6-165">Připojení můžete sledovat graf nových nebo stávajících entit</span><span class="sxs-lookup"><span data-stu-id="489a6-165">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="489a6-166">Jádro EF podporuje automatické generování hodnoty klíče prostřednictvím řady různých mechanismy.</span><span class="sxs-lookup"><span data-stu-id="489a6-166">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="489a6-167">Při použití této funkce, hodnota je vygenerováno, pokud vlastnost klíče je výchozí CLR – obvykle nula nebo hodnota null.</span><span class="sxs-lookup"><span data-stu-id="489a6-167">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="489a6-168">To znamená, že graf entit se dá předat do `DbContext.Attach` nebo `DbSet.Attach` a EF základní označíte tyto entity, které mají klíč již nastaven jako `Unchanged` při tyto entity, které nemají sady klíčů budou označeny jako `Added`.</span><span class="sxs-lookup"><span data-stu-id="489a6-168">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="489a6-169">To umožňuje snadno připojit graf smíšený nové a stávající entity při použití generují klíče.</span><span class="sxs-lookup"><span data-stu-id="489a6-169">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="489a6-170">`DbContext.Update` a `DbSet.Update` fungovat stejným způsobem, s tím rozdílem, že entity s sady klíčů jsou označeny jako `Modified` místo `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="489a6-170">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="489a6-171">Dotazy</span><span class="sxs-lookup"><span data-stu-id="489a6-171">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="489a6-172">Překlad vylepšené LINQ</span><span class="sxs-lookup"><span data-stu-id="489a6-172">Improved LINQ Translation</span></span>

<span data-ttu-id="489a6-173">Umožňuje další dotazy k byl úspěšně spuštěn s další logiku vyhodnocovaný v databázi (namísto v paměti) a méně dat zbytečně načítány z databáze.</span><span class="sxs-lookup"><span data-stu-id="489a6-173">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="489a6-174">GroupJoin vylepšení</span><span class="sxs-lookup"><span data-stu-id="489a6-174">GroupJoin improvements</span></span>

<span data-ttu-id="489a6-175">Tento pracovní zlepšuje SQL, který se vygeneruje pro skupiny spojení.</span><span class="sxs-lookup"><span data-stu-id="489a6-175">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="489a6-176">Group JOIN – klauzule jsou nejčastěji způsobeno poddotazech na volitelné navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="489a6-176">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="489a6-177">Řetězec interpolace v FromSql a ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="489a6-177">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="489a6-178">C# 6 zavedla řetězec interpolace, funkce, která umožňuje výrazy jazyka C# a přímo vložený textové literály, poskytuje dobrý způsob vytváření řetězců za běhu.</span><span class="sxs-lookup"><span data-stu-id="489a6-178">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="489a6-179">V rámci EF základní 2.0 jsme přidali speciální podporu pro interpolované řetězce pro naše dvě primární rozhraní API, které přijímají nezpracovaná řetězce SQL: ```FromSql``` a ```ExecuteSqlCommand```.</span><span class="sxs-lookup"><span data-stu-id="489a6-179">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="489a6-180">Tato nová podpora umožňuje C# řetězec interpolace použije "bezpečnou" způsobem.</span><span class="sxs-lookup"><span data-stu-id="489a6-180">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="489a6-181">Tj způsobem, který chrání před běžných chyb vkládání SQL, které může dojít, když dynamické konstruování SQL za běhu.</span><span class="sxs-lookup"><span data-stu-id="489a6-181">I.e. in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="489a6-182">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="489a6-182">Here is an example:</span></span>

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

<span data-ttu-id="489a6-183">V tomto příkladu jsou dvě proměnné, které jsou součástí řetězec formátu SQL.</span><span class="sxs-lookup"><span data-stu-id="489a6-183">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="489a6-184">Základní EF vytvoří následující příkaz SQL:</span><span class="sxs-lookup"><span data-stu-id="489a6-184">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="489a6-185">EF. Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="489a6-185">EF.Functions.Like()</span></span>

<span data-ttu-id="489a6-186">Jsme přidali EF. Vlastnost funkce, která lze použít EF jádra nebo poskytovatelé definovat metody, které jsou mapovány na funkce databáze nebo operátory tak, aby těch, které nelze vyvolat v dotazech LINQ.</span><span class="sxs-lookup"><span data-stu-id="489a6-186">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="489a6-187">V prvním příkladu tato metoda je Like():</span><span class="sxs-lookup"><span data-stu-id="489a6-187">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%");
    select c;
```

<span data-ttu-id="489a6-188">Všimněte si, že Like() dodává s implementace v paměti, které mohou být užitečné při práci s databázi v paměti, nebo při vyhodnocení predikátu musí proběhnout na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="489a6-188">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="489a6-189">Správa databáze</span><span class="sxs-lookup"><span data-stu-id="489a6-189">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="489a6-190">Pluralizační háku pro DbContext generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="489a6-190">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="489a6-191">Základní EF 2.0 představuje novou *IPluralizer* služba, která se používá k singularizovat entity zadejte názvy a pluralizovat DbSet názvy.</span><span class="sxs-lookup"><span data-stu-id="489a6-191">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="489a6-192">Výchozí implementace je žádná operace, to je právě háku, kde můžete snadno připojit zaměstnance ve svých vlastních pluralizer.</span><span class="sxs-lookup"><span data-stu-id="489a6-192">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="489a6-193">Zde je, jak vypadá pro vývojáře napojit ve svých vlastních pluralizer:</span><span class="sxs-lookup"><span data-stu-id="489a6-193">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a><span data-ttu-id="489a6-194">Ostatním uživatelům</span><span class="sxs-lookup"><span data-stu-id="489a6-194">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="489a6-195">Zprostředkovatel ADO.NET SQLite přesunuty SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="489a6-195">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="489a6-196">To nám poskytuje robustnější řešení v Microsoft.Data.Sqlite pro distribuci nativní SQLite binárních souborů na různých platformách.</span><span class="sxs-lookup"><span data-stu-id="489a6-196">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="489a6-197">Pouze jednoho poskytovatele na modelu</span><span class="sxs-lookup"><span data-stu-id="489a6-197">Only one provider per model</span></span>
<span data-ttu-id="489a6-198">Výrazně rozšiřuje, jak mohou poskytovatelé komunikovat s modelem a zjednodušuje, jak pracovat s různé zprostředkovatele konvence, poznámky a rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="489a6-198">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="489a6-199">Základní EF 2.0 nyní sestavení jiné [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) pro každý používaný jiný zprostředkovatel.</span><span class="sxs-lookup"><span data-stu-id="489a6-199">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="489a6-200">To je obvykle transparentní k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="489a6-200">This is usually transparent to the application.</span></span> <span data-ttu-id="489a6-201">To má usnadňují zjednodušení nižší úrovně metadat rozhraní API, tak, aby žádný přístup k *běžné koncepty relační metadata* je vždy provedeny prostřednictvím volání `.Relational` místo `.SqlServer`, `.Sqlite`atd.</span><span class="sxs-lookup"><span data-stu-id="489a6-201">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="489a6-202">Konsolidované protokolování a diagnostiky</span><span class="sxs-lookup"><span data-stu-id="489a6-202">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="489a6-203">(Podle objektu ILogger) protokolování a diagnostiky (podle DiagnosticSource) mechanismy nyní sdílet další kód.</span><span class="sxs-lookup"><span data-stu-id="489a6-203">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="489a6-204">ID události pro zprávy odeslané do objektu ILogger změnily v 2.0.</span><span class="sxs-lookup"><span data-stu-id="489a6-204">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="489a6-205">ID událostí jsou nyní jedinečné v rámci EF jádro kódu.</span><span class="sxs-lookup"><span data-stu-id="489a6-205">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="489a6-206">Tyto zprávy teď také použít standardní vzor strukturovaných protokolování používá, například MVC.</span><span class="sxs-lookup"><span data-stu-id="489a6-206">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="489a6-207">Změnily také kategorie protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="489a6-207">Logger categories have also changed.</span></span> <span data-ttu-id="489a6-208">Má nyní dobře známé sadu kategorií přistupovat prostřednictvím [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="489a6-208">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="489a6-209">Události DiagnosticSource teď použít stejné názvy ID událostí jako odpovídající `ILogger` zprávy.</span><span class="sxs-lookup"><span data-stu-id="489a6-209">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
