---
title: Klíče (primární) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655951"
---
# <a name="keys-primary"></a><span data-ttu-id="4b0fb-102">Klíče (primární)</span><span class="sxs-lookup"><span data-stu-id="4b0fb-102">Keys (primary)</span></span>

<span data-ttu-id="4b0fb-103">Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="4b0fb-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="4b0fb-104">Při použití relační databáze se tato mapa mapuje na koncept *primárního klíče*.</span><span class="sxs-lookup"><span data-stu-id="4b0fb-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="4b0fb-105">Můžete také nakonfigurovat jedinečný identifikátor, který není primárním klíčem (Další informace najdete v tématu [alternativní klíče](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="4b0fb-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="4b0fb-106">Jednu z následujících metod lze použít k nastavení nebo vytvoření primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="4b0fb-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="4b0fb-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="4b0fb-107">Conventions</span></span>

<span data-ttu-id="4b0fb-108">Podle konvence bude vlastnost s názvem `Id` nebo `<type name>Id` nakonfigurovaná jako klíč entity.</span><span class="sxs-lookup"><span data-stu-id="4b0fb-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a><span data-ttu-id="4b0fb-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="4b0fb-109">Data Annotations</span></span>

<span data-ttu-id="4b0fb-110">Pomocí datových poznámek můžete nakonfigurovat jednu vlastnost, která bude klíč entity.</span><span class="sxs-lookup"><span data-stu-id="4b0fb-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="4b0fb-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="4b0fb-111">Fluent API</span></span>

<span data-ttu-id="4b0fb-112">Pomocí rozhraní Fluent API můžete nakonfigurovat jedinou vlastnost, která bude klíč entity.</span><span class="sxs-lookup"><span data-stu-id="4b0fb-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="4b0fb-113">Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, které budou klíč entity (označované jako složený klíč).</span><span class="sxs-lookup"><span data-stu-id="4b0fb-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="4b0fb-114">Složené klíče se dají nakonfigurovat jenom pomocí konvencí rozhraní API Fluent nikdy nenastaví složený klíč a nemůžete použít datové poznámky ke konfiguraci jednoho.</span><span class="sxs-lookup"><span data-stu-id="4b0fb-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
