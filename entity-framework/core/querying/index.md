---
title: Dotazování na Data – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949272"
---
# <a name="querying-data"></a><span data-ttu-id="eed81-102">Dotazování na data</span><span class="sxs-lookup"><span data-stu-id="eed81-102">Querying Data</span></span>

<span data-ttu-id="eed81-103">Entity Framework Core Language Integrated Query (LINQ) používá k dotazování na data z databáze.</span><span class="sxs-lookup"><span data-stu-id="eed81-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="eed81-104">LINQ umožňuje používat C# (nebo svůj jazyk .NET) pro zápis silného typu dotazů založených na odvozené třídy kontextu a entity.</span><span class="sxs-lookup"><span data-stu-id="eed81-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="eed81-105">Reprezentace dotazu LINQ je předáno poskytovateli databáze přeložit v konkrétní databázi dotazovacího jazyka (například SQL pro relační databáze).</span><span class="sxs-lookup"><span data-stu-id="eed81-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="eed81-106">Podrobnější informace o zpracování dotazu, najdete v článku [jak funguje dotaz](overview.md).</span><span class="sxs-lookup"><span data-stu-id="eed81-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
