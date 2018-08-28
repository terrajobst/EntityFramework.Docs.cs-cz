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
# <a name="new-features-in-ef-core-20"></a>Novinky v EF Core 2.0

## <a name="net-standard-20"></a>.NET standard 2.0
EF Core teď cílí na .NET Standard 2.0, což znamená, že můžete pracovat s .NET Core 2.0, rozhraní .NET Framework 4.6.1 a dalších knihoven, které implementují rozhraní .NET Standard 2.0.
Zobrazit [implementace .NET nepodporuje](../platforms/index.md) podrobné informace o podporovaných.

## <a name="modeling"></a>Modelování

### <a name="table-splitting"></a>Rozdělení tabulky

Nyní je možné mapovat dvěma nebo více typů entit do stejné tabulky, kde primární klíč sloupců, které se bude sdílet, a každý řádek odpovídá dva nebo více entit.

Pokud chcete používat tabulky rozdělení identifikační vztah (kde vlastnosti cizího klíče tvoří primární klíč) musí nakonfigurovat mezi všemi typy entit v tabulce pro sdílení obsahu:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a>Vlastněné typy

Typ vlastnictví entity můžete sdílet stejný typ CLR s jiným typem vlastnictví entity, ale protože nebylo možné identifikovat jenom podle typu CLR musí být navigaci k němu z jiného typu entity. Entita obsahující definující navigace je vlastníkem. Při dotazování na vlastníka vlastněné typy budou zahrnuty ve výchozím nastavení.

Podle konvence se vytvoří stínové primární klíč pro typ vlastnictví a ho budou zmapována do stejné tabulky jako vlastník pomocí rozdělení tabulky. To umožňuje použití vlastní typy podobně jako na tom, jak komplexní typy, které se používají v EF6:

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
Přečtěte si [věnované vlastněné typy entit](xref:core/modeling/owned-entities) Další informace o této funkci.

### <a name="model-level-query-filters"></a>Filtry na úrovni modelu dotazu

EF Core 2.0 obsahuje novou funkci označujeme je jako filtry dotazu modelu. Tato funkce umožňuje predikáty dotazu LINQ (logický výraz předaný operátoru dotazu LINQ kde obvykle) Chcete-li definovat přímo na typy entit v modelu metadat (obvykle v OnModelCreating). Tyto filtry se automaticky použijí na všechny LINQ dotazy zahrnující tyto typy entit, včetně odkazy na typy entit odkazované nepřímo, jako například pomocí zahrnutí nebo přímé navigační vlastnosti. Jsou některé běžné aplikace tuto funkci:

- Obnovitelné odstranění – typy entit definuje vlastnost IsDeleted.
- Víceklientská architektura – typ Entity definuje vlastnost TenantId.

Tady je ukázka funkce pro dva scénáře uvedené výše jednoduchý příklad:

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
Definujeme filtr modelu, který implementuje více tenantů a obnovitelného odstranění pro instance ```Post``` typu Entity. Všimněte si použití úrovně vlastnost instance DbContext: ```TenantId```. Filtry na úrovni modelu bude používat hodnotu z instance správného kontextu (to znamená, že tato instance kontextu, který spouští dotaz).

U jednotlivých dotazů LINQ pomocí operátoru IgnoreQueryFilters() může být zakázáno filtry.

#### <a name="limitations"></a>Omezení

- Navigační odkazy nejsou povoleny. Tato funkce se můžou přidávat na základě zpětné vazby.
- Filtry je možné definovat pouze na kořenový typ Entity v hierarchii.

### <a name="database-scalar-function-mapping"></a>Mapování skalární funkce databáze

