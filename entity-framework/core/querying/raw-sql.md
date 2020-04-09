---
title: Nezpracované dotazy SQL – jádro EF
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417668"
---
# <a name="raw-sql-queries"></a>Nezpracované dotazy SQL

Entity Framework Core umožňuje rozbalit na nezpracované dotazy SQL při práci s relační databází. Nezpracované dotazy SQL jsou užitečné, pokud dotaz, který chcete nelze vyjádřit pomocí LINQ. Nezpracovaná dotazy SQL se také používají, pokud použití dotazu LINQ vede k neefektivnímu dotazu SQL. Nezpracovaná dotazy SQL můžete vrátit normální typy entit nebo [bezklíčové typy entit,](xref:core/modeling/keyless-entity-types) které jsou součástí vašeho modelu.

> [!TIP]  
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) můžete zobrazit na GitHubu.

## <a name="basic-raw-sql-queries"></a>Základní nezpracované dotazy SQL

Metodu `FromSqlRaw` rozšíření můžete použít k zahájení dotazu LINQ na základě nezpracovaného dotazu SQL. `FromSqlRaw`lze použít pouze pro kořeny dotazu, který je přímo na `DbSet<>`.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

Nezpracované dotazy SQL lze použít ke spuštění uložené procedury.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Předávání parametrů

> [!WARNING]
> **Vždy používat parametrizaci pro nezpracované dotazy SQL**
>
> Při zavádění všech uživatelských zadaných hodnot do nezpracovaného dotazu SQL je třeba dbát na to, aby se zabránilo útokům injektáže SQL. Kromě ověření, že tyto hodnoty neobsahují neplatné znaky, vždy používejte parametrizaci, která odesílá hodnoty oddělené od textu SQL.
>
> Zejména nikdy nepředávat zřetězený nebo interpolovaný řetězec (`$""`) s neověřenými hodnotami poskytnutými uživatelem do `FromSqlRaw` nebo `ExecuteSqlRaw`. Metody `FromSqlInterpolated` `ExecuteSqlInterpolated` a umožňují použití syntaxe interpolace řetězce způsobem, který chrání před útoky injektáže SQL.

Následující příklad předá jeden parametr uložené proceduře zahrnutím zástupného symbolu parametru do řetězce dotazu SQL a poskytnutím dalšího argumentu. Zatímco tato syntaxe `String.Format` může vypadat jako syntaxe, `DbParameter` zadaná hodnota je zabalena v a a vložen ý název generovaného parametru, kde byl zadán `{0}` zástupný symbol.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated`je podobná, `FromSqlRaw` ale umožňuje použít syntaxi interpolace řetězců. Stejně `FromSqlRaw` `FromSqlInterpolated` jako , lze použít pouze na kořeny dotazu. Stejně jako v předchozím příkladu je `DbParameter` hodnota převedena na a není ohrožena injektáží SQL.

> [!NOTE]
> Před verzí 3.0 `FromSqlRaw` `FromSqlInterpolated` a byly dvě `FromSql`přetížení s názvem . Další informace naleznete v [části předchozí verze](#previous-versions).

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Můžete také sestavit DbParameter a zadat jako hodnotu parametru. Vzhledem k tomu, že se používá zástupný `FromSqlRaw` symbol běžného parametru SQL, nikoli zástupný symbol řetězce, lze bezpečně použít:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw`umožňuje použít pojmenované parametry v řetězci dotazu SQL, což je užitečné, když uložená procedura má volitelné parametry:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Skládání s LINQ

Můžete sestavit na počáteční nezpracovaný dotaz SQL pomocí linq operátory. EF Core bude považovat za poddotaz a skládání přes něj v databázi. Následující příklad používá nezpracovaný dotaz SQL, který vybírá z funkce s hodnotou tabulky (TVF). A pak skládá na to pomocí LINQ dělat filtrování a třídění.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

Výše uvedený dotaz generuje následující SQL:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>Včetně souvisejících údajů

Metodu `Include` lze použít k zahrnutí souvisejících dat, stejně jako u jiných dotazů LINQ:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

Skládání s LINQ vyžaduje, aby byl váš nezpracovaný dotaz SQL kompozitelný, protože EF Core bude považovat zadaný SQL jako poddotaz. Dotazy SQL, které mohou být `SELECT` složeny na začít s klíčovým slovem. Předání SQL by dále nemělo obsahovat žádné znaky nebo možnosti, které nejsou platné v poddotazu, například:

- Koncový středník
- Na serveru SQL Server je to ukázka `OPTION (HASH JOIN)`koncové úrovně dotazu (například)
- Na serveru SQL `ORDER BY` Server klauzule, `OFFSET 0` která `TOP 100 PERCENT` není `SELECT` použita s OR v klauzuli

SQL Server neumožňuje skládání přes volání uložené procedury, takže jakýkoli pokus o použití dalších operátorů dotazu na takové volání bude mít za následek neplatný SQL. Použijte `AsEnumerable` `AsAsyncEnumerable` nebo metoda `FromSqlRaw` `FromSqlInterpolated` hned po nebo metody, aby se ujistil, že EF Core nepokouší vytvořit přes uloženou proceduru.

## <a name="change-tracking"></a>Sledování změn

Dotazy, které `FromSqlRaw` používají `FromSqlInterpolated` metody nebo, se řídí přesně stejnými pravidly sledování změn jako jakýkoli jiný dotaz LINQ v EF Core. Pokud například typy entit projektů dotazu, budou výsledky ve výchozím nastavení sledovány.

Následující příklad používá nezpracovaný dotaz SQL, který vybere z funkce s hodnotou tabulky (TVF), pak zakáže sledování změn s voláním `AsNoTracking`:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Omezení

Při použití nezpracovaných dotazů SQL je třeba znát několik omezení:

- Dotaz SQL musí vrátit data pro všechny vlastnosti typu entity.
- Názvy sloupců v sadě výsledků se musí shodovat s názvy sloupců, na které jsou namapovány vlastnosti. Všimněte si, že toto chování se liší od EF6. EF6 ignoroval vlastnost na sloupce mapování pro nezpracované dotazy SQL a názvy sloupců sady výsledků musel odpovídat názvy vlastností.
- Dotaz SQL nemůže obsahovat související data. V mnoha případech však můžete vytvořit nad dotaz `Include` pomocí operátoru vrátit související data (viz [Včetně souvisejících dat](#including-related-data)).

## <a name="previous-versions"></a>Předchozí verze

EF Core verze 2.2 a dříve měl `FromSql`dvě přetížení metody s názvem , `FromSqlRaw` které `FromSqlInterpolated`se chovaly stejným způsobem jako novější a . Bylo snadné náhodně volat metodu raw string, když záměrem bylo volání interpolované metody řetězce a naopak. Volání nesprávné přetížení náhodně může mít za následek dotazy nejsou parametrizovány, když by měly být.
