---
title: Předvyplnění dat – EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994476"
---
# <a name="data-seeding"></a><span data-ttu-id="84372-102">Předvyplnění dat</span><span class="sxs-lookup"><span data-stu-id="84372-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="84372-103">Tato funkce je nového v EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="84372-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="84372-104">Předvyplnění dat umožňuje poskytovat počáteční data k naplnění databáze.</span><span class="sxs-lookup"><span data-stu-id="84372-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="84372-105">Na rozdíl od v EF6 do EF Core předvyplnění dat souvisí s typem entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="84372-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="84372-106">Potom EF Core [migrace](xref:core/managing-schemas/migrations/index) dokáže automaticky výpočetní co vložit, aktualizovat nebo odstranit operací nutné použít při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="84372-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="84372-107">Jako příklad, můžete nakonfigurovat počáteční data `Blog` v `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="84372-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="84372-108">Přidání entity, které mají hodnoty cizího klíče relace musí být zadána.</span><span class="sxs-lookup"><span data-stu-id="84372-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="84372-109">Často jsou vlastnosti cizího klíče v stínové stavu, tak abyste mohli nastavit hodnoty anonymní třída by měla sloužit:</span><span class="sxs-lookup"><span data-stu-id="84372-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="84372-110">Po přidání entity, doporučuje se použít [migrace](xref:core/managing-schemas/migrations/index) změny projeví.</span><span class="sxs-lookup"><span data-stu-id="84372-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="84372-111">Alternativně můžete použít `context.Database.EnsureCreated()` k vytvoření nové databáze obsahující počáteční hodnoty dat, jako je například testovací databázi nebo při použití zprostředkovatele v paměti.</span><span class="sxs-lookup"><span data-stu-id="84372-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="84372-112">Všimněte si, že pokud databáze již existuje, `EnsureCreated()` ani aktualizuje schéma ani počáteční hodnoty data v databázi.</span><span class="sxs-lookup"><span data-stu-id="84372-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
