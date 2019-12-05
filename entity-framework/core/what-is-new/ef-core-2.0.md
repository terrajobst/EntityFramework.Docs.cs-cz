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
# <a name="new-features-in-ef-core-20"></a>Nové funkce v EF Core 2,0

## <a name="net-standard-20"></a>.NET Standard 2.0

EF Core teď cílí na .NET Standard 2,0, což znamená, že může pracovat s .NET Core 2,0, .NET Framework 4.6.1 a dalšími knihovnami, které implementují .NET Standard 2,0.
Další podrobnosti o tom, co je podporováno, najdete v tématu [podporované implementace rozhraní .NET](../platforms/index.md) .

## <a name="modeling"></a>Modelování

### <a name="table-splitting"></a>Rozdělení tabulky

Nyní je možné mapovat dva nebo více typů entit na stejnou tabulku, ve které budou sdíleny sloupce primárního klíče, a každý řádek bude odpovídat dvěma nebo více entitám.

Chcete-li použít tabulku rozdělením identifikačního vztahu (kde vlastnosti cizího klíče tvoří primární klíč), je nutné nakonfigurovat mezi všemi typy entit sdílení tabulky:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

Další informace o této funkci najdete v [části rozdělení tabulky](xref:core/modeling/table-splitting) .

### <a name="owned-types"></a>Vlastněné typy

Vlastněný typ entity může sdílet stejný typ .NET s jiným vlastněným typem entity, ale vzhledem k tomu, že jej nelze identifikovat pouze pomocí typu .NET, musí být k němu navigace z jiného typu entity. Entita obsahující definici navigace je vlastníkem. Při dotazování vlastníka budou ve výchozím nastavení zahrnuty vlastněné typy.

Podle konvence se vytvoří stínový primární klíč pro vlastní typ a namapuje se na stejnou tabulku jako vlastník pomocí rozdělení tabulky. To umožňuje použít vlastněné typy podobně jako složité typy používané v EF6:

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

Další informace o této funkci najdete v [části vlastní typy entit](xref:core/modeling/owned-entities) .

### <a name="model-level-query-filters"></a>Filtry dotazů na úrovni modelu

EF Core 2,0 obsahuje novou funkci, která volá filtry dotazů na úrovni modelu. Tato funkce umožňuje predikáty dotazů LINQ (logický výraz je obvykle předán operátoru dotazu LINQ), který má být definován přímo na typech entit v modelu metadat (obvykle v OnModelCreating). Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti. Jsou některé běžné aplikace tuto funkci:

- Obnovitelné odstranění – typy entit definují vlastnost IsDeleted.
- Víceklientská architektura – typ entity definuje vlastnost TenantId.

Tady je jednoduchý příklad, který demonstruje funkci pro dva výše uvedené scénáře:

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

Definujeme filtr na úrovni modelu, který implementuje víceklientské a obnovitelné odstranění instancí typu entity `Post`. Všimněte si použití `DbContext` vlastnosti na úrovni instance: `TenantId`. Filtry na úrovni modelu budou používat hodnotu ze správné instance kontextu (to znamená instance kontextu, která spouští dotaz).

Filtry mohou být zakázány pro jednotlivé dotazy LINQ pomocí operátoru IgnoreQueryFilters ().

#### <a name="limitations"></a>Omezení

- Navigační odkazy nejsou povoleny. Tato funkce se dá přidat na základě zpětné vazby.
- Filtry lze definovat pouze pro typ kořenové entity v hierarchii.

### <a name="database-scalar-function-mapping"></a>Mapování skalární funkce databáze

