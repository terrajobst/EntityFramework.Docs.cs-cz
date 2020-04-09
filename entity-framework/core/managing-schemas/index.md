---
title: Správa schémat databáze – základní ef
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416305"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="11d9d-102">Správa schémat databází</span><span class="sxs-lookup"><span data-stu-id="11d9d-102">Managing Database Schemas</span></span>

<span data-ttu-id="11d9d-103">EF Core poskytuje dva primární způsoby synchronizace modelu EF Core a schématu databáze. Chcete-li si vybrat mezi těmito dvěma, rozhodněte se, zda je zdrojem pravdy model EF Core nebo schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="11d9d-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="11d9d-104">Pokud chcete, aby byl zdroj pravdy model EF Core, použijte [migrace][1].</span><span class="sxs-lookup"><span data-stu-id="11d9d-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="11d9d-105">Při provádění změn modelu EF Core tento přístup postupně použije odpovídající změny schématu do databáze tak, aby zůstalkompatibilní s modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="11d9d-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="11d9d-106">Pokud chcete, aby bylo zdrojem pravdy schéma databáze, použijte [reverzní inženýrství.][2]</span><span class="sxs-lookup"><span data-stu-id="11d9d-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="11d9d-107">Tento přístup umožňuje sešité číslo DbContext a třídy typu entity pomocí zpětného inženýrství schéma databáze do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="11d9d-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="11d9d-108">Vytvoření [a přetažení api][3] můžete také vytvořit schéma databáze z modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="11d9d-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="11d9d-109">Jsou však především pro testování, prototypování a další scénáře, kde je přijatelné uvolnění databáze.</span><span class="sxs-lookup"><span data-stu-id="11d9d-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
