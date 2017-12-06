---
title: "Správa databáze schémata - EF jádra"
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="managing-database-schemas"></a><span data-ttu-id="6d809-102">Správa schémat databáze</span><span class="sxs-lookup"><span data-stu-id="6d809-102">Managing Database Schemas</span></span>
<span data-ttu-id="6d809-103">Základní EF poskytuje dva primární způsoby udržování schéma modelu a databázi EF základní synchronizace. Vybrat mezi nimi, rozhodnete, jestli váš model EF jádra nebo schéma databáze zdroj pravdivosti.</span><span class="sxs-lookup"><span data-stu-id="6d809-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="6d809-104">Pokud chcete, aby váš model EF základní jako zdroj pravdivosti, použijte [migrace][1].</span><span class="sxs-lookup"><span data-stu-id="6d809-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="6d809-105">Při provádění změn pro svůj model EF základní přírůstkově vztahuje se odpovídající změny schématu na vaší databáze, aby zůstala kompatibilní s EF základní modelu.</span><span class="sxs-lookup"><span data-stu-id="6d809-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="6d809-106">Použití [Reverse Engineering] [ 2] Pokud chcete, aby schéma databáze se stane zdroje pravdivosti.</span><span class="sxs-lookup"><span data-stu-id="6d809-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="6d809-107">Tento přístup umožňuje vygenerovat konstruktoru DbContext a tříd entit, typ podle zpětnou analýzu svého schématu databáze do model EF jádra.</span><span class="sxs-lookup"><span data-stu-id="6d809-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="6d809-108">[Vytvořit a vyřadit rozhraní API] [ 3] můžete také vytvořit schéma databáze z modelu EF jádra.</span><span class="sxs-lookup"><span data-stu-id="6d809-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="6d809-109">Jsou to ale především pro účely testování, při vytváření prototypu a další scénáře, kde vyřazením databáze je přijatelná.</span><span class="sxs-lookup"><span data-stu-id="6d809-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
