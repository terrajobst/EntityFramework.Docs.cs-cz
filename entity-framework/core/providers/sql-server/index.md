---
title: Zprostředkovatel databáze Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 70e7f3d4bc1f57f1b23d9b3e0bd6264236ddbd27
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149205"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="f022e-102">Poskytovatel databáze Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="f022e-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="f022e-103">Tento poskytovatel databáze umožňuje použití Entity Framework Core s Microsoft SQL Server (včetně SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="f022e-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="f022e-104">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="f022e-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="f022e-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="f022e-105">Install</span></span>

<span data-ttu-id="f022e-106">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="f022e-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="supported-database-engines"></a><span data-ttu-id="f022e-107">Podporované databázové stroje</span><span class="sxs-lookup"><span data-stu-id="f022e-107">Supported Database Engines</span></span>

* <span data-ttu-id="f022e-108">Microsoft SQL Server (2012 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="f022e-108">Microsoft SQL Server (2012 onwards)</span></span>
