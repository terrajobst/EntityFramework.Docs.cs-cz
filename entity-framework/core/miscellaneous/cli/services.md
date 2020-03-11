---
title: Služby v době návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416726"
---
# <a name="design-time-services"></a>Služby v době návrhu

Některé služby používané nástroji jsou používány pouze v době návrhu. Tyto služby se spravují odděleně od EF Core běhových služeb, aby se zabránilo jejich nasazení ve vaší aplikaci. Chcete-li potlačit jednu z těchto služeb (například služba pro generování migračních souborů), přidejte implementaci `IDesignTimeServices` do projektu po spuštění.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
