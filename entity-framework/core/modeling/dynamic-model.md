---
title: Střídání mezi několika modely se stejným typem DbContext-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781128"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Střídání mezi několika modely se stejným typem DbContext

Model sestavený v `OnModelCreating` může použít vlastnost v kontextu ke změně způsobu sestavení modelu. Předpokládejme například, že jste chtěli konfigurovat entitu odlišně v závislosti na některé vlastnosti:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

Tento kód bohužel nefunguje tak, jak je, protože EF sestaví model a spustí `OnModelCreating` pouze jednou a ukládá do mezipaměti výsledek z důvodů výkonu. Je však možné připojit se k mechanismu ukládání modelu do mezipaměti, aby měl EF vědět, že vlastnost vytvářející různé modely vytváří různé modely.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

EF používá `IModelCacheKeyFactory` k vygenerování klíčů mezipaměti pro modely; ve výchozím nastavení předpokládá, že pro každý daný typ kontextu bude model stejný, takže výchozí implementace této služby vrátí klíč, který pouze obsahuje typ kontextu. Chcete-li vydávat různé modely ze stejného typu kontextu, je nutné nahradit službu `IModelCacheKeyFactory` správnou implementací. vygenerovaný klíč bude porovnán s ostatními klíči modelů pomocí metody `Equals`, přičemž vezme v úvahu všechny proměnné ovlivňující model:

Následující implementace vezme při vytváření klíče mezipaměti modelu `IgnoreIntProperty` v úvahu:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Nakonec Zaregistrujte novou `IModelCacheKeyFactory` do `OnConfiguring`vašeho kontextu:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) pro další kontext.
