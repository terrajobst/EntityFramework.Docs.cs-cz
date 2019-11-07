---
title: Tabulka historie vlastní migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655709"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="82192-102">Tabulka historie vlastních migrací</span><span class="sxs-lookup"><span data-stu-id="82192-102">Custom Migrations History Table</span></span>

<span data-ttu-id="82192-103">Ve výchozím nastavení EF Core udržuje přehled o tom, které migrace byly pro databázi aplikovány, jejich nahráním do tabulky s názvem `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="82192-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="82192-104">Z různých důvodů možná budete chtít přizpůsobit tuto tabulku, aby lépe vyhovovala vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="82192-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="82192-105">Pokud tabulku historie migrace *po* použití migrace přizpůsobíte, zodpovídáte za aktualizaci existující tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="82192-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="82192-106">Název schématu a tabulky</span><span class="sxs-lookup"><span data-stu-id="82192-106">Schema and table name</span></span>

<span data-ttu-id="82192-107">Název schématu a tabulky můžete změnit pomocí metody `MigrationsHistoryTable()` v `OnConfiguring()` (nebo `ConfigureServices()` na ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="82192-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="82192-108">Tady je příklad s použitím poskytovatele SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="82192-108">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="82192-109">Další změny</span><span class="sxs-lookup"><span data-stu-id="82192-109">Other changes</span></span>

<span data-ttu-id="82192-110">Pokud chcete nakonfigurovat další aspekty tabulky, přepište a nahraďte službu `IHistoryRepository` pro konkrétního zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="82192-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="82192-111">Tady je příklad změny názvu sloupce MigrationId na hodnotu *ID* na SQL Server.</span><span class="sxs-lookup"><span data-stu-id="82192-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="82192-112">`SqlServerHistoryRepository` se nachází uvnitř interního oboru názvů a v budoucích verzích se může změnit.</span><span class="sxs-lookup"><span data-stu-id="82192-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
