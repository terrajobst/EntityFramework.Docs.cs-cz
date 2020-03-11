---
title: Tabulka historie vlastní migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416841"
---
# <a name="custom-migrations-history-table"></a>Tabulka historie vlastních migrací

Ve výchozím nastavení EF Core udržuje přehled o tom, které migrace byly pro databázi aplikovány, jejich nahráním do tabulky s názvem `__EFMigrationsHistory`. Z různých důvodů možná budete chtít přizpůsobit tuto tabulku, aby lépe vyhovovala vašim potřebám.

> [!IMPORTANT]
> Pokud tabulku historie migrace *po* použití migrace přizpůsobíte, zodpovídáte za aktualizaci existující tabulky v databázi.

## <a name="schema-and-table-name"></a>Název schématu a tabulky

Název schématu a tabulky můžete změnit pomocí metody `MigrationsHistoryTable()` v `OnConfiguring()` (nebo `ConfigureServices()` na ASP.NET Core). Tady je příklad s použitím poskytovatele SQL Server EF Core.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Další změny

Pokud chcete nakonfigurovat další aspekty tabulky, přepište a nahraďte službu `IHistoryRepository` pro konkrétního zprostředkovatele. Tady je příklad změny názvu sloupce MigrationId na hodnotu *ID* na SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` se nachází uvnitř interního oboru názvů a v budoucích verzích se může změnit.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
