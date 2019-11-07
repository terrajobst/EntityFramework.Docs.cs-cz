---
title: Služby v době návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655826"
---
# <a name="design-time-services"></a>Služby v době návrhu

Některé služby používané nástroji jsou používány pouze v době návrhu. Tyto služby se spravují odděleně od EF Core běhových služeb, aby se zabránilo jejich nasazení ve vaší aplikaci. Chcete-li potlačit jednu z těchto služeb (například služba pro generování migračních souborů), přidejte implementaci `IDesignTimeServices` do projektu po spuštění.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
