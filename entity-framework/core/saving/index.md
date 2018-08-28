---
title: Ukládání dat – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998177"
---
# <a name="saving-data"></a><span data-ttu-id="5093e-102">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="5093e-102">Saving Data</span></span>

<span data-ttu-id="5093e-103">Každá instance kontextu má `ChangeTracker` , která zodpovídá za udržování přehledu o změny, které musí být zapsána do databáze.</span><span class="sxs-lookup"><span data-stu-id="5093e-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="5093e-104">Při provádění změn do instance třídy entity, tyto změny se zaznamenávají do `ChangeTracker` a následně zapsána do databáze při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="5093e-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="5093e-105">Poskytovatel databáze je zodpovědný za převod změny do konkrétních databází operations (například `INSERT`, `UPDATE`, a `DELETE` příkazy pro relační databáze).</span><span class="sxs-lookup"><span data-stu-id="5093e-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
