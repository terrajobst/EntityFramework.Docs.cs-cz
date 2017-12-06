---
title: "Základní EF EFSecondLevelCache.Core - nástrojů a rozšíření-"
author: ErikEJ
ms.author: divega
ms.date: 01/19/2017
ms.assetid: 6021D246-151D-4634-B0CB-413BAAA82D17
ms.technology: entity-framework-core
uid: core/extensions/efsecondlevelcache-core
ms.openlocfilehash: d54a770bae7c769613cfc14306880a28e923cb05
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="efsecondlevelcachecore-extension"></a><span data-ttu-id="2dc9c-102">EFSecondLevelCache.Core rozšíření</span><span class="sxs-lookup"><span data-stu-id="2dc9c-102">EFSecondLevelCache.Core Extension</span></span>

> [!NOTE]  
> <span data-ttu-id="2dc9c-103">Toto rozšíření se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2dc9c-103">This extension is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="2dc9c-104">Až budete zvažovat rozšíření třetích stran, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="2dc9c-104">When considering a third party extension, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

<span data-ttu-id="2dc9c-105">Knihovna druhé úrovně ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="2dc9c-105">Second Level Caching Library.</span></span> <span data-ttu-id="2dc9c-106">Druhé úrovně mezipaměti je mezipaměť a dotazu.</span><span class="sxs-lookup"><span data-stu-id="2dc9c-106">Second level caching is a query cache.</span></span> <span data-ttu-id="2dc9c-107">Výsledky EF příkazy se uloží do mezipaměti, aby stejné příkazy EF načte data z mezipaměti namísto je znovu prováděna v databázi.</span><span class="sxs-lookup"><span data-stu-id="2dc9c-107">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

<span data-ttu-id="2dc9c-108">V následujícím zdroji vám pomůže začít pracovat s EFSecondLevelCache.Core.</span><span class="sxs-lookup"><span data-stu-id="2dc9c-108">The following resource will help you get started with EFSecondLevelCache.Core.</span></span>
* [<span data-ttu-id="2dc9c-109">Úložiště EFSecondLevelCache.Core GitHub</span><span class="sxs-lookup"><span data-stu-id="2dc9c-109">EFSecondLevelCache.Core GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)
