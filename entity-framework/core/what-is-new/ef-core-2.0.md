---
title: Co je nového v EF Core 2,0-EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 83f6b819409d502dba17a678d44a0746a4a77f4b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824873"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="3d866-102">Nové funkce v EF Core 2,0</span><span class="sxs-lookup"><span data-stu-id="3d866-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="3d866-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="3d866-103">.NET Standard 2.0</span></span>

<span data-ttu-id="3d866-104">EF Core teď cílí na .NET Standard 2,0, což znamená, že může pracovat s .NET Core 2,0, .NET Framework 4.6.1 a dalšími knihovnami, které implementují .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="3d866-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="3d866-105">Další podrobnosti o tom, co je podporováno, najdete v tématu [podporované implementace rozhraní .NET](../platforms/index.md) .</span><span class="sxs-lookup"><span data-stu-id="3d866-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="3d866-106">Modelování</span><span class="sxs-lookup"><span data-stu-id="3d866-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="3d866-107">Rozdělení tabulky</span><span class="sxs-lookup"><span data-stu-id="3d866-107">Table splitting</span></span>

<span data-ttu-id="3d866-108">Nyní je možné mapovat dva nebo více typů entit na stejnou tabulku, ve které budou sdíleny sloupce primárního klíče, a každý řádek bude odpovídat dvěma nebo více entitám.</span><span class="sxs-lookup"><span data-stu-id="3d866-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="3d866-109">Chcete-li použít tabulku rozdělením identifikačního vztahu (kde vlastnosti cizího klíče tvoří primární klíč), je nutné nakonfigurovat mezi všemi typy entit sdílení tabulky:</span><span class="sxs-lookup"><span data-stu-id="3d866-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

<span data-ttu-id="3d866-110">Další informace o této funkci najdete v [části rozdělení tabulky](xref:core/modeling/table-splitting) .</span><span class="sxs-lookup"><span data-stu-id="3d866-110">Read the [section on table splitting](xref:core/modeling/table-splitting) for more information on this feature.</span></span>

### <a name="owned-types"></a><span data-ttu-id="3d866-111">Vlastněné typy</span><span class="sxs-lookup"><span data-stu-id="3d866-111">Owned types</span></span>

<span data-ttu-id="3d866-112">Vlastněný typ entity může sdílet stejný typ .NET s jiným vlastněným typem entity, ale vzhledem k tomu, že jej nelze identifikovat pouze pomocí typu .NET, musí být k němu navigace z jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="3d866-112">An owned entity type can share the same .NET type with another owned entity type, but since it cannot be identified just by the .NET type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="3d866-113">Entita obsahující definici navigace je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="3d866-113">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="3d866-114">Při dotazování vlastníka budou ve výchozím nastavení zahrnuty vlastněné typy.</span><span class="sxs-lookup"><span data-stu-id="3d866-114">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="3d866-115">Podle konvence se vytvoří stínový primární klíč pro vlastní typ a namapuje se na stejnou tabulku jako vlastník pomocí rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="3d866-115">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="3d866-116">To umožňuje použít vlastněné typy podobně jako složité typy používané v EF6:</span><span class="sxs-lookup"><span data-stu-id="3d866-116">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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

<span data-ttu-id="3d866-117">Další informace o této funkci najdete v [části vlastní typy entit](xref:core/modeling/owned-entities) .</span><span class="sxs-lookup"><span data-stu-id="3d866-117">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="3d866-118">Filtry dotazů na úrovni modelu</span><span class="sxs-lookup"><span data-stu-id="3d866-118">Model-level query filters</span></span>

