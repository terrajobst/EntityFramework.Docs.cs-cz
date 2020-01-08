---
title: Typy entit – EF Core
description: Postup konfigurace a mapování typů entit pomocí Entity Framework Core
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502451"
---
# <a name="entity-types"></a>Typy entit

Zahrnutí Negenerickými typu v kontextu znamená, že je zahrnuto v modelu EF Core; obvykle na takový typ odkazujeme jako na *entitu*. EF Core může číst a zapisovat instance entit z/do databáze a pokud používáte relační databázi, EF Core může vytvořit tabulky pro entity pomocí migrací.

## <a name="including-types-in-the-model"></a>Zahrnutí typů do modelu

Podle konvence jsou typy, které jsou zpřístupněny ve vlastnostech Negenerickými ve vašem kontextu, zahrnuty v modelu jako entity. Typy entit, které jsou zadány v metodě `OnModelCreating`, jsou také zahrnuty, stejně jako všechny typy, které jsou nalezeny rekurzivním zkoumáním vlastností navigace jiných zjištěných typů entit.

V níže uvedeném příkladu kódu jsou zahrnuty všechny typy:

* `Blog` je obsažena, protože je zpřístupněna v vlastnosti Negenerickými v kontextu.
* `Post` je obsažena, protože je zjištěna prostřednictvím vlastnosti navigace `Blog.Posts`.
* `AuditEntry`, protože je zadaný v `OnModelCreating`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a>Vyloučení typů z modelu

Pokud nechcete, aby se typ zahrnul do modelu, můžete ho vyloučit:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a>Název tabulky

Podle konvence se každý typ entity nastaví tak, aby se namapoval na databázovou tabulku se stejným názvem, jako má vlastnost Negenerickými, která zpřístupňuje entitu. Pokud pro danou entitu neexistuje žádný Negenerickými, použije se název třídy.

Název tabulky můžete nakonfigurovat ručně:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a>Schéma tabulky

Při použití relační databáze jsou tabulky podle konvence vytvořené ve výchozím schématu vaší databáze. Například Microsoft SQL Server použije schéma `dbo` (SQLite nepodporuje schémata).

Tabulky můžete nakonfigurovat tak, aby se vytvořily v určitém schématu následujícím způsobem:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

Místo zadání schématu pro každou tabulku můžete také definovat výchozí schéma na úrovni modelu pomocí rozhraní Fluent API:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

Všimněte si, že nastavení výchozího schématu ovlivní také další databázové objekty, jako jsou například sekvence.
