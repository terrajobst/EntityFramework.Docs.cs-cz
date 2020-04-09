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
# <a name="new-features-in-ef-core-20"></a>Nové funkce v EF Core 2.0

## <a name="net-standard-20"></a>.NET Standard 2.0

EF Core nyní cílí na standard .NET Standard 2.0, což znamená, že může pracovat s rozhraními .NET Core 2.0, .NET Framework 4.6.1 a dalšími knihovnami, které implementují standard .NET Standard 2.0.
Další podrobnosti o podporovaných tématech Podporované implementace rozhraní .NET naleznete v tématu [Podporované implementace.NET Implementations.](../platforms/index.md)

## <a name="modeling"></a>Modelování

### <a name="table-splitting"></a>Rozdělení tabulky

Nyní je možné mapovat dva nebo více typů entit na stejnou tabulku, kde budou sdíleny sloupce primárního klíče a každý řádek bude odpovídat dvěma nebo více entitám.

Chcete-li použít rozdělení tabulky identifikační relace (kde vlastnosti cizího klíče tvoří primární klíč) musí být nakonfigurován mezi všechny typy entit sdílení tabulky:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

Další informace o této funkci naleznete v [části o rozdělení tabulky.](xref:core/modeling/table-splitting)

### <a name="owned-types"></a>Vlastněné typy

Typ vlastní entity může sdílet stejný typ .NET s jiným typem vlastněné entity, ale protože jej nelze identifikovat pouze podle typu .NET, musí k němu být navigace z jiného typu entity. Entita obsahující definující navigaci je vlastníkem. Při dotazování vlastníka budou ve výchozím nastavení zahrnuty vlastněné typy.

Podle konvence bude vytvořen stínový primární klíč pro vlastněný typ a bude mapován na stejnou tabulku jako vlastník pomocí rozdělení tabulky. To umožňuje používat vlastněné typy podobně jako složité typy se používají v EF6:

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

Další informace o této funkci naleznete v [části o typech vlastněných entit.](xref:core/modeling/owned-entities)

### <a name="model-level-query-filters"></a>Filtry dotazů na úrovni modelu

EF Core 2.0 obsahuje novou funkci, které nazýváme filtry dotazů na úrovni modelu. Tato funkce umožňuje predikáty dotazu LINQ (logický výraz obvykle předaný operátoru dotazu LINQ Where), který má být definován přímo na typech entit v modelu metadat (obvykle v OnModelCreating). Tyto filtry jsou automaticky použity na všechny dotazy LINQ zahrnující tyto typy entit, včetně typů entit, na které se nepřímo odkazuje, například pomocí odkazů na vlastnosti Include nebo přímé navigace. Některé běžné aplikace této funkce jsou:

- Obnovitelné odstranění - Entity Typy definuje IsDeleted vlastnost.
- Víceklientský - Typ entity definuje vlastnost TenantId.

Zde je jednoduchý příklad demonstrující funkci pro dva výše uvedené scénáře:

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

Definujeme filtr na úrovni modelu, který implementuje víceklientské a `Post` obnovitelné odstranění pro instance typu entity. Všimněte si `DbContext` použití vlastnosti `TenantId`na úrovni instance: . Filtry na úrovni modelu budou používat hodnotu z instance správného kontextu (to znamená, že instance kontextu, která provádí dotaz).

Filtry mohou být zakázány pro jednotlivé dotazy LINQ pomocí operátoru IgnoreQueryFilters().

#### <a name="limitations"></a>Omezení

- Odkazy na navigaci nejsou povoleny. Tato funkce může být přidána na základě zpětné vazby.
- Filtry lze definovat pouze v kořenovém typu entity hierarchie.

### <a name="database-scalar-function-mapping"></a>Mapování skalárních funkcí databáze

