---
title: Omezení cizího klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656000"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="08144-102">Omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="08144-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="08144-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="08144-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="08144-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="08144-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="08144-105">Pro každou relaci v modelu je představeno omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="08144-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="08144-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="08144-106">Conventions</span></span>

<span data-ttu-id="08144-107">Podle konvence jsou omezení cizího klíče pojmenována `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="08144-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="08144-108">Pro složené cizí klíče se `<foreign key property name>` jako podtržítko oddělený seznam názvů vlastností cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="08144-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="08144-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="08144-109">Data Annotations</span></span>

<span data-ttu-id="08144-110">Názvy omezení cizího klíče nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="08144-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="08144-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="08144-111">Fluent API</span></span>

<span data-ttu-id="08144-112">Rozhraní Fluent API můžete použít ke konfiguraci názvu omezení cizího klíče pro relaci.</span><span class="sxs-lookup"><span data-stu-id="08144-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
