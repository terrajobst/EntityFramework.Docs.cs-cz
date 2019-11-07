---
title: Zahrnutí & s výjimkou typů – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655743"
---
# <a name="including--excluding-types"></a>Zahrnutí a vyloučení typů

Zahrnutí typu v modelu znamená, že EF má metadata o tomto typu a pokusí se číst a zapisovat instance z/do databáze.

## <a name="conventions"></a>Konvence

Podle konvence jsou typy, které jsou zpřístupněny v `DbSet` vlastnosti vašeho kontextu, zahrnuty v modelu. Kromě toho jsou zahrnuty i typy, které jsou uvedeny v metodě `OnModelCreating`. Nakonec všechny typy, které jsou nalezeny rekurzivním zkoumáním navigačních vlastností zjištěných typů, jsou také zahrnuty v modelu.

**Například v následujícím kódu se objevují všechny tři typy:**

* `Blog`, protože je vystavené ve vlastnosti `DbSet` v kontextu

* `Post`, protože je zjištěna prostřednictvím navigační vlastnosti `Blog.Posts`

* `AuditEntry`, protože se zmiňuje o `OnModelCreating`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>Datové poznámky

K vyloučení typu z modelu můžete použít datové poznámky.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k vyloučení typu z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