EF Core 2.0 obsahuje důležitý příspěvek od [Paul Middleton,](https://github.com/pmiddleton) který umožňuje mapování databázi skalární funkce metody zástupné procedury tak, aby mohly být použity v linq dotazy a přeloženy do SQL.

Zde je stručný popis toho, jak lze funkci použít:

Deklarujte statickou metodu a `DbContext` `DbFunctionAttribute`oznamujte ji poznámkami pomocí :

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

Metody, jako je tento, jsou automaticky registrovány. Po registraci volání metody v dotazu LINQ lze přeložit do volání funkce v SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Pár věcí, které stojí za zmínku:

- Podle konvence název metody se používá jako název funkce (v tomto případě uživatelem definované funkce) při generování SQL, ale můžete přepsat název a schéma během registrace metody.
- V současné době jsou podporovány pouze skalární funkce.
- Je nutné vytvořit mapované funkce v databázi. EF Core migrace nebude starat o jeho vytvoření.

### <a name="self-contained-type-configuration-for-code-first"></a>Nejprve samostatná konfigurace typu pro kód

V EF6 bylo možné zapouzdřit kód první konfigurace konkrétního typu entity odvozením z *EntityTypeConfiguration*. V EF Core 2.0 přinášíme tento vzor zpět:

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

### <a name="dbcontext-pooling"></a>Sdružování kontextu DbContext

Základní vzor pro použití EF Core v ASP.NET základní aplikace obvykle zahrnuje registraci vlastního typu DbContext do systému vkládání závislostí a později získání instancí tohoto typu prostřednictvím parametrů konstruktoru v řadičích. To znamená, že je vytvořena nová instance DbContext pro každý požadavek.

Ve verzi 2.0 zavádíme nový způsob registrace vlastních typů DbContext v vkládání závislostí, které transparentně zavádí fond opakovaně použitelných instancí DbContext. Chcete-li použít sdružování DbContext, použijte `AddDbContextPool` místo při registraci `AddDbContext` služby:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Pokud je použita tato metoda, v době, kdy dbContext instance je požadováno řadič nejprve zkontrolujeme, pokud je k dispozici instance ve fondu. Jakmile je zpracování požadavku dokončeno, jakýkoli stav instance se resetuje a samotná instance je vrácena do fondu.

To je koncepčně podobné, jak sdružování připojení funguje v ADO.NET zprostředkovatelé a má tu výhodu, že ukládání některé náklady na inicializaci DbContext instance.

### <a name="limitations"></a>Omezení

Nová metoda zavádí několik omezení na co lze `OnConfiguring()` provést v metodě DbContext.

> [!WARNING]  
> Vyhněte se použití DbContext sdružování, pokud budete udržovat svůj vlastní stav (například soukromá pole) v odvozené DbContext třídy, které by neměly být sdíleny mezi požadavky. EF Core pouze obnoví stav, který je vědoma před přidáním DbContext instance do fondu.

### <a name="explicitly-compiled-queries"></a>Explicitně zkompilované dotazy

Toto je druhá funkce výkonu opt-in navržená tak, aby nabízela výhody ve scénářích ve velkém měřítku.

Ruční nebo explicitně zkompilované dotazy rozhraní API byly k dispozici v předchozích verzích EF a také v LINQ na SQL povolit aplikacím do mezipaměti překlad dotazů tak, aby mohly být vypočítány pouze jednou a provedeny mnohokrát.

Ačkoli obecně EF Core může automaticky kompilovat a mezipaměti dotazy na základě hash reprezentace výrazů dotazu, tento mechanismus lze získat malý nárůst výkonu vynecháním výpočtu hash a vyhledávání mezipaměti, což umožňuje aplikaci používat již zkompilovaný dotaz prostřednictvím vyvolání delegáta.

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

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Připojit můžete sledovat graf nových a existujících entit

EF Core podporuje automatické generování klíčových hodnot prostřednictvím různých mechanismů. Při použití této funkce je generována hodnota, pokud je vlastnost klíče výchozí CLR -- obvykle nula nebo null. To znamená, že graf entit `DbContext.Attach` může `DbSet.Attach` být předán nebo a EF Core označí entity, které mají klíč již nastavený jako `Unchanged` zatímco entity, které nemají sadu klíčů, budou označeny jako `Added`. To usnadňuje připojení grafu smíšených nových a existujících entit při použití generovaných klíčů. `DbContext.Update`a `DbSet.Update` pracovat stejným způsobem, s tím rozdílem, že `Modified` entity `Unchanged`se sadou klíčů jsou označeny jako místo .

## <a name="query"></a>Dotaz

### <a name="improved-linq-translation"></a>Vylepšený překlad LINQ

Umožňuje úspěšné spuštění více dotazů, přičemž v databázi (spíše než v paměti) se vyhodnocuje více logiky a z databáze je zbytečně načítáno méně dat.

### <a name="groupjoin-improvements"></a>Vylepšení groupjoin

Tato práce vylepšuje SQL, který je generován pro připojení ke skupině. Spojení skupin jsou nejčastěji výsledkem dílčích dotazů na volitelné navigační vlastnosti.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Interpolace řetězců v příkazech FromSql a ExecuteSqlCommand

C# 6 představil řetězec interpolace, funkce, která umožňuje C# výrazy přímo vložené do literály řetězce, poskytuje pěkný způsob vytváření řetězců za běhu. V EF Core 2.0 jsme přidali speciální podporu pro interpolované řetězce do našich dvou primárních api, které přijímají nezpracované řetězce SQL: `FromSql` a `ExecuteSqlCommand`. Tato nová podpora umožňuje interpolaci řetězce Jazyka C# použít "bezpečným" způsobem. To znamená způsobem, který chrání před běžnými chybami injektáže SQL, ke kterým může dojít při dynamicky vytváření SQL za běhu.

Zde naleznete příklad:

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

V tomto příkladu jsou dvě proměnné vložené do řetězce formátu SQL. EF Core vytvoří následující SQL:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>Ef. Functions.Like()

Přidali jsme EF. Funkce vlastnost, která může být použita EF Core nebo zprostředkovatelé definovat metody, které mapovat na databázové funkce nebo operátory tak, aby tyto mohou být vyvolány v linq dotazy. První příklad takové metody je Like():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Všimněte si, že Like() je dodáván s implementací v paměti, které mohou být užitečné při práci s databází v paměti nebo při vyhodnocení predikátu musí dojít na straně klienta.

## <a name="database-management"></a>Správa databází

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Háček za pluralitu pro lešení DbContext

EF Core 2.0 zavádí novou službu *IPluralizer,* která se používá k singularizaci názvů typů entit a pluralitě názvů DbSet. Výchozí implementace je no-op, takže je to jen háček, kde lidé mohou snadno připojit své vlastní pluralizer.

Zde je to, co to vypadá pro vývojáře na háček v jejich vlastní pluralizer:

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

## <a name="others"></a>Ostatní

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Přesunout ADO.NET poskytovatele SQLite na SQLitePCL.raw

To nám dává robustnější řešení v Microsoft.Data.Sqlite pro distribuci nativních binárních souborů SQLite na různých platformách.

### <a name="only-one-provider-per-model"></a>Pouze jeden zprostředkovatel na model

Výrazně rozšiřuje způsob, jakým mohou poskytovatelé komunikovat s modelem, a zjednodušuje způsob, jakým konvence, poznámky a plynulá rozhraní API fungují s různými poskytovateli.

EF Core 2.0 nyní vytvoří jiný [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) pro každého jiného poskytovatele, který se používá. To je obvykle transparentní pro aplikaci. To usnadnilo zjednodušení rozhraní API metadat nižší úrovně tak, aby byl přístup ke *společným konceptům relačních metadat* vždy proveden prostřednictvím volání namísto `.Relational` `.SqlServer`, `.Sqlite`atd.

### <a name="consolidated-logging-and-diagnostics"></a>Konsolidované protokolování a diagnostika

Protokolování (na základě ILogger) a diagnostika (na základě DiagnosticSource) mechanismy nyní sdílet více kódu.

ID událostí pro zprávy odeslané do iLoggeru se změnily v 2.0. ID událostí jsou nyní jedinečné v rámci kódu EF Core. Tyto zprávy nyní také postupujte podle standardnívzor pro strukturované protokolování používá, například MVC.

Kategorie loggerů se také změnily. Nyní je dobře známá sada kategorií přístupných prostřednictvím [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).

Události DiagnosticSource nyní používají stejné názvy `ILogger` ID událostí jako odpovídající zprávy.