EF Core 2.0 obsahuje důležité příspěvku [Paul Middleton](https://github.com/pmiddleton) umožňující skalární funkce databáze mapování metody tříd stub tak, aby bylo možné použít v dotazech LINQ a do kódu SQL.

Tady je stručný popis, jak je možné tuto funkci:

Deklarovat statickou metodu na vaše `DbContext` a opatřovat je poznámkami s `DbFunctionAttribute`:

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

Metody tímto způsobem je automaticky zaregistrovaná. Po registraci volání metody v dotazu LINQ lze přeložit do volání funkcí v SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Mějte na paměti několik věcí:

- Podle konvence, název metody se používá jako název funkce (v tomto případě uživatelsky definovaná funkce) při generování SQL, ale můžete přepsat název a schéma během registrace – metoda
- Aktuálně jsou podporovány pouze skalární funkce
- V databázi je nutné vytvořit mapované funkce. Migrace EF Core se postará o jeho vytvoření

### <a name="self-contained-type-configuration-for-code-first"></a>Typ samostatné konfigurace pro kód první

V EF6 bylo možné k zapouzdření kódu první konfigurace konkrétní entitu typu odvozením z *EntityTypeConfiguration*. V EF Core 2.0 přinášíme tento vzor zpět:

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

## <a name="high-performance"></a>Vysoký výkon

### <a name="dbcontext-pooling"></a>Sdružování DbContext

Základní vzor pro pomocí EF Core v aplikaci ASP.NET Core obvykle zahrnuje registrace vlastního typu DbContext do systému injektáž závislostí a později získání instance tohoto typu pomocí konstruktoru parametry v seznamu zařízení. To znamená, že pro každé žádosti se vytvoří novou instanci třídy DbContext.

Ve verzi 2.0 přinášíme nový způsob, jak zaregistrovat vlastní typy DbContext v injektáž závislostí, které transparentně představuje fond opakovaně použitelných instancí DbContext. Pokud chcete použít, sdružování DbContext, použijte ```AddDbContextPool``` místo ```AddDbContext``` během registrace služby:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Pokud tato metoda se používá, v době instanci DbContext požádá o kontroleru, že jsme se nejprve zkontrolujte, jestli instance k dispozici ve fondu. Po zpracování žádosti se dokončí, jakýkoli stav instance se resetuje a instance je vrácen do fondu.

To se koncepčně podobá jak sdružování připojení funguje ve zprostředkovatelích ADO.NET a nabízí výhodu v podobě ukládá některé náklady na inicializaci instance DbContext.

### <a name="limitations"></a>Omezení

Nová metoda zavádí několik omezení na co se dá dělat ```OnConfiguring()``` metoda uvolněn objekt DbContext.

> [!WARNING]  
> Vyhněte se použití sdružování kontext databáze. Pokud chcete zachovat vlastní stav (například privátní pole) v odvozené třídy DbContext, který by neměl být sdíleny napříč požadavky. EF Core pouze resetuje stav, který je vědět před přidáním DbContext instance do fondu.

### <a name="explicitly-compiled-queries"></a>Explicitně kompilované dotazy

Toto je funkce druhé vyjádřit výslovný souhlas výkonu, které jsou navržené tak, aby nabízí výhody ve velkém měřítku scénářích.

Ruční nebo explicitně zkompilovaného dotazu byly k dispozici v předchozích verzích EF a také v technologii LINQ to SQL, pokud chcete umožnit aplikacím pro ukládání do mezipaměti překlad dotazů, takže mohou být vypočítány pouze jednou a spustit v mnoha případech rozhraní API.

I když EF Core lze obecně automaticky zkompilují a dotazy podle hodnoty hash reprezentace výrazy dotazu do mezipaměti, tento mechanismus je možné získat malé výkonnější obejitím výpočtu hodnoty hash a mezipaměti vyhledávání, umožňuje aplikace pro použití už zkompilovaný dotaz prostřednictvím vyvolání delegáta.

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

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Připojit můžete sledovat graf nová a existující entity

EF Core podporuje automatické generování hodnoty klíče prostřednictvím různých mechanismů. Při použití této funkce, hodnoty je vygenerováno, pokud vlastnost klíče je výchozí CLR – obvykle nula nebo hodnota null. To znamená, že graf entit může být předán `DbContext.Attach` nebo `DbSet.Attach` a EF Core označí tyto entity, které mají klíč již nastaven jako `Unchanged` během těchto entit, které nemají sady klíčů se označí jako `Added`. To usnadňuje připojení graf smíšené nové a existující entity při použití vygenerované klíče. `DbContext.Update` a `DbSet.Update` fungovat stejným způsobem, s tím rozdílem, že entity, které sady klíčů jsou označeny jako `Modified` místo `Unchanged`.

## <a name="query"></a>Dotazy

### <a name="improved-linq-translation"></a>Překlad lepší LINQ

Umožňuje další dotazy k byl úspěšně spuštěn s další logiku právě vyhodnocuje v databázi (namísto v paměti) a méně dat zbytečně načtených z databáze.

### <a name="groupjoin-improvements"></a>GroupJoin vylepšení

Tato práce zlepšuje SQL, který je generován pro group JOIN – klauzule. Group JOIN – klauzule jsou nejčastěji výsledkem poddotazy na volitelné navigační vlastnosti.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Interpolace řetězců v FromSql a ExecuteSqlCommand

C# 6 zavedené interpolace řetězců, funkce, která umožňuje výrazy jazyka C# má být vložen přímo v řetězcové literály, dobrý způsob vytváření řetězců v době běhu. V EF Core 2.0 jsme přidali zvláštní podporu pro interpolované řetězce do našich dva primární rozhraní API, které přijímají nezpracovaných řetězců SQL: ```FromSql``` a ```ExecuteSqlCommand```. Tato nová podpora umožňuje jazyka C# interpolace řetězců pro použití v podobě "bezpečné". To znamená způsobem, který chrání před běžných chyb prostřednictvím injektáže SQL, které může dojít, když dynamicky vytváření SQL za běhu.

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

V tomto příkladu jsou dvě proměnné, vložený ve formátovacím řetězci SQL. EF Core vytvoří následující příkaz SQL:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF. Functions.Like()

Přidali jsme EF. Vlastnosti funkce, která je možné definovat metody, které mapují na funkce databáze nebo operátory tak, aby ty mohou být vyvolány v dotazech LINQ pomocí EF Core nebo poskytovatelů. První příklad této metody je Like():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Všimněte si, že Like() přinášejí implementaci v paměti, což může být užitečný při práci databázi v paměti nebo když vyhodnocení predikátu je potřeba provést na straně klienta.

## <a name="database-management"></a>Správa databáze

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Pluralizace vidlici pro generování uživatelského rozhraní DbContext

EF Core 2.0 představuje nové *IPluralizer* služba, která se používá k množné číslo entity zadejte názvy a pluralize DbSet názvy. Výchozí implementace je no-op, takže je to právě hook, kde můžete snadno připojit uživatelům ve své vlastní pluralizer.

Zde je, jak to vypadá pro vývojáře k připojení do své vlastní pluralizer:

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

## <a name="others"></a>Ostatním uživatelům

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Přesunout do SQLitePCL.raw zprostředkovatele ADO.NET SQLite
To nám poskytuje informace o robustnějším řešení v Microsoft.Data.Sqlite distribuce nativní binární soubory SQLite na různých platformách.

### <a name="only-one-provider-per-model"></a>Pouze jednoho poskytovatele na modelu
Výrazně argumentech, jak mohou poskytovatelé komunikovat s modelem a zjednodušuje vytváření, poznámky a rozhraní fluent API fungování s jiným poskytovatelům.

EF Core 2.0 nyní vytvořit jiný [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) pro každý jiný poskytovatel používá. Toto je obvykle zřejmé do aplikace. To má zajišťované zjednodušení nižší úrovně metadat rozhraní API tak, aby žádný přístup k *běžné koncepty relační metadat* se vždycky provádí pomocí volání `.Relational` místo `.SqlServer`, `.Sqlite`atd.

### <a name="consolidated-logging-and-diagnostics"></a>Konsolidované protokolování a Diagnostika

Protokolování (podle objektu ILogger) a mechanismy diagnostiky (podle DiagnosticSource) teď sdílet další kód.

ID události pro zprávy odeslané do ILogger změnili ve verzi 2.0. ID událostí jsou nyní jedinečné v EF Core kódu. Tyto zprávy nyní také použít standardní vzor strukturované protokolování používá, například MVC.

Kategorie protokolovací nástroj se také změnily. Má nyní prostřednictvím dobře známé sady kategorií [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

Události DiagnosticSource teď používají stejné ID názvy událostí jako odpovídající `ILogger` zprávy.
