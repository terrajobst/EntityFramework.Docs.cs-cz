---
title: Generované hodnoty – EF Core
description: Postup konfigurace generování hodnoty pro vlastnosti při použití Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 9c616e157ff1bdb9700f436a7ae2788330fe5d45
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502029"
---
# <a name="generated-values"></a>Vygenerované hodnoty

## <a name="value-generation-patterns"></a>Vzorce generování hodnoty

Existují tři vzorce generování hodnoty, které lze použít pro vlastnosti:

* Bez generování hodnoty
* Hodnota vygenerovaná při přidání
* Hodnota vygenerovaná při přidání nebo aktualizaci

### <a name="no-value-generation"></a>Bez generování hodnoty

Žádná hodnota generování hodnoty znamená, že budete vždycky zadávat platnou hodnotu, která se uloží do databáze. Tato platná hodnota musí být přiřazena novým entitám před jejich přidáním do kontextu.

### <a name="value-generated-on-add"></a>Hodnota vygenerovaná při přidání

Hodnota vygenerovaná při přidání znamená, že se pro nové entity vygeneruje hodnota.

V závislosti na použitém poskytovateli databáze mohou být hodnoty generovány na straně klienta podle EF nebo v databázi. Pokud je hodnota vygenerována databází, pak EF může přiřadit dočasnou hodnotu, když přidáte entitu do kontextu. Tato dočasná hodnota se pak nahradí vygenerovanou hodnotou databáze během `SaveChanges()`.

Pokud přidáte entitu do kontextu, který má hodnotu přiřazenou k vlastnosti, pak EF se pokusí tuto hodnotu vložit místo vygenerování nového. Vlastnost je považována za přiřazenou hodnotu, pokud není přiřazena výchozí hodnota CLR (`null` pro `string`, `0` pro `int`, `Guid.Empty` pro `Guid`atd.). Další informace najdete v tématu [explicitní hodnoty pro vygenerované vlastnosti](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Způsob generování hodnoty pro přidané entity bude záviset na používaném poskytovateli databáze. Poskytovatelé databází můžou automaticky nastavit generování hodnot u některých typů vlastností, ale jiné můžou vyžadovat ruční nastavení způsobu generování hodnoty.
>
> Například při použití SQL Server se hodnoty automaticky vygenerují pro `GUID` vlastnosti (pomocí SQL Server sekvenčního algoritmu GUID). Pokud však zadáte, že vlastnost `DateTime` je generována při přidání, je nutné nastavit způsob, jakým mají být generovány hodnoty. Jedním ze způsobů, jak to provést, je nakonfigurovat výchozí hodnotu `GETDATE()`, najdete v tématu [výchozí hodnoty](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Hodnota vygenerovaná při přidání nebo aktualizaci

Hodnota generovaná při přidání nebo aktualizaci znamená, že nová hodnota je generována při každém uložení záznamu (vložit nebo aktualizovat).

Podobně jako u `value generated on add`platí, že pokud zadáte hodnotu vlastnosti u nově přidané instance entity, bude tato hodnota vložena místo vygenerování hodnoty. Při aktualizaci je také možné nastavit explicitní hodnotu. Další informace najdete v tématu [explicitní hodnoty pro vygenerované vlastnosti](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Způsob generování hodnoty pro přidané a aktualizované entity bude záviset na používaném poskytovateli databáze. Poskytovatelé databáze mohou automaticky nastavit generování hodnot u některých typů vlastností, zatímco ostatní budou vyžadovat ruční nastavení způsobu generování hodnoty.
>
> Například při použití SQL Server, `byte[]` vlastností, které jsou nastaveny jako generované při přidání nebo aktualizaci a označené jako tokeny souběžnosti, bude instalační program s datovým typem `rowversion`, takže budou v databázi vygenerovány hodnoty. Pokud však zadáte, že se vlastnost `DateTime` vygeneruje při přidání nebo aktualizaci, musíte nastavit způsob, jakým se mají hodnoty generovat. Jedním ze způsobů, jak to provést, je nakonfigurovat výchozí hodnotu `GETDATE()` (viz [výchozí hodnoty](relational/default-values.md)) a generovat hodnoty pro nové řádky. Pak můžete použít Trigger databáze k vygenerování hodnot během aktualizací (například pomocí následujícího příkladu triggeru).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="value-generated-on-add"></a>Hodnota vygenerovaná při přidání

Podle konvence, nesložené primární klíče typu short, int, Long nebo GUID jsou nastaveny tak, aby měly hodnoty generované pro vložené entity, pokud hodnota není poskytována aplikací. Váš poskytovatel databáze obvykle postará o nezbytnou konfiguraci. například numerický primární klíč v SQL Server je automaticky nastaven jako sloupec IDENTITY.

Libovolnou vlastnost můžete nakonfigurovat tak, aby se vygenerovala její hodnota pro vložené entity následujícím způsobem:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

***

> [!WARNING]
> To umožňuje, aby EF věděl, že jsou pro přidané entity vygenerovány hodnoty. nezaručujeme tak, že EF nastaví skutečný mechanismus pro generování hodnot. Další podrobnosti najdete v části věnované [hodnotě vygenerované v doplňku](#value-generated-on-add) .

### <a name="default-values"></a>Výchozí hodnoty

U relačních databází lze nakonfigurovat sloupec s výchozí hodnotou. Pokud je řádek vložen bez hodnoty pro tento sloupec, bude použita výchozí hodnota.

Můžete nakonfigurovat výchozí hodnotu vlastnosti:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValue.cs?name=DefaultValue&highlight=5)]

Můžete také zadat fragment SQL, který se použije k výpočtu výchozí hodnoty:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

Zadáním výchozí hodnoty se vlastnost implicitně nakonfiguruje jako hodnota generovaná při přidání.

## <a name="value-generated-on-add-or-update"></a>Hodnota vygenerovaná při přidání nebo aktualizaci

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

***

> [!WARNING]
> To umožňuje, aby EF věděl, že jsou pro přidané nebo aktualizované entity vygenerovány hodnoty. nezaručujeme tak, že EF nastaví skutečný mechanismus pro generování hodnot. Další podrobnosti najdete v části věnované [Přidání nebo aktualizaci](#value-generated-on-add-or-update) .

### <a name="computed-columns"></a>Vypočítané sloupce

V některých relačních databázích může být sloupec nakonfigurovaný tak, aby se jeho hodnota vypočítala v databázi, obvykle s výrazem odkazujícím na jiné sloupce:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=ComputedColumn&highlight=5)]

> [!NOTE]
> V některých případech je hodnota sloupce vypočítána pokaždé, když je načtena (někdy se jim říká *virtuální* sloupce), a v ostatních případech je vypočítána při každé aktualizaci řádku a uložených (někdy označovaných jako *uložené* nebo *trvalé* sloupce). To se liší od jiných poskytovatelů databáze.

## <a name="no-value-generation"></a>Bez generování hodnoty

Zakázání generování hodnoty na vlastnost je obvykle nezbytné, pokud je konvence nakonfiguruje pro generování hodnoty. Například pokud máte primární klíč typu int, bude implicitně nastaven jako hodnota vygenerovaná při přidání; tuto možnost můžete zakázat prostřednictvím následujících kroků:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=5)]

***
