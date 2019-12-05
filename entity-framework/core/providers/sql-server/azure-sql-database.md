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
# <a name="specifying-azure-sql-database-options"></a>Určení možností Azure SQL Database

>[!NOTE]
> Toto rozhraní API je v EF Core 3,1 nové.

Azure SQL Database poskytuje [celou řadu cenových možností](https://azure.microsoft.com/pricing/details/sql-database/single/) , které se obvykle konfigurují prostřednictvím webu Azure Portal. Pokud ale spravujete schéma pomocí [EF Core migrace](xref:core/managing-schemas/migrations/index) , můžete zadat požadované možnosti v samotném modelu.

Úroveň služby databáze (edice) můžete zadat pomocí [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

Můžete zadat maximální velikost databáze pomocí [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

Úroveň výkonu databáze (SERVICE_OBJECTIVE) můžete určit pomocí [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

Použijte [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) ke konfiguraci elastického fondu, protože tato hodnota není řetězcový literál:

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> Všechny podporované hodnoty najdete v [dokumentaci k příkazu ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).