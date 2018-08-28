---
title: Novinky v EF Core 2.0 – EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: b52b1fe6b2d5a585f4d55b0299891f61cbc968a3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997567"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="e1e8d-102">Novinky v EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="e1e8d-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="e1e8d-103">.NET standard 2.0</span><span class="sxs-lookup"><span data-stu-id="e1e8d-103">.NET Standard 2.0</span></span>
<span data-ttu-id="e1e8d-104">EF Core teď cílí na .NET Standard 2.0, což znamená, že můžete pracovat s .NET Core 2.0, rozhraní .NET Framework 4.6.1 a dalších knihoven, které implementují rozhraní .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="e1e8d-105">Zobrazit [implementace .NET nepodporuje](../platforms/index.md) podrobné informace o podporovaných.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="e1e8d-106">Modelování</span><span class="sxs-lookup"><span data-stu-id="e1e8d-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="e1e8d-107">Rozdělení tabulky</span><span class="sxs-lookup"><span data-stu-id="e1e8d-107">Table splitting</span></span>

<span data-ttu-id="e1e8d-108">Nyní je možné mapovat dvěma nebo více typů entit do stejné tabulky, kde primární klíč sloupců, které se bude sdílet, a každý řádek odpovídá dva nebo více entit.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="e1e8d-109">Pokud chcete používat tabulky rozdělení identifikační vztah (kde vlastnosti cizího klíče tvoří primární klíč) musí nakonfigurovat mezi všemi typy entit v tabulce pro sdílení obsahu:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="e1e8d-110">Vlastněné typy</span><span class="sxs-lookup"><span data-stu-id="e1e8d-110">Owned types</span></span>

<span data-ttu-id="e1e8d-111">Typ vlastnictví entity můžete sdílet stejný typ CLR s jiným typem vlastnictví entity, ale protože nebylo možné identifikovat jenom podle typu CLR musí být navigaci k němu z jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="e1e8d-112">Entita obsahující definující navigace je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="e1e8d-113">Při dotazování na vlastníka vlastněné typy budou zahrnuty ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="e1e8d-114">Podle konvence se vytvoří stínové primární klíč pro typ vlastnictví a ho budou zmapována do stejné tabulky jako vlastník pomocí rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="e1e8d-115">To umožňuje použití vlastní typy podobně jako na tom, jak komplexní typy, které se používají v EF6:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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
<span data-ttu-id="e1e8d-116">Přečtěte si [věnované vlastněné typy entit](xref:core/modeling/owned-entities) Další informace o této funkci.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-116">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="e1e8d-117">Filtry na úrovni modelu dotazu</span><span class="sxs-lookup"><span data-stu-id="e1e8d-117">Model-level query filters</span></span>

<span data-ttu-id="e1e8d-118">EF Core 2.0 obsahuje novou funkci označujeme je jako filtry dotazu modelu.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-118">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="e1e8d-119">Tato funkce umožňuje predikáty dotazu LINQ (logický výraz předaný operátoru dotazu LINQ kde obvykle) Chcete-li definovat přímo na typy entit v modelu metadat (obvykle v OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="e1e8d-119">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="e1e8d-120">Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-120">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="e1e8d-121">Jsou některé běžné aplikace tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-121">Some common applications of this feature are:</span></span>

- <span data-ttu-id="e1e8d-122">Obnovitelné odstranění – typy entit definuje vlastnost IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-122">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="e1e8d-123">Víceklientská architektura – typ Entity definuje vlastnost TenantId.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-123">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="e1e8d-124">Tady je ukázka funkce pro dva scénáře uvedené výše jednoduchý příklad:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-124">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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
<span data-ttu-id="e1e8d-125">Definujeme filtr modelu, který implementuje více tenantů a obnovitelného odstranění pro instance ```Post``` typu Entity.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-125">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="e1e8d-126">Všimněte si použití úrovně vlastnost instance DbContext: ```TenantId```.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-126">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="e1e8d-127">Filtry na úrovni modelu bude používat hodnotu z instance správného kontextu (to znamená, že tato instance kontextu, který spouští dotaz).</span><span class="sxs-lookup"><span data-stu-id="e1e8d-127">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="e1e8d-128">U jednotlivých dotazů LINQ pomocí operátoru IgnoreQueryFilters() může být zakázáno filtry.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-128">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="e1e8d-129">Omezení</span><span class="sxs-lookup"><span data-stu-id="e1e8d-129">Limitations</span></span>

