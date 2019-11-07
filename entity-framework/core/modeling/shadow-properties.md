---
title: Vlastnosti stínu – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655952"
---
# <a name="shadow-properties"></a>Stínové vlastnosti

Vlastnosti stínu jsou vlastnosti, které nejsou definovány ve vaší třídě entity .NET, ale jsou definovány pro daný typ entity v modelu EF Core. Hodnota a stav těchto vlastností se v sledování změn uchovávají čistě.

Vlastnosti stínu jsou užitečné, pokud jsou v databázi data, která by neměla být vystavena na mapovaných typech entit. Nejčastěji se používají pro vlastnosti cizího klíče, ve kterých je vztah mezi dvěma entitami reprezentován hodnotou cizího klíče v databázi, ale vztah je spravován na typech entit pomocí navigačních vlastností mezi typy entit.

Hodnoty vlastností stínů se dají získat a změnit prostřednictvím rozhraní `ChangeTracker` API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Na vlastnosti stínu lze odkazovat v dotazech LINQ prostřednictvím statické metody `EF.Property`.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konvence

Vlastnosti stínu lze vytvořit podle konvence při zjištění relace, ale v třídě závislé entity není nalezena žádná vlastnost cizího klíče. V tomto případě se zavede vlastnost stínového cizího klíče. Vlastnost stínového cizího klíče bude pojmenována `<navigation property name><principal key property name>` (navigace na závislé entitě, která odkazuje na hlavní entitu, se používá pro pojmenování). Pokud název vlastnosti klíč objektu zabezpečení obsahuje název vlastnosti navigace, pak bude název pouze `<principal key property name>`. Pokud není k dispozici žádná navigační vlastnost závislá entita, bude na svém místě použit název typu objektu zabezpečení.

Například následující výpis kódu bude mít za následek zavlečení `BlogId` vlastnosti Shadow do `Post` entity.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>Datové poznámky

Vlastnosti stínu nelze vytvořit s datovými poznámkami.

## <a name="fluent-api"></a>Rozhraní Fluent API

Ke konfiguraci vlastností stínu můžete použít rozhraní Fluent API. Po volání přetížení řetězce `Property` můžete zřetězit libovolné volání konfigurace, které byste měli pro jiné vlastnosti.

Pokud se název zadaný metodě `Property` shoduje s názvem existující vlastnosti (vlastnost Shadow nebo jedna definovaná na třídě entity), pak kód nakonfiguruje tuto vlastnost na místo zavedení nové vlastnosti stínu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
