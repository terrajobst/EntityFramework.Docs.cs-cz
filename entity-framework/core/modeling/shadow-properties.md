---
title: Vlastnosti stínu – EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417410"
---
# <a name="shadow-properties"></a>Stínové vlastnosti

Vlastnosti stínu jsou vlastnosti, které nejsou definovány ve vaší třídě entity .NET, ale jsou definovány pro daný typ entity v modelu EF Core. Hodnota a stav těchto vlastností se v sledování změn uchovávají čistě. Vlastnosti stínu jsou užitečné, pokud jsou v databázi data, která by neměla být vystavena na mapovaných typech entit.

## <a name="foreign-key-shadow-properties"></a>Vlastnosti stínování cizího klíče

Vlastnosti stínu se nejčastěji používají pro vlastnosti cizího klíče, kde vztah mezi dvěma entitami představuje hodnota cizího klíče v databázi, ale vztah je spravován na typech entit pomocí navigačních vlastností mezi entitou. druhy. Podle konvence zavádí EF vlastnost Shadow při zjištění vztahu, ale v třídě závislé entity není nalezena žádná vlastnost cizího klíče.

Vlastnost bude pojmenována `<navigation property name><principal key property name>` (navigace na závislé entitě, která odkazuje na hlavní entitu, se používá pro pojmenování). Pokud název vlastnosti klíč objektu zabezpečení obsahuje název vlastnosti navigace, pak bude název pouze `<principal key property name>`. Pokud není k dispozici žádná navigační vlastnost závislá entita, bude na svém místě použit název typu objektu zabezpečení.

Například následující výpis kódu bude mít za následek zavlečení `BlogId` vlastnosti Shadow do `Post` entity:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a>Konfigurace vlastností stínu

Ke konfiguraci vlastností stínu můžete použít rozhraní Fluent API. Po volání přetížení řetězce `Property`můžete zřetězit libovolné volání konfigurace, které byste měli pro jiné vlastnosti. V následující ukázce, protože `Blog` nemá žádnou vlastnost CLR s názvem `LastUpdated`, je vytvořena vlastnost Shadow:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

Pokud se název zadaný metodě `Property` shoduje s názvem existující vlastnosti (vlastnost Shadow nebo jedna definovaná na třídě entity), pak kód nakonfiguruje tuto vlastnost na místo zavedení nové vlastnosti stínu.

## <a name="accessing-shadow-properties"></a>Přístup k vlastnostem Shadow

Hodnoty vlastností stínů se dají získat a změnit prostřednictvím rozhraní `ChangeTracker` API:

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Na vlastnosti stínu se dá odkazovat v dotazech LINQ prostřednictvím statické metody `EF.Property`:

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
