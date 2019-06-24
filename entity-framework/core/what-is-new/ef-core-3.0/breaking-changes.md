---
title: Zásadní změny v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 1d2853cfc7f6eadfc76000f91a723f8b0b8c201f
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333831"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>Rozbíjející změny zahrnuté v EF Core 3.0 (aktuálně ve verzi preview)

> [!IMPORTANT]
> Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.

Následující změny chování a rozhraní API by mohly přerušit aplikace vyvinuté pro jádro EF Core 2.2.x při upgradu na 3.0.0.
Změny, které chceme ovlivní jen poskytovatelé databází jsou popsány v části [poskytovatel změny](../../providers/provider-log.md).
Tady nejsou uvedené konce v nové vlastnosti představené z jednoho 3.0 ve verzi preview další 3.0 ve verzi Preview.

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Dotazy LINQ se už nevyhodnocuje na straně klienta

[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zjistit problém #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Než 3.0 když EF Core nelze převést výraz, který byl součástí sady dotazů SQL nebo parametr, automaticky vyhodnotí výraz na straně klienta.
Ve výchozím nastavení hodnocení klientů potencionálně nákladným výrazů aktivuje pouze upozornění.

**Nové chování**

Od verze 3.0, EF Core umožňuje pouze výrazy v projekci nejvyšší úrovně (poslední `Select()` volání v dotazu) k vyhodnocení na straně klienta.
Když výrazy v další části dotazu nelze převést na SQL nebo parametr, je vyvolána výjimka.

**Proč**

Vyhodnocení automatickou klientskou dotazů umožňuje mnoho dotazů, který se spustí i v případě, že je důležité části nelze přeložit.
Toto chování může způsobit neočekávané a potenciálně škodlivé chování, které může přestat pouze zřejmé v produkčním prostředí.
Například podmínku v `Where()` volání, které nelze přeložit může způsobit, že všechny řádky z tabulky přesunou z databázového serveru a filtr, který bude použit na straně klienta.
Tato situace může pokračovat nezjištěné po snadno Pokud tabulka obsahuje jenom pár řádků v vývoje, ale přístupů pevné přesun aplikace do produkčního prostředí, kde tabulka může obsahovat miliony řádků.
Upozornění vyhodnocení klienta dokázaly také příliš jednoduché ignorovat během vývoje.

Kromě toho může automatickou klientskou vyhodnocení způsobit problémy, ve kterých vylepšení překladu dotazu pro konkrétní výrazy způsobila neúmyslnému zásadní změny mezi verzí.

**Zmírnění rizik**

Pokud dotaz nelze přeložit plně, pak buď přepište dotaz ve formuláři, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`, nebo podobný explicitně přenést data zpět do klienta ve kterém pak může být dále zpracovány pomocí LINQ na objekty.

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core už nejsou součástí sdíleného rozhraní ASP.NET Core

[Oznámení týkající se sledování problému #325](https://github.com/aspnet/Announcements/issues/325)

Tato změna se používá v ASP.NET Core 3.0 ve verzi preview 1. 

**Staré chování**

Před ASP.NET Core 3.0, když se přidá odkaz na balíček pro `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`, měl by obsahovat EF Core a některé EF Core zprostředkovatele dat, jako jsou zprostředkovatele SQL Server.

**Nové chování**

Od verze 3.0, rozhraní ASP.NET Core sdílené neobsahuje EF Core nebo žádným zprostředkovatelům dat. EF Core.

**Proč**

Před touto změnou získávání EF Core vyžaduje jiný postup v závislosti na tom, jestli aplikace cílené ASP.NET Core a SQL Server, nebo ne. Upgrade ASP.NET Core také vynutit upgrade EF Core a zprostředkovatele SQL Server, který nemusí být vždy žádoucí.

Díky této změně se možnosti načítání EF Core je stejná ve všech zprostředkovatelů, podporovaná implementace .NET a typy aplikací.
Vývojáři také nyní mohou ovládat přesně při upgradu EF Core a EF Core zprostředkovatelé dat.

**Zmírnění rizik**

V aplikaci ASP.NET Core 3.0 nebo jakékoli jiné podporované aplikace použít EF Core, explicitně přidáte odkaz na balíček k poskytovateli databáze EF Core, který bude aplikace používat.

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>EF Core nástroji příkazového řádku dotnet ef, už nejsou součástí sady .NET Core SDK

[Sledování problému #14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4 a odpovídající verze sady .NET Core SDK.

**Staré chování**

Než 3.0 `dotnet ef` nástroj je zahrnutý v .NET Core SDK a se snadno k dispozici pro použití z příkazového řádku z libovolného projektu bez nutnosti pár kroků navíc. 

**Nové chování**

Od verze 3.0, sady .NET SDK se nenachází `dotnet ef` nástroj, takže než budete moct použít, budete mít explicitně ji nainstalovat jako nástroj pro místní nebo globální. 

**Proč**

Tato změna umožňuje distribuci a aktualizaci `dotnet ef` jako regulární nástroj rozhraní příkazového řádku .NET na webu NuGet, konzistentní s skutečnost, že je EF Core 3.0 také vždy distribuován jako balíček NuGet.

**Zmírnění rizik**

Abyste mohli spravovat migrace nebo vygenerované uživatelské rozhraní `DbContext`, nainstalovat `dotnet-ef` pomocí `dotnet tool install` příkazu.
Například můžete ji nainstalovat jako globální nástroj, zadejte následující příkaz:

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

Můžete také získat jeho místní nástroj při obnovování závislosti projektu, který deklaruje jako závislost pomocí nástroje [soubor manifestu nástroje](https://github.com/dotnet/cli/issues/10288).

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>Byla přejmenovaná FromSql ExecuteSql a ExecuteSqlAsync

[Sledování problému #10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0 byly tyto názvy metod přetížení pro práci s normální řetězec nebo řetězec, který by měl být interpolovaných do SQL a parametry.

**Nové chování**

Od verze EF Core 3.0, použijte `FromSqlRaw`, `ExecuteSqlRaw`, a `ExecuteSqlRawAsync` vytvořit parametrický dotaz, kde parametry jsou předány samostatně z řetězce dotazu.
Příklad:

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Použití `FromSqlInterpolated`, `ExecuteSqlInterpolated`, a `ExecuteSqlInterpolatedAsync` vytvořit parametrický dotaz, kde parametry jsou předány jako součást dotazu interpolované řetězce.
Příklad:

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Všimněte si, že oba dotazy výše uvedené vytvoří stejný parametrizovaného dotazu SQL se stejnými parametry SQL.

**Proč**

Přetížení metody, jako je to velmi usnadňují omylem volat metodu nezpracovaného řetězce, pokud bylo záměrem pro volání metody interpolovaný řetězec a naopak.
To může vést k dotazům naprostou když měla být parametrizovány.

**Zmírnění rizik**

Přepněte na nové názvy metod.

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>FromSql metody lze zadat pouze na dotaz kořeny

[Sledování problému #15704](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

Tato změna je zavedená v EF Core 3.0-preview 6.

**Staré chování**

Před EF Core 3.0 `FromSql` může být zadaná metoda kdekoli v dotazu.

**Nové chování**

Od verze EF Core 3.0, nový `FromSqlRaw` a `FromSqlInterpolated` metody (které nahradit `FromSql`) lze zadat pouze na dotaz kořeny, to znamená přímo na `DbSet<>`. Pokus zadat je někde jinde způsobí chybu kompilace.

**Proč**

Určení `FromSql` kdekoli jiné než na `DbSet` bez přidání význam nebo přidanou hodnotu a v některých případech může způsobit nejednoznačnost.

**Zmírnění rizik**

`FromSql` volání by měl být přímo na přesunout `DbSet` na které se vztahují.

## <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~Provádění dotazu se protokoluje při ladění na úrovni~~ obnoveny

[Sledování problému #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Tato změna se vrátí zpět v EF Core 3.0 – ve verzi preview 7.

Jsme vrátit tuto změnu, protože nové konfigurace v EF Core 3.0 umožňuje úroveň protokolování pro události, které chcete být určená aplikací. Například chcete-li přepnout protokolování SQL `Debug`, explicitně nakonfigurovat na úrovni `OnConfiguring` nebo `AddDbContext`:
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Dočasné hodnoty klíče už nejsou nastavené na instancí entit

[Sledování problému #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

Tato změna je zavedená v EF Core 3.0 – náhled 2.

**Staré chování**

Před EF Core 3.0 dočasné hodnoty přiřadily se všechny vlastnosti klíče, které by později skutečné hodnoty generován databází.
Obvykle tyto dočasné hodnoty byly velké záporná čísla.

**Nové chování**

Od verze 3.0, EF Core ukládá dočasné hodnotu klíče jako součást informací o sledování entity a ponechá samotné beze změny vlastnosti klíče.

**Proč**

Tato změna byla provedena zabránit chybně stávají trvalé při entita, která byla dříve sledován pomocí funkce některé dočasné hodnoty klíče `DbContext` instance se přesune na jiný `DbContext` instance. 

**Zmírnění rizik**

Na staré chování může záviset aplikace, které přiřazují hodnoty primárního klíče na cizí klíče formuláře přidružení mezi entitami, pokud primární klíče generované úložištěm a patří do entity v `Added` stavu.
Toho se lze vyvarovat podle:
* Bez použití klíče generované úložištěm.
* Nastavení vlastnosti navigace na vztahů namísto nastavování hodnot cizího klíče.
* Získejte skutečné hodnoty dočasné klíče z informací o sledování entity.
Například `context.Entry(blog).Property(e => e.Id).CurrentValue` i v případě vrátí hodnotu dočasné `blog.Id` samotný nebyla nastavena.

## <a name="detectchanges-honors-store-generated-key-values"></a>Metoda DetectChanges respektuje klíčových hodnot generovaných úložištěm

[Sledování problému #14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0 Nesledované entity objevila `DetectChanges` mají sledovat v `Added` stavu a vložené jako nový řádek, kdy `SaveChanges` je volána.

**Nové chování**

Od verze EF Core 3.0, pokud používá entity generované hodnoty klíče a některá z hodnot klíče je nastavena, pak entity se bude sledovat v `Modified` stavu.
To znamená, že řádek entity se předpokládá, že existují a budou aktualizace `SaveChanges` je volána.
Pokud není nastavena hodnota klíče, nebo pokud není typ entity pomocí vygenerované klíče, pak nová entita se bude dál sledovat jako `Added` stejně jako v předchozích verzích.

**Proč**

Tato změna byla provedena na umožňují snadněji a konzistentnější pro práci s grafy odpojené entity při použití klíče generované úložištěm.

**Zmírnění rizik**

Tato změna může přerušit aplikace, pokud typ entity je nakonfigurovaný na použití vygenerovat klíče, ale hodnoty klíče jsou explicitně nastavené pro nové instance.
Oprava je explicitně konfigurovat klíčové vlastnosti nejsou generovány pomocí hodnoty.
Například pomocí rozhraní fluent API:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Nebo s anotacemi dat:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a>Kaskádové odstranění teď dojde okamžitě ve výchozím nastavení

[Sledování problému #10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Než 3.0 EF Core použít kaskádové akce (odstranění závislých položek při odstranění požadovaný instanční objekt nebo když porušeno vztah k požadované instanční objekt) nebyly provedeny dokud byla volána metoda SaveChanges.

**Nové chování**

Od verze 3.0, EF Core provede kaskádové akce ihned po zjištění spouštěcí podmínky.
Například volání `context.Remove()` odstranit instančního objektu entity se výsledek ve všech sledovat související požadované položky závislé na také nastavena na `Deleted` okamžitě.

**Proč**

Tato změna byla provedena na zlepšení uživatelského rozhraní pro datových vazbách a auditování scénáře, kdy je potřeba vysvětlit entit, které se odstraní _před_ `SaveChanges` je volána.

**Zmírnění rizik**

Předchozí chování lze obnovit nastavením na `context.ChangedTracker`.
Příklad:

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior.Restrict má sémantiku čisticího modulu

[Sledování problému #12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 5.

**Staré chování**

Než 3.0 `DeleteBehavior.Restrict` vytvořit cizí klíče v databázi s `Restrict` sémantiku, ale také změněné interní opravy není zřejmé způsobem.

**Nové chování**

Od verze 3.0, `DeleteBehavior.Restrict` zajistí, že cizí klíče jsou vytvořeny pomocí `Restrict` sémantiku – tedy žádná cascades; throw na porušení omezení – bez vliv i na EF interní opravy.

**Proč**

Tato změna byla provedena na zlepšení uživatelského rozhraní pro použití `DeleteBehavior` intuitivní způsobem, bez neočekávané vedlejší účinky.

**Zmírnění rizik**

Předchozí chování můžete obnovit pomocí `DeleteBehavior.ClientNoAction`.

## <a name="query-types-are-consolidated-with-entity-types"></a>Typy dotazů jsou spojeny s typy entit

[Sledování problému #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0 [typy dotazů](xref:core/modeling/query-types) byly prostředky provádět dotazy na data, která nedefinuje primární klíč strukturovaným způsobem.
To znamená typ dotazu byla použita pro mapování typů entit bez klíčů (pravděpodobně ze zobrazení, ale pravděpodobně z tabulky) při pravidelných entity typ byl použit při klíč byl k dispozici (více pravděpodobně z tabulky, ale pravděpodobně ze zobrazení).

**Nové chování**

Typ dotazu teď bude pouze typ entity bez primárního klíče.
Typy entit bez kódu mají stejné funkce jako typy dotazů v předchozích verzích.

**Proč**

Tato změna byla provedena, abyste snížili nejasnosti kolem účel typy dotazů.
Konkrétně jsou typy bez klíčů entit a z tohoto důvodu jsou ze své podstaty jen pro čtení, ale by neměl být použit pouze z důvodu typu entity musí být jen pro čtení.
Podobně jsou často namapované na zobrazení, ale je to jenom, protože zobrazení není často definují klíče.

**Zmírnění rizik**

Následující části rozhraní API jsou teď zastaralé:
* **`ModelBuilder.Query<>()`** – Místo toho `ModelBuilder.Entity<>().HasNoKey()` musí být volána k označení typu entity tak, že má žádné klíče.
To by stále probíhá konfigurace podle konvence se vyhnete chybné konfigurace, když primární klíč se očekává, ale neodpovídá konvenci.
* **`DbQuery<>`** – Místo toho `DbSet<>` by měla sloužit.
* **`DbContext.Query<>()`** – Místo toho `DbContext.Set<>()` by měla sloužit.

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a>Došlo ke změně konfigurace rozhraní API pro vlastní typ relace

[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sledování problému #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0 byla provedena konfigurace vlastněné vztah přímo po `OwnsOne` nebo `OwnsMany` volání. 

**Nové chování**

Od verze EF Core 3.0, že už fluent API pro konfiguraci vlastnosti navigace vlastníkovi použitím `WithOwner()`.
Příklad:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

Konfigurace související se o vztah mezi vlastníka a vlastní by měl být zřetězené teď po `WithOwner()` podobně jako na konfiguraci jiné vztahy.
Při konfiguraci pro vlastní typ, samotný by stále možné zřetězit po `OwnsOne()/OwnsMany()`.
Příklad:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

Kromě volá `Entity()`, `HasOne()`, nebo `Set()` s typem vlastnictví cíl se vyvolají výjimku.

**Proč**

Tato změna byla provedena vytvořit jasnější oddělení mezi konfigurací samotného typu vlastnictví a _vztah k_ typ vlastnictví.
Tím zase nejednoznačnosti a záměny kolem metody, jako je `HasForeignKey`.

**Zmírnění rizik**

Změna konfigurace vlastněné typ vztahů používat nové plochy rozhraní API, jak je znázorněno v příkladu výše.

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Závislých položek sdílení v tabulce k objektu zabezpečení jsou teď nepovinné.

[Sledování problému #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Vezměte v úvahu následující model:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku a `OrderDetails` instance byla požadována vždy při přidání nového `Order`.


**Nové chování**

Od verze 3.0, EF Core umožňuje přidat `Order` bez `OrderDetails` a mapuje všechny `OrderDetails` vlastnosti s výjimkou primární klíč pro sloupce s možnou hodnotou Null.
Při dotazování na EF Core sady `OrderDetails` k `null` Pokud některou z jejích požadovaných vlastností nemá žádnou hodnotu nebo nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.

**Zmírnění rizik**

Pokud váš model obsahuje tabulku sdílení závislé se všemi sloupci volitelné, ale navigace ukazatel není má být `null` pak aplikace by měla upravit tak, aby zpracovat případy při navigaci `null`. Pokud to není možné požadovanou vlastnost měla být přidána do typu entity nebo musí mít alespoň jednu vlastnost non -`null` přiřazena hodnota.

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Všechny entity sdílení tabulku se sloupcem tokenů souběžnosti mít mapování na vlastnost

[Sledování problému #14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Vezměte v úvahu následující model:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku následně aktualizovat jenom `OrderDetails` neaktualizuje `Version` hodnoty na klientovi a příští aktualizace se nezdaří.


**Nové chování**

Od verze 3.0, rozšíří EF Core nové `Version` hodnota, která se `Order` Pokud vlastní `OrderDetails`. V opačném případě dojde k výjimce během ověření modelu.

**Proč**

Tato změna byla provedena, když se aktualizuje jenom jeden z entity, které jsou namapované na stejnou tabulku, aby hodnota tokenu zastaralé souběžnosti.

**Zmírnění rizik**

Všechny entity v tabulce pro sdílení obsahu se mají zahrnout vlastnost, která je namapovaná na sloupci tokenů souběžnosti. Je možné vytvořit, jeden v stínové stavu:
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Vlastnosti zděděné nenamapované typy jsou nyní mapovány do jednoho sloupce pro všechny odvozené typy

[Sledování problému #13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Vezměte v úvahu následující model:
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

Před EF Core 3.0 `ShippingAddress` vlastnost by být mapována k oddělení sloupců pro `BulkOrder` a `Order` ve výchozím nastavení.

**Nové chování**

Od verze 3.0, EF Core vytvoří pouze jeden sloupec pro `ShippingAddress`.

**Proč**

Staré chování nebyl očekáván.

**Zmírnění rizik**

Vlastnost je stále explicitně mapovat pro oddělení sloupců v odvozených typů:

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Konvence vlastnost cizího klíče už neodpovídá stejný název jako vlastnost instančního objektu

[Sledování problému #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Vezměte v úvahu následující model:
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
Před EF Core 3.0 `CustomerId` vlastnost se použije pro cizí klíč konvencí.
Nicméně pokud `Order` je typ vlastnictví, a potom to by také provést `CustomerId` primární klíč a to se většinou očekává.

**Nové chování**

Od verze 3.0, EF Core nesnaží při využívání vlastností cizího klíče podle konvence, pokud mají stejný název jako vlastnost instančního objektu.
Název instančního objektu typu zřetězený s názvem hlavní vlastnosti a název navigační zřetězená s vzory názvů vlastnosti principal budou stále odpovídat.
Příklad:

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Proč**

Tato změna byla provedena na chybně nedefinujte vlastnost primárního klíče typ vlastnictví.

**Zmírnění rizik**

Pokud byla vlastnost má být cizí klíč a proto část primárního klíče a pak explicitně jako takový ho nakonfigurujte.

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>Připojení k databázi je nyní uzavřeno, pokud není využito už před objektu TransactionScope byla dokončena.

[Sledování problému #14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0, pokud kontext otevře připojení k uvnitř `TransactionScope`, připojení zůstane otevřená, při aktuální `TransactionScope` je aktivní.

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

**Nové chování**

Od verze 3.0, EF Core uzavře připojení co nejdříve po dokončení jeho použití.

**Proč**

Tato změna umožňuje použití několika kontextech v rámci stejného `TransactionScope`. Nové chování také odpovídající EF6.

**Zmírnění rizik**

Pokud je nutné připojení zůstat otevřené explicitní volání konstruktoru `OpenConnection()` zajistí, že EF Core nezavírá je předčasně ukončena:

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Jednotlivé vlastnosti pomocí generování klíčů nezávislé celé číslo v paměti

[Sledování problému #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0 se použil jeden generátor sdíleného hodnot pro všechny vlastnosti klíče celé číslo v paměti.

**Nové chování**

Od verze EF Core 3.0, každé celé číslo klíčová vlastnost získá vlastní generátor hodnot při použití databáze v paměti.
Také pokud se odstraní databáze, pak generování klíčů je obnovit pro všechny tabulky.

**Proč**

Tato změna byla provedena zarovnat generování klíče v paměti blíže k generování klíčů skutečná databáze a zlepšit schopnost izolace testů od sebe navzájem při použití databáze v paměti.

**Zmírnění rizik**

Toto může rozbít aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti nastavit.
Zvažte místo toho spoléhat na konkrétní hodnoty klíče, ani aktualizaci tak, aby odpovídala nové chování.

## <a name="backing-fields-are-used-by-default"></a>Základní pole se používají ve výchozím nastavení

[Sledování problému #12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

Tato změna je zavedená v EF Core 3.0 – náhled 2.

**Staré chování**

Než 3.0 i v případě, že pomocné pole vlastnosti zná, EF Core by stále ve výchozím nastavení čtení a zápisu hodnoty vlastnosti pomocí metody getter a setter vlastnosti.
K tomuto výjimka provádění dotazů, kde pomocné pole se nastavuje přímo Pokud jsou známé.

**Nové chování**

Počínaje EF Core 3.0, pokud jsou známé vlastnosti pole zálohování, potom EF Core budou vždy číst a Zapisovat vlastnosti pomocí pole zálohování.
Pokud aplikace se spoléhá na další chování zakódovaný do metody getter nebo setter to může způsobit přerušení aplikace.

**Proč**

Tato změna byla provedena zabránit EF Core chybně aktivací obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnující entity.

**Zmírnění rizik**

Pre-3.0 chování lze obnovit prostřednictvím konfigurace režim přístupu k vlastnosti na `ModelBuilder`.
Příklad:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Vyvolání výjimky, pokud je nalezeno více polí kompatibilní zálohování

[Sledování problému #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0 Pokud odpovídá více polí pravidel pro vyhledání pomocným polem vlastnosti, pak vybere jedno pole na základě některých pořadí priority.
To může způsobit nesprávné pole pro použití v případech, nejednoznačný.

**Nové chování**

Od verze EF Core 3.0, pokud více polí budou odpovídat na stejnou vlastnost, pak je vyvolána výjimka.

**Proč**

Tato změna byla provedena, abyste se vyhnuli použití tiše jedno pole před jiným při může obsahovat jen jedno správné.

**Zmírnění rizik**

Vlastnosti s nejednoznačný základní pole musí mít pole pro použití explicitně zadán.
Například pomocí rozhraní fluent API:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a>Vlastnost jen pro pole názvů by měl odpovídat názvu pole

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0 vlastnost může být určeno hodnotu řetězce a pokud nebyla nalezena žádná vlastnost s tímto názvem na typu CLR EF Core by opakujte tak, aby odpovídaly na pole pomocí pravidel pro vytváření názvů.
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Nové chování**

Od verze EF Core 3.0, vlastnost jen pro pole musí odpovídat názvu pole přesně.

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Proč**

Tato změna byla provedena, abyste se vyhnuli použití stejné pole pro dvě vlastnosti s názvem podobně, také udržuje pravidla pro porovnávání vlastností pouze pole je stejný jako u vlastnosti, které jsou namapovány na vlastnosti CLR.

**Zmírnění rizik**

Vlastnosti pouze pro pole musí mít název jako pole, které jsou mapovány na stejný.
V novější verzi preview EF Core 3.0 Plánujeme znovu povolit explicitně konfigurace název pole, které se liší od názvu vlastnosti:

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool zavolejte už AddLogging a AddMemoryCache

[Sledování problému #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0, volání `AddDbContext` nebo `AddDbContextPool` by také zaregistrovat protokolování a ukládání služeb s D.I prostřednictvím volání do mezipaměti [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Nové chování**

Od verze EF Core 3.0, `AddDbContext` a `AddDbContextPool` nebude registru tyto služby s DI (Dependency Injection).

**Proč**

EF Core 3.0 nevyžaduje, že tyto služby jsou v kontejneru DI vaší aplikace. Nicméně pokud `ILoggerFactory` je zaregistrovaný v aplikačním kontejneru DI, pak bude i nadále využívat EF Core.

**Zmírnění rizik**

Pokud vaše aplikace potřebuje tyto služby, registrujte je explicitně s využitím kontejnerů DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry teď provádí místní metoda DetectChanges

[Sledování problému #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0, volání `DbContext.Entry` způsobí změny, aby se rozpoznal pro všechny sledované entity.
Tím zajistíte, že stav zpřístupněn v `EntityEntry` byla aktuální.

**Nové chování**

Spouští se s EF Core 3.0, volání `DbContext.Entry` se teď jenom pokus o zjištění změn v dané entitě a všechny sledované instančního objektu entity, které s ním souvisejí.
To znamená, že se změní jinde nemusí byl zjištěn zavoláním této metody, které by mohly mít vliv na stav aplikace.

Všimněte si, že pokud `ChangeTracker.AutoDetectChangesEnabled` je nastavena na `false` pak i tato detekce místní změny se deaktivuje.

Jiné metody, které způsobují detekce změn – například `ChangeTracker.Entries` a `SaveChanges`– způsobí úplné `DetectChanges` všech sledovat entity.

**Proč**

Tato změna byla provedena pro zlepšení výkonu výchozí použití `context.Entry`.

**Zmírnění rizik**

Volání `ChgangeTracker.DetectChanges()` explicitně před voláním `Entry` k zajištění chování pre-3.0.

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Řetězec a bajtové pole klíčů se nerozlišují klientem generovaná ve výchozím nastavení

[Sledování problému #14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0 `string` a `byte[]` klíčové vlastnosti použití explicitním nastavením nenulová hodnota.
V takovém případě by se vygenerovala hodnota klíče na klientovi jako identifikátor GUID serializovat do bajtů pro `byte[]`.

**Nové chování**

Od verze EF Core 3.0 výjimka bude vyvolána označující, že nebyla nastavena žádná hodnota klíče.

**Proč**

Tato změna byla provedena, protože klient generován `string` / `byte[]` hodnoty nejsou obecně užitečné a výchozí chování dostal pevné argumentovat o generované hodnoty klíče v běžným způsobem.

**Zmírnění rizik**

Chování pre-3.0 je možné získat tak, že explicitně zadáte, by měl klíčové vlastnosti použít generované hodnoty, pokud je nastavena žádná hodnota jiná než null.
Například pomocí rozhraní fluent API:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Nebo s anotacemi dat:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a>Implementaci třídy ILoggerFactory je nyní vymezené služby

[Sledování problému #14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0 `ILoggerFactory` bylo zaregistrováno jako služba typu singleton.

**Nové chování**

Od verze EF Core 3.0, `ILoggerFactory` je teď zaregistrovaný jako obor.

**Proč**

Tato změna byla provedena umožňující přidružení protokolovací nástroj se `DbContext` instanci, která povoluje další funkce a odebírá někdy patologických chování, jako je například obrovské množství poskytovatelů vnitřní chybě služby.

**Zmírnění rizik**

Tato změna by neměla mít vliv kód aplikace, pokud není registrace a použití vlastních služeb v poskytovateli EF Core vnitřní chybě služby.
Tato akce není běžné.
V těchto případech většinu toho, co bude dál fungovat, ale žádné služby typu singleton, která byla v závislosti na `ILoggerFactory` bude nutné změnit tak, aby získat `ILoggerFactory` jiným způsobem.

Pokud narazíte na situace tímto způsobem, založte prosím problém na na [sledování problémů Githubu EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) a dejte nám vědět, jak používáte `ILoggerFactory` tak, aby nám můžete lépe porozumět nechcete v budoucnu znovu rozdělit.

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Opožděné načtení proxy už předpokládají, že jsou plně načteny navigační vlastnosti

[Sledování problému #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0, jednou `DbContext` byl odstraněn neexistoval způsob, jak zjistit, zda danou navigační vlastnost s entitou získané z daného kontextu byl plně načten či nikoli.
Proxy by místo toho se předpokládá, že je načtena navigační odkaz, pokud má nenulovou hodnotu a, že je načtena navigace kolekce, pokud není prázdná.
V těchto případech by pokus o opožděné načtení no-op.

**Nové chování**

Od verze EF Core 3.0, proxy servery sledovat, jestli je načtena navigační vlastnost.
To znamená, že při pokusu o přístup k navigační vlastnost, která je načtena po kontext se vyřadil. bude vždycky no-op, i v případě, že je načteno navigace je prázdný nebo mít hodnotu null.
Naopak pokusu o přístup k navigační vlastnosti, které není načteno vyvolají výjimku pokud uvolnění kontextu, i když je navigační vlastnost kolekce není prázdná.
Pokud k této situaci dochází, znamená to kód aplikace se pokouší použít opožděné načtení na neplatný čas a aplikace by měla být změněna na tuto akci.

**Proč**

Tato změna byla provedena, aby chování konzistentní a správné při pokusu o opožděné načtení na vyřazený `DbContext` instance.

**Zmírnění rizik**

Kód aplikace, který nebude pokoušet opožděné načtení vyřazený kontextu, nebo nakonfigurovat to být no-op. jak je popsáno v zpráva o výjimce.

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>Nadměrné vytváření zprostředkovatelů vnitřní chybě služby je teď k chybě ve výchozím nastavení

[Sledování problému #10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Upozornění by před EF Core 3.0 zaznamenávané aplikace vytváří patologických několik poskytovatelů vnitřní chybě služby.

**Nové chování**

Od verze EF Core 3.0, toto upozornění se teď považuje za a chyby a výjimky je vyvolána výjimka. 

**Proč**

Tato změna byla provedena na připravovat lepší kód aplikace prostřednictvím vystavení takovém patologických více explicitně.

**Zmírnění rizik**

Nejvhodnější příčinu akce při výskytu této chyby je zjistěte původní příčinu a nevytvářet mnoho poskytovatelů vnitřní chybě služby.
Však chyba může být převést zpět na upozornění (nebo ignorovat) prostřednictvím konfigurace na `DbContextOptionsBuilder`.
Příklad:

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Nové chování pro HasOne/HasMany volat jeden řetězec

[Sledování problému #9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0 kód volání `HasOne` nebo `HasMany` s jeden řetězec byl interpretován tak matoucí.
Příklad:
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Kód vypadá je týkající se `Samurai` některé jiné entity typu použití `Entrance` navigační vlastnost, která může být privátní.

Ve skutečnosti, tento kód se pokouší vytvořit relaci některé typ entity s názvem `Entrance` se žádné navigační vlastnost.

**Nové chování**

Od verze EF Core 3.0, výše uvedený kód nyní dělá co podívali jako její by měl mít byla činnosti před.

**Proč**

Staré chování bylo velmi matoucí, zejména při čtení konfigurace kód a hledání chyb.

**Zmírnění rizik**

Tímto přerušíte pouze aplikace, které jsou explicitně konfigurace relace používání řetězců pro názvy typů a bez explicitním zadáním navigační vlastnost.
Není běžné.
Předchozí chování můžete získat prostřednictvím explicitně předávání `null` pro název navigační vlastnosti.
Příklad:

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Návratový typ pro několik asynchronních metod se změnil z úlohy na ValueTask

[Sledování problému #15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Dříve vrátila následující asynchronní metody `Task<T>`:

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` (a odvozených tříd)

**Nové chování**

Výše uvedené metody nyní vrací `ValueTask<T>` přes stejné `T` stejně jako předtím.

**Proč**

Tato změna snižuje počet přidělení haldy vzniklé při volání těchto metod, zlepšení obecné informace o výkonu.

**Zmírnění rizik**

Aplikace jednoduše čeká na výše uvedené rozhraní API pouze muset být překompilovány - žádné změny zdroje jsou nezbytné.
Složitější využití (například předání vráceného `Task` k `Task.WhenAny()`) obvykle vyžadují, aby vráceného `ValueTask<T>` převedou na hodnoty `Task<T>` voláním `AsTask()` na něm.
Všimněte si, že to Neguje snížení přidělení, které tato změna přináší.

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>Poznámka relační: TypeMapping je teď stejně TypeMapping

[Sledování problému #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

Tato změna je zavedená v EF Core 3.0 – náhled 2.

**Staré chování**

Název poznámky pro mapování anotace typu byl "Relační: TypeMapping".

**Nové chování**

Název poznámky pro mapování anotace typu je nyní "TypeMapping".

**Proč**

Mapování typu jsou teď používá pro více než jen relační databáze poskytovatele.

**Zmírnění rizik**

Tímto přerušíte jenom aplikace s přístupem k mapování typů přímo jako poznámka, která není běžné.
Nejvhodnější opatření na opravu se má používat rovinu rozhraní API pro mapování typů přístupu spíše než přímo pomocí anotace.

## <a name="totable-on-a-derived-type-throws-an-exception"></a>Vyvolá výjimku, ToTable na odvozený typ. 

[Sledování problému #11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0 `ToTable()` volalo odvozený typ bude ignorovat, protože pouze dědičnosti mapování strategie byl TPH, kdy to není platný. 

**Nové chování**

Spouští se s EF Core 3.0 a při přípravě na přidání TPT a TPC podporují v pozdější verzi `ToTable()` volalo odvozený typ bude nyní vyvolání výjimky v budoucnu vyhnuli o změnu neočekávané mapování.

**Proč**

Aktuálně není platná pro mapování odvozeného typu na jiné tabulky.
Tato změna se vyhnete přerušení v budoucnu, kdy bude platná věc udělat.

**Zmírnění rizik**

Odeberte všechny pokusy o mapování odvozené typy s jinými tabulkami.

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a>Nahradí HasIndex ForSqlServerHasIndex 

[Sledování problému #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytuje způsob, jak konfigurovat sloupců použitých s `INCLUDE`.

**Nové chování**

Od verze EF Core 3.0, pomocí `Include` na indexu se teď podporuje relační úrovni.
Použití `HasIndex().ForSqlServerInclude()`.

**Proč**

Tato změna byla provedena konsolidovat rozhraní API pro indexy s `Include` do jednoho umístění pro všechny databáze poskytovatele.

**Zmírnění rizik**

Pomocí nového rozhraní API, jak je znázorněno výše.

## <a name="metadata-api-changes"></a>Změn metadat rozhraní API

[Sledování problému #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Nové chování**

Následující vlastnosti byly převedeny na rozšiřující metody:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Proč**

Tato změna usnadňuje provádění výše uvedených rozhraní.

**Zmírnění rizik**

Pomocí nové metody rozšíření.

## <a name="provider-specific-metadata-api-changes"></a>Změny specifickým pro zprostředkovatele metadat rozhraní API

[Sledování problému #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

Tato změna je zavedená v EF Core 3.0-preview 6.

**Nové chování**

Metody rozšíření specifické pro zprostředkovatele bude zjednodušen navýšení kapacity:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

**Proč**

Tato změna usnadňuje provádění metody výše uvedená rozšíření.

**Zmírnění rizik**

Pomocí nové metody rozšíření.

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core už odešle – Direktiva pragma pro vynucení SQLite FK

[Sledování problému #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0, bude posílat EF Core `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.

**Nové chování**

Od verze EF Core 3.0, už EF Core odešle `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.

**Proč**

Tato změna byla provedena, protože používá EF Core `SQLitePCLRaw.bundle_e_sqlite3` ve výchozím nastavení, která zase znamená, že vynucení cizího klíče je ve výchozím nastavení zapnuté a není potřeba explicitně povolit pokaždé, když je otevřeno připojení.

**Zmírnění rizik**

Cizí klíče jsou povolena ve výchozím nastavení SQLitePCLRaw.bundle_e_sqlite3, který se používá ve výchozím nastavení pro jádro EF Core.
Pro jiných případech může být povoleno cizích klíčů tak, že zadáte `Foreign Keys=True` v připojovacím řetězci.

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3

**Staré chování**

Před EF Core 3.0 používá EF Core `SQLitePCLRaw.bundle_green`.

**Nové chování**

Od verze EF Core 3.0, pomocí EF Core `SQLitePCLRaw.bundle_e_sqlite3`.

**Proč**

Tato změna byla provedena tak, aby používalo verzi SQLite v Iosu konzistentní s jinými platformami.

**Zmírnění rizik**

Pokud chcete použít nativní verzi SQLite v Iosu, nakonfigurovat `Microsoft.Data.Sqlite` použít jinou `SQLitePCLRaw` sady.

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT na SQLite

[Sledování problému #15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Identifikátor GUID hodnoty byly dříve sored jako hodnoty objektu BLOB na SQLite.

**Nové chování**

Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT.

**Proč**

Binární formát GUID není standardizované. Uložení hodnot jako TEXT díky databáze více kompatibilní s jinými technologiemi.

**Zmírnění rizik**

Spuštěním SQL takto můžete migrovat existující databáze na nový formát.

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

V EF Core můžete také pokračovat pomocí předchozí chování nakonfigurováním převaděč hodnoty na tyto vlastnosti.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft.Data.Sqlite zůstává schopný načíst hodnoty identifikátoru Guid z objektu BLOB a TEXTOVÉHO sloupce; ale vzhledem k tomu, že došlo ke změně výchozího formátu pro parametry a konstant bude pravděpodobně potřeba provést akci pro většinu scénářů zahrnující identifikátory GUID.

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Hodnoty char jsou nyní uloženy jako TEXT na SQLite

[Sledování problému #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Hodnoty char byly dříve sored jako CELOČÍSELNÉ hodnoty na SQLite. Například znak hodnotu *A* byl uložen jako celočíselnou hodnotu 65.

**Nové chování**

Hodnoty char jsou nyní uloženy jako TEXT.

**Proč**

Uložení hodnot jako TEXT je přirozenější a vytvoří databáze více kompatibilní s jinými technologiemi.

**Zmírnění rizik**

Spuštěním SQL takto můžete migrovat existující databáze na nový formát.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

V EF Core můžete také pokračovat pomocí předchozí chování nakonfigurováním převaděč hodnoty na tyto vlastnosti.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft.Data.Sqlite také zbývá schopný načíst znakových hodnot z celé číslo a TEXT sloupců, takže určitých scénářích nevyžadují žádnou akci.

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>ID migrace jsou generovány pomocí neutrální jazykové verze kalendáře

[Sledování problému #12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

ID migrace neúmyslně byly vygenerovány pomocí kalendář aktuální jazykové verze.

**Nové chování**

ID migrace jsou teď vždy generovány pomocí neutrální jazykové verze kalendáře (gregoriánského).

**Proč**

Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při sloučení. Pomocí neutrální kalendáře se vyhnete řazení problémy, které můžou být výsledkem členy týmu s jiným kalendáře.

**Zmírnění rizik**

Tato změna ovlivní těm, kdo používají jiných gregoriánském kalendáři, kde rok je větší než gregoriánském kalendáři (např. thajský buddhistický kalendář). Migrace stávající ID bude potřeba aktualizovat tak, aby nové migrace jsou řazeny za stávající migrace.

ID migrace najdete v atributu migrace soubory návrháře migrace.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Tabulky historie migrace je také potřeba aktualizovat.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>Informace o rozšíření nebo metadata byla odebrána z IDbContextOptionsExtension

[Sledování problému #16119](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 7.

**Staré chování**

`IDbContextOptionsExtension` obsahuje metody pro získání metadat o rozšíření.

**Nové chování**

Tyto metody byly přesunuty do nové `DbContextOptionsExtensionInfo` abstraktní základní třída, která je vrácena z nového `IDbContextOptionsExtension.Info` vlastnost.

**Proč**

Nad verzí z verze 2.0, 3.0, potřebujeme přidat nebo změnit tyto metody několikrát.
Do nové abstraktní základní třída je zásadní navýšení kapacity vám usnadní vytvořit tento druh změny bez narušení existující rozšíření.

**Zmírnění rizik**

Aktualizujte rozšíření mají nový tvar.
Příklady jsou součástí různými implementacemi tohoto `IDbContextOptionsExtension` pro různé typy rozšíření v EF Core zdrojový kód.

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.

[Sledování problému #10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Změna**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` byl přejmenován na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.

**Proč**

Zarovná pojmenování Tato událost upozornění s jinými událostmi upozornění.

**Zmírnění rizik**

Použití nového názvu. (Všimněte si, že nedošlo ke změně číslo ID události.)

## <a name="clarify-api-for-foreign-key-constraint-names"></a>Vysvětlení rozhraní API pro názvy omezení pro cizí klíč

[Sledování problému #10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.

**Staré chování**

Před EF Core 3.0 omezení pro cizí klíč názvy označovaly jako jednoduše "name". Příklad:

```C#
var constraintName = myForeignKey.Name;
```

**Nové chování**

Od verze EF Core 3.0, omezení pro cizí klíč názvy jsou dnes označovány jako "název omezení". Příklad:

```C#
var constraintName = myForeignKey.ConstraintName;
```

**Proč**

Tato změna přináší konzistenci pro názvy v této oblasti a také vysvětluje, že se jedná o název název cizího klíče omezení a nikoli na sloupec nebo vlastnost, která je definována cizího klíče na.

**Zmírnění rizik**

Použití nového názvu.

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>IRelationalDatabaseCreator.HasTables/HasTablesAsync byly provedeny veřejné

[Sledování problému #15997](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

Tato změna je zavedená v EF Core 3.0 – ve verzi preview 7.

**Staré chování**

Před EF Core 3.0 byly chráněné tyto metody.

```C#
var constraintName = myForeignKey.Name;
```

**Nové chování**

Od verze EF Core 3.0, tyto metody jsou veřejné.

**Proč**

Tyto metody jsou používány EF k určení, zda je databáze vytvořená, ale prázdný. To může také být užitečné z mimo EF při určování, jestli se mají použít migrace.

**Zmírnění rizik**

Změňte přístupnost nějaká přepsání.
