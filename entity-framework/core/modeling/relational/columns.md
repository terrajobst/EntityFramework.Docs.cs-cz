---
title: Mapování sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929859"
---
# <a name="column-mapping"></a><span data-ttu-id="965bc-102">Mapování sloupce</span><span class="sxs-lookup"><span data-stu-id="965bc-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="965bc-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="965bc-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="965bc-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="965bc-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="965bc-105">Mapování sloupce určuje, jaká data sloupec by měl posílat dotaz z a uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="965bc-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="965bc-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="965bc-106">Conventions</span></span>

<span data-ttu-id="965bc-107">Podle konvence se k mapování sloupců se stejným názvem jako vlastnost nastavení každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="965bc-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="965bc-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="965bc-108">Data Annotations</span></span>

<span data-ttu-id="965bc-109">Anotací dat můžete použít ke konfiguraci sloupců, ke kterému je namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="965bc-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="965bc-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="965bc-110">Fluent API</span></span>

<span data-ttu-id="965bc-111">Rozhraní Fluent API můžete použít ke konfiguraci sloupců, ke kterému je namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="965bc-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