<span data-ttu-id="3d866-119">EF Core 2,0 obsahuje novou funkci, která volá filtry dotazů na úrovni modelu.</span><span class="sxs-lookup"><span data-stu-id="3d866-119">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="3d866-120">Tato funkce umožňuje predikáty dotazů LINQ (logický výraz je obvykle předán operátoru dotazu LINQ), který má být definován přímo na typech entit v modelu metadat (obvykle v OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="3d866-120">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="3d866-121">Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3d866-121">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="3d866-122">Jsou některé běžné aplikace tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="3d866-122">Some common applications of this feature are:</span></span>

- <span data-ttu-id="3d866-123">Obnovitelné odstranění – typy entit definují vlastnost IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="3d866-123">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="3d866-124">Víceklientská architektura – typ entity definuje vlastnost TenantId.</span><span class="sxs-lookup"><span data-stu-id="3d866-124">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="3d866-125">Tady je jednoduchý příklad, který demonstruje funkci pro dva výše uvedené scénáře:</span><span class="sxs-lookup"><span data-stu-id="3d866-125">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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
            && p.TenantId == this.TenantId);
    }
}
```

<span data-ttu-id="3d866-126">Definujeme filtr na úrovni modelu, který implementuje víceklientské a obnovitelné odstranění instancí typu entity `Post`.</span><span class="sxs-lookup"><span data-stu-id="3d866-126">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the `Post` Entity Type.</span></span> <span data-ttu-id="3d866-127">Všimněte si použití `DbContext` vlastnosti na úrovni instance: `TenantId`.</span><span class="sxs-lookup"><span data-stu-id="3d866-127">Note the use of a `DbContext` instance-level property: `TenantId`.</span></span> <span data-ttu-id="3d866-128">Filtry na úrovni modelu budou používat hodnotu ze správné instance kontextu (to znamená instance kontextu, která spouští dotaz).</span><span class="sxs-lookup"><span data-stu-id="3d866-128">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="3d866-129">Filtry mohou být zakázány pro jednotlivé dotazy LINQ pomocí operátoru IgnoreQueryFilters ().</span><span class="sxs-lookup"><span data-stu-id="3d866-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="3d866-130">Omezení</span><span class="sxs-lookup"><span data-stu-id="3d866-130">Limitations</span></span>

- <span data-ttu-id="3d866-131">Navigační odkazy nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="3d866-131">Navigation references are not allowed.</span></span> <span data-ttu-id="3d866-132">Tato funkce se dá přidat na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="3d866-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="3d866-133">Filtry lze definovat pouze pro typ kořenové entity v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="3d866-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="3d866-134">Mapování skalární funkce databáze</span><span class="sxs-lookup"><span data-stu-id="3d866-134">Database scalar function mapping</span></span>

<span data-ttu-id="3d866-135">EF Core 2,0 obsahuje důležitý příspěvek z [Paul Middleton](https://github.com/pmiddleton) , který umožňuje mapování skalárních funkcí databáze na metodu, aby bylo možné je použít v dotazech LINQ a PŘELOŽIT na SQL.</span><span class="sxs-lookup"><span data-stu-id="3d866-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="3d866-136">Zde je stručný popis toho, jak lze funkci použít:</span><span class="sxs-lookup"><span data-stu-id="3d866-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="3d866-137">Deklarujete statickou metodu na svém `DbContext` a přidejte ji do `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="3d866-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new NotImplementedException();
    }
}
```

<span data-ttu-id="3d866-138">Metody, jako jsou tyto, se zaregistrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="3d866-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="3d866-139">Po registraci lze volání metody v dotazu LINQ přeložit na volání funkcí v SQL:</span><span class="sxs-lookup"><span data-stu-id="3d866-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="3d866-140">Pár věcí, které stojí za zmínku:</span><span class="sxs-lookup"><span data-stu-id="3d866-140">A few things to note:</span></span>

- <span data-ttu-id="3d866-141">Podle konvence je název metody používán jako název funkce (v tomto případě uživatelsky definovaná funkce) při generování kódu SQL, ale můžete přepsat název a schéma během registrace metody.</span><span class="sxs-lookup"><span data-stu-id="3d866-141">By convention the name of the method is used as the name of a function (in this case a user-defined function) when generating the SQL, but you can override the name and schema during method registration.</span></span>
- <span data-ttu-id="3d866-142">V současné době jsou podporovány pouze skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="3d866-142">Currently only scalar functions are supported.</span></span>
- <span data-ttu-id="3d866-143">V databázi je nutné vytvořit namapovanou funkci.</span><span class="sxs-lookup"><span data-stu-id="3d866-143">You must create the mapped function in the database.</span></span> <span data-ttu-id="3d866-144">EF Core migrace se postará o jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="3d866-144">EF Core migrations will not take care of creating it.</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="3d866-145">Samostatná konfigurace typu pro první kód</span><span class="sxs-lookup"><span data-stu-id="3d866-145">Self-contained type configuration for code first</span></span>

<span data-ttu-id="3d866-146">V EF6 bylo možné zapouzdřit první konfiguraci konkrétního typu entity odvozenou z *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="3d866-146">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="3d866-147">V EF Core 2,0 přinášíme tento vzor zpátky:</span><span class="sxs-lookup"><span data-stu-id="3d866-147">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="3d866-148">High Performance</span><span class="sxs-lookup"><span data-stu-id="3d866-148">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="3d866-149">Sdružování DbContext</span><span class="sxs-lookup"><span data-stu-id="3d866-149">DbContext pooling</span></span>

<span data-ttu-id="3d866-150">Základní vzor pro použití EF Core v ASP.NET Core aplikace obvykle zahrnuje registraci vlastního typu DbContext do systému vkládání závislostí a pozdější získání instancí tohoto typu prostřednictvím parametrů konstruktoru v řadičích.</span><span class="sxs-lookup"><span data-stu-id="3d866-150">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="3d866-151">To znamená, že pro každý požadavek se vytvoří nová instance DbContext.</span><span class="sxs-lookup"><span data-stu-id="3d866-151">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="3d866-152">Ve verzi 2,0 zavádíme nový způsob, jak registrovat vlastní DbContext typy v injektáže závislostí, který transparentně zavádí fond opakovaně použitelných instancí DbContext.</span><span class="sxs-lookup"><span data-stu-id="3d866-152">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="3d866-153">Pokud chcete použít sdružování DbContext, použijte místo `AddDbContext` při registraci služby `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="3d866-153">To use DbContext pooling, use the `AddDbContextPool` instead of `AddDbContext` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="3d866-154">Pokud se tato metoda používá v okamžiku, kdy řadič požaduje instanci DbContext, nejdřív zkontrolujeme, jestli je ve fondu k dispozici nějaká instance.</span><span class="sxs-lookup"><span data-stu-id="3d866-154">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="3d866-155">Jakmile se zpracování žádosti dokončí, veškerý stav instance se resetuje a instance se vrátí do fondu.</span><span class="sxs-lookup"><span data-stu-id="3d866-155">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="3d866-156">Tento postup je koncepčně podobný tomu, jak sdružování připojení funguje v poskytovatelích ADO.NET a má výhodu při ukládání některých nákladů na inicializaci instance DbContext.</span><span class="sxs-lookup"><span data-stu-id="3d866-156">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="3d866-157">Omezení</span><span class="sxs-lookup"><span data-stu-id="3d866-157">Limitations</span></span>

