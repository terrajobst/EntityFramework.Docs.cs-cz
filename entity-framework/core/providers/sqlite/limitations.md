---
title: "Databáze SQLite zprostředkovatele - omezení - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="cf1ab-102">Omezení poskytovatele SQLite EF základní databáze</span><span class="sxs-lookup"><span data-stu-id="cf1ab-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="cf1ab-103">Zprostředkovatel SQLite má několik omezení migrace.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="cf1ab-104">Většinu těchto omezení jsou výsledkem omezení v příslušný modul databáze SQLite a nejsou specifické pro EF.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="cf1ab-105">Omezení modelování</span><span class="sxs-lookup"><span data-stu-id="cf1ab-105">Modeling limitations</span></span>

<span data-ttu-id="cf1ab-106">Běžné knihovny relační (sdílené zprostředkovatelé relační databáze Entity Framework) definuje rozhraní API pro modelování koncepty, které jsou společné pro většinu modulů relační databáze.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="cf1ab-107">Pár těchto pojmech nejsou podporována zprostředkovatelem SQLite.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="cf1ab-108">Schémata</span><span class="sxs-lookup"><span data-stu-id="cf1ab-108">Schemas</span></span>
* <span data-ttu-id="cf1ab-109">Sekvence</span><span class="sxs-lookup"><span data-stu-id="cf1ab-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="cf1ab-110">Omezení migrace</span><span class="sxs-lookup"><span data-stu-id="cf1ab-110">Migrations limitations</span></span>

<span data-ttu-id="cf1ab-111">Databázový stroj SQLite nepodporuje počet operací schématu, které jsou podporovány většinou jiných relačních databází.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="cf1ab-112">Pokud se pokusíte provést některé z nepodporované operací k databázi SQLite pak `NotSupportedException` bude vyvolána.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="cf1ab-113">Operace</span><span class="sxs-lookup"><span data-stu-id="cf1ab-113">Operation</span></span>            | <span data-ttu-id="cf1ab-114">Podporovány?</span><span class="sxs-lookup"><span data-stu-id="cf1ab-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="cf1ab-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="cf1ab-115">AddColumn</span></span>            | <span data-ttu-id="cf1ab-116">✔</span><span class="sxs-lookup"><span data-stu-id="cf1ab-116">✔</span></span>          |
| <span data-ttu-id="cf1ab-117">Addforeignkey a</span><span class="sxs-lookup"><span data-stu-id="cf1ab-117">AddForeignKey</span></span>        | <span data-ttu-id="cf1ab-118">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-118">✗</span></span>          |
| <span data-ttu-id="cf1ab-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="cf1ab-119">AddPrimaryKey</span></span>        | <span data-ttu-id="cf1ab-120">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-120">✗</span></span>          |
| <span data-ttu-id="cf1ab-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="cf1ab-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="cf1ab-122">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-122">✗</span></span>          |
| <span data-ttu-id="cf1ab-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="cf1ab-123">AlterColumn</span></span>          | <span data-ttu-id="cf1ab-124">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-124">✗</span></span>          |
| <span data-ttu-id="cf1ab-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="cf1ab-125">CreateIndex</span></span>          | <span data-ttu-id="cf1ab-126">✔</span><span class="sxs-lookup"><span data-stu-id="cf1ab-126">✔</span></span>          |
| <span data-ttu-id="cf1ab-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="cf1ab-127">CreateTable</span></span>          | <span data-ttu-id="cf1ab-128">✔</span><span class="sxs-lookup"><span data-stu-id="cf1ab-128">✔</span></span>          |
| <span data-ttu-id="cf1ab-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="cf1ab-129">DropColumn</span></span>           | <span data-ttu-id="cf1ab-130">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-130">✗</span></span>          |
| <span data-ttu-id="cf1ab-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="cf1ab-131">DropForeignKey</span></span>       | <span data-ttu-id="cf1ab-132">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-132">✗</span></span>          |
| <span data-ttu-id="cf1ab-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="cf1ab-133">DropIndex</span></span>            | <span data-ttu-id="cf1ab-134">✔</span><span class="sxs-lookup"><span data-stu-id="cf1ab-134">✔</span></span>          |
| <span data-ttu-id="cf1ab-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="cf1ab-135">DropPrimaryKey</span></span>       | <span data-ttu-id="cf1ab-136">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-136">✗</span></span>          |
| <span data-ttu-id="cf1ab-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="cf1ab-137">DropTable</span></span>            | <span data-ttu-id="cf1ab-138">✔</span><span class="sxs-lookup"><span data-stu-id="cf1ab-138">✔</span></span>          |
| <span data-ttu-id="cf1ab-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="cf1ab-139">DropUniqueConstraint</span></span> | <span data-ttu-id="cf1ab-140">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-140">✗</span></span>          |
| <span data-ttu-id="cf1ab-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="cf1ab-141">RenameColumn</span></span>         | <span data-ttu-id="cf1ab-142">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-142">✗</span></span>          |
| <span data-ttu-id="cf1ab-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="cf1ab-143">RenameIndex</span></span>          | <span data-ttu-id="cf1ab-144">✗</span><span class="sxs-lookup"><span data-stu-id="cf1ab-144">✗</span></span>          |
| <span data-ttu-id="cf1ab-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="cf1ab-145">RenameTable</span></span>          | <span data-ttu-id="cf1ab-146">✔</span><span class="sxs-lookup"><span data-stu-id="cf1ab-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="cf1ab-147">Alternativní řešení omezení migrace</span><span class="sxs-lookup"><span data-stu-id="cf1ab-147">Migrations limitations workaround</span></span>

<span data-ttu-id="cf1ab-148">Alternativní řešení můžete některé z těchto omezení podle ručně psaní kódu v vaše migrace k provedení tabulku znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="cf1ab-149">Opětovné sestavení tabulky zahrnuje přejmenování existující tabulky, vytvořit novou tabulku, kopírování dat do nové tabulky a vyřadit staré tabulky.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="cf1ab-150">Budete muset použít `Sql(string)` metoda provést některé z těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="cf1ab-151">V tématu [provedení další typy z změny schématu tabulky](http://sqlite.org/lang_altertable.html#otheralter) v dokumentaci k SQLite další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="cf1ab-152">V budoucnu EF může podporovat některé z těchto operací pomocí přístup opětovné sestavení tabulky v pozadí.</span><span class="sxs-lookup"><span data-stu-id="cf1ab-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="cf1ab-153">Můžete [sledovat tuto funkci na našem projektu Githubu](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="cf1ab-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
