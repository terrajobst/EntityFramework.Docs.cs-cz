---
title: "Generované hodnoty - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 892494461bcf49ee10d05c972da0ba19ca003c35
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="generated-values"></a>Generované hodnoty

## <a name="value-generation-patterns"></a>Vzorce generování hodnota

Existují tři vzorce generování hodnoty, které lze použít pro vlastnosti.

### <a name="no-value-generation"></a>Generování žádná hodnota

Generaci bez hodnota znamená, že budou vždy zadejte platnou hodnotu uložit do databáze. Tato platná hodnota musí být přiřazeni k nové entity předtím, než jsou přidány do kontextu.

### <a name="value-generated-on-add"></a>Přidejte hodnotu generovanou na

Hodnotu generovanou na přidání znamená generovaný hodnotu pro nové entity.

V závislosti na poskytovateli databáze používá hodnoty mohou být vygenerována na straně klienta EF nebo v databázi. Pokud hodnota je generován databáze, pak EF může přiřadit dočasnou hodnotou při Přidat entitu do kontextu. Toto dočasný hodnota nahradí pak podle hodnoty generovaný databází během `SaveChanges()`.

Pokud přidáte entitu do kontextu, který má hodnotu přiřazeno k vlastnosti, EF se pokusí vložit tuto hodnotu místo generování novou. Vlastnost má mít hodnota přiřazená, pokud není přiřazen CLR výchozí hodnota (`null` pro `string`, `0` pro `int`, `Guid.Empty` pro `Guid`atd.). Další informace najdete v tématu [explicitní hodnoty vlastností generovaného](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Jak hodnota je generována pro přidání entity bude záviset na používaný zprostředkovatel databáze. Zprostředkovatelé databáze může automaticky nastavit hodnotu generování pro některé typy vlastností, ale jiné může vyžadovat ručně nastavit, jak je generována hodnota.
>
> Například při použití SQL serveru, hodnoty budou automaticky generovány pro `GUID` vlastnosti (pomocí algoritmu sekvenční GUID systému SQL Server). Ale pokud určíte `DateTime` vlastnost se generuje na Přidat, a pak je potřeba nastavit způsob, jak hodnoty má být vygenerován. Jeden způsob, jak to provést, je výchozí hodnota je konfigurace `GETDATE()`, najdete v části [výchozí hodnoty](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Hodnotu generovanou při přidání nebo aktualizaci

Hodnotu generovanou na přidání nebo aktualizace znamená, že nové hodnoty je vygenerováno pokaždé, když uložení záznamu (vložení nebo aktualizace).

Jako `value generated on add`, pokud zadáte hodnotu pro vlastnost na nově přidané instanci entity, že bude místo hodnotu generován vložit hodnotu. Také je možné nastavit explicitní hodnotu, při aktualizaci. Další informace najdete v tématu [explicitní hodnoty vlastností generovaného](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Jak hodnota je generována pro entity added a aktualizované bude záviset na používaný zprostředkovatel databáze. Zprostředkovatele databáze může automaticky nastavit generování hodnoty pro některé typy vlastností, zatímco ostatní bude vyžadovat, abyste ručně nastavit, jak je generována hodnota.
>
> Při použití SQL serveru, například `byte[]` vlastnosti, které jsou nastavené generovaná na Přidat nebo aktualizovat a označen jako tokeny souběžnosti, bude instalační program se `rowversion` datový typ - tak, aby hodnoty se budou generovat v databázi. Ale pokud určíte `DateTime` vlastnost se generuje na přidáte nebo aktualizujete, pak je potřeba nastavit způsob, jak hodnoty má být vygenerován. Jeden způsob, jak to provést, je výchozí hodnota je konfigurace `GETDATE()` (viz [výchozí hodnoty](relational/default-values.md)) ke generování hodnot pro nové řádky. Pak můžete použít aktivační událost databáze ke generování hodnot během aktualizací (jako jsou následující příklad aktivační události).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konvence

Podle konvence dlouhé bez složené primární klíče typu short, int, nebo identifikátor Guid bude instalační program a hodnoty generované na Přidat. Všechny ostatní vlastnosti bude instalační program se generací žádná hodnota.

## <a name="data-annotations"></a>Datových poznámek

### <a name="no-value-generation-data-annotations"></a>Žádná hodnota generace (datových poznámek)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Hodnotu generovanou na přidání (datových poznámek)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Díky tomu právě EF vědět, že hodnoty jsou generovány pro přidání entity, není zaručeno, že bude EF nastavit vlastní mechanizmus ke generování hodnot. V tématu [přidat hodnotu generovanou na](#value-generated-on-add) další podrobnosti.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Hodnotu generovanou na přidání nebo aktualizace (datových poznámek)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Díky tomu právě EF vědět, že hodnoty jsou generovány pro přidané nebo aktualizované entity, není zaručeno, že bude EF nastavit vlastní mechanizmus ke generování hodnot. V tématu [hodnota generovaná přidáte nebo aktualizujete](#value-generated-on-add-or-update) další podrobnosti.

## <a name="fluent-api"></a>Rozhraní Fluent API

Chcete-li změnit hodnotu generování vzor pro danou vlastnost můžete použít rozhraní Fluent API.

### <a name="no-value-generation-fluent-api"></a>Žádná hodnota generace (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Hodnotu generovanou na přidání (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` právě umožňuje EF vědět, že hodnoty jsou generovány pro přidání entity, není zaručeno, že bude EF nastavit vlastní mechanizmus ke generování hodnot.  V tématu [přidat hodnotu generovanou na](#value-generated-on-add) další podrobnosti.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Hodnotu generovanou na přidání nebo aktualizace (rozhraní Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Díky tomu právě EF vědět, že hodnoty jsou generovány pro přidané nebo aktualizované entity, není zaručeno, že bude EF nastavit vlastní mechanizmus ke generování hodnot. V tématu [hodnota generovaná přidáte nebo aktualizujete](#value-generated-on-add-or-update) další podrobnosti.
