---
title: Mapování sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197214"
---
# <a name="column-mapping"></a><span data-ttu-id="1f30d-102">Mapování sloupců</span><span class="sxs-lookup"><span data-stu-id="1f30d-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="1f30d-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="1f30d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1f30d-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="1f30d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1f30d-105">Mapování sloupce určuje, která data sloupce by se měla dotazovat a Uložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="1f30d-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="1f30d-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="1f30d-106">Conventions</span></span>

<span data-ttu-id="1f30d-107">Podle konvence se každá vlastnost nastaví tak, aby se namapovala na sloupec se stejným názvem, jako má vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1f30d-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1f30d-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="1f30d-108">Data Annotations</span></span>

<span data-ttu-id="1f30d-109">Můžete použít datové poznámky ke konfiguraci sloupce, ke kterému je mapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1f30d-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="1f30d-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="1f30d-110">Fluent API</span></span>

<span data-ttu-id="1f30d-111">Rozhraní Fluent API můžete použít ke konfiguraci sloupce, ke kterému je namapovaná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1f30d-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
