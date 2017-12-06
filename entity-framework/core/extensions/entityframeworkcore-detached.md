---
title: "Základní EF Detached.EntityFramework - nástrojů a rozšíření-"
author: ErikEJ
ms.author: divega
ms.date: 01/19/2017
ms.assetid: DE1C3FEA-F618-4C11-932F-7C392A1B2C94
ms.technology: entity-framework-core
uid: core/extensions/entityframeworkcore-detached
ms.openlocfilehash: 93937eee07a9e1a0b94bd92140faec7d1753332f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="efdetachedentityframework-extension"></a><span data-ttu-id="bb894-102">EFDetached.EntityFramework rozšíření</span><span class="sxs-lookup"><span data-stu-id="bb894-102">EFDetached.EntityFramework Extension</span></span>

> [!NOTE]  
> <span data-ttu-id="bb894-103">Toto rozšíření se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="bb894-103">This extension is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="bb894-104">Až budete zvažovat rozšíření třetích stran, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="bb894-104">When considering a third party extension, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

<span data-ttu-id="bb894-105">Načítá a ukládá grafy celý odpojit entity (entita s jejich podřízených entit a seznamy).</span><span class="sxs-lookup"><span data-stu-id="bb894-105">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="bb894-106">INSPIROVANÉ [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="bb894-106">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="bb894-107">Cílem je také přidat simplificate některé moduly plug-in některé opakované úkoly, jako je auditování a stránkování.</span><span class="sxs-lookup"><span data-stu-id="bb894-107">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

<span data-ttu-id="bb894-108">V následujícím zdroji vám pomůže začít pracovat s Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="bb894-108">The following resource will help you get started with Detached.EntityFramework</span></span>
* [<span data-ttu-id="bb894-109">Úložiště Detached.EntityFramework GitHub</span><span class="sxs-lookup"><span data-stu-id="bb894-109">Detached.EntityFramework GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)
