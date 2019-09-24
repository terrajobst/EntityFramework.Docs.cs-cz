---
title: Alternativní klíče (jedinečná omezení) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197612"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="63ed2-102">Alternativní klíče (jedinečná omezení)</span><span class="sxs-lookup"><span data-stu-id="63ed2-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="63ed2-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="63ed2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="63ed2-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="63ed2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="63ed2-105">Pro každý alternativní klíč v modelu se zavádí jedinečné omezení.</span><span class="sxs-lookup"><span data-stu-id="63ed2-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="63ed2-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="63ed2-106">Conventions</span></span>

<span data-ttu-id="63ed2-107">Podle konvence se pojmenuje `AK_<type name>_<property name>`index a omezení, které jsou představené pro alternativní klíč.</span><span class="sxs-lookup"><span data-stu-id="63ed2-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="63ed2-108">U složených alternativních klíčů `<property name>` se jako seznam názvů vlastností oddělí znak podtržení.</span><span class="sxs-lookup"><span data-stu-id="63ed2-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="63ed2-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="63ed2-109">Data Annotations</span></span>

<span data-ttu-id="63ed2-110">Jedinečná omezení nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="63ed2-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="63ed2-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="63ed2-111">Fluent API</span></span>

<span data-ttu-id="63ed2-112">Rozhraní Fluent API můžete použít ke konfiguraci indexu a názvu omezení pro alternativní klíč.</span><span class="sxs-lookup"><span data-stu-id="63ed2-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
