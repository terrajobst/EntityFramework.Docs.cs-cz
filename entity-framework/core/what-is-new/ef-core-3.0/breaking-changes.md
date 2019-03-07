---
title: Zásadní změny v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: a7e1a03bf1131cd53123f5cc39b07bed94619b44
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463392"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>Rozbíjející změny zahrnuté v EF Core 3.0 (aktuálně ve verzi preview)

> [!IMPORTANT]
> Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.

Následující změny chování a rozhraní API by mohly přerušit aplikace vyvinuté pro jádro EF Core 2.2.x při upgradu na 3.0.0.
Změny, které chceme ovlivní jen poskytovatelé databází jsou popsány v části [poskytovatel změny](../../providers/provider-log.md).
Tady nejsou uvedené konce v nové vlastnosti představené z jednoho 3.0 ve verzi preview další 3.0 ve verzi Preview.

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Dotazy LINQ se už nevyhodnocuje na straně klienta

[Sledování problému #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

> [!IMPORTANT]
> Oznamujeme předem toto přerušení.
Dosud nebyl dodáno ve všech 3.0 ve verzi preview.

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

Tato změna byla zavedena v ASP.NET Core 3.0 ve verzi preview 1. 

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

## <a name="query-execution-is-logged-at-debug-level"></a>Provádění dotazu se protokoluje při ladění na úrovni

[Sledování problému #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0, provádění dotazů a jiných příkazů protokolu byla zaznamenána v `Info` úroveň.

**Nové chování**

Od verze EF Core 3.0, protokolování spuštění příkazu/SQL je na `Debug` úroveň.

**Proč**

Tato změna byla provedena jak snížit šum na `Info` úrovně protokolování.

**Zmírnění rizik**

Tato událost protokolování je definována `RelationalEventId.CommandExecuting` s ID události 20100.
Do protokolu SQL na `Info` úroveň znovu, zapnout protokolování na `Debug` úrovně a vyfiltrovat pouze této události.


## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Dočasné hodnoty klíče už nejsou nastavené na instancí entit

[Sledování problému #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

Tato změna byla zavedená v EF Core 3.0 – náhled 2.

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

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

## <a name="query-types-are-consolidated-with-entity-types"></a>Typy dotazů jsou spojeny s typy entit

[Sledování problému #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Konvence vlastnost cizího klíče už neodpovídá stejný název jako vlastnost instančního objektu

[Sledování problému #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

Od verze 3.0, nebude zkoušet EF Core při využívání vlastností cizího klíče podle konvence, pokud mají stejný název jako vlastnost instančního objektu.
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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Jednotlivé vlastnosti pomocí generování klíčů nezávislé celé číslo v paměti

[Sledování problému #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.

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

Tato změna byla zavedená v EF Core 3.0 – náhled 2.

**Staré chování**

Než 3.0 i v případě, že pomocné pole vlastnosti zná, EF Core by stále ve výchozím nastavení čtení a zápisu hodnoty vlastnosti pomocí metody getter a setter vlastnosti.
K tomuto výjimka provádění dotazů, kde pomocné pole se nastavuje přímo Pokud jsou známé.

**Nové chování**

Počínaje EF Core 3.0, pokud je znám pomocným polem vlastnosti potom budou vždy číst a Zapisovat vlastnosti pomocí pole zálohování.
Pokud aplikace se spoléhá na další chování zakódovaný do metody getter nebo setter to může způsobit přerušení aplikace.

**Proč**

Tato změna byla provedena zabránit EF Core chybně aktivací obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnující entity.

**Zmírnění rizik**

Prostřednictvím konfigurace režim přístupu k vlastnosti v rozhraní API fluent modelBuilder lze obnovit chování pre-3.0.
Příklad:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Vyvolání výjimky, pokud je nalezeno více polí kompatibilní zálohování

[Sledování problému #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.

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

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry teď provádí místní metoda DetectChanges

[Sledování problému #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.

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

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a>Sloučí IDbContextOptionsExtension IDbContextOptionsExtensionWithDebugInfo

[Sledování problému #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

`IDbContextOptionsExtensionWithDebugInfo` Další volitelné rozhraní byla prodloužena z `IDbContextOptionsExtension` pro vyvarování rozbíjející změně rozhraní během cyklu vydání verze 2.x.

**Nové chování**

Rozhraní jsou nyní sloučeny do `IDbContextOptionsExtension`.

**Proč**

Tato změna byla provedena, protože rozhraní jsou koncepčně jednou.

**Zmírnění rizik**

Žádné implementace `IDbContextOptionsExtension` bude muset být aktualizované kvůli podpoře nového člena.

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Opožděné načtení proxy už předpokládají, že jsou plně načteny navigační vlastnosti

[Sledování problému #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.

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

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>Poznámka relační: TypeMapping je teď stejně TypeMapping

[Sledování problému #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

Tato změna byla zavedená v EF Core 3.0 – náhled 2.

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

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

**Staré chování**

Před EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytuje způsob, jak konfigurovat sloupců použitých s `INCLUDE`.

**Nové chování**

Od verze EF Core 3.0, pomocí `Include` na indexu se teď podporuje relační úrovni.
Použití `HasIndex().ForSqlServerInclude()`.

**Proč**

Tato změna byla provedena konsolidovat rozhraní API pro indexy s `Includes` do jednoho umístění pro všechny databáze poskytovatele.

**Zmírnění rizik**

Pomocí nového rozhraní API, jak je znázorněno výše.

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core už odešle – Direktiva pragma pro vynucení SQLite FK

[Sledování problému #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.

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
