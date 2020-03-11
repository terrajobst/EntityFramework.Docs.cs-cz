---
title: Ukládání dat – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417548"
---
# <a name="saving-data"></a><span data-ttu-id="7a6dd-102">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="7a6dd-102">Saving Data</span></span>

<span data-ttu-id="7a6dd-103">Každá instance kontextu má `ChangeTracker` zodpovědný za sledování změn, které je nutné zapsat do databáze.</span><span class="sxs-lookup"><span data-stu-id="7a6dd-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="7a6dd-104">Při provádění změn instancí tříd entit jsou tyto změny zaznamenávány do `ChangeTracker` a následně zapsány do databáze při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="7a6dd-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="7a6dd-105">Poskytovatel databáze zodpovídá za překlady změn do operací specifických pro databázi (například `INSERT`, `UPDATE`a `DELETE` příkazy pro relační databázi).</span><span class="sxs-lookup"><span data-stu-id="7a6dd-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
