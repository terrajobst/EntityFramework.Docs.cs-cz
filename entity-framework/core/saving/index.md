---
title: Ukládání dat – jádro EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417548"
---
# <a name="saving-data"></a><span data-ttu-id="ae0dd-102">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="ae0dd-102">Saving Data</span></span>

<span data-ttu-id="ae0dd-103">Každá instance kontextu `ChangeTracker` má, který je zodpovědný za sledování změn, které je třeba zapsat do databáze.</span><span class="sxs-lookup"><span data-stu-id="ae0dd-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="ae0dd-104">Při provádění změn instancí tříd entity jsou tyto `ChangeTracker` změny zaznamenány v databázi `SaveChanges`a poté zapsány do databáze při volání .</span><span class="sxs-lookup"><span data-stu-id="ae0dd-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="ae0dd-105">Poskytovatel databáze je zodpovědný za překlad změn do operací specifických `INSERT` `UPDATE`pro `DELETE` databázi (například , a příkazy pro relační databázi).</span><span class="sxs-lookup"><span data-stu-id="ae0dd-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
