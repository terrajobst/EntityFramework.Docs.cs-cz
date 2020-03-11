---
title: Sekvence – EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417438"
---
# <a name="sequences"></a>Sekvence

> [!NOTE]  
> Sekvence jsou funkce obvykle podporované pouze relačními databázemi. Pokud používáte nerelační databázi, jako je třeba Cosmos, Projděte si dokumentaci k databázi, která vám umožní generovat jedinečné hodnoty.

Sekvence generuje jedinečné a sekvenční číselné hodnoty v databázi. Sekvence nejsou přidružené ke konkrétní tabulce a pro vykreslování hodnot ze stejné sekvence lze nastavit více tabulek.

## <a name="basic-usage"></a>Základní využití

V modelu můžete nastavit sekvencování a potom ji použít ke generování hodnot pro vlastnosti:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

Všimněte si, že konkrétní SQL použitý k vygenerování hodnoty z sekvence je specifický pro databázi; výše uvedený příklad funguje na SQL Server, ale v ostatních databázích se nezdaří. Další informace najdete v dokumentaci ke konkrétní databázi.

## <a name="configuring-sequence-settings"></a>Konfigurace nastavení sekvence

Můžete také nakonfigurovat další aspekty sekvence, jako je jeho schéma, počáteční hodnota, přírůstek atd.:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
