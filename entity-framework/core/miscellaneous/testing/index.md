---
title: Testování komponent pomocí EF Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181314"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="e7ad9-102">Testování součástí pomocí EF Core</span><span class="sxs-lookup"><span data-stu-id="e7ad9-102">Testing components using EF Core</span></span>

<span data-ttu-id="e7ad9-103">Můžete chtít testovat komponenty pomocí něčeho, co se blíží skutečnému připojení k reálné databázi bez režie skutečných vstupně-výstupních operací databáze.</span><span class="sxs-lookup"><span data-stu-id="e7ad9-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="e7ad9-104">K dispozici jsou dvě hlavní možnosti:</span><span class="sxs-lookup"><span data-stu-id="e7ad9-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="e7ad9-105">[V režimu v paměti](sqlite.md) je možné zapisovat efektivní testy pro poskytovatele, který se chová jako relační databáze.</span><span class="sxs-lookup"><span data-stu-id="e7ad9-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="e7ad9-106">[Poskytovatel inMemory](in-memory.md) je jednoduchý zprostředkovatel, který má minimální závislosti, ale nemusí se vždy chovat jako relační databáze.</span><span class="sxs-lookup"><span data-stu-id="e7ad9-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
