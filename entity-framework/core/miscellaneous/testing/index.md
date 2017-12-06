---
title: "Test komponent pomocí rozhraní Entity Framework - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a><span data-ttu-id="80acd-102">Testování</span><span class="sxs-lookup"><span data-stu-id="80acd-102">Testing</span></span>

<span data-ttu-id="80acd-103">Můžete otestovat komponent pomocí něco, co blíží připojení k databázi skutečné bez režie skutečné databáze vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="80acd-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="80acd-104">Existují dvě hlavní možnosti tohoto postupu:</span><span class="sxs-lookup"><span data-stu-id="80acd-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="80acd-105">[Režim v paměti SQLite](sqlite.md) umožňuje psát efektivní testů proti zprostředkovatele, který se chová jako relační databáze.</span><span class="sxs-lookup"><span data-stu-id="80acd-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="80acd-106">[Zprostředkovatel InMemory](in-memory.md) je lightweight zprostředkovatele, který má minimální závislosti, ale není vždy chovat jako relační databáze.</span><span class="sxs-lookup"><span data-stu-id="80acd-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
