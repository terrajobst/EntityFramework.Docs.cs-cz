---
title: Alternativní klíče (jedinečná omezení) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994188"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="d6050-102">Alternativní klíče (jedinečná omezení)</span><span class="sxs-lookup"><span data-stu-id="d6050-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="d6050-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="d6050-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d6050-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="d6050-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d6050-105">Jedinečné omezení se používá pro každý alternativního klíče v modelu.</span><span class="sxs-lookup"><span data-stu-id="d6050-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="d6050-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="d6050-106">Conventions</span></span>

<span data-ttu-id="d6050-107">Podle konvence, bude mít název indexu a omezení, které jsou zavedeny pro alternativního klíče `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="d6050-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="d6050-108">Pro složené alternativní klíče `<property name>` stane podtržítka oddělený seznam názvů vlastností.</span><span class="sxs-lookup"><span data-stu-id="d6050-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d6050-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="d6050-109">Data Annotations</span></span>

<span data-ttu-id="d6050-110">Jedinečné omezení nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="d6050-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d6050-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="d6050-111">Fluent API</span></span>

<span data-ttu-id="d6050-112">Rozhraní Fluent API můžete nakonfigurovat položku Název indexu a omezení pro alternativního klíče.</span><span class="sxs-lookup"><span data-stu-id="d6050-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
