---
title: Sekvence – EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502479"
---
# <a name="sequences"></a><span data-ttu-id="ae1cf-102">Sekvence</span><span class="sxs-lookup"><span data-stu-id="ae1cf-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="ae1cf-103">Sekvence jsou funkce obvykle podporované pouze relačními databázemi.</span><span class="sxs-lookup"><span data-stu-id="ae1cf-103">Sequences are a feature typically supported only by relational databases.</span></span> <span data-ttu-id="ae1cf-104">Pokud používáte nerelační databázi, jako je třeba Cosmos, Projděte si dokumentaci k databázi, která vám umožní generovat jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ae1cf-104">If you're using a non-relational database such as Cosmos, check your database documentation on generating unique values.</span></span>

<span data-ttu-id="ae1cf-105">Sekvence generuje jedinečné a sekvenční číselné hodnoty v databázi.</span><span class="sxs-lookup"><span data-stu-id="ae1cf-105">A sequence generates unique, sequential numeric values in the database.</span></span> <span data-ttu-id="ae1cf-106">Sekvence nejsou přidružené ke konkrétní tabulce a pro vykreslování hodnot ze stejné sekvence lze nastavit více tabulek.</span><span class="sxs-lookup"><span data-stu-id="ae1cf-106">Sequences are not associated with a specific table, and multiple tables can be set up to draw values from the same sequence.</span></span>

## <a name="basic-usage"></a><span data-ttu-id="ae1cf-107">Základní použití</span><span class="sxs-lookup"><span data-stu-id="ae1cf-107">Basic usage</span></span>

<span data-ttu-id="ae1cf-108">V modelu můžete nastavit sekvencování a potom ji použít ke generování hodnot pro vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="ae1cf-108">You can set up a sequence in the model, and then use it to generate values for properties:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

<span data-ttu-id="ae1cf-109">Všimněte si, že konkrétní SQL použitý k vygenerování hodnoty z sekvence je specifický pro databázi; výše uvedený příklad funguje na SQL Server, ale v ostatních databázích se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="ae1cf-109">Note that the specific SQL used to generate a value from a sequence is database-specific; the above example works on SQL Server but will fail on other databases.</span></span> <span data-ttu-id="ae1cf-110">Další informace najdete v dokumentaci ke konkrétní databázi.</span><span class="sxs-lookup"><span data-stu-id="ae1cf-110">Consult your specific database's documentation for more information.</span></span>

## <a name="configuring-sequence-settings"></a><span data-ttu-id="ae1cf-111">Konfigurace nastavení sekvence</span><span class="sxs-lookup"><span data-stu-id="ae1cf-111">Configuring sequence settings</span></span>

<span data-ttu-id="ae1cf-112">Můžete také nakonfigurovat další aspekty sekvence, jako je jeho schéma, počáteční hodnota, přírůstek atd.:</span><span class="sxs-lookup"><span data-stu-id="ae1cf-112">You can also configure additional aspects of the sequence, such as its schema, start value, increment, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
