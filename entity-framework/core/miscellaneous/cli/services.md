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
# <a name="design-time-services"></a><span data-ttu-id="a74eb-102">Služby v době návrhu</span><span class="sxs-lookup"><span data-stu-id="a74eb-102">Design-time services</span></span>

<span data-ttu-id="a74eb-103">Některé služby používané nástroji jsou používány pouze v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="a74eb-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="a74eb-104">Tyto služby se spravují odděleně od EF Core běhových služeb, aby se zabránilo jejich nasazení ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a74eb-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="a74eb-105">Chcete-li potlačit jednu z těchto služeb (například služba pro generování migračních souborů), přidejte implementaci `IDesignTimeServices` do projektu po spuštění.</span><span class="sxs-lookup"><span data-stu-id="a74eb-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