EF Core 2,0 obsahuje důležitý příspěvek z [Paul Middleton](https://github.com/pmiddleton) , který umožňuje mapování skalárních funkcí databáze na metodu, aby bylo možné je použít v dotazech LINQ a PŘELOŽIT na SQL.

Zde je stručný popis toho, jak lze funkci použít:

Deklarujete statickou metodu na svém `DbContext` a přidejte ji do `DbFunctionAttribute`:

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

Metody, jako jsou tyto, se zaregistrují automaticky. Po registraci lze volání metody v dotazu LINQ přeložit na volání funkcí v SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Pár věcí, které stojí za zmínku:

- Podle konvence je název metody používán jako název funkce (v tomto případě uživatelsky definovaná funkce) při generování kódu SQL, ale můžete přepsat název a schéma během registrace metody.
- V současné době jsou podporovány pouze skalární funkce.
- V databázi je nutné vytvořit namapovanou funkci. EF Core migrace se postará o jejich vytvoření.

### <a name="self-contained-type-configuration-for-code-first"></a>Samostatná konfigurace typu pro první kód

V EF6 bylo možné zapouzdřit první konfiguraci konkrétního typu entity odvozenou z *EntityTypeConfiguration*. V EF Core 2,0 přinášíme tento vzor zpátky:

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

## <a name="high-performance"></a>High Performance

### <a name="dbcontext-pooling"></a>Sdružování DbContext

Základní vzor pro použití EF Core v ASP.NET Core aplikace obvykle zahrnuje registraci vlastního typu DbContext do systému vkládání závislostí a pozdější získání instancí tohoto typu prostřednictvím parametrů konstruktoru v řadičích. To znamená, že pro každý požadavek se vytvoří nová instance DbContext.

Ve verzi 2,0 zavádíme nový způsob, jak registrovat vlastní DbContext typy v injektáže závislostí, který transparentně zavádí fond opakovaně použitelných instancí DbContext. Pokud chcete použít sdružování DbContext, použijte místo `AddDbContext` při registraci služby `AddDbContextPool`.

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Pokud se tato metoda používá v okamžiku, kdy řadič požaduje instanci DbContext, nejdřív zkontrolujeme, jestli je ve fondu k dispozici nějaká instance. Jakmile se zpracování žádosti dokončí, veškerý stav instance se resetuje a instance se vrátí do fondu.

Tento postup je koncepčně podobný tomu, jak sdružování připojení funguje v poskytovatelích ADO.NET a má výhodu při ukládání některých nákladů na inicializaci instance DbContext.

### <a name="limitations"></a>Omezení

Nová metoda zavádí několik omezení na to, co je možné provést v metodě `OnConfiguring()` DbContext.

> [!WARNING]  
> Nepoužívejte sdružování DbContext, pokud udržujete vlastní stav (například soukromá pole) v odvozené třídě DbContext, kterou byste neměli sdílet mezi požadavky. EF Core obnoví pouze stav, který je vědomý před přidáním instance DbContext do fondu.

### <a name="explicitly-compiled-queries"></a>Explicitně kompilované dotazy

Toto je druhá funkce pro zajištění výkonu, která je navržená tak, aby nabízela výhody ve vysoce škálovatelných scénářích.

V předchozích verzích EF byly k dispozici manuální nebo explicitně kompilovaná rozhraní API pro dotazy a také v LINQ to SQL, aby aplikace mohly ukládat překlady dotazů, aby je bylo možné vypočítat jen jednou a spustit mnohokrát.

I když v obecné EF Core může automaticky kompilovat a ukládat dotazy do mezipaměti na základě reprezentace hash výrazů dotazů, tento mechanismus lze použít k získání malého nárůstu výkonu obdržením výpočtu hodnoty hash a vyhledáváním v mezipaměti, což umožňuje aplikace pro použití již zkompilovaného dotazu prostřednictvím vyvolání delegáta.

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

## <a name="change-tracking"></a>Sledování změn

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Připojit může sledovat graf nových a existujících entit.

EF Core podporuje automatickou generaci hodnot klíčů prostřednictvím různých mechanismů. Při použití této funkce je vygenerována hodnota, pokud je klíčovou vlastností výchozí hodnota CLR, obvykle nula nebo null. To znamená, že graf entit lze předat `DbContext.Attach` nebo `DbSet.Attach` a EF Core označí tyto entity, které mají klíč již nastaven jako `Unchanged`, zatímco ty entity, které nemají sadu klíčů, budou označeny jako `Added`. Díky tomu je při použití vygenerovaných klíčů snadné připojit graf smíšených nových a existujících entit. `DbContext.Update` a `DbSet.Update` fungují stejným způsobem, s tím rozdílem, že entity se sadou klíčů jsou označené jako `Modified` namísto `Unchanged`.

## <a name="query"></a>Dotazy

### <a name="improved-linq-translation"></a>Vylepšený překlad LINQ

Umožňuje úspěšně spustit více dotazů, přičemž v databázi je vyhodnocena další logika (spíše než v paměti) a méně dat, která jsou zbytečně načítána z databáze.

### <a name="groupjoin-improvements"></a>Vylepšení GroupJoin

Tato práce zlepšuje SQL generovaný pro spojení skupin. Spojení skupin jsou nejčastěji výsledkem poddotazů k volitelným vlastnostem navigace.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Interpolace řetězců v Z tabulek a ExecuteSqlCommand

C#6 zavedla se interpolace řetězců, funkce C# , která umožňuje, aby byly výrazy přímo vloženy do řetězcových literálů a poskytovaly dobrý způsob sestavování řetězců za běhu. V EF Core 2,0 jsme přidali speciální podporu pro interpolované řetězce do našich dvou primárních rozhraní API, která přijímají nezpracované řetězce SQL: `FromSql` a `ExecuteSqlCommand`. Tato nová podpora umožňuje C# použití řetězcové interpolace v bezpečném způsobu. To je způsobem, který chrání před běžnými chybami při vkládání kódu SQL, ke kterým může dojít při dynamickém vytváření kódu SQL za běhu.

Tady je příklad:

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

V tomto příkladu jsou k dispozici dvě proměnné vložené do formátovacího řetězce SQL. EF Core vytvoří následující SQL:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF.Functions.Like()

Přidali jsme EF. Vlastnost Functions, kterou mohou používat EF Core nebo poskytovatelé k definování metod, které jsou mapovány na funkce nebo operátory databáze tak, aby mohly být vyvolány v dotazech LINQ. První příklad takové metody je jako ():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Všimněte si, že like () přináší implementaci v paměti, což může být užitečné při práci s databází v paměti nebo při vyhodnocování predikátu, který se musí nacházet na straně klienta.

## <a name="database-management"></a>Správa databází

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Zavěšení plurality pro generování uživatelského rozhraní DbContext

EF Core 2,0 zavádí novou službu *IPluralizer* , která se používá k množné číslo názvů typů entit a názvů doplnit jednotné negenerickými. Výchozí implementace je no-op, takže se jedná o pouze zavěšení, kde se může lidé snadno zapojit do své vlastní pluralizer.

Tady je to, co vypadá vývojářům, aby se připojili vlastní pluralizer:

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

## <a name="others"></a>Jiné

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Přesunout poskytovatele SQLite ADO.NET do SQLitePCL. Raw

Díky tomu máme robustnější řešení v Microsoft. data. sqlite pro distribuci nativních binárních souborů SQLite na různých platformách.

### <a name="only-one-provider-per-model"></a>Pouze jeden zprostředkovatel na model

Významně rozšiřuje způsob, jakým můžou poskytovatelé pracovat s modelem a zjednodušuje, jak konvence, poznámky a rozhraní API Fluent pracují s různými poskytovateli.

EF Core 2,0 teď pro každého jiného zprostředkovatele vytvoří jiný [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) . To je obvykle transparentní pro aplikaci. Usnadnili jsme tak zjednodušení rozhraní API na nižší úrovni tak, aby jakýkoliv přístup k *běžným koncepcím relačních metadat* byl vždy proveden voláním `.Relational` místo `.SqlServer`, `.Sqlite`atd.

### <a name="consolidated-logging-and-diagnostics"></a>Konsolidovaná protokolování a diagnostika

Protokolování (založené na ILogger) a diagnostické mechanismy (založené na DiagnosticSource) nyní sdílí více kódů.

ID událostí pro zprávy odeslané do ILogger se změnila v 2,0. Identifikátory událostí jsou teď v rámci EF Core kódu jedinečné. Tyto zprávy se teď také řídí standardním vzorem pro strukturované protokolování, které používá, například MVC.

Změnily se také kategorie protokolovacího nástroje. K dispozici je teď známá sada kategorií, ke které se dostanete prostřednictvím [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).

Události DiagnosticSource nyní používají stejné názvy ID události jako odpovídající zprávy `ILogger`.
