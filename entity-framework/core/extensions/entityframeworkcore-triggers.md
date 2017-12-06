---
title: "Základní EF EntityFrameworkCore.Triggers - nástrojů a rozšíření-"
author: ErikEJ
ms.author: divega
ms.date: 01/19/2017
ms.assetid: 52966E86-5B01-4E45-BAB9-DB92DCBB63C5
ms.technology: entity-framework-core
uid: core/extensions/entityframeworkcore-triggers
ms.openlocfilehash: c0ec59ddc70c94ea520e84be6ca2616e3336261f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="entityframeworkcoretriggers-extension"></a><span data-ttu-id="8e5d3-102">EntityFrameworkCore.Triggers rozšíření</span><span class="sxs-lookup"><span data-stu-id="8e5d3-102">EntityFrameworkCore.Triggers Extension</span></span>

> [!NOTE]  
> <span data-ttu-id="8e5d3-103">Toto rozšíření se neuchovávají v rámci projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8e5d3-103">This extension is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="8e5d3-104">Až budete zvažovat rozšíření třetích stran, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="8e5d3-104">When considering a third party extension, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

<span data-ttu-id="8e5d3-105">Přidejte aktivační události na váš entity s vložit, aktualizovat a odstraňovat události.</span><span class="sxs-lookup"><span data-stu-id="8e5d3-105">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="8e5d3-106">Existují tři události pro každou: před, po a při selhání.</span><span class="sxs-lookup"><span data-stu-id="8e5d3-106">There are three events for each: before, after, and upon failure.</span></span>

<span data-ttu-id="8e5d3-107">V následujícím zdroji vám pomůže začít pracovat s EntityFrameworkCore.Triggers.</span><span class="sxs-lookup"><span data-stu-id="8e5d3-107">The following resource will help you get started with EntityFrameworkCore.Triggers.</span></span>
* [<span data-ttu-id="8e5d3-108">Úložiště EntityFrameworkCore.Triggers GitHub</span><span class="sxs-lookup"><span data-stu-id="8e5d3-108">EntityFrameworkCore.Triggers GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)
