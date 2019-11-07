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
# <a name="including--excluding-types"></a><span data-ttu-id="cb77a-102">Zahrnutí a vyloučení typů</span><span class="sxs-lookup"><span data-stu-id="cb77a-102">Including & Excluding Types</span></span>

<span data-ttu-id="cb77a-103">Zahrnutí typu v modelu znamená, že EF má metadata o tomto typu a pokusí se číst a zapisovat instance z/do databáze.</span><span class="sxs-lookup"><span data-stu-id="cb77a-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="cb77a-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="cb77a-104">Conventions</span></span>

<span data-ttu-id="cb77a-105">Podle konvence jsou typy, které jsou zpřístupněny v `DbSet` vlastnosti vašeho kontextu, zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="cb77a-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="cb77a-106">Kromě toho jsou zahrnuty i typy, které jsou uvedeny v metodě `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="cb77a-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="cb77a-107">Nakonec všechny typy, které jsou nalezeny rekurzivním zkoumáním navigačních vlastností zjištěných typů, jsou také zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="cb77a-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="cb77a-108">**Například v následujícím kódu se objevují všechny tři typy:**</span><span class="sxs-lookup"><span data-stu-id="cb77a-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="cb77a-109">`Blog`, protože je vystavené ve vlastnosti `DbSet` v kontextu</span><span class="sxs-lookup"><span data-stu-id="cb77a-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="cb77a-110">`Post`, protože je zjištěna prostřednictvím navigační vlastnosti `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="cb77a-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="cb77a-111">`AuditEntry`, protože se zmiňuje o `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="cb77a-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="cb77a-112">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="cb77a-112">Data Annotations</span></span>

<span data-ttu-id="cb77a-113">K vyloučení typu z modelu můžete použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="cb77a-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="cb77a-114">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="cb77a-114">Fluent API</span></span>

<span data-ttu-id="cb77a-115">Rozhraní Fluent API můžete použít k vyloučení typu z modelu.</span><span class="sxs-lookup"><span data-stu-id="cb77a-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
