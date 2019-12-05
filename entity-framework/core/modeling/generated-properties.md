---
title: Generované hodnoty – EF Core
description: Postup konfigurace generování hodnoty pro vlastnosti při použití Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 7fa3eae5e2edb7b4c40ed4f99ce4a29f367e622a
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824700"
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

## <a name="conventions"></a>Konvence

Ve výchozím nastavení se nesložené primární klíče typu short, int, Long nebo GUID nastaví tak, aby se hodnoty vygenerovaly při přidání. Všechny ostatní vlastnosti se nastaví bez generování hodnoty.

## <a name="data-annotations"></a>Datové poznámky

### <a name="no-value-generation-data-annotations"></a>Žádná generace hodnot (datové poznámky)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Hodnota vygenerovaná při přidání (datové poznámky)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> To umožňuje, aby EF věděl, že jsou pro přidané entity vygenerovány hodnoty. nezaručujeme tak, že EF nastaví skutečný mechanismus pro generování hodnot. Další podrobnosti najdete v části věnované [hodnotě vygenerované v doplňku](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-data-annotations"></a>Hodnota vygenerovaná při přidání nebo aktualizaci (datové poznámky)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> To umožňuje, aby EF věděl, že jsou pro přidané nebo aktualizované entity vygenerovány hodnoty. nezaručujeme tak, že EF nastaví skutečný mechanismus pro generování hodnot. Další podrobnosti najdete v části věnované [Přidání nebo aktualizaci](#value-generated-on-add-or-update) .

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke změně vzoru pro generování hodnot pro danou vlastnost.

### <a name="no-value-generation-fluent-api"></a>Bez generování hodnoty (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Hodnota vygenerovaná při přidání (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` jen pro EF ví, že jsou pro přidané entity vygenerované hodnoty, nezaručuje, že EF nastaví skutečný mechanismus pro generování hodnot.  Další podrobnosti najdete v části věnované [hodnotě vygenerované v doplňku](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-fluent-api"></a>Hodnota vygenerovaná při přidání nebo aktualizaci (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> To umožňuje, aby EF věděl, že jsou pro přidané nebo aktualizované entity vygenerovány hodnoty. nezaručujeme tak, že EF nastaví skutečný mechanismus pro generování hodnot. Další podrobnosti najdete v části věnované [Přidání nebo aktualizaci](#value-generated-on-add-or-update) .
