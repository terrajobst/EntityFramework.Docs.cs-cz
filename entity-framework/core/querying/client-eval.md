---
title: Klient vs. vyhodnocení serveru – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417758"
---
# <a name="client-vs-server-evaluation"></a>Hodnocení klienta vs. Server

Obecně platí, Entity Framework Core se pokusí vyhodnotit dotaz na serveru co nejvíce. EF Core převede části dotazu na parametry, které lze vyhodnotit na straně klienta. Zbytek dotazu (spolu s vygenerovanými parametry) se předávají poskytovateli databáze k určení ekvivalentního databázového dotazu, který se má na serveru vyhodnotit. EF Core podporuje částečné vyhodnocení klientů v projekci nejvyšší úrovně (v podstatě poslední volání `Select()`). Pokud se projekce nejvyšší úrovně v dotazu nedá přeložit na server, EF Core načte všechna požadovaná data ze serveru a vyhodnotí zbývající části dotazu na klientovi. Pokud EF Core detekuje výraz, na jakémkoli jiném místě než projekcí nejvyšší úrovně, které nelze přeložit na server, vyvolá výjimku za běhu. Podívejte se, [Jak dotaz funguje](xref:core/querying/how-query-works) pro pochopení, jak EF Core určuje, co se nedá přeložit na server.

> [!NOTE]
> Před verzí 3,0 Entity Framework Core podporované vyhodnocení klientů kdekoli v dotazu. Další informace najdete v [části předchozí verze](#previous-versions).

> [!TIP]
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tohoto článku můžete zobrazit na GitHubu.

## <a name="client-evaluation-in-the-top-level-projection"></a>Hodnocení klienta v projekci nejvyšší úrovně

V následujícím příkladu se pomocná metoda používá ke standardizaci adres URL pro Blogy, které se vracejí z databáze SQL Server. Vzhledem k tomu, že poskytovatel SQL Server nemá žádné informace o tom, jak je tato metoda implementována, není možné ji přeložit do jazyka SQL. Všechny ostatní aspekty dotazu jsou vyhodnocovány v databázi, ale předání vrácených `URL` prostřednictvím této metody je provedeno na klientovi.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Nepodporované Hodnocení klientů

I když je hodnocení klienta užitečné, může někdy způsobit špatný výkon. Vezměte v úvahu následující dotaz, ve kterém se nyní v filtru Where používá pomocná metoda. Vzhledem k tomu, že filtr nelze v databázi použít, je nutné do paměti načíst všechna data, aby bylo možné použít filtr na klientovi. Na základě filtru a množství dat na serveru může vyhodnocení klientů způsobit špatný výkon. Takže Entity Framework Core zablokuje takové vyhodnocení klientů a vyvolá výjimku za běhu.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Explicitní vyhodnocení klientů

Je možné, že bude nutné vynutit vyhodnocení klienta explicitně v některých případech, například následujícím.

- Množství dat je malé, takže hodnocení na klientovi nevede k obrovským snížení výkonu.
- Použitý operátor LINQ nemá žádný překlad na straně serveru.

V takových případech se můžete explicitně vyjádřit k vyhodnocení klienta voláním metod, jako je `AsEnumerable` nebo `ToList` (`AsAsyncEnumerable` nebo `ToListAsync` pro Async). Pomocí `AsEnumerable` byste streamování výsledků, ale použití `ToList` by způsobilo ukládání do vyrovnávací paměti vytvořením seznamu, který také převezme další paměť. I když se vytváří výčet několikrát, pak výsledky uložení do seznamu pomohou více, protože existuje pouze jeden dotaz na databázi. V závislosti na konkrétním použití byste měli vyhodnotit, která metoda je pro případ užitečnější.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>Potenciální nevracení paměti při vyhodnocování klienta

Vzhledem k tomu, že překlad dotazů a kompilace jsou nákladné, EF Core ukládá do mezipaměti kompilovaný plán dotazů. Delegát v mezipaměti může používat klientský kód při hodnocení projekce na nejvyšší úrovni. EF Core generuje parametry pro části stromu vyhodnocené klientem a znovu použije plán dotazu nahrazením hodnot parametrů. Některé konstanty ve stromu výrazů ale nelze převést na parametry. Pokud delegát v mezipaměti obsahuje takové konstanty, pak tyto objekty nemůžou být shromážděny z paměti, protože jsou pořád odkazovány. Pokud takový objekt obsahuje DbContext nebo jiné služby, může to způsobit, že se využití paměti aplikace v průběhu času zvětšuje. Toto chování obvykle představuje znaménko nevracení paměti. EF Core vyvolá výjimku vždy, když dojde v rámci konstant typu, který nelze namapovat pomocí aktuálního poskytovatele databáze. Běžné příčiny a jejich řešení jsou následující:

- **Použití metody instance**: při použití metod instance v projekci klienta obsahuje strom výrazu konstantu instance. Pokud vaše metoda nepoužívá žádná data z instance, zvažte vytvoření metody jako statické. Pokud potřebujete data instance v těle metody a pak předejte konkrétní data jako argument metody.
- **Předání argumentů konstant do metody**: v tomto případě se obvykle používá `this` v argumentu metody klienta. Zvažte rozdělení argumentu do na více skalárních argumentů, které mohou být mapovány poskytovatelem databáze.
- **Jiné konstanty**: Pokud se konstanta nachází napříč v jakémkoli jiném případě, můžete vyhodnotit, zda je konstanta potřebná ke zpracování. Pokud je nutné mít konstantu, nebo pokud nemůžete použít řešení z výše uvedených případů, vytvořte místní proměnnou pro uložení hodnoty a použijte místní proměnnou v dotazu. EF Core převede místní proměnnou na parametr.

## <a name="previous-versions"></a>Předchozí verze

Následující část se vztahuje na verze EF Core před 3,0.

Starší verze EF Core podporovaly vyhodnocování klientů v jakékoli části dotazu – ne pouze projekce nejvyšší úrovně. To je důvod, proč se dotazy podobné těm, které jsou publikovány v [nepodporované části hodnocení klienta](#unsupported-client-evaluation) , správně pracovaly. Vzhledem k tomu, že toto chování může způsobit problémy s nevyřešeným výkonem, EF Core zaznamenalo upozornění hodnocení klienta. Další informace o zobrazení výstupu protokolování najdete v tématu [protokolování](xref:core/miscellaneous/logging).

Volitelně EF Core možné změnit výchozí chování tak, aby buď vyvolalo výjimku, nebo neprováděli žádnou akci při hodnocení klienta (s výjimkou v projekci). Chování při vyvolání výjimky by se podobá chování v 3,0. Chcete-li změnit chování, je třeba nakonfigurovat upozornění při nastavování možností pro váš kontext – obvykle v `DbContext.OnConfiguring`, nebo v `Startup.cs`, pokud používáte ASP.NET Core.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
