---
title: Co je nového v EF Core 2.0 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 83f6b819409d502dba17a678d44a0746a4a77f4b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417494"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="33fa8-102">Nové funkce v EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="33fa8-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="33fa8-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="33fa8-103">.NET Standard 2.0</span></span>

<span data-ttu-id="33fa8-104">EF Core nyní cílí na standard .NET Standard 2.0, což znamená, že může pracovat s rozhraními .NET Core 2.0, .NET Framework 4.6.1 a dalšími knihovnami, které implementují standard .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="33fa8-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="33fa8-105">Další podrobnosti o podporovaných tématech Podporované implementace rozhraní .NET naleznete v tématu [Podporované implementace.NET Implementations.](../platforms/index.md)</span><span class="sxs-lookup"><span data-stu-id="33fa8-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="33fa8-106">Modelování</span><span class="sxs-lookup"><span data-stu-id="33fa8-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="33fa8-107">Rozdělení tabulky</span><span class="sxs-lookup"><span data-stu-id="33fa8-107">Table splitting</span></span>

<span data-ttu-id="33fa8-108">Nyní je možné mapovat dva nebo více typů entit na stejnou tabulku, kde budou sdíleny sloupce primárního klíče a každý řádek bude odpovídat dvěma nebo více entitám.</span><span class="sxs-lookup"><span data-stu-id="33fa8-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="33fa8-109">Chcete-li použít rozdělení tabulky identifikační relace (kde vlastnosti cizího klíče tvoří primární klíč) musí být nakonfigurován mezi všechny typy entit sdílení tabulky:</span><span class="sxs-lookup"><span data-stu-id="33fa8-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

<span data-ttu-id="33fa8-110">Další informace o této funkci naleznete v [části o rozdělení tabulky.](xref:core/modeling/table-splitting)</span><span class="sxs-lookup"><span data-stu-id="33fa8-110">Read the [section on table splitting](xref:core/modeling/table-splitting) for more information on this feature.</span></span>

### <a name="owned-types"></a><span data-ttu-id="33fa8-111">Vlastněné typy</span><span class="sxs-lookup"><span data-stu-id="33fa8-111">Owned types</span></span>

<span data-ttu-id="33fa8-112">Typ vlastní entity může sdílet stejný typ .NET s jiným typem vlastněné entity, ale protože jej nelze identifikovat pouze podle typu .NET, musí k němu být navigace z jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="33fa8-112">An owned entity type can share the same .NET type with another owned entity type, but since it cannot be identified just by the .NET type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="33fa8-113">Entita obsahující definující navigaci je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="33fa8-113">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="33fa8-114">Při dotazování vlastníka budou ve výchozím nastavení zahrnuty vlastněné typy.</span><span class="sxs-lookup"><span data-stu-id="33fa8-114">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="33fa8-115">Podle konvence bude vytvořen stínový primární klíč pro vlastněný typ a bude mapován na stejnou tabulku jako vlastník pomocí rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="33fa8-115">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="33fa8-116">To umožňuje používat vlastněné typy podobně jako složité typy se používají v EF6:</span><span class="sxs-lookup"><span data-stu-id="33fa8-116">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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

<span data-ttu-id="33fa8-117">Další informace o této funkci naleznete v [části o typech vlastněných entit.](xref:core/modeling/owned-entities)</span><span class="sxs-lookup"><span data-stu-id="33fa8-117">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="33fa8-118">Filtry dotazů na úrovni modelu</span><span class="sxs-lookup"><span data-stu-id="33fa8-118">Model-level query filters</span></span>

<span data-ttu-id="33fa8-119">EF Core 2.0 obsahuje novou funkci, které nazýváme filtry dotazů na úrovni modelu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-119">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="33fa8-120">Tato funkce umožňuje predikáty dotazu LINQ (logický výraz obvykle předaný operátoru dotazu LINQ Where), který má být definován přímo na typech entit v modelu metadat (obvykle v OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="33fa8-120">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="33fa8-121">Tyto filtry jsou automaticky použity na všechny dotazy LINQ zahrnující tyto typy entit, včetně typů entit, na které se nepřímo odkazuje, například pomocí odkazů na vlastnosti Include nebo přímé navigace.</span><span class="sxs-lookup"><span data-stu-id="33fa8-121">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="33fa8-122">Některé běžné aplikace této funkce jsou:</span><span class="sxs-lookup"><span data-stu-id="33fa8-122">Some common applications of this feature are:</span></span>

