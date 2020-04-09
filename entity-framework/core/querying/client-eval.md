---
title: Vyhodnocení klienta vs. serveru – ef jádro
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417758"
---
# <a name="client-vs-server-evaluation"></a>Vyhodnocení klienta vs. serveru

Jako obecné pravidlo Entity Framework Core pokusí vyhodnotit dotaz na serveru co nejvíce. EF Core převede části dotazu na parametry, které lze vyhodnotit na straně klienta. Zbytek dotazu (spolu s generovanými parametry) je dán poskytovateli databáze k určení ekvivalentního databázového dotazu k vyhodnocení na serveru. EF Core podporuje částečné vyhodnocení klienta v projekci `Select()`nejvyšší úrovně (v podstatě poslední volání). Pokud projekce nejvyšší úrovně v dotazu nelze přeložit na server, EF Core načte všechna požadovaná data ze serveru a vyhodnotí zbývající části dotazu na straně klienta. Pokud EF Core zjistí výraz, na jakémkoli jiném místě než projekce nejvyšší úrovně, které nelze přeložit na server, pak vyvolá výjimku za běhu. Podívejte [se, jak funguje dotaz,](xref:core/querying/how-query-works) abyste pochopili, jak EF Core určuje, co nelze přeložit na server.

> [!NOTE]
> Před verzí 3.0 core entity framework podporuje vyhodnocení klienta kdekoli v dotazu. Další informace naleznete v [části předchozí verze](#previous-versions).

> [!TIP]
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) můžete zobrazit na GitHubu.

## <a name="client-evaluation-in-the-top-level-projection"></a>Hodnocení klienta v projekci nejvyšší úrovně

V následujícím příkladu pomocná metoda se používá ke standardizaci adres URL pro blogy, které jsou vráceny z databáze serveru SQL Server. Vzhledem k tomu, že poskytovatel SERVERU SQL server nemá žádný přehled o tom, jak je tato metoda implementována, není možné ji přeložit do SQL. Všechny ostatní aspekty dotazu jsou vyhodnocovány v databázi, ale předání vrácené `URL` prostřednictvím této metody se provádí na straně klienta.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Nepodporované hodnocení klienta

Zatímco hodnocení klienta je užitečné, může mít za následek nízký výkon někdy. Zvažte následující dotaz, ve kterém pomocná metoda se nyní používá v where filtr. Vzhledem k tomu, že filtr nelze použít v databázi, všechna data je třeba vtáhnout do paměti použít filtr na straně klienta. Na základě filtru a množství dat na serveru může mít hodnocení klienta za následek nízký výkon. Takže Entity Framework Core blokuje takové vyhodnocení klienta a vyvolá výjimku za běhu.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Explicitní vyhodnocení klienta

V některých případech, jako je sledování, může být nutné vynutit vyhodnocení klienta výslovně

- Množství dat je malé, takže vyhodnocení na klienta nevzniká obrovská penalizace výkonu.
- Používaný operátor LINQ nemá žádný překlad na straně serveru.

V takových případech můžete explicitně přihlásit do `AsEnumerable` `ToList` hodnocení`AsAsyncEnumerable` `ToListAsync` klienta voláním metody jako nebo (nebo pro asynchronní). Pomocí `AsEnumerable` budete streamování výsledků, ale `ToList` pomocí by způsobit ukládání do vyrovnávací paměti vytvořením seznamu, který také trvá další paměti. I když pokud jste výčet vícekrát, pak ukládání výsledků v seznamu pomáhá více, protože je pouze jeden dotaz do databáze. V závislosti na konkrétním použití byste měli vyhodnotit, která metoda je pro případ užitečnější.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>Potenciální nevracení paměti při hodnocení klienta

Vzhledem k tomu, že překlad dotazu a kompilace jsou nákladné, EF Core ukládá do mezipaměti zkompilovaný plán dotazů. Delegát uložený v mezipaměti může používat klientský kód při hodnocení klienta projekce nejvyšší úrovně. EF Core generuje parametry pro klientem vyhodnocené části stromu a znovu použije plán dotazů nahrazením hodnot parametrů. Ale určité konstanty ve stromu výrazů nelze převést na parametry. Pokud delegát v mezipaměti obsahuje takové konstanty, pak tyto objekty nemohou být uvolněny, protože jsou stále odkazovány. Pokud takový objekt obsahuje DbContext nebo jiné služby v něm, pak může způsobit využití paměti aplikace růst v průběhu času. Toto chování je obecně známkou nevracení paměti. EF Core vyvolá výjimku vždy, když narazíte na konstanty typu, který nelze namapovat pomocí aktuálního poskytovatele databáze. Běžné příčiny a jejich řešení jsou následující:

- **Použití metody instance**: Při použití metod instance v klientské projekci obsahuje strom výrazů konstantu instance. Pokud vaše metoda nepoužívá žádná data z instance, zvažte nastavení metody statické. Pokud potřebujete data instance v těle metody, předajte konkrétní data jako argument metodě.
- **Předávání konstantní argumenty metody**: Tento případ `this` vzniká obecně pomocí v argument u metody klienta. Zvažte rozdělení argumentu do více skalární argumenty, které mohou být mapovány poskytovatelem databáze.
- **Další konstanty**: Pokud konstanta narazíte v jakémkoli jiném případě, pak můžete vyhodnotit, zda konstanta je potřeba při zpracování. Pokud je nutné mít konstantu nebo pokud nelze použít řešení z výše uvedených případů, vytvořte místní proměnnou pro uložení hodnoty a použití místní proměnné v dotazu. EF Core převede místní proměnnou na parametr.

## <a name="previous-versions"></a>Předchozí verze

Následující část se vztahuje na verze EF Core před 3.0.

Starší verze EF Core podporovaly vyhodnocení klienta v libovolné části dotazu – nejen projekce nejvyšší úrovně. To je důvod, proč dotazy podobné jednomu zaúčtované v části [Hodnocení nepodporovaného klienta](#unsupported-client-evaluation) fungovaly správně. Vzhledem k tomu, že toto chování může způsobit problémy s výkonem bez povšimnutí, EF Core zaznamenal upozornění na vyhodnocení klienta. Další informace o zobrazení výstupu protokolování naleznete v [tématu Protokolování](xref:core/miscellaneous/logging).

Volitelně EF Core umožňuje změnit výchozí chování buď vyvolat výjimku nebo nedělat nic při hodnocení klienta (s výjimkou v projekci). Chování vyvolání výjimky by bylo podobné chování v 3.0. Chcete-li změnit chování, je třeba nakonfigurovat upozornění při nastavování možností pro váš kontext – obvykle v `DbContext.OnConfiguring`aplikaci nebo v `Startup.cs` aplikaci, pokud používáte ASP.NET core.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
