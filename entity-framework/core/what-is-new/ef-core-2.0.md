---
title: "Co je nového v EF základní 2.0 - EF jádra"
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
---
# <a name="new-features-in-ef-core-20"></a>Nové funkce v EF základní 2.0

## <a name="net-standard-20"></a>Rozhraní .NET standard 2.0
Základní EF teď cílí standardní 2.0 rozhraní .NET, která znamená, že můžete pracovat s .NET Core 2.0, .NET Framework 4.6.1 a další knihovny, které implementují rozhraní .NET 2.0 standardní.
V tématu [podporované implementace rozhraní .NET](../platforms/index.md) další podrobnosti o co je podporováno.

## <a name="modeling"></a>Modelování

### <a name="table-splitting"></a>Rozdělení tabulky

Nyní je možné namapovat minimálně dva typy entit na stejnou tabulku, kde budou sdílet primární klíče sloupce a každý řádek odpovídá položce dvě nebo více entit.

Chcete-li tabulka rozdělení identifikační vztah (kde vlastnosti cizího klíče formuláři primární klíč) nutné nakonfigurovat mezi všemi typy entit sdílení v tabulce:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a>Vlastní typy

Typ entity ve vlastnictví můžete sdílet stejný typ CLR s jiným typem vlastní entity, ale vzhledem k tomu, že ho nelze identifikovat právě typ CLR musí být navigaci k němu z jiného typu entity. Entita obsahující definující navigační je vlastníkem. Při dotazování vlastník, budou ve výchozím nastavení zahrnuty vlastní typy.

Podle konvence se vytvoří stínové primární klíč pro vlastní typ a bude být namapované na stejné tabulce jako vlastník pomocí rozdělení tabulky. To umožňuje použití vlastní typy podobně jako na tom, jak komplexní typy, které se používají v EF6:

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
Pro čtení [části na typy entit ve vlastnictví](xref:core/modeling/owned-entities) Další informace o této funkci.

### <a name="model-level-query-filters"></a>Dotaz na úrovni modelu filtry

Základní EF 2.0 obsahuje novou funkci říkáme filtry dotazu na úrovni modelu. Tato funkce umožňuje predikáty dotaz LINQ (logický výraz obvykle předaný operátor dotazu LINQ kde) aby byla definována přímo na typy entit v modelu metadat (obvykle v OnModelCreating). Tyto filtry jsou automaticky použita pro všechny dotazy LINQ zahrnující tyto typy entit, včetně typů entit odkazované nepřímo, jako prostřednictvím zahrnout nebo přímé navigační vlastnost odkazů. Některé běžné aplikace této funkce jsou:

- Soft odstranit – typy entit definuje vlastnost IsDeleted.
- Víceklientský – typ Entity definuje vlastnost TenantId.

Zde je jednoduchý příklad ukázka funkce pro dva scénáře uvedené výše:

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
Jsme definovali filtr na úrovni modelu, který implementuje víceklientský a konfigurace soft odstranění pro instance ```Post``` typ Entity. Všimněte si použití úrovně vlastnost instance DbContext: ```TenantId```. Filtry na úrovni modelu použije hodnotu z instance správný kontext. Například ten, který je provádění dotazu.

U jednotlivých dotazů LINQ pomocí operátoru IgnoreQueryFilters() může být zakázáno filtry.

#### <a name="limitations"></a>Omezení

- Navigační odkazy nejsou povoleny. Tuto funkci lze přidat podle zpětnou vazbu.
- Filtry lze definovat pouze v kořenovém adresáři typ Entity v hierarchii.

### <a name="database-scalar-function-mapping"></a>Mapování skalární funkce databáze

