---
title: "Dotazování na Data - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="querying-data"></a><span data-ttu-id="37467-102">Dotazování na Data</span><span class="sxs-lookup"><span data-stu-id="37467-102">Querying Data</span></span>

<span data-ttu-id="37467-103">Entity Framework Core jazykové integrují dotazu (LINQ) používá k dotazování na data z databáze.</span><span class="sxs-lookup"><span data-stu-id="37467-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="37467-104">LINQ umožňuje používat C# (nebo vámi zvolený jazyk rozhraní .NET) pro zápis silného typu dotazů založených na odvozené třídy kontextu a entity.</span><span class="sxs-lookup"><span data-stu-id="37467-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="37467-105">Reprezentace dotaz LINQ je předána zprostředkovateli databáze k převodu v konkrétní databáze dotazovací jazyk (např. SQL pro relační databázi).</span><span class="sxs-lookup"><span data-stu-id="37467-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (e.g. SQL for a relational database).</span></span> <span data-ttu-id="37467-106">Podrobnější informace o zpracování dotazu, najdete v části [jak funguje dotazu](overview.md).</span><span class="sxs-lookup"><span data-stu-id="37467-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
