---
title: Nové funkce v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005559"
---
# <a name="new-features-included-in-ef-core-30"></a>Nové funkce, které jsou součástí EF Core 3,0

Následující seznam obsahuje hlavní nové funkce, které jsou plánovány pro EF Core 3,0.

EF Core 3,0 je hlavní vydání a obsahuje také mnoho podstatných [změn](xref:core/what-is-new/ef-core-3.0/breaking-changes), což jsou vylepšení rozhraní API, která mohou mít negativní dopad na existující aplikace.  

## <a name="linq-improvements"></a>Vylepšení LINQ 

LINQ umožňuje psát databázové dotazy bez nutnosti opustit svůj jazyk a využít bohatých informací o typech k tomu, aby nabízel IntelliSense a kontrolu typu při kompilaci.
Ale LINQ také umožňuje napsat neomezený počet složitých dotazů, které obsahují libovolné výrazy (volání metod nebo operace).
Zpracování všech těchto kombinací vždy bylo významnou výzvou pro poskytovatele LINQ.
V EF Core 3,0 jsme přepsali naši implementaci LINQ tak, aby povolovala překlady dalších výrazů do SQL, aby bylo možné ve více případech generovat efektivní dotazy, aby nedocházelo k nezjistitelným dotazům, aby bylo možné postupně zavést nové dotazy. schopnosti a výkon improvementswithout přerušují stávající aplikace a poskytovatele dat.

### <a name="client-evaluation"></a>Vyhodnocení klientů

Hlavní změna v EF Core 3,0 musí dělat s tím, jak zpracovává výrazy LINQ, které nejde přeložit na SQL nebo parametry:

V prvních několika verzích EF Core jednoduše zjistili, jaké části dotazu by mohly být přeloženy do SQL a aby se na klientovi spustil zbytek dotazu.
Tento typ spuštění na straně klienta může být v některých situacích žádoucí, ale v mnoha dalších případech může docházet k neefektivním dotazům.
Například pokud EF Core 2,2 nemohl převést predikát ve `Where()` volání, provedl příkaz SQL bez filtru, přečte všechny řádky z databáze a pak je vyfiltruje v paměti.
To může být přijatelné, pokud databáze obsahuje malý počet řádků, ale může způsobit významné problémy s výkonem nebo i selhání aplikace, pokud databáze obsahuje velké číslo nebo řádky.
V EF Core 3,0 jsme omezili Hodnocení klientů jenom na projekci nejvyšší úrovně (poslední volání do `Select()`).
Když EF Core 3,0 detekuje výrazy, které nelze přeložit nikde jinde v dotazu, vyvolá výjimku za běhu.

## <a name="cosmos-db-support"></a>Podpora Cosmos DB 

Poskytovatel Cosmos DB pro EF Core umožňuje vývojářům, kteří znají model programu EF, snadno cílit Azure Cosmos DB jako databázi aplikace.
Cílem je udělat si některé z výhod Cosmos DB, jako je globální distribuce, "Always On", pružná škálovatelnost a nízká latence, dokonce i přístup k vývojářům v rozhraní .NET.
Zprostředkovatel povolí většinu funkcí EF Core, jako je automatické sledování změn, LINQ a převod hodnot, s rozhraním SQL API v Cosmos DB.

## <a name="c-80-support"></a>C#podpora 8,0

EF Core 3,0 využívá výhod některých nových funkcí v C# 8,0:

### <a name="asynchronous-streams"></a>Asynchronní proudy

Výsledky asynchronního dotazu jsou nyní zpřístupněny pomocí nového `IAsyncEnumerable<T>` standardního rozhraní a lze je spotřebovat pomocí. `await foreach`

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a>Odkazové typy s možnou hodnotou null 

Pokud je tato nová funkce ve vašem kódu povolená, EF Core může mít důvod k hodnotě null vlastností refrence typů (buď primitivních typů jako řetězcové nebo navigační vlastnosti), aby se rozhodla hodnota null sloupců a relací v databázi.

## <a name="interception"></a>Zachycení

Nové rozhraní API zachycení v EF Core 3,0 umožňuje programově a úpravu výsledku databázových operací nízké úrovně, ke kterým dochází jako součást běžné operace EF Core, jako je například otevření připojení, transakce initating a provádění příkazů. 

## <a name="reverse-engineering-of-database-views"></a>Zpětná analýza zobrazení databáze

Typy entit bez klíčů (dříve označované jako [typy dotazů](xref:core/modeling/query-types)) reprezentují data, která je možné číst z databáze, ale nelze je aktualizovat.
Tato vlastnost nabízí vynikající možnosti mapování zobrazení databáze ve většině scénářů, takže jsme při zpětné analýze zobrazení databáze automatizováni vytváření typů entit bez použití klíčů.

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.

Počínaje EF Core 3,0, pokud `OrderDetails` je `Order` vlastněná nebo explicitně namapovaná na stejnou tabulku, bude možné přidat `Order` bez `OrderDetails` a všechny `OrderDetails` vlastnosti, s výjimkou, že primární klíč bude namapován na sloupce s možnou hodnotou null.

Při dotazování se EF Core nastaví `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se kromě primárního `null`klíče a všech vlastností nevyžadují žádné vlastnosti.

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

Chápeme, že mnoho existujících aplikací používá předchozí verze EF a že je bude EF Core jenom k tomu, aby využila výhody .NET Core, může někdy vyžadovat značné úsilí.
Z tohoto důvodu jsme povolili použití verze newewst EF 6 v .NET Core 3,0.
Existují určitá omezení, například:
- Pro práci na .NET Core se vyžadují Noví poskytovatelé.
- Prostorová podpora v SQL Server nebude povolena.

## <a name="postponed-features"></a>Odložené funkce

Některé funkce původně plánované pro EF Core 3,0 byly odloženy do budoucích verzí: 

- Možnost ingore části modelu v migracích, které jsou sledovány [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Entity kontejneru objektů a dat, které jsou sledovány dvěma samostatnými problémy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) o entitách Shared type a [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) o podpoře mapování indexovaných vlastností.