Základní EF 2.0 zahrnuje důležitým příspěvkem z [Paul Middleton](https://github.com/pmiddleton) která umožňuje mapování databáze skalární funkce metodě zástupných procedur tak, aby bylo možné použít v dotazech LINQ a převedeny na SQL.

Zde je stručný popis, jak můžete použít funkci:

Deklarovat statickou metodu na vaše `DbContext` a její přidání poznámek ke `DbFunctionAttribute`:

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

Metody jako to jsou automaticky registruje. Po registraci můžete volání metody v dotazu LINQ převedeny na volání funkcí v systému SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Všimněte si, několik akcí:

- Podle konvence, které název metody, je použita jako název funkce (v tomto případě funkce definované uživatelem) při generování SQL, ale můžete přepsat název a schéma během registrace – metoda
- Aktuálně jsou podporovány pouze skalární funkce
- Je nutné vytvořit funkci namapované v databázi, například základní EF migrace nebude postará o její vytvoření

### <a name="self-contained-type-configuration-for-code-first"></a>Samostatný typ konfigurace pro kód první

V EF6 bylo možné zapouzdření kód první konfiguraci určitý typ entity odvozené z *EntityTypeConfiguration*. V rámci EF základní 2.0 jsme zpět přináší tento vzor:

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

Základní vzor pro používání jádra EF v aplikaci ASP.NET Core obvykle zahrnuje registrace vlastního typu DbContext do systému vkládání závislostí a později získání instance tohoto typu prostřednictvím parametrů konstruktor v seznamu zařízení. To znamená, že se pro každý požadavky vytvoří novou instanci třídy DbContext.

Ve verzi 2.0 Představujeme nový způsob, jak zaregistrovat vlastní typy DbContext v vkládání závislostí, které transparentně představuje fond opakovaně použitelné instance DbContext. Pokud chcete použít, sdružování DbContext, použijte ```AddDbContextPool``` místo ```AddDbContext``` během registraci služby:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Pokud tato metoda se používá, v době DbContext instance je vyžádala řadič že jsme se nejdřív zkontrolujte, jestli instance k dispozici ve fondu. Po zpracování žádosti dokončí, je resetovat jakýkoli stav v instanci a instance se vrátí do fondu.

Toto je koncepčně podobá jak sdružování připojení funguje ve zprostředkovatelích ADO.NET a nabízí výhodu v podobě ukládání některé náklady na inicializaci instance DbContext.

### <a name="limitations"></a>Omezení

Metoda new zavádí několik omezení co můžete udělat v ```OnConfiguring()``` metoda DbContext.

> [!WARNING]  
> Vyhněte se používání sdružování DbContext, pokud chcete zachovat vlastní stavu (např. privátním polím) odvozené třídy DbContext, který by neměl být sdílení napříč požadavky. Základní EF pouze obnoví stav, který je vědět před přidáním DbContext instance do fondu.

### <a name="explicitly-compiled-queries"></a>Explicitně kompilované dotazy

Toto je druhý souhlas výkonu funkce určená k poskytování výhody ve scénářích velkého rozsahu.

Ruční nebo explicitně kompilovaném dotazu byly k dispozici v předchozích verzích EF a také v technologii LINQ to SQL k aplikacím pro ukládání do mezipaměti překlad dotazů, takže může být pouze jednou počítaný a provést mnohokrát rozhraní API.

I když obecně možné EF základní automaticky zkompilovat a dotazy založené na hash reprezentace výrazy dotazů do mezipaměti, tento mechanismus lze použít k získání malé výkonnější vynecháte výpočet hodnoty hash a mezipaměti vyhledávání, což aplikace pro používání již kompilovaném dotazu prostřednictvím vyvolání delegáta.

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

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Připojení můžete sledovat graf nových nebo stávajících entit

Jádro EF podporuje automatické generování hodnoty klíče prostřednictvím řady různých mechanismy. Při použití této funkce, hodnota je vygenerováno, pokud vlastnost klíče je výchozí CLR – obvykle nula nebo hodnota null. To znamená, že graf entit se dá předat do `DbContext.Attach` nebo `DbSet.Attach` a EF základní označíte tyto entity, které mají klíč již nastaven jako `Unchanged` při tyto entity, které nemají sady klíčů budou označeny jako `Added`. To umožňuje snadno připojit graf smíšený nové a stávající entity při použití generují klíče. `DbContext.Update` a `DbSet.Update` fungovat stejným způsobem, s tím rozdílem, že entity s sady klíčů jsou označeny jako `Modified` místo `Unchanged`.

## <a name="query"></a>Dotazy

### <a name="improved-linq-translation"></a>Překlad vylepšené LINQ

Umožňuje další dotazy k byl úspěšně spuštěn s další logiku vyhodnocovaný v databázi (namísto v paměti) a méně dat zbytečně načítány z databáze.

### <a name="groupjoin-improvements"></a>GroupJoin vylepšení

Tento pracovní zlepšuje SQL, který se vygeneruje pro skupiny spojení. Group JOIN – klauzule jsou nejčastěji způsobeno poddotazech na volitelné navigační vlastnosti.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Řetězec interpolace v FromSql a ExecuteSqlCommand

C# 6 zavedla řetězec interpolace, funkce, která umožňuje výrazy jazyka C# a přímo vložený textové literály, poskytuje dobrý způsob vytváření řetězců za běhu. V rámci EF základní 2.0 jsme přidali speciální podporu pro interpolované řetězce pro naše dvě primární rozhraní API, které přijímají nezpracovaná řetězce SQL: ```FromSql``` a ```ExecuteSqlCommand```. Tato nová podpora umožňuje C# řetězec interpolace použije "bezpečnou" způsobem. Tj způsobem, který chrání před běžných chyb vkládání SQL, které může dojít, když dynamické konstruování SQL za běhu.

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

V tomto příkladu jsou dvě proměnné, které jsou součástí řetězec formátu SQL. Základní EF vytvoří následující příkaz SQL:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF. Functions.Like()

Jsme přidali EF. Vlastnost funkce, která lze použít EF jádra nebo poskytovatelé definovat metody, které jsou mapovány na funkce databáze nebo operátory tak, aby těch, které nelze vyvolat v dotazech LINQ. V prvním příkladu tato metoda je Like():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%");
    select c;
```

Všimněte si, že Like() dodává s implementace v paměti, které mohou být užitečné při práci s databázi v paměti, nebo při vyhodnocení predikátu musí proběhnout na straně klienta.

## <a name="database-management"></a>Správa databáze

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Pluralizační háku pro DbContext generování uživatelského rozhraní

Základní EF 2.0 představuje novou *IPluralizer* služba, která se používá k singularizovat entity zadejte názvy a pluralizovat DbSet názvy. Výchozí implementace je žádná operace, to je právě háku, kde můžete snadno připojit zaměstnance ve svých vlastních pluralizer.

Zde je, jak vypadá pro vývojáře napojit ve svých vlastních pluralizer:

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

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Zprostředkovatel ADO.NET SQLite přesunuty SQLitePCL.raw
To nám poskytuje robustnější řešení v Microsoft.Data.Sqlite pro distribuci nativní SQLite binárních souborů na různých platformách.

### <a name="only-one-provider-per-model"></a>Pouze jednoho poskytovatele na modelu
Výrazně rozšiřuje, jak mohou poskytovatelé komunikovat s modelem a zjednodušuje, jak pracovat s různé zprostředkovatele konvence, poznámky a rozhraní fluent API.

Základní EF 2.0 nyní sestavení jiné [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) pro každý používaný jiný zprostředkovatel. To je obvykle transparentní k aplikaci. To má usnadňují zjednodušení nižší úrovně metadat rozhraní API, tak, aby žádný přístup k *běžné koncepty relační metadata* je vždy provedeny prostřednictvím volání `.Relational` místo `.SqlServer`, `.Sqlite`atd.

### <a name="consolidated-logging-and-diagnostics"></a>Konsolidované protokolování a diagnostiky

(Podle objektu ILogger) protokolování a diagnostiky (podle DiagnosticSource) mechanismy nyní sdílet další kód.

ID události pro zprávy odeslané do objektu ILogger změnily v 2.0. ID událostí jsou nyní jedinečné v rámci EF jádro kódu. Tyto zprávy teď také použít standardní vzor strukturovaných protokolování používá, například MVC.

Změnily také kategorie protokolovacího nástroje. Má nyní dobře známé sadu kategorií přistupovat prostřednictvím [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

Události DiagnosticSource teď použít stejné názvy ID událostí jako odpovídající `ILogger` zprávy.
