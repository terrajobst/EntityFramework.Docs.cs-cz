---
title: Ukládání dat – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054208"
---
# <a name="saving-data"></a><span data-ttu-id="c6f46-102">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="c6f46-102">Saving Data</span></span>

<span data-ttu-id="c6f46-103">Každá instance kontextu má `ChangeTracker` zodpovídá pro udržování přehledu o změny, které musí být zapsána do databáze.</span><span class="sxs-lookup"><span data-stu-id="c6f46-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="c6f46-104">Jak provedete změny instance třídy entity, se zaznamenávají tyto změny `ChangeTracker` a pak zapsána do databáze při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="c6f46-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="c6f46-105">Zprostředkovatel databáze je zodpovědná za překladu změn v operacích specifických pro databázi (například `INSERT`, `UPDATE`, a `DELETE` příkazy pro relační databázi).</span><span class="sxs-lookup"><span data-stu-id="c6f46-105">The database provider is responsible for translating the changes into database-specific operations (e.g. `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
