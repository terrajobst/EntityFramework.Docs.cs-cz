---
title: Nezpracované dotazy SQL – EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417668"
---
# <a name="raw-sql-queries"></a>Nezpracované dotazy SQL

Entity Framework Core umožňuje vyřadit z provozu nezpracované dotazy SQL při práci s relační databází. Nezpracované dotazy SQL jsou užitečné v případě, že požadovaný dotaz nelze vyjádřit pomocí LINQ. Nezpracované dotazy SQL se používají také v případě, že použití dotazu LINQ má za následek neefektivní dotaz SQL. Nezpracované dotazy SQL mohou vracet běžné typy entit nebo [typy entit bez klíčů](xref:core/modeling/keyless-entity-types) , které jsou součástí vašeho modelu.

> [!TIP]  
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) tohoto článku můžete zobrazit na GitHubu.

## <a name="basic-raw-sql-queries"></a>Základní nezpracované dotazy SQL

Pomocí metody rozšíření `FromSqlRaw` můžete spustit dotaz LINQ založený na nezpracovaném dotazu SQL. `FromSqlRaw` lze použít pouze v kořenech dotazů, který je přímo na `DbSet<>`.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

Nezpracované dotazy SQL lze použít ke spuštění uložené procedury.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Předávání parametrů

> [!WARNING]
> **Vždy používat parametrizace pro nezpracované dotazy SQL**
>
> Při zavádění jakýchkoli uživatelsky zadaných hodnot do nezpracovaného dotazu SQL je nutné dbát na to, aby se zabránilo útokům prostřednictvím injektáže SQL. Kromě ověření, že tyto hodnoty neobsahují neplatné znaky, vždy použijte Parametrizace, který odesílá hodnoty oddělené od textu SQL.
>
> Konkrétně nikdy předejte zřetězený nebo interpolující řetězec (`$""`) s neověřenými uživatelskými hodnotami do `FromSqlRaw` nebo `ExecuteSqlRaw`. Metody `FromSqlInterpolated` a `ExecuteSqlInterpolated` umožňují použití syntaxe řetězcové interpolace způsobem, který chrání před útoky prostřednictvím injektáže SQL.

Následující příklad předává jeden parametr uložené proceduře zahrnutím zástupného symbolu parametru do řetězce dotazu SQL a zadáním dalšího argumentu. I když tato syntaxe může vypadat jako syntaxe `String.Format`, dodaná hodnota je zabalena v `DbParameter` a vygenerovaný název parametru vložený tam, kde byl zadán zástupný symbol `{0}`.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated` je podobná `FromSqlRaw`, ale umožňuje použít syntaxi řetězcové interpolace. Stejně jako u `FromSqlRaw``FromSqlInterpolated` lze použít pouze v kořenech dotazů. Stejně jako v předchozím příkladu je hodnota převedena na `DbParameter` a není zranitelná při injektáže SQL.

> [!NOTE]
> Před verzí 3,0 byly `FromSqlRaw` a `FromSqlInterpolated` dvě přetížení s názvem `FromSql`. Další informace najdete v [části předchozí verze](#previous-versions).

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Můžete také vytvořit DbParameter a dodat ji jako hodnotu parametru. Vzhledem k tomu, že se místo zástupného symbolu řetězce používá regulární zástupný parametr SQL, `FromSqlRaw` se dá bezpečně použít:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw` umožňuje použít pojmenované parametry v řetězci dotazu SQL, což je užitečné, pokud má uložená procedura volitelné parametry:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Vytváření pomocí LINQ

Pomocí operátorů LINQ můžete vytvořit počáteční nezpracovaný dotaz SQL. EF Core se bude považovat za poddotaz a pak je v databázi přetvořit. Následující příklad používá nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF). A pak je vytvoří pomocí LINQ k filtrování a řazení.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

Výše uvedený dotaz generuje následující příkaz SQL:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>Včetně souvisejících dat

Metodu `Include` lze použít k zahrnutí souvisejících dat, stejně jako u jakéhokoli jiného dotazu LINQ:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

Sestavování pomocí LINQ vyžaduje, aby váš nezpracovaný dotaz SQL byl sestavitelný, protože EF Core považuje dodaný příkaz SQL za poddotaz. Dotazy SQL, které mohou být tvořeny na začátku s klíčovým slovem `SELECT`. Předaný SQL by neměl obsahovat žádné znaky ani možnosti, které nejsou platné pro poddotaz, například:

- Koncový středník
- Na SQL Server, na koncové doporučení na úrovni dotazu (například `OPTION (HASH JOIN)`)
- V SQL Server klauzule `ORDER BY`, která se nepoužívá v `OFFSET 0` nebo `TOP 100 PERCENT` v klauzuli `SELECT`

SQL Server neumožňuje sestavovat volání uložených procedur, takže případný pokus o použití dalších operátorů dotazu na takové volání způsobí neplatnost SQL. Použijte metodu `AsEnumerable` nebo `AsAsyncEnumerable` přímo po `FromSqlRaw` nebo `FromSqlInterpolated` metod, abyste se ujistili, že se EF Core nepokusí vytvořit uloženou proceduru.

## <a name="change-tracking"></a>Sledování změn

Dotazy, které používají metody `FromSqlRaw` nebo `FromSqlInterpolated`, dodržují přesná stejná pravidla sledování změn jako jakékoli jiné dotazy LINQ v EF Core. Například pokud se jedná o typy entit dotazování, výsledky budou ve výchozím nastavení sledovány.

Následující příklad používá nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a potom zakáže sledování změn s voláním `AsNoTracking`:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Omezení

Při použití nezpracovaných dotazů SQL je potřeba vědět o několika omezeních:

- Dotaz SQL musí vracet data pro všechny vlastnosti typu entity.
- Názvy sloupců v sadě výsledků se musí shodovat s názvy sloupců, na které jsou vlastnosti namapovány. Všimněte si, že toto chování se liší od EF6. EF6 ignorovaná vlastnost na mapování sloupce pro nezpracované dotazy SQL a názvy sloupců sady výsledků musí odpovídat názvům vlastností.
- Dotaz SQL nemůže obsahovat související data. V mnoha případech však můžete vytvořit dotaz nad dotazem pomocí operátoru `Include`, který vrátí související data (viz [zahrnutí souvisejících dat](#including-related-data)).

## <a name="previous-versions"></a>Předchozí verze

EF Core verze 2,2 a starší obsahovaly dvě přetížení metody s názvem `FromSql`, která se chovají stejným způsobem jako novější `FromSqlRaw` a `FromSqlInterpolated`. Nechtěně zavolejte metodu nezpracovaného řetězce, pokud by byl záměr volal interpolovaná řetězcová metoda a druhým způsobem. Volání chybného přetížení by mohlo vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.
