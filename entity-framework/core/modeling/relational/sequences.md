---
title: Sekvence – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656118"
---
# <a name="sequences"></a><span data-ttu-id="ec83b-102">Sekvence</span><span class="sxs-lookup"><span data-stu-id="ec83b-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="ec83b-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="ec83b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ec83b-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="ec83b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ec83b-105">Sekvence generuje sekvenční číselné hodnoty v databázi.</span><span class="sxs-lookup"><span data-stu-id="ec83b-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="ec83b-106">Sekvence nejsou spojeny s konkrétní tabulkou.</span><span class="sxs-lookup"><span data-stu-id="ec83b-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="ec83b-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="ec83b-107">Conventions</span></span>

<span data-ttu-id="ec83b-108">Podle konvence nejsou sekvence do modelu zavedeny.</span><span class="sxs-lookup"><span data-stu-id="ec83b-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ec83b-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="ec83b-109">Data Annotations</span></span>

<span data-ttu-id="ec83b-110">Nelze konfigurovat sekvenci pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="ec83b-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ec83b-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="ec83b-111">Fluent API</span></span>

<span data-ttu-id="ec83b-112">Rozhraní Fluent API můžete použít k vytvoření sekvence v modelu.</span><span class="sxs-lookup"><span data-stu-id="ec83b-112">You can use the Fluent API to create a sequence in the model.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

<span data-ttu-id="ec83b-113">Můžete také nakonfigurovat další aspekt sekvence, například její schéma, počáteční hodnotu a přírůstek.</span><span class="sxs-lookup"><span data-stu-id="ec83b-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

<span data-ttu-id="ec83b-114">Po zavedení sekvence je můžete použít ke generování hodnot vlastností v modelu.</span><span class="sxs-lookup"><span data-stu-id="ec83b-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="ec83b-115">Můžete například použít [výchozí hodnoty](default-values.md) pro vložení další hodnoty z sekvence.</span><span class="sxs-lookup"><span data-stu-id="ec83b-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
