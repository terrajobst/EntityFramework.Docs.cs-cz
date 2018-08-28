---
title: Správa schémat databází – EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994382"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="1a862-102">Správa schémat databází</span><span class="sxs-lookup"><span data-stu-id="1a862-102">Managing Database Schemas</span></span>
<span data-ttu-id="1a862-103">EF Core poskytuje dvěma základními způsoby udržování EF Core modelu a databázové schéma synchronizace. K výběru mezi těmito dvěma, rozhodněte, zda modelu EF Core nebo schéma databáze je zdroj informací.</span><span class="sxs-lookup"><span data-stu-id="1a862-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="1a862-104">Pokud chcete modelu EF Core zdroj pravdivých informací, použijte [migrace][1].</span><span class="sxs-lookup"><span data-stu-id="1a862-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="1a862-105">Při provádění změn do modelu EF Core, postupně vztahuje se odpovídající změny schématu na vaši databázi, aby zůstala kompatibilní s modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="1a862-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="1a862-106">Použití [Reverse Engineering] [ 2] Pokud chcete schéma databáze se zdroji pravdivých informací.</span><span class="sxs-lookup"><span data-stu-id="1a862-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="1a862-107">Tento přístup umožňuje generování uživatelského rozhraní DbContext a třídy typu entity ve zpětné analýze schématu databáze do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="1a862-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="1a862-108">[Vytvoření a přemístění rozhraní API] [ 3] můžete také vytvořit schéma databáze z modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="1a862-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="1a862-109">Nicméně jsou primárně pro testování, vytváření prototypů a další scénáře, kde vyřazením databáze je přijatelné.</span><span class="sxs-lookup"><span data-stu-id="1a862-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
