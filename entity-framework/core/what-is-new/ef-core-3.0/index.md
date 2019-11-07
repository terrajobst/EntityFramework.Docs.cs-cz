---
title: Nové funkce v Entity Framework Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: 24368b4c87e785e779b3f2b2f10de19766451c9b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656237"
---
# <a name="new-features-in-entity-framework-core-30"></a>Nové funkce v Entity Framework Core 3,0

Následující seznam obsahuje hlavní nové funkce EF Core 3,0.

V případě hlavní verze EF Core 3,0 také obsahuje několik podstatných [změn](xref:core/what-is-new/ef-core-3.0/breaking-changes), což jsou vylepšení rozhraní API, která mohou mít negativní dopad na existující aplikace.  

## <a name="linq-overhaul"></a>Renovace LINQ

LINQ umožňuje psát databázové dotazy pomocí jazyka .NET, který si zvolíte, a využít bohatých informací o typech k tomu, aby nabízel IntelliSense a kontrolu typu při kompilaci.
Ale LINQ také umožňuje napsat neomezený počet složitých dotazů, které obsahují libovolné výrazy (volání metod nebo operace).
Způsob zpracování všech těchto kombinací je hlavní výzvou pro poskytovatele LINQ.

V EF Core 3,0 jsme od našeho poskytovatele LINQ převedli překlady dalších vzorů dotazů do SQL, generování efektivních dotazů ve více případech a zabráníte nezjištěným neefektivním dotazům. Nový poskytovatel LINQ je základem, ve kterém budeme moci nabízet nové možnosti dotazů a vylepšení výkonu v budoucích verzích, aniž byste museli přerušit stávající aplikace a poskytovatele dat.

### <a name="restricted-client-evaluation"></a>Omezené Hodnocení klientů

Nejdůležitější změnou návrhu je udělat, jak zpracováváme LINQ výrazy, které se nedají převést na parametry nebo přeložit na SQL.

V předchozích verzích EF Core identifikovaly, které části dotazu by mohly být přeloženy na SQL, a spustilo zbytek dotazu na klientovi.
Tento typ spuštění na straně klienta je v některých situacích žádoucí, ale v mnoha dalších případech může docházet k neefektivním dotazům.

Například pokud EF Core 2,2 nemohl převést predikát ve volání `Where()`, provedl příkaz SQL bez filtru, přenesl z databáze všechny řádky a pak je vyfiltroval v paměti:

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

To může být přijatelné, pokud databáze obsahuje malý počet řádků, ale může způsobit významné problémy s výkonem nebo i selhání aplikace, pokud databáze obsahuje velké číslo nebo řádky.

V EF Core 3,0 jsme omezili Hodnocení klientů jenom na projekci nejvyšší úrovně (v podstatě na poslední volání `Select()`).
Když EF Core 3,0 detekuje výrazy, které nelze přeložit nikde jinde v dotazu, vyvolá výjimku za běhu.

Aby vývojáři mohli vyhodnotit podmínku predikátu na klientovi jako v předchozím příkladu, je teď potřeba explicitně přepínat vyhodnocení dotazu na LINQ to Objects:

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

Další podrobnosti o tom, jak to může ovlivnit existující aplikace, najdete v [dokumentaci k přerušující změny](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) .

### <a name="single-sql-statement-per-linq-query"></a>Jeden příkaz SQL na dotaz LINQ

Dalším aspektem návrhu, který se významně změnil v 3,0, je, že nyní vždy generujeme jeden příkaz SQL na dotaz LINQ. V předchozích verzích jsme v určitých případech vygenerovali více příkazů SQL, přeložili jsme `Include()` volání vlastností navigace do kolekce a přeložili dotazy, které následují vzory s poddotazy. I když je to v některých případech pohodlné a pro `Include()`, že se i tak pomohl vyhnout posílání redundantních dat po drátě, implementace byla složitá a výsledkem je několik mimořádně neefektivních chování (N + 1 dotazů). V některých situacích byly data vrácená mezi více dotazy potenciálně nekonzistentní.

Podobně jako u hodnocení klienta, pokud EF Core 3,0 nemůže přeložit dotaz LINQ na jeden příkaz jazyka SQL, vyvolá výjimku za běhu. Ale nastavili jsme EF Core schopné přeložit mnoho běžných vzorů, které se používají ke generování několika dotazů na jeden dotaz s spojeními.

## <a name="cosmos-db-support"></a>Podpora Cosmos DB

