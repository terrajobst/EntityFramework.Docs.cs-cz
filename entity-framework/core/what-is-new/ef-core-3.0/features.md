---
title: Nové funkce v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d61fa884f4669daa220ffc96ae59dd63518e6d5a
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921678"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Nové funkce, které jsou součástí EF Core 3,0 (aktuálně ve verzi Preview)

> [!IMPORTANT]
> Počítejte s tím, že sady funkcí a plány budoucích verzí se vždycky mění, a i když se pokusíme tuto stránku uchovávat v aktuálním stavu, nemusí se po celou dobu projevit naše nejnovější plány.

Následující seznam obsahuje hlavní nové funkce, které jsou plánovány pro EF Core 3,0.
Většina těchto funkcí není součástí aktuální verze Preview, ale bude k dispozici v průběhu vývoje směrem k RTM.

Důvodem je to, že na začátku vydání se zaměřujeme na implementaci plánovaných [nejnovějších změn](xref:core/what-is-new/ef-core-3.0/breaking-changes).
Mnohé z těchto nejnovějších změn jsou vylepšení EF Core vlastními.
K odblokování dalších vylepšení je potřeba řada dalších. 

Úplný seznam oprav chyb a vylepšení, které probíhají, najdete [v našem sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>Vylepšení LINQ 

[Sledování problému #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Práce na této funkci začala, ale není součástí aktuální verze Preview.

LINQ umožňuje psát databázové dotazy bez nutnosti opustit svůj jazyk a využít bohatých informací o typech k získání IntelliSense a kontrole typu při kompilaci.
Ale LINQ také umožňuje napsat neomezený počet složitých dotazů a který má vždycky pro poskytovatele LINQ velmi velkou výzvu.
V prvních několika verzích EF Core jsme vyřešili, které části dotazu by mohly být přeložené do SQL, a pak tím, že se zbytek dotazu spustí v paměti na klientovi.
Toto spuštění na straně klienta může být v některých situacích žádoucí, ale v mnoha dalších případech může dojít k neefektivním dotazům, které nemusí být identifikované, dokud se aplikace nasadí do produkčního prostředí.
V EF Core 3,0 plánujeme, abychom provedli velmi snadné změny v tom, jak naše implementace LINQ funguje a jak ji testujeme.
Cílem je zvýšit robustnost (například aby se zabránilo neúmyslnému dotazování ve verzích oprav), aby se povolilo překládání dalších výrazů do SQL, aby se vytvořily efektivní dotazy ve více případech a aby nedocházelo k nezjistitelným dotazům.

## <a name="cosmos-db-support"></a>Podpora Cosmos DB 

[Sledování problému #8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Tato funkce je součástí aktuální verze Preview, ale ještě není dokončená. 

Pracujeme na poskytovateli Cosmos DB pro EF Core, aby mohli vývojáři, kteří znají model programu EF, snadno cílit Azure Cosmos DB jako databázi aplikace.
Cílem je udělat si některé z výhod Cosmos DB, jako je globální distribuce, "Always On", pružná škálovatelnost a nízká latence, dokonce i přístup k vývojářům v rozhraní .NET.
Zprostředkovatel povolí většinu funkcí EF Core, jako je automatické sledování změn, LINQ a převod hodnot, s rozhraním SQL API v Cosmos DB.
Tuto snahu jsme zahájili před EF Core 2,2 a [provedli jsme několik verzí Preview, které poskytovatel nabízí](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
Novým plánem je pokračovat ve vývoji poskytovatele vedle EF Core 3,0. 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.

[Sledování problému #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Tato funkce bude zavedena ve verzi EF Core 3,0-Preview 4.

Vezměte v úvahu následující model:
```C#
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

Počínaje EF Core 3,0, pokud `OrderDetails` je `Order` vlastněná nebo explicitně namapovaná na stejnou tabulku, bude možné přidat `Order` bez `OrderDetails` a všechny `OrderDetails` vlastnosti, s výjimkou, že primární klíč bude namapován na sloupce s možnou hodnotou null.

Při dotazování se EF Core nastaví `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se kromě primárního `null`klíče a všech vlastností nevyžadují žádné vlastnosti.

## <a name="c-80-support"></a>C#podpora 8,0

[Sledování problému #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[sledování problému #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Práce na této funkci začala, ale není součástí aktuální verze Preview.

Chceme, aby naši zákazníci využili výhod některých [nových funkcí v C# 8,0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) , jako jsou asynchronní streamy (včetně `await foreach`) a typy s možnou hodnotou null při použití EF Core.

## <a name="reverse-engineering-of-database-views"></a>Zpětná analýza zobrazení databáze

[Sledování problému #1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Tato funkce není součástí aktuální verze Preview.

[Typy dotazů](xref:core/modeling/query-types), představené v EF Core 2,1 a považované za typy entit bez klíčů v EF Core 3,0, reprezentují data, která je možné číst z databáze, ale nelze je aktualizovat.
Tato vlastnost je ve většině scénářů vhodná pro zobrazení databáze, takže při zpětné analýze zobrazení databáze plánujeme automatizaci vytváření typů entit bez použití klíčů.

## <a name="ef-63-on-net-core"></a>EF 6,3 pro .NET Core

[Sledování problému EF6 # 271](https://github.com/aspnet/EntityFramework6/issues/271)

Práce na této funkci začala, ale není součástí aktuální verze Preview. 

Chápeme, že mnoho existujících aplikací používá předchozí verze EF a že je bude EF Core jenom k tomu, aby využila výhody .NET Core, může někdy vyžadovat značné úsilí.
Z tohoto důvodu budeme přizpůsobovat další verzi EF 6, která bude běžet v .NET Core 3,0.
Provedeme to, abychom usnadnili přenos stávajících aplikací s minimálními změnami.
Existují určitá omezení. Příklad:
- Bude vyžadovat, aby noví poskytovatelé pracovali s dalšími databázemi kromě zahrnuté podpory SQL Server podpoře .NET Core.
- Prostorová podpora v SQL Server nebude povolena.

Všimněte si také, že v tomto okamžiku nejsou plánovány žádné nové funkce pro EF 6.