- <span data-ttu-id="33fa8-123">Obnovitelné odstranění - Entity Typy definuje IsDeleted vlastnost.</span><span class="sxs-lookup"><span data-stu-id="33fa8-123">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="33fa8-124">Víceklientský - Typ entity definuje vlastnost TenantId.</span><span class="sxs-lookup"><span data-stu-id="33fa8-124">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="33fa8-125">Zde je jednoduchý příklad demonstrující funkci pro dva výše uvedené scénáře:</span><span class="sxs-lookup"><span data-stu-id="33fa8-125">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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

<span data-ttu-id="33fa8-126">Definujeme filtr na úrovni modelu, který implementuje víceklientské a `Post` obnovitelné odstranění pro instance typu entity.</span><span class="sxs-lookup"><span data-stu-id="33fa8-126">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the `Post` Entity Type.</span></span> <span data-ttu-id="33fa8-127">Všimněte si `DbContext` použití vlastnosti `TenantId`na úrovni instance: .</span><span class="sxs-lookup"><span data-stu-id="33fa8-127">Note the use of a `DbContext` instance-level property: `TenantId`.</span></span> <span data-ttu-id="33fa8-128">Filtry na úrovni modelu budou používat hodnotu z instance správného kontextu (to znamená, že instance kontextu, která provádí dotaz).</span><span class="sxs-lookup"><span data-stu-id="33fa8-128">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="33fa8-129">Filtry mohou být zakázány pro jednotlivé dotazy LINQ pomocí operátoru IgnoreQueryFilters().</span><span class="sxs-lookup"><span data-stu-id="33fa8-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="33fa8-130">Omezení</span><span class="sxs-lookup"><span data-stu-id="33fa8-130">Limitations</span></span>

- <span data-ttu-id="33fa8-131">Odkazy na navigaci nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="33fa8-131">Navigation references are not allowed.</span></span> <span data-ttu-id="33fa8-132">Tato funkce může být přidána na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="33fa8-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="33fa8-133">Filtry lze definovat pouze v kořenovém typu entity hierarchie.</span><span class="sxs-lookup"><span data-stu-id="33fa8-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="33fa8-134">Mapování skalárních funkcí databáze</span><span class="sxs-lookup"><span data-stu-id="33fa8-134">Database scalar function mapping</span></span>