<span data-ttu-id="3d866-158">Nová metoda zavádí několik omezení na to, co je možné provést v metodě `OnConfiguring()` DbContext.</span><span class="sxs-lookup"><span data-stu-id="3d866-158">The new method introduces a few limitations on what can be done in the `OnConfiguring()` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="3d866-159">Nepoužívejte sdružování DbContext, pokud udržujete vlastní stav (například soukromá pole) v odvozené třídě DbContext, kterou byste neměli sdílet mezi požadavky.</span><span class="sxs-lookup"><span data-stu-id="3d866-159">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="3d866-160">EF Core obnoví pouze stav, který je vědomý před přidáním instance DbContext do fondu.</span><span class="sxs-lookup"><span data-stu-id="3d866-160">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="3d866-161">Explicitně kompilované dotazy</span><span class="sxs-lookup"><span data-stu-id="3d866-161">Explicitly compiled queries</span></span>

<span data-ttu-id="3d866-162">Toto je druhá funkce pro zajištění výkonu, která je navržená tak, aby nabízela výhody ve vysoce škálovatelných scénářích.</span><span class="sxs-lookup"><span data-stu-id="3d866-162">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="3d866-163">V předchozích verzích EF byly k dispozici manuální nebo explicitně kompilovaná rozhraní API pro dotazy a také v LINQ to SQL, aby aplikace mohly ukládat překlady dotazů, aby je bylo možné vypočítat jen jednou a spustit mnohokrát.</span><span class="sxs-lookup"><span data-stu-id="3d866-163">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="3d866-164">I když v obecné EF Core může automaticky kompilovat a ukládat dotazy do mezipaměti na základě reprezentace hash výrazů dotazů, tento mechanismus lze použít k získání malého nárůstu výkonu obdržením výpočtu hodnoty hash a vyhledáváním v mezipaměti, což umožňuje aplikace pro použití již zkompilovaného dotazu prostřednictvím vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="3d866-164">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="3d866-165">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="3d866-165">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="3d866-166">Připojit může sledovat graf nových a existujících entit.</span><span class="sxs-lookup"><span data-stu-id="3d866-166">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="3d866-167">EF Core podporuje automatickou generaci hodnot klíčů prostřednictvím různých mechanismů.</span><span class="sxs-lookup"><span data-stu-id="3d866-167">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="3d866-168">Při použití této funkce je vygenerována hodnota, pokud je klíčovou vlastností výchozí hodnota CLR, obvykle nula nebo null.</span><span class="sxs-lookup"><span data-stu-id="3d866-168">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="3d866-169">To znamená, že graf entit lze předat `DbContext.Attach` nebo `DbSet.Attach` a EF Core označí tyto entity, které mají klíč již nastaven jako `Unchanged`, zatímco ty entity, které nemají sadu klíčů, budou označeny jako `Added`.</span><span class="sxs-lookup"><span data-stu-id="3d866-169">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="3d866-170">Díky tomu je při použití vygenerovaných klíčů snadné připojit graf smíšených nových a existujících entit.</span><span class="sxs-lookup"><span data-stu-id="3d866-170">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="3d866-171">`DbContext.Update` a `DbSet.Update` fungují stejným způsobem, s tím rozdílem, že entity se sadou klíčů jsou označené jako `Modified` namísto `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="3d866-171">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="3d866-172">Dotazy</span><span class="sxs-lookup"><span data-stu-id="3d866-172">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="3d866-173">Vylepšený překlad LINQ</span><span class="sxs-lookup"><span data-stu-id="3d866-173">Improved LINQ translation</span></span>

<span data-ttu-id="3d866-174">Umožňuje úspěšně spustit více dotazů, přičemž v databázi je vyhodnocena další logika (spíše než v paměti) a méně dat, která jsou zbytečně načítána z databáze.</span><span class="sxs-lookup"><span data-stu-id="3d866-174">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="3d866-175">Vylepšení GroupJoin</span><span class="sxs-lookup"><span data-stu-id="3d866-175">GroupJoin improvements</span></span>

<span data-ttu-id="3d866-176">Tato práce zlepšuje SQL generovaný pro spojení skupin.</span><span class="sxs-lookup"><span data-stu-id="3d866-176">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="3d866-177">Spojení skupin jsou nejčastěji výsledkem poddotazů k volitelným vlastnostem navigace.</span><span class="sxs-lookup"><span data-stu-id="3d866-177">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="3d866-178">Interpolace řetězců v Z tabulek a ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="3d866-178">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="3d866-179">C#6 zavedla se interpolace řetězců, funkce C# , která umožňuje, aby byly výrazy přímo vloženy do řetězcových literálů a poskytovaly dobrý způsob sestavování řetězců za běhu.</span><span class="sxs-lookup"><span data-stu-id="3d866-179">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="3d866-180">V EF Core 2,0 jsme přidali speciální podporu pro interpolované řetězce do našich dvou primárních rozhraní API, která přijímají nezpracované řetězce SQL: `FromSql` a `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="3d866-180">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: `FromSql` and `ExecuteSqlCommand`.</span></span> <span data-ttu-id="3d866-181">Tato nová podpora umožňuje C# použití řetězcové interpolace v bezpečném způsobu.</span><span class="sxs-lookup"><span data-stu-id="3d866-181">This new support allows C# string interpolation to be used in a "safe" manner.</span></span> <span data-ttu-id="3d866-182">To je způsobem, který chrání před běžnými chybami při vkládání kódu SQL, ke kterým může dojít při dynamickém vytváření kódu SQL za běhu.</span><span class="sxs-lookup"><span data-stu-id="3d866-182">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="3d866-183">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="3d866-183">Here is an example:</span></span>

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

