---
title: Nové funkce v core entity frameworku 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebc676930ffc396aa70bb8afb91cf5a0cd43e04d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417942"
---
# <a name="new-features-in-entity-framework-core-30"></a>Nové funkce v jádru entity frameworku 3.0

Následující seznam obsahuje hlavní nové funkce v EF Core 3.0.

Jako hlavní verze EF Core 3.0 obsahuje také několik [narušující změny](xref:core/what-is-new/ef-core-3.0/breaking-changes), které jsou vylepšení rozhraní API, které mohou mít negativní dopad na stávající aplikace.  

## <a name="linq-overhaul"></a>Generální oprava LINQ

LINQ umožňuje psát databázové dotazy pomocí zvoleného jazyka .NET s využitím rozšířených informací o typu a nabízí kontrolu typu IntelliSense a kompilace.
Ale LINQ také umožňuje psát neomezený počet složitých dotazů obsahujících libovolné výrazy (volání metod nebo operace).
Jak zpracovat všechny tyto kombinace je hlavní výzvou pro poskytovatele LINQ.

V EF Core 3.0 jsme překontovali našeho poskytovatele LINQ, abychom umožnili překlad dalších vzorů dotazů do SQL, generování efektivních dotazů ve více případech a zabránění neefektivním dotazům, aby nezjistili. Nový poskytovatel LINQ je základem, nad kterým budeme moci nabídnout nové možnosti dotazu a vylepšení výkonu v budoucích verzích, aniž bychom porušili stávající aplikace a poskytovatele dat.

### <a name="restricted-client-evaluation"></a>Omezené vyhodnocení klienta

Nejdůležitější změna návrhu má co do činění s tím, jak zpracováváme výrazy LINQ, které nelze převést na parametry nebo přeložit do SQL.

V předchozích verzích EF Core identifikoval, jaké části dotazu by mohly být přeloženy do SQL a provedeny zbytek dotazu na straně klienta.
Tento typ spuštění na straně klienta je žádoucí v některých situacích, ale v mnoha jiných případech může mít za následek neefektivní dotazy.

Například pokud EF Core 2.2 nelze přeložit predikát ve `Where()` volání, provedl příkaz SQL bez filtru, převedl všechny řádky z databáze a pak je filtroval v paměti:

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

To může být přijatelné, pokud databáze obsahuje malý počet řádků, ale může mít za následek významné problémy s výkonem nebo dokonce selhání aplikace, pokud databáze obsahuje velký počet řádků.

V EF Core 3.0 jsme omezili hodnocení klientů, aby se stalo pouze v `Select()`projekci nejvyšší úrovně (v podstatě poslední volání).
Když EF Core 3.0 zjistí výrazy, které nelze přeložit nikde jinde v dotazu, vyvolá výjimku za běhu.

Chcete-li vyhodnotit podmínku predikátu na straně klienta jako v předchozím příkladu, vývojáři nyní musí explicitně přepnout vyhodnocení dotazu na LINQ na objekty:

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

Další [podrobnosti](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) o tom, jak to může ovlivnit stávající aplikace, naleznete v dokumentaci k nejnovějším změnám.

### <a name="single-sql-statement-per-linq-query"></a>Jeden příkaz SQL na dotaz LINQ

Dalším aspektem návrhu, který se výrazně změnil v 3.0 je, že nyní vždy generovat jeden příkaz SQL na linq dotazu. V předchozích verzích jsme v určitých případech generovali více příkazů SQL, překládali `Include()` volání navigačních vlastností kolekce a překládali dotazy, které sledovaly určité vzory s poddotazy. I když to bylo v `Include()` některých případech pohodlné a dokonce pomohlo vyhnout se odesílání nadbytečných dat po drátě, implementace byla složitá a vyústila v některé extrémně neefektivní chování (dotazy N + 1). Byly situace, ve kterých data vrácená přes více dotazů byla potenciálně nekonzistentní.

Podobně jako hodnocení klienta, pokud EF Core 3.0 nelze přeložit linq dotaz do jednoho příkazu SQL, vyvolá výjimku modulu runtime. Ale udělali jsme EF Core schopné překládat mnoho běžných vzorů, které slouží ke generování více dotazů na jeden dotaz s JOINs.

## <a name="cosmos-db-support"></a>Podpora pro Cosmos DB

Zprostředkovatel Cosmos DB pro EF Core umožňuje vývojářům obeznámeným s modelem programování EF snadno cílit na Azure Cosmos DB jako aplikační databázi. Cílem je, aby některé z výhod Cosmos DB, jako je globální distribuce, "vždy na" dostupnost, elastická škálovatelnost a nízká latence, ještě přístupnější pro vývojáře .NET. Zprostředkovatel umožňuje většinu funkcí EF Core, jako je automatické sledování změn, LINQ a převody hodnot, proti SQL API v Cosmos DB.