<span data-ttu-id="33fa8-135">EF Core 2.0 obsahuje důležitý příspěvek od [Paul Middleton,](https://github.com/pmiddleton) který umožňuje mapování databázi skalární funkce metody zástupné procedury tak, aby mohly být použity v linq dotazy a přeloženy do SQL.</span><span class="sxs-lookup"><span data-stu-id="33fa8-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="33fa8-136">Zde je stručný popis toho, jak lze funkci použít:</span><span class="sxs-lookup"><span data-stu-id="33fa8-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="33fa8-137">Deklarujte statickou metodu a `DbContext` `DbFunctionAttribute`oznamujte ji poznámkami pomocí :</span><span class="sxs-lookup"><span data-stu-id="33fa8-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

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

<span data-ttu-id="33fa8-138">Metody, jako je tento, jsou automaticky registrovány.</span><span class="sxs-lookup"><span data-stu-id="33fa8-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="33fa8-139">Po registraci volání metody v dotazu LINQ lze přeložit do volání funkce v SQL:</span><span class="sxs-lookup"><span data-stu-id="33fa8-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="33fa8-140">Pár věcí, které stojí za zmínku:</span><span class="sxs-lookup"><span data-stu-id="33fa8-140">A few things to note:</span></span>

- <span data-ttu-id="33fa8-141">Podle konvence název metody se používá jako název funkce (v tomto případě uživatelem definované funkce) při generování SQL, ale můžete přepsat název a schéma během registrace metody.</span><span class="sxs-lookup"><span data-stu-id="33fa8-141">By convention the name of the method is used as the name of a function (in this case a user-defined function) when generating the SQL, but you can override the name and schema during method registration.</span></span>
- <span data-ttu-id="33fa8-142">V současné době jsou podporovány pouze skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="33fa8-142">Currently only scalar functions are supported.</span></span>
- <span data-ttu-id="33fa8-143">Je nutné vytvořit mapované funkce v databázi.</span><span class="sxs-lookup"><span data-stu-id="33fa8-143">You must create the mapped function in the database.</span></span> <span data-ttu-id="33fa8-144">EF Core migrace nebude starat o jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="33fa8-144">EF Core migrations will not take care of creating it.</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="33fa8-145">Nejprve samostatná konfigurace typu pro kód</span><span class="sxs-lookup"><span data-stu-id="33fa8-145">Self-contained type configuration for code first</span></span>

<span data-ttu-id="33fa8-146">V EF6 bylo možné zapouzdřit kód první konfigurace konkrétního typu entity odvozením z *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="33fa8-146">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="33fa8-147">V EF Core 2.0 přinášíme tento vzor zpět:</span><span class="sxs-lookup"><span data-stu-id="33fa8-147">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="33fa8-148">High Performance</span><span class="sxs-lookup"><span data-stu-id="33fa8-148">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="33fa8-149">Sdružování kontextu DbContext</span><span class="sxs-lookup"><span data-stu-id="33fa8-149">DbContext pooling</span></span>

<span data-ttu-id="33fa8-150">Základní vzor pro použití EF Core v ASP.NET základní aplikace obvykle zahrnuje registraci vlastního typu DbContext do systému vkládání závislostí a později získání instancí tohoto typu prostřednictvím parametrů konstruktoru v řadičích.</span><span class="sxs-lookup"><span data-stu-id="33fa8-150">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="33fa8-151">To znamená, že je vytvořena nová instance DbContext pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="33fa8-151">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="33fa8-152">Ve verzi 2.0 zavádíme nový způsob registrace vlastních typů DbContext v vkládání závislostí, které transparentně zavádí fond opakovaně použitelných instancí DbContext.</span><span class="sxs-lookup"><span data-stu-id="33fa8-152">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="33fa8-153">Chcete-li použít sdružování DbContext, použijte `AddDbContextPool` místo při registraci `AddDbContext` služby:</span><span class="sxs-lookup"><span data-stu-id="33fa8-153">To use DbContext pooling, use the `AddDbContextPool` instead of `AddDbContext` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="33fa8-154">Pokud je použita tato metoda, v době, kdy dbContext instance je požadováno řadič nejprve zkontrolujeme, pokud je k dispozici instance ve fondu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-154">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="33fa8-155">Jakmile je zpracování požadavku dokončeno, jakýkoli stav instance se resetuje a samotná instance je vrácena do fondu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-155">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="33fa8-156">To je koncepčně podobné, jak sdružování připojení funguje v ADO.NET zprostředkovatelé a má tu výhodu, že ukládání některé náklady na inicializaci DbContext instance.</span><span class="sxs-lookup"><span data-stu-id="33fa8-156">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="33fa8-157">Omezení</span><span class="sxs-lookup"><span data-stu-id="33fa8-157">Limitations</span></span>

<span data-ttu-id="33fa8-158">Nová metoda zavádí několik omezení na co lze `OnConfiguring()` provést v metodě DbContext.</span><span class="sxs-lookup"><span data-stu-id="33fa8-158">The new method introduces a few limitations on what can be done in the `OnConfiguring()` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="33fa8-159">Vyhněte se použití DbContext sdružování, pokud budete udržovat svůj vlastní stav (například soukromá pole) v odvozené DbContext třídy, které by neměly být sdíleny mezi požadavky.</span><span class="sxs-lookup"><span data-stu-id="33fa8-159">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="33fa8-160">EF Core pouze obnoví stav, který je vědoma před přidáním DbContext instance do fondu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-160">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="33fa8-161">Explicitně zkompilované dotazy</span><span class="sxs-lookup"><span data-stu-id="33fa8-161">Explicitly compiled queries</span></span>

<span data-ttu-id="33fa8-162">Toto je druhá funkce výkonu opt-in navržená tak, aby nabízela výhody ve scénářích ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="33fa8-162">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="33fa8-163">Ruční nebo explicitně zkompilované dotazy rozhraní API byly k dispozici v předchozích verzích EF a také v LINQ na SQL povolit aplikacím do mezipaměti překlad dotazů tak, aby mohly být vypočítány pouze jednou a provedeny mnohokrát.</span><span class="sxs-lookup"><span data-stu-id="33fa8-163">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="33fa8-164">Ačkoli obecně EF Core může automaticky kompilovat a mezipaměti dotazy na základě hash reprezentace výrazů dotazu, tento mechanismus lze získat malý nárůst výkonu vynecháním výpočtu hash a vyhledávání mezipaměti, což umožňuje aplikaci používat již zkompilovaný dotaz prostřednictvím vyvolání delegáta.</span><span class="sxs-lookup"><span data-stu-id="33fa8-164">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="33fa8-165">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="33fa8-165">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="33fa8-166">Připojit můžete sledovat graf nových a existujících entit</span><span class="sxs-lookup"><span data-stu-id="33fa8-166">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="33fa8-167">EF Core podporuje automatické generování klíčových hodnot prostřednictvím různých mechanismů.</span><span class="sxs-lookup"><span data-stu-id="33fa8-167">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="33fa8-168">Při použití této funkce je generována hodnota, pokud je vlastnost klíče výchozí CLR -- obvykle nula nebo null.</span><span class="sxs-lookup"><span data-stu-id="33fa8-168">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="33fa8-169">To znamená, že graf entit `DbContext.Attach` může `DbSet.Attach` být předán nebo a EF Core označí entity, které mají klíč již nastavený jako `Unchanged` zatímco entity, které nemají sadu klíčů, budou označeny jako `Added`.</span><span class="sxs-lookup"><span data-stu-id="33fa8-169">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="33fa8-170">To usnadňuje připojení grafu smíšených nových a existujících entit při použití generovaných klíčů.</span><span class="sxs-lookup"><span data-stu-id="33fa8-170">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="33fa8-171">`DbContext.Update`a `DbSet.Update` pracovat stejným způsobem, s tím rozdílem, že `Modified` entity `Unchanged`se sadou klíčů jsou označeny jako místo .</span><span class="sxs-lookup"><span data-stu-id="33fa8-171">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="33fa8-172">Dotaz</span><span class="sxs-lookup"><span data-stu-id="33fa8-172">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="33fa8-173">Vylepšený překlad LINQ</span><span class="sxs-lookup"><span data-stu-id="33fa8-173">Improved LINQ translation</span></span>

<span data-ttu-id="33fa8-174">Umožňuje úspěšné spuštění více dotazů, přičemž v databázi (spíše než v paměti) se vyhodnocuje více logiky a z databáze je zbytečně načítáno méně dat.</span><span class="sxs-lookup"><span data-stu-id="33fa8-174">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="33fa8-175">Vylepšení groupjoin</span><span class="sxs-lookup"><span data-stu-id="33fa8-175">GroupJoin improvements</span></span>

<span data-ttu-id="33fa8-176">Tato práce vylepšuje SQL, který je generován pro připojení ke skupině.</span><span class="sxs-lookup"><span data-stu-id="33fa8-176">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="33fa8-177">Spojení skupin jsou nejčastěji výsledkem dílčích dotazů na volitelné navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="33fa8-177">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="33fa8-178">Interpolace řetězců v příkazech FromSql a ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="33fa8-178">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="33fa8-179">C# 6 představil řetězec interpolace, funkce, která umožňuje C# výrazy přímo vložené do literály řetězce, poskytuje pěkný způsob vytváření řetězců za běhu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-179">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="33fa8-180">V EF Core 2.0 jsme přidali speciální podporu pro interpolované řetězce do našich dvou primárních api, které přijímají nezpracované řetězce SQL: `FromSql` a `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="33fa8-180">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: `FromSql` and `ExecuteSqlCommand`.</span></span> <span data-ttu-id="33fa8-181">Tato nová podpora umožňuje interpolaci řetězce Jazyka C# použít "bezpečným" způsobem.</span><span class="sxs-lookup"><span data-stu-id="33fa8-181">This new support allows C# string interpolation to be used in a "safe" manner.</span></span> <span data-ttu-id="33fa8-182">To znamená způsobem, který chrání před běžnými chybami injektáže SQL, ke kterým může dojít při dynamicky vytváření SQL za běhu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-182">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="33fa8-183">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="33fa8-183">Here is an example:</span></span>

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

<span data-ttu-id="33fa8-184">V tomto příkladu jsou dvě proměnné vložené do řetězce formátu SQL.</span><span class="sxs-lookup"><span data-stu-id="33fa8-184">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="33fa8-185">EF Core vytvoří následující SQL:</span><span class="sxs-lookup"><span data-stu-id="33fa8-185">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="33fa8-186">Ef. Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="33fa8-186">EF.Functions.Like()</span></span>

<span data-ttu-id="33fa8-187">Přidali jsme EF. Funkce vlastnost, která může být použita EF Core nebo zprostředkovatelé definovat metody, které mapovat na databázové funkce nebo operátory tak, aby tyto mohou být vyvolány v linq dotazy.</span><span class="sxs-lookup"><span data-stu-id="33fa8-187">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="33fa8-188">První příklad takové metody je Like():</span><span class="sxs-lookup"><span data-stu-id="33fa8-188">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="33fa8-189">Všimněte si, že Like() je dodáván s implementací v paměti, které mohou být užitečné při práci s databází v paměti nebo při vyhodnocení predikátu musí dojít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="33fa8-189">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="33fa8-190">Správa databází</span><span class="sxs-lookup"><span data-stu-id="33fa8-190">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="33fa8-191">Háček za pluralitu pro lešení DbContext</span><span class="sxs-lookup"><span data-stu-id="33fa8-191">Pluralization hook for DbContext scaffolding</span></span>

<span data-ttu-id="33fa8-192">EF Core 2.0 zavádí novou službu *IPluralizer,* která se používá k singularizaci názvů typů entit a pluralitě názvů DbSet.</span><span class="sxs-lookup"><span data-stu-id="33fa8-192">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="33fa8-193">Výchozí implementace je no-op, takže je to jen háček, kde lidé mohou snadno připojit své vlastní pluralizer.</span><span class="sxs-lookup"><span data-stu-id="33fa8-193">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="33fa8-194">Zde je to, co to vypadá pro vývojáře na háček v jejich vlastní pluralizer:</span><span class="sxs-lookup"><span data-stu-id="33fa8-194">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="33fa8-195">Ostatní</span><span class="sxs-lookup"><span data-stu-id="33fa8-195">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="33fa8-196">Přesunout ADO.NET poskytovatele SQLite na SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="33fa8-196">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>

<span data-ttu-id="33fa8-197">To nám dává robustnější řešení v Microsoft.Data.Sqlite pro distribuci nativních binárních souborů SQLite na různých platformách.</span><span class="sxs-lookup"><span data-stu-id="33fa8-197">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="33fa8-198">Pouze jeden zprostředkovatel na model</span><span class="sxs-lookup"><span data-stu-id="33fa8-198">Only one provider per model</span></span>

<span data-ttu-id="33fa8-199">Výrazně rozšiřuje způsob, jakým mohou poskytovatelé komunikovat s modelem, a zjednodušuje způsob, jakým konvence, poznámky a plynulá rozhraní API fungují s různými poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="33fa8-199">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="33fa8-200">EF Core 2.0 nyní vytvoří jiný [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) pro každého jiného poskytovatele, který se používá.</span><span class="sxs-lookup"><span data-stu-id="33fa8-200">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="33fa8-201">To je obvykle transparentní pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="33fa8-201">This is usually transparent to the application.</span></span> <span data-ttu-id="33fa8-202">To usnadnilo zjednodušení rozhraní API metadat nižší úrovně tak, aby byl přístup ke *společným konceptům relačních metadat* vždy proveden prostřednictvím volání namísto `.Relational` `.SqlServer`, `.Sqlite`atd.</span><span class="sxs-lookup"><span data-stu-id="33fa8-202">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="33fa8-203">Konsolidované protokolování a diagnostika</span><span class="sxs-lookup"><span data-stu-id="33fa8-203">Consolidated logging and diagnostics</span></span>

<span data-ttu-id="33fa8-204">Protokolování (na základě ILogger) a diagnostika (na základě DiagnosticSource) mechanismy nyní sdílet více kódu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-204">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="33fa8-205">ID událostí pro zprávy odeslané do iLoggeru se změnily v 2.0.</span><span class="sxs-lookup"><span data-stu-id="33fa8-205">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="33fa8-206">ID událostí jsou nyní jedinečné v rámci kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="33fa8-206">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="33fa8-207">Tyto zprávy nyní také postupujte podle standardnívzor pro strukturované protokolování používá, například MVC.</span><span class="sxs-lookup"><span data-stu-id="33fa8-207">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="33fa8-208">Kategorie loggerů se také změnily.</span><span class="sxs-lookup"><span data-stu-id="33fa8-208">Logger categories have also changed.</span></span> <span data-ttu-id="33fa8-209">Nyní je dobře známá sada kategorií přístupných prostřednictvím [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="33fa8-209">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="33fa8-210">Události DiagnosticSource nyní používají stejné názvy `ILogger` ID událostí jako odpovídající zprávy.</span><span class="sxs-lookup"><span data-stu-id="33fa8-210">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