- <span data-ttu-id="e1e8d-130">Navigační odkazy nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-130">Navigation references are not allowed.</span></span> <span data-ttu-id="e1e8d-131">Tato funkce se můžou přidávat na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-131">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="e1e8d-132">Filtry je možné definovat pouze na kořenový typ Entity v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-132">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="e1e8d-133">Mapování skalární funkce databáze</span><span class="sxs-lookup"><span data-stu-id="e1e8d-133">Database scalar function mapping</span></span>

<span data-ttu-id="e1e8d-134">EF Core 2.0 obsahuje důležité příspěvku [Paul Middleton](https://github.com/pmiddleton) umožňující skalární funkce databáze mapování metody tříd stub tak, aby bylo možné použít v dotazech LINQ a do kódu SQL.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-134">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="e1e8d-135">Tady je stručný popis, jak je možné tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-135">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="e1e8d-136">Deklarovat statickou metodu na vaše `DbContext` a opatřovat je poznámkami s `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-136">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

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

<span data-ttu-id="e1e8d-137">Metody tímto způsobem je automaticky zaregistrovaná.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-137">Methods like this are automatically registered.</span></span> <span data-ttu-id="e1e8d-138">Po registraci volání metody v dotazu LINQ lze přeložit do volání funkcí v SQL:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-138">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="e1e8d-139">Mějte na paměti několik věcí:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-139">A few things to note:</span></span>

- <span data-ttu-id="e1e8d-140">Podle konvence, název metody se používá jako název funkce (v tomto případě uživatelsky definovaná funkce) při generování SQL, ale můžete přepsat název a schéma během registrace – metoda</span><span class="sxs-lookup"><span data-stu-id="e1e8d-140">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="e1e8d-141">Aktuálně jsou podporovány pouze skalární funkce</span><span class="sxs-lookup"><span data-stu-id="e1e8d-141">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="e1e8d-142">V databázi je nutné vytvořit mapované funkce.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-142">You must create the mapped function in the database.</span></span> <span data-ttu-id="e1e8d-143">Migrace EF Core se postará o jeho vytvoření</span><span class="sxs-lookup"><span data-stu-id="e1e8d-143">EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="e1e8d-144">Typ samostatné konfigurace pro kód první</span><span class="sxs-lookup"><span data-stu-id="e1e8d-144">Self-contained type configuration for code first</span></span>

<span data-ttu-id="e1e8d-145">V EF6 bylo možné k zapouzdření kódu první konfigurace konkrétní entitu typu odvozením z *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-145">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="e1e8d-146">V EF Core 2.0 přinášíme tento vzor zpět:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-146">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="e1e8d-147">Vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="e1e8d-147">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="e1e8d-148">Sdružování DbContext</span><span class="sxs-lookup"><span data-stu-id="e1e8d-148">DbContext pooling</span></span>

<span data-ttu-id="e1e8d-149">Základní vzor pro pomocí EF Core v aplikaci ASP.NET Core obvykle zahrnuje registrace vlastního typu DbContext do systému injektáž závislostí a později získání instance tohoto typu pomocí konstruktoru parametry v seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-149">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="e1e8d-150">To znamená, že pro každé žádosti se vytvoří novou instanci třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-150">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="e1e8d-151">Ve verzi 2.0 přinášíme nový způsob, jak zaregistrovat vlastní typy DbContext v injektáž závislostí, které transparentně představuje fond opakovaně použitelných instancí DbContext.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-151">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="e1e8d-152">Pokud chcete použít, sdružování DbContext, použijte ```AddDbContextPool``` místo ```AddDbContext``` během registrace služby:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-152">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="e1e8d-153">Pokud tato metoda se používá, v době instanci DbContext požádá o kontroleru, že jsme se nejprve zkontrolujte, jestli instance k dispozici ve fondu.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-153">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="e1e8d-154">Po zpracování žádosti se dokončí, jakýkoli stav instance se resetuje a instance je vrácen do fondu.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-154">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="e1e8d-155">To se koncepčně podobá jak sdružování připojení funguje ve zprostředkovatelích ADO.NET a nabízí výhodu v podobě ukládá některé náklady na inicializaci instance DbContext.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-155">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="e1e8d-156">Omezení</span><span class="sxs-lookup"><span data-stu-id="e1e8d-156">Limitations</span></span>

<span data-ttu-id="e1e8d-157">Nová metoda zavádí několik omezení na co se dá dělat ```OnConfiguring()``` metoda uvolněn objekt DbContext.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-157">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="e1e8d-158">Vyhněte se použití sdružování kontext databáze. Pokud chcete zachovat vlastní stav (například privátní pole) v odvozené třídy DbContext, který by neměl být sdíleny napříč požadavky.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-158">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="e1e8d-159">EF Core pouze resetuje stav, který je vědět před přidáním DbContext instance do fondu.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-159">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="e1e8d-160">Explicitně kompilované dotazy</span><span class="sxs-lookup"><span data-stu-id="e1e8d-160">Explicitly compiled queries</span></span>

<span data-ttu-id="e1e8d-161">Toto je funkce druhé vyjádřit výslovný souhlas výkonu, které jsou navržené tak, aby nabízí výhody ve velkém měřítku scénářích.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-161">This is the second opt-in performance features designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="e1e8d-162">Ruční nebo explicitně zkompilovaného dotazu byly k dispozici v předchozích verzích EF a také v technologii LINQ to SQL, pokud chcete umožnit aplikacím pro ukládání do mezipaměti překlad dotazů, takže mohou být vypočítány pouze jednou a spustit v mnoha případech rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-162">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="e1e8d-163">I když EF Core lze obecně automaticky zkompilují a dotazy podle hodnoty hash reprezentace výrazy dotazu do mezipaměti, tento mechanismus je možné získat malé výkonnější obejitím výpočtu hodnoty hash a mezipaměti vyhledávání, umožňuje aplikace pro použití už zkompilovaný dotaz prostřednictvím vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-163">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="e1e8d-164">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="e1e8d-164">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="e1e8d-165">Připojit můžete sledovat graf nová a existující entity</span><span class="sxs-lookup"><span data-stu-id="e1e8d-165">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="e1e8d-166">EF Core podporuje automatické generování hodnoty klíče prostřednictvím různých mechanismů.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-166">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="e1e8d-167">Při použití této funkce, hodnoty je vygenerováno, pokud vlastnost klíče je výchozí CLR – obvykle nula nebo hodnota null.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-167">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="e1e8d-168">To znamená, že graf entit může být předán `DbContext.Attach` nebo `DbSet.Attach` a EF Core označí tyto entity, které mají klíč již nastaven jako `Unchanged` během těchto entit, které nemají sady klíčů se označí jako `Added`.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-168">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="e1e8d-169">To usnadňuje připojení graf smíšené nové a existující entity při použití vygenerované klíče.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-169">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="e1e8d-170">`DbContext.Update` a `DbSet.Update` fungovat stejným způsobem, s tím rozdílem, že entity, které sady klíčů jsou označeny jako `Modified` místo `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-170">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="e1e8d-171">Dotazy</span><span class="sxs-lookup"><span data-stu-id="e1e8d-171">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="e1e8d-172">Překlad lepší LINQ</span><span class="sxs-lookup"><span data-stu-id="e1e8d-172">Improved LINQ Translation</span></span>

<span data-ttu-id="e1e8d-173">Umožňuje další dotazy k byl úspěšně spuštěn s další logiku právě vyhodnocuje v databázi (namísto v paměti) a méně dat zbytečně načtených z databáze.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-173">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="e1e8d-174">GroupJoin vylepšení</span><span class="sxs-lookup"><span data-stu-id="e1e8d-174">GroupJoin improvements</span></span>

<span data-ttu-id="e1e8d-175">Tato práce zlepšuje SQL, který je generován pro group JOIN – klauzule.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-175">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="e1e8d-176">Group JOIN – klauzule jsou nejčastěji výsledkem poddotazy na volitelné navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-176">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="e1e8d-177">Interpolace řetězců v FromSql a ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="e1e8d-177">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="e1e8d-178">C# 6 zavedené interpolace řetězců, funkce, která umožňuje výrazy jazyka C# má být vložen přímo v řetězcové literály, dobrý způsob vytváření řetězců v době běhu.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-178">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="e1e8d-179">V EF Core 2.0 jsme přidali zvláštní podporu pro interpolované řetězce do našich dva primární rozhraní API, které přijímají nezpracovaných řetězců SQL: ```FromSql``` a ```ExecuteSqlCommand```.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-179">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="e1e8d-180">Tato nová podpora umožňuje jazyka C# interpolace řetězců pro použití v podobě "bezpečné".</span><span class="sxs-lookup"><span data-stu-id="e1e8d-180">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="e1e8d-181">To znamená způsobem, který chrání před běžných chyb prostřednictvím injektáže SQL, které může dojít, když dynamicky vytváření SQL za běhu.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-181">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="e1e8d-182">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-182">Here is an example:</span></span>

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

<span data-ttu-id="e1e8d-183">V tomto příkladu jsou dvě proměnné, vložený ve formátovacím řetězci SQL.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-183">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="e1e8d-184">EF Core vytvoří následující příkaz SQL:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-184">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="e1e8d-185">EF. Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="e1e8d-185">EF.Functions.Like()</span></span>

<span data-ttu-id="e1e8d-186">Přidali jsme EF. Vlastnosti funkce, která je možné definovat metody, které mapují na funkce databáze nebo operátory tak, aby ty mohou být vyvolány v dotazech LINQ pomocí EF Core nebo poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-186">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="e1e8d-187">První příklad této metody je Like():</span><span class="sxs-lookup"><span data-stu-id="e1e8d-187">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="e1e8d-188">Všimněte si, že Like() přinášejí implementaci v paměti, což může být užitečný při práci databázi v paměti nebo když vyhodnocení predikátu je potřeba provést na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-188">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="e1e8d-189">Správa databáze</span><span class="sxs-lookup"><span data-stu-id="e1e8d-189">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="e1e8d-190">Pluralizace vidlici pro generování uživatelského rozhraní DbContext</span><span class="sxs-lookup"><span data-stu-id="e1e8d-190">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="e1e8d-191">EF Core 2.0 představuje nové *IPluralizer* služba, která se používá k množné číslo entity zadejte názvy a pluralize DbSet názvy.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-191">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="e1e8d-192">Výchozí implementace je no-op, takže je to právě hook, kde můžete snadno připojit uživatelům ve své vlastní pluralizer.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-192">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="e1e8d-193">Zde je, jak to vypadá pro vývojáře k připojení do své vlastní pluralizer:</span><span class="sxs-lookup"><span data-stu-id="e1e8d-193">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="e1e8d-194">Ostatním uživatelům</span><span class="sxs-lookup"><span data-stu-id="e1e8d-194">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="e1e8d-195">Přesunout do SQLitePCL.raw zprostředkovatele ADO.NET SQLite</span><span class="sxs-lookup"><span data-stu-id="e1e8d-195">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="e1e8d-196">To nám poskytuje informace o robustnějším řešení v Microsoft.Data.Sqlite distribuce nativní binární soubory SQLite na různých platformách.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-196">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="e1e8d-197">Pouze jednoho poskytovatele na modelu</span><span class="sxs-lookup"><span data-stu-id="e1e8d-197">Only one provider per model</span></span>
<span data-ttu-id="e1e8d-198">Výrazně argumentech, jak mohou poskytovatelé komunikovat s modelem a zjednodušuje vytváření, poznámky a rozhraní fluent API fungování s jiným poskytovatelům.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-198">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="e1e8d-199">EF Core 2.0 nyní vytvořit jiný [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) pro každý jiný poskytovatel používá.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-199">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="e1e8d-200">Toto je obvykle zřejmé do aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-200">This is usually transparent to the application.</span></span> <span data-ttu-id="e1e8d-201">To má zajišťované zjednodušení nižší úrovně metadat rozhraní API tak, aby žádný přístup k *běžné koncepty relační metadat* se vždycky provádí pomocí volání `.Relational` místo `.SqlServer`, `.Sqlite`atd.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-201">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="e1e8d-202">Konsolidované protokolování a Diagnostika</span><span class="sxs-lookup"><span data-stu-id="e1e8d-202">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="e1e8d-203">Protokolování (podle objektu ILogger) a mechanismy diagnostiky (podle DiagnosticSource) teď sdílet další kód.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-203">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="e1e8d-204">ID události pro zprávy odeslané do ILogger změnili ve verzi 2.0.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-204">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="e1e8d-205">ID událostí jsou nyní jedinečné v EF Core kódu.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-205">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="e1e8d-206">Tyto zprávy nyní také použít standardní vzor strukturované protokolování používá, například MVC.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-206">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="e1e8d-207">Kategorie protokolovací nástroj se také změnily.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-207">Logger categories have also changed.</span></span> <span data-ttu-id="e1e8d-208">Má nyní prostřednictvím dobře známé sady kategorií [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="e1e8d-208">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="e1e8d-209">Události DiagnosticSource teď používají stejné ID názvy událostí jako odpovídající `ILogger` zprávy.</span><span class="sxs-lookup"><span data-stu-id="e1e8d-209">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