Další podrobnosti najdete v [dokumentaci k poskytovateli Cosmos DB.](xref:core/providers/cosmos/index)

## <a name="c-80-support"></a>Podpora pro C# 8.0

EF Core 3.0 využívá několik [nových funkcí v C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):

### <a name="asynchronous-streams"></a>Asynchronní datové proudy

Výsledky asynchronních dotazů jsou nyní `IAsyncEnumerable<T>` vystaveny pomocí nového `await foreach`standardního rozhraní a mohou být spotřebovány pomocí .

``` csharp
var orders =
    from o in context.Orders
    where o.Status == OrderStatus.Pending
    select o;

await foreach(var o in orders.AsAsyncEnumerable())
{
    Process(o);
}
```

Další [podrobnosti naleznete v asynchronních proudech v dokumentaci jazyka C#.](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams)

### <a name="nullable-reference-types"></a>Odkazové typy s možnou hodnotou null

Pokud je tato nová funkce povolena ve vašem kódu, EF Core zkoumá nullability vlastnosti typu odkazu a použije ji na odpovídající sloupce a vztahy `[Required]` v databázi: vlastnosti typů odkazů s nenulovou hodnotou jsou považovány za atribut anotace dat.

Například v následující třídě budou vlastnosti `string?` označené jako typ nakonfigurovány jako volitelné, zatímco budou konfigurovány `string` podle potřeby:

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

Další podrobnosti naleznete v [tématu Práce s typy odkazů s možnou hodnotou null](xref:core/miscellaneous/nullable-reference-types) v dokumentaci EF Core.

## <a name="interception-of-database-operations"></a>Zachycení databázových operací

Nové rozhraní API pro zachycení v EF Core 3.0 umožňuje poskytovat vlastní logiku, která má být vyvolána automaticky vždy, když dojde k operacím databáze nižší úrovně jako součást normální operace EF Core. Například při otevírání připojení, potvrzení transakcí nebo provádění příkazů.

Podobně jako funkce zachycení, které existovaly v EF 6, zachycovače umožňují zachytit operace před nebo po jejich dosažení. Když je zachytíte dříve, než k nim dojde, můžete obcházet spuštění a zadat alternativní výsledky z logiky zachycení.

Chcete-li například manipulovat s textem `IDbCommandInterceptor`příkazu, můžete vytvořit :

``` csharp
public class HintCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult result)
    {
        // Manipulate the command text, etc. here...
        command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
        return result;
    }
}
```

A zaregistrujte `DbContext`jej u svého :

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a>Reverzní inženýrství zobrazení databáze

Typy dotazů, které představují data, která lze číst z databáze, ale nejsou aktualizována, byly přejmenovány na [typy bezklíčových entit](xref:core/modeling/keyless-entity-types).
Vzhledem k tomu, že jsou vynikající pro mapování zobrazení databáze ve většině scénářů, EF Core nyní automaticky vytvoří bezklíčové typy entit při zobrazení databáze zpětné analýzy.

Například pomocí [nástroje příkazového řádku dotnet ef](xref:core/miscellaneous/cli/dotnet) můžete zadat:

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

A nástroj bude nyní automaticky přilebené typy pro pohledy a tabulky bez klíčů:

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Names>(entity =>
    {
        entity.HasNoKey();
        entity.ToView("Names");
    });

    modelBuilder.Entity<Things>(entity =>
    {
        entity.HasNoKey();
    });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Závislé entity sdílející tabulku s objektem zabezpečení jsou nyní volitelné.

Počínaje EF Core 3.0, `OrderDetails` pokud `Order` je vlastněna nebo explicitně mapována na `Order` stejnou `OrderDetails` tabulku, `OrderDetails` bude možné přidat bez a všechny vlastnosti, s výjimkou primární klíč bude mapován na sloupce s možnou hodnotou null.

Při dotazování ef core `OrderDetails` `null` nastaví, pokud některé z jeho požadovaných vlastností nemá hodnotu, nebo pokud `null`nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou .

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>EF 6.3 na jádru rozhraní .NET

Ve skutečnosti se nejedná o funkci EF Core 3.0, ale myslíme si, že je důležitá pro mnoho našich současných zákazníků.

Chápeme, že mnoho existujících aplikací používá předchozí verze EF a že jejich přenos na EF Core pouze pro využití .NET Core může vyžadovat značné úsilí.
Z tohoto důvodu jsme se rozhodli portovat nejnovější verzi EF 6 pro spuštění na .NET Core 3.0.

Další podrobnosti naleznete [v tématu Co je nového v EF 6](xref:ef6/what-is-new/index).

## <a name="postponed-features"></a>Odložené funkce

Některé funkce původně plánované pro EF Core 3.0 byly odloženy na budoucí verze:

- Možnost ignorovat části modelu při migraci, sledována jako [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Entity vačků vlastností, sledované jako dva samostatné problémy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) o entitách sdíleného typu a [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) o podpoře mapování indexovaných vlastností.
