---
title: Testovat komponenty pomocí Entity Frameworku – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997834"
---
# <a name="testing"></a><span data-ttu-id="d489f-102">Testování</span><span class="sxs-lookup"><span data-stu-id="d489f-102">Testing</span></span>

<span data-ttu-id="d489f-103">Můžete testovat komponenty pomocí nějakého nástroje, které se blíží připojení k databázi skutečné bez režie skutečné databáze vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="d489f-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="d489f-104">Jak to udělat dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="d489f-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="d489f-105">[Režimu in-memory SQLite](sqlite.md) díky kterému lze vytvářet efektivní testování poskytovatele, který se chová jako relační databáze.</span><span class="sxs-lookup"><span data-stu-id="d489f-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="d489f-106">[Zprostředkovatel InMemory](in-memory.md) je jednoduchý zprostředkovatel, který má minimální závislosti, ale vždy se nechová stejně jako relační databáze.</span><span class="sxs-lookup"><span data-stu-id="d489f-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
