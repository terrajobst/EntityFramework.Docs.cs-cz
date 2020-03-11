---
title: Indexy – EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416393"
---
# <a name="indexes"></a>Indexy

Indexy představují společný pojem v mnoha úložištích dat. I když se jejich implementace v úložišti dat může lišit, používají se k tomu, aby vyhledávání na základě sloupce (nebo sady sloupců) bylo efektivnější.

Indexy nelze vytvořit pomocí datových poznámek. Rozhraní Fluent API můžete použít k určení indexu v jednom sloupci následujícím způsobem:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

Index můžete zadat také pro více než jeden sloupec:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> Podle konvence se v každé vlastnosti (nebo sadě vlastností) vytvoří index, který se používá jako cizí klíč.
>
> EF Core podporuje pouze jeden index na jednu jedinečnou sadu vlastností. Pokud použijete rozhraní Fluent API ke konfiguraci indexu pro sadu vlastností, které už mají definovaný index, buď podle konvence, nebo z předchozí konfigurace, bude změna definice tohoto indexu. To je užitečné, pokud chcete dále konfigurovat index, který byl vytvořen pomocí konvence.

## <a name="index-uniqueness"></a>Jedinečnost indexu

Ve výchozím nastavení nejsou indexy jedinečné: více řádků může mít pro sadu sloupců indexu stejné hodnoty (y). Rejstřík můžete označit jedinečným způsobem:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

Pokud se pokusíte vložit více než jednu entitu se stejnými hodnotami pro sadu sloupců indexu, vyvolá se výjimka.

## <a name="index-name"></a>Název indexu

Podle konvence jsou indexy vytvořené v relační databázi pojmenované `IX_<type name>_<property name>`. U složených indexů se `<property name>` jako podtržítko oddělený seznam názvů vlastností.

Rozhraní Fluent API můžete použít k nastavení názvu indexu vytvořeného v databázi:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a>Filtr indexu

Některé relační databáze umožňují zadat filtrovaný nebo částečný index. To umožňuje indexovat pouze podmnožinu hodnot sloupců, zmenšení velikosti indexu a zlepšení využití výkonu i místa na disku. Další informace o SQL Server filtrovaných indexech [najdete v dokumentaci](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).

Rozhraní Fluent API můžete použít k určení filtru pro index, který je k dispozici jako výraz SQL:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

Při použití poskytovatele SQL Server EF přidá `'IS NOT NULL'` filtr pro všechny sloupce s možnou hodnotou null, které jsou součástí jedinečného indexu. K přepsání této konvence můžete uvést `null`ou hodnotu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a>Zahrnuté sloupce

Některé relační databáze umožňují nakonfigurovat sadu sloupců, které jsou zahrnuté do indexu, ale nejsou součástí jeho "klíče". To může významně zlepšit výkon dotazů, když jsou všechny sloupce v dotazu zahrnuté do indexu jako sloupce Key nebo nonkey, protože k samotné tabulce nepotřebujete mít pøístup. Další informace o zahrnutých sloupcích SQL Server [najdete v dokumentaci](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).

V následujícím příkladu je sloupec `Url` součástí klíče indexu, takže jakékoli filtrování dotazů v tomto sloupci může index použít. Dotazy, které přistupují pouze k `Title` a `PublishedOn` sloupců, ale navíc nebudou potřebovat přístup k tabulce a budou fungovat efektivněji:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
