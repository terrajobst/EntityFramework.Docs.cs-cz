---
title: Synchronizace replik indexů data - EF jádra
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
---
# <a name="data-seeding"></a><span data-ttu-id="31745-102">Data synchronizace replik indexů</span><span class="sxs-lookup"><span data-stu-id="31745-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="31745-103">Tato funkce je nového v EF základní 2.1.</span><span class="sxs-lookup"><span data-stu-id="31745-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="31745-104">Synchronizace replik indexů dat umožňuje poskytovat počáteční data k naplnění databáze.</span><span class="sxs-lookup"><span data-stu-id="31745-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="31745-105">Na rozdíl od v EF6 v EF základní synchronizace replik indexů dat je přidružen typ entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="31745-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="31745-106">Potom základní EF [migrace](xref:core/managing-schemas/migrations/index) můžete automaticky vypočítat co vložit, aktualizovat nebo odstranit potřeba operations použijí při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="31745-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="31745-107">Jako příklad, můžete to konfigurovat počáteční data pro `Blog` v `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="31745-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="31745-108">Chcete-li přidat entit, které mají relaci cizího klíče hodnoty je potřeba zadat.</span><span class="sxs-lookup"><span data-stu-id="31745-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="31745-109">Často jsou vlastnosti cizího klíče v stínové stavu, proto být schopni nastavit hodnoty anonymní třída by měl použít:</span><span class="sxs-lookup"><span data-stu-id="31745-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="31745-110">Po přidání entity, se doporučuje použít [migrace](xref:core/managing-schemas/migrations/index) chcete změny použít.</span><span class="sxs-lookup"><span data-stu-id="31745-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="31745-111">Alternativně můžete použít `context.Database.EnsureCreated()` k vytvoření nové databáze obsahující počáteční hodnoty data, například pro testovací databáze nebo při použití zprostředkovatele v paměti.</span><span class="sxs-lookup"><span data-stu-id="31745-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="31745-112">Všimněte si, že pokud databáze již existuje, `EnsureCreated()` bude ani aktualizovat schéma ani počáteční hodnoty data v databázi.</span><span class="sxs-lookup"><span data-stu-id="31745-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