<span data-ttu-id="3d866-184">V tomto příkladu jsou k dispozici dvě proměnné vložené do formátovacího řetězce SQL.</span><span class="sxs-lookup"><span data-stu-id="3d866-184">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="3d866-185">EF Core vytvoří následující SQL:</span><span class="sxs-lookup"><span data-stu-id="3d866-185">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="3d866-186">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="3d866-186">EF.Functions.Like()</span></span>

<span data-ttu-id="3d866-187">Přidali jsme EF. Vlastnost Functions, kterou mohou používat EF Core nebo poskytovatelé k definování metod, které jsou mapovány na funkce nebo operátory databáze tak, aby mohly být vyvolány v dotazech LINQ.</span><span class="sxs-lookup"><span data-stu-id="3d866-187">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="3d866-188">První příklad takové metody je jako ():</span><span class="sxs-lookup"><span data-stu-id="3d866-188">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="3d866-189">Všimněte si, že like () přináší implementaci v paměti, což může být užitečné při práci s databází v paměti nebo při vyhodnocování predikátu, který se musí nacházet na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="3d866-189">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="3d866-190">Správa databází</span><span class="sxs-lookup"><span data-stu-id="3d866-190">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="3d866-191">Zavěšení plurality pro generování uživatelského rozhraní DbContext</span><span class="sxs-lookup"><span data-stu-id="3d866-191">Pluralization hook for DbContext scaffolding</span></span>

