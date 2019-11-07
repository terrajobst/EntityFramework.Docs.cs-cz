---
title: Počítané sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655917"
---
# <a name="computed-columns"></a><span data-ttu-id="8a0bc-102">Vypočítané sloupce</span><span class="sxs-lookup"><span data-stu-id="8a0bc-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="8a0bc-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="8a0bc-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8a0bc-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="8a0bc-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8a0bc-105">Vypočítaný sloupec je sloupec, jehož hodnota je vypočítána v databázi.</span><span class="sxs-lookup"><span data-stu-id="8a0bc-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="8a0bc-106">Vypočítaný sloupec může použít jiné sloupce v tabulce k výpočtu jeho hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8a0bc-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="8a0bc-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="8a0bc-107">Conventions</span></span>

<span data-ttu-id="8a0bc-108">Podle konvence se v modelu nevytvoří počítané sloupce.</span><span class="sxs-lookup"><span data-stu-id="8a0bc-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8a0bc-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="8a0bc-109">Data Annotations</span></span>

<span data-ttu-id="8a0bc-110">U počítaných sloupců nelze konfigurovat datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="8a0bc-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8a0bc-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="8a0bc-111">Fluent API</span></span>

<span data-ttu-id="8a0bc-112">Rozhraní Fluent API můžete použít k určení, že vlastnost by měla být namapována na vypočítaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="8a0bc-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
