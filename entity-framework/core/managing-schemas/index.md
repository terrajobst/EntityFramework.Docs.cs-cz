---
title: Správa schémat databáze – EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655649"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="1d466-102">Správa schémat databází</span><span class="sxs-lookup"><span data-stu-id="1d466-102">Managing Database Schemas</span></span>

<span data-ttu-id="1d466-103">EF Core poskytuje dva hlavní způsoby udržování modelu EF Core a schématu databáze v synchronizaci. Pokud si chcete vybrat mezi těmito dvěma možnostmi, rozhodněte se, jestli je model EF Core nebo schéma databáze zdrojem pravdy.</span><span class="sxs-lookup"><span data-stu-id="1d466-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="1d466-104">Pokud chcete, aby byl model EF Core zdrojem pravdy, použijte [migrace][1].</span><span class="sxs-lookup"><span data-stu-id="1d466-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="1d466-105">Při provádění změn v modelu EF Core tento postup přírůstkově aplikuje odpovídající změny schématu do vaší databáze tak, aby zůstal kompatibilní s modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d466-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="1d466-106">Pokud chcete, aby bylo vaše schéma databáze zdrojem pravdy, použijte [zpětný technický vývoj][2] .</span><span class="sxs-lookup"><span data-stu-id="1d466-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="1d466-107">Tento přístup vám umožní vytvořit si třídu typu DbContext a třídy typů entit, a to tak, že vaše schéma databáze vrátí zpět do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d466-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="1d466-108">[Rozhraní API pro vytváření a vyřazení][3] můžou také vytvořit schéma databáze z modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d466-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="1d466-109">Jsou ale primárně určené pro testování, vytváření prototypů a další scénáře, kdy je odstranění databáze přijatelné.</span><span class="sxs-lookup"><span data-stu-id="1d466-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