<span data-ttu-id="3d866-192">EF Core 2,0 zavádí novou službu *IPluralizer* , která se používá k množné číslo názvů typů entit a názvů doplnit jednotné negenerickými.</span><span class="sxs-lookup"><span data-stu-id="3d866-192">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="3d866-193">Výchozí implementace je no-op, takže se jedná o pouze zavěšení, kde se může lidé snadno zapojit do své vlastní pluralizer.</span><span class="sxs-lookup"><span data-stu-id="3d866-193">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="3d866-194">Tady je to, co vypadá vývojářům, aby se připojili vlastní pluralizer:</span><span class="sxs-lookup"><span data-stu-id="3d866-194">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="3d866-195">Jiné</span><span class="sxs-lookup"><span data-stu-id="3d866-195">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="3d866-196">Přesunout poskytovatele SQLite ADO.NET do SQLitePCL. Raw</span><span class="sxs-lookup"><span data-stu-id="3d866-196">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>

<span data-ttu-id="3d866-197">Díky tomu máme robustnější řešení v Microsoft. data. sqlite pro distribuci nativních binárních souborů SQLite na různých platformách.</span><span class="sxs-lookup"><span data-stu-id="3d866-197">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="3d866-198">Pouze jeden zprostředkovatel na model</span><span class="sxs-lookup"><span data-stu-id="3d866-198">Only one provider per model</span></span>

<span data-ttu-id="3d866-199">Významně rozšiřuje způsob, jakým můžou poskytovatelé pracovat s modelem a zjednodušuje, jak konvence, poznámky a rozhraní API Fluent pracují s různými poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="3d866-199">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="3d866-200">EF Core 2,0 teď pro každého jiného zprostředkovatele vytvoří jiný [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) .</span><span class="sxs-lookup"><span data-stu-id="3d866-200">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="3d866-201">To je obvykle transparentní pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3d866-201">This is usually transparent to the application.</span></span> <span data-ttu-id="3d866-202">Usnadnili jsme tak zjednodušení rozhraní API na nižší úrovni tak, aby jakýkoliv přístup k *běžným koncepcím relačních metadat* byl vždy proveden voláním `.Relational` místo `.SqlServer`, `.Sqlite`atd.</span><span class="sxs-lookup"><span data-stu-id="3d866-202">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="3d866-203">Konsolidovaná protokolování a diagnostika</span><span class="sxs-lookup"><span data-stu-id="3d866-203">Consolidated logging and diagnostics</span></span>

<span data-ttu-id="3d866-204">Protokolování (založené na ILogger) a diagnostické mechanismy (založené na DiagnosticSource) nyní sdílí více kódů.</span><span class="sxs-lookup"><span data-stu-id="3d866-204">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="3d866-205">ID událostí pro zprávy odeslané do ILogger se změnila v 2,0.</span><span class="sxs-lookup"><span data-stu-id="3d866-205">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="3d866-206">Identifikátory událostí jsou teď v rámci EF Core kódu jedinečné.</span><span class="sxs-lookup"><span data-stu-id="3d866-206">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="3d866-207">Tyto zprávy se teď také řídí standardním vzorem pro strukturované protokolování, které používá, například MVC.</span><span class="sxs-lookup"><span data-stu-id="3d866-207">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="3d866-208">Změnily se také kategorie protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="3d866-208">Logger categories have also changed.</span></span> <span data-ttu-id="3d866-209">K dispozici je teď známá sada kategorií, ke které se dostanete prostřednictvím [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="3d866-209">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="3d866-210">Události DiagnosticSource nyní používají stejné názvy ID události jako odpovídající zprávy `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="3d866-210">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
