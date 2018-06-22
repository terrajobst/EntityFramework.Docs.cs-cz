---
title: Alternativní klíče (jedinečná omezení) - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054202"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="557f0-102">Alternativní klíče (jedinečná omezení)</span><span class="sxs-lookup"><span data-stu-id="557f0-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="557f0-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="557f0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="557f0-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="557f0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="557f0-105">Pro každý alternativní klíč v modelu uvádíme jedinečné omezení.</span><span class="sxs-lookup"><span data-stu-id="557f0-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="557f0-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="557f0-106">Conventions</span></span>

<span data-ttu-id="557f0-107">Podle konvence, bude mít název indexu a omezení, které jsou zavedené pro alternativní klíč `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="557f0-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="557f0-108">Pro složené klíče alternativní `<property name>` stane podtržítka oddělený seznam názvů vlastností.</span><span class="sxs-lookup"><span data-stu-id="557f0-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="557f0-109">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="557f0-109">Data Annotations</span></span>

<span data-ttu-id="557f0-110">Jedinečná omezení nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="557f0-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="557f0-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="557f0-111">Fluent API</span></span>

<span data-ttu-id="557f0-112">Rozhraní Fluent API můžete nakonfigurovat položku Název indexu a omezení pro alternativní klíč.</span><span class="sxs-lookup"><span data-stu-id="557f0-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
