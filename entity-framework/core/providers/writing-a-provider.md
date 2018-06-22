---
title: Zápis poskytovatele databáze - EF jádra
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 4bddf5858ab2c6b2fd22571a20edb3f7c85e2853
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678957"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="53ca4-102">Zápis poskytovatele databáze</span><span class="sxs-lookup"><span data-stu-id="53ca4-102">Writing a Database Provider</span></span>

<span data-ttu-id="53ca4-103">Informace o vytváření zprostředkovatele databáze Entity Framework Core najdete v tématu [tak chcete napsat poskytovatele EF základní](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) podle [podle Arthur Vickerse](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="53ca4-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

<span data-ttu-id="53ca4-104">Základní EF základní kód je open source a obsahuje několik poskytovatelů databáze, které lze použít jako referenci.</span><span class="sxs-lookup"><span data-stu-id="53ca4-104">The EF Core code base is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="53ca4-105">Zdrojový kód najdete na https://github.com/aspnet/EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="53ca4-105">You can find the source code at https://github.com/aspnet/EntityFrameworkCore.</span></span>

## <a name="the-providers-beware-label"></a><span data-ttu-id="53ca4-106">Zprostředkovatele pozor popisek</span><span class="sxs-lookup"><span data-stu-id="53ca4-106">The providers-beware label</span></span>

<span data-ttu-id="53ca4-107">Jakmile začnete pracovat na poskytovatele, sledovat [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) popisek na našem Githubu problémy a žádosti o přijetí změn.</span><span class="sxs-lookup"><span data-stu-id="53ca4-107">Once you begin work on a provider, watch for the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) label on our GitHub issues and pull requests.</span></span> <span data-ttu-id="53ca4-108">Tento popisek používáme k identifikaci změny, které můžou mít vliv poskytovatel zapisovače.</span><span class="sxs-lookup"><span data-stu-id="53ca4-108">We use this label to identify changes that may impact provider writers.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="53ca4-109">Navrhované pojmenování poskytovatelů třetích stran</span><span class="sxs-lookup"><span data-stu-id="53ca4-109">Suggested naming of third party providers</span></span>

<span data-ttu-id="53ca4-110">Doporučujeme používat následující pojmenování pro balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="53ca4-110">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="53ca4-111">To je konzistentní s názvy zásilek tým EF jádra.</span><span class="sxs-lookup"><span data-stu-id="53ca4-111">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="53ca4-112">Příklad:</span><span class="sxs-lookup"><span data-stu-id="53ca4-112">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
