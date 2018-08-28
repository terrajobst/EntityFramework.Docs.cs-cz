---
title: Vygenerované hodnoty – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: a3656eb1d2dc79ceead04e3a142a58e8afb3cbce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996774"
---
# <a name="generated-values"></a>Vygenerované hodnoty

## <a name="value-generation-patterns"></a>Vzory generování hodnoty

Existují tři vzory generování hodnoty, které lze použít pro vlastnosti.

### <a name="no-value-generation"></a>Generování žádná hodnota

Žádná hodnota generace znamená, že budou vždy zadejte platnou hodnotu bude uložena do databáze. Tato hodnota platná musí přiřadit do nové entity předtím, než jsou přidány do kontextu.

### <a name="value-generated-on-add"></a>Přidat hodnotu vygenerovat na

Přidat hodnotu vygenerovat na prostředky, které se generuje hodnotu pro nové entity.

V závislosti na poskytovateli databáze používá hodnoty mohou být generovány na straně klienta pomocí EF nebo v databázi. Pokud hodnota je generován databází, pak EF může přiřadit dočasná hodnota při přidání entity do kontextu. Tato dočasná hodnota se pak nahradí hodnotu databáze, které jsou generovány během `SaveChanges()`.

Pokud Přidání entity do kontextu, který se má hodnota přiřazená k vlastnosti EF se pokusí vložit tuto hodnotu místo generování nové. Vlastnost je považován za mít přiřazenu Pokud není přiřazen výchozí hodnotu CLR hodnotu (`null` pro `string`, `0` pro `int`, `Guid.Empty` pro `Guid`atd.). Další informace najdete v tématu [explicitní hodnoty pro generované vlastnosti](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Jak hodnota je generována pro přidání entity bude záviset na poskytovateli databáze používá. Poskytovatelé databází může automaticky nastavit generování hodnoty pro některé typy vlastností, ale jiná můžou vyžadovat ručně nastavit, jak hodnota je generována.
>
> Například při použití SQL serveru, hodnoty budou automaticky generovány pro `GUID` vlastností (pomocí sekvenčním algoritmem identifikátor GUID systému SQL Server). Nicméně pokud určíte, `DateTime` se vygeneruje vlastnost při přidání, pak musíte nastavit způsob, jak hodnoty, které mají být generovány. Jeden způsob, jak to provést, je k nastavení výchozí hodnoty `GETDATE()`, naleznete v tématu [výchozí hodnoty](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Hodnota generovaná při přidání nebo aktualizaci

Hodnota generovaná při přidání nebo aktualizace znamená, že nová hodnota je generována při každém uložení záznamu (insert nebo update).

Stejně jako `value generated on add`, pokud chcete zadat hodnotu pro vlastnost na nově přidané instanci entity, že hodnota se vloží ne jen hodnotu generována. Je také možné nastavit explicitní hodnotu při aktualizaci. Další informace najdete v tématu [explicitní hodnoty pro generované vlastnosti](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Jak hodnota je generována pro přidaných a aktualizovaných entit bude záviset na poskytovateli databáze používá. Poskytovatelé databází může automaticky nastavit generování hodnoty pro některé typy vlastností, zatímco ostatní bude nutné ručně nastavit, jak hodnota je generována.
> 
> Při použití SQL serveru, například `byte[]` vlastnosti, které jsou nastaveny vygenerovaný na Přidat nebo aktualizovat a označena jako tokeny souběžnosti, bude instalace s `rowversion` datového typu – tak, aby se v databázi vygeneruje hodnoty. Ale pokud určíte, `DateTime` vlastnost se generuje při přidání nebo aktualizaci, pak musíte nastavit způsob, jak hodnoty, které mají být generovány. Jeden způsob, jak to provést, je k nastavení výchozí hodnoty `GETDATE()` (viz [výchozí hodnoty](relational/default-values.md)) ke generování hodnoty pro nové řádky. Aktivační procedury databáze můžete pak použít ke generování hodnoty během aktualizací (jako je například následující příklad aktivační procedura).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konvence

Podle konvence bez složený primární klíče typu short, int, long nebo identifikátor Guid bude nastavení hodnoty vygeneruje na Přidat. Všechny ostatní vlastnosti budou nastavení generaci bez hodnoty.

## <a name="data-annotations"></a>Datové poznámky

### <a name="no-value-generation-data-annotations"></a>Žádná hodnota generace (anotacemi dat)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Hodnota generovaná na Přidat (anotacemi dat)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Díky tomu stačí vědět, že hodnoty jsou generovány pro přidání entity, nezaručuje, že EF nastavíte vlastní mechanizmus ke generování hodnoty EF. Zobrazit [přidat hodnotu vygenerovat na](#value-generated-on-add) části Další podrobnosti.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Hodnota generovaná na přidání nebo aktualizace (anotacemi dat)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Díky tomu stačí vědět, že hodnoty jsou generovány pro přidané nebo aktualizované entity, nezaručuje, že EF nastavíte vlastní mechanizmus ke generování hodnoty EF. Zobrazit [hodnotu vygenerovat na Přidat nebo aktualizovat](#value-generated-on-add-or-update) části Další podrobnosti.

## <a name="fluent-api"></a>Rozhraní Fluent API

Chcete-li změnit generování vzor hodnota dané vlastnosti můžete použít rozhraní Fluent API.

### <a name="no-value-generation-fluent-api"></a>Žádná hodnota generace (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Hodnota generovaná na přidání (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` stačí vám umožní EF vědět, že hodnoty jsou generovány pro přidání entity, nezaručuje, že EF nastavíte vlastní mechanizmus ke generování hodnoty.  Zobrazit [přidat hodnotu vygenerovat na](#value-generated-on-add) části Další podrobnosti.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Hodnota generovaná na přidání nebo aktualizace (rozhraní Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Díky tomu stačí vědět, že hodnoty jsou generovány pro přidané nebo aktualizované entity, nezaručuje, že EF nastavíte vlastní mechanizmus ke generování hodnoty EF. Zobrazit [hodnotu vygenerovat na Přidat nebo aktualizovat](#value-generated-on-add-or-update) části Další podrobnosti.