Poskytovatel Cosmos DB pro EF Core umožňuje vývojářům, kteří znají model programu EF, snadno cílit Azure Cosmos DB jako databázi aplikace. Cílem je udělat si některé z výhod Cosmos DB, jako je globální distribuce, "Always On", pružná škálovatelnost a nízká latence, dokonce i přístup k vývojářům v rozhraní .NET. Zprostředkovatel umožňuje většinu funkcí EF Core, jako je automatické sledování změn, LINQ a převod hodnot, oproti rozhraní SQL API v Cosmos DB.

Další podrobnosti najdete v [dokumentaci poskytovatele Cosmos DB](xref:core/providers/cosmos/index) .

## <a name="c-80-support"></a>C#podpora 8,0

EF Core 3,0 využívá několik [nových funkcí v C# 8,0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):

### <a name="asynchronous-streams"></a>Asynchronní proudy

Výsledky asynchronního dotazu jsou nyní zpřístupněny pomocí nového rozhraní Standard `IAsyncEnumerable<T>` a lze je využít pomocí `await foreach`.

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

Další podrobnosti najdete [v tématu asynchronní C# datové proudy v dokumentaci](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) .

### <a name="nullable-reference-types"></a>Odkazové typy s možnou hodnotou null

Pokud je tato nová funkce ve vašem kódu povolená, EF Core prověřuje vlastnost s hodnotou null vlastností typu odkazu a použije ji pro odpovídající sloupce a relace v databázi: vlastnosti odkazů na typy, které neumožňují hodnotu null, se považují za, jako kdyby měly @no__t_ atribut poznámky k datům 0_

Například v následující třídě jsou vlastnosti označené jako typu `string?` konfigurovány jako volitelné, zatímco `string` budou konfigurovány podle požadavků:

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

Další podrobnosti najdete v tématu [práce s typy odkazů s možnou hodnotou null](xref:core/miscellaneous/nullable-reference-types) v dokumentaci k EF Core.

## <a name="interception-of-database-operations"></a>Zachycení databázových operací

Nové rozhraní API zachycení v EF Core 3,0 umožňuje, aby se vlastní logika vyvolala automaticky, kdykoli dojde k provozu databáze nízké úrovně jako součást běžné operace EF Core. Například při otevírání připojení, potvrzování transakcí nebo provádění příkazů.

Podobně jako funkce zachycení, které existovaly v EF 6, můžou zachytit operace zachycení operací před nebo po nich. Pokud je zachytíte předtím, než k nim dojde, můžete provést překonání a zadat alternativní výsledky z logiky zachycení.

Pokud například chcete manipulovat s textem příkazu, můžete vytvořit `IDbCommandInterceptor`:

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

A zaregistrujte ho u svého `DbContext`:

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a>Zpětná analýza zobrazení databáze

Typy dotazů, které reprezentují data, která je možné číst z databáze, ale nejsou aktualizované, byly přejmenovány na [bez klíčů typu entity](xref:core/modeling/keyless-entity-types).
Vzhledem k tomu, že se jedná o vynikající možnosti mapování zobrazení databáze ve většině scénářů, EF Core nyní automaticky vytvoří typy entit bez klíčů při zpětné analýze zobrazení databáze.

Například pomocí [nástroje příkazového řádku dotnet EF](xref:core/miscellaneous/cli/dotnet) můžete zadat:

``` console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

A nástroj nyní automaticky vytvoří pro zobrazení a tabulky bez klíčů následující typy generování uživatelského rozhraní:

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

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.

Od EF Core 3,0 platí, že pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku, bude možné přidat `Order` bez `OrderDetails` a všech `OrderDetails` vlastností, s výjimkou, že primární klíč bude mapován na sloupce s možnou hodnotou null.

Při dotazování se EF Core nastaví `OrderDetails` na `null`, pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se kromě primárního klíče a všech `null`vlastností nevyžadují žádné vlastnosti.

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

## <a name="ef-63-on-net-core"></a>EF 6,3 pro .NET Core

Nejedná se skutečně o funkci EF Core 3,0, ale myslíme si, že je důležité mít mnoho našich současných zákazníků.

Chápeme, že mnoho existujících aplikací používá předchozí verze EF a že je bude EF Core jenom k tomu, aby využila výhody .NET Core, může vyžadovat značné úsilí.
Z tohoto důvodu jsme se rozhodli vystavit nejnovější verzi EF 6, která bude běžet v .NET Core 3,0.

Další podrobnosti najdete v tématu [co je nového v EF 6](xref:ef6/what-is-new/index).

## <a name="postponed-features"></a>Odložené funkce

Některé funkce původně plánované pro EF Core 3,0 byly odloženy do budoucích verzí:

- Možnost Ignorovat části modelu v migracích, které jsou sledovány jako [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Entity kontejneru objektů a dat, které se sledují jako dva samostatné problémy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) o entitách Shared type a [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) o podpoře mapování indexovaných vlastností.
