---
title: Zprostředkovatel databáze Microsoft SQL Server – Azure SQL Database možnosti – EF Core
description: Určení úrovně služby a úrovně výkonu pro Azure SQL Database s poskytovatelem databáze SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824783"
---
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="9dcc1-103">Určení možností Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="9dcc1-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="9dcc1-104">Toto rozhraní API je v EF Core 3,1 nové.</span><span class="sxs-lookup"><span data-stu-id="9dcc1-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="9dcc1-105">Azure SQL Database poskytuje [celou řadu cenových možností](https://azure.microsoft.com/pricing/details/sql-database/single/) , které se obvykle konfigurují prostřednictvím webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9dcc1-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="9dcc1-106">Pokud ale spravujete schéma pomocí [EF Core migrace](xref:core/managing-schemas/migrations/index) , můžete zadat požadované možnosti v samotném modelu.</span><span class="sxs-lookup"><span data-stu-id="9dcc1-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="9dcc1-107">Úroveň služby databáze (edice) můžete zadat pomocí [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span><span class="sxs-lookup"><span data-stu-id="9dcc1-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="9dcc1-108">Můžete zadat maximální velikost databáze pomocí [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span><span class="sxs-lookup"><span data-stu-id="9dcc1-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="9dcc1-109">Úroveň výkonu databáze (SERVICE_OBJECTIVE) můžete určit pomocí [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span><span class="sxs-lookup"><span data-stu-id="9dcc1-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="9dcc1-110">Použijte [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) ke konfiguraci elastického fondu, protože tato hodnota není řetězcový literál:</span><span class="sxs-lookup"><span data-stu-id="9dcc1-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> <span data-ttu-id="9dcc1-111">Všechny podporované hodnoty najdete v [dokumentaci k příkazu ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span><span class="sxs-lookup"><span data-stu-id="9dcc1-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span></span>