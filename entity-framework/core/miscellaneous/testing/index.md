---
title: Testování komponent pomocí EF Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416561"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="ce74a-102">Testování součástí pomocí EF Core</span><span class="sxs-lookup"><span data-stu-id="ce74a-102">Testing components using EF Core</span></span>

<span data-ttu-id="ce74a-103">Můžete chtít testovat komponenty pomocí něčeho, co se blíží skutečnému připojení k reálné databázi bez režie skutečných vstupně-výstupních operací databáze.</span><span class="sxs-lookup"><span data-stu-id="ce74a-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="ce74a-104">K dispozici jsou dvě hlavní možnosti:</span><span class="sxs-lookup"><span data-stu-id="ce74a-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="ce74a-105">[V režimu v paměti](sqlite.md) je možné zapisovat efektivní testy pro poskytovatele, který se chová jako relační databáze.</span><span class="sxs-lookup"><span data-stu-id="ce74a-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="ce74a-106">[Poskytovatel inMemory](in-memory.md) je jednoduchý zprostředkovatel, který má minimální závislosti, ale nemusí se vždy chovat jako relační databáze.</span><span class="sxs-lookup"><span data-stu-id="ce74a-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
