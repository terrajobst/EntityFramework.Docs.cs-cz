---
title: Dědičnost (relační databáze) – EF Core
description: Postup konfigurace dědičnosti typů entit v relační databázi pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824754"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="698b4-103">Dědičnost (relační databáze)</span><span class="sxs-lookup"><span data-stu-id="698b4-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="698b4-104">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="698b4-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="698b4-105">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="698b4-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="698b4-106">Dědičnost v modelu EF se používá k řízení způsobu reprezentace dědičnosti tříd entit v databázi.</span><span class="sxs-lookup"><span data-stu-id="698b4-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="698b4-107">V současné době je v EF Core implementován pouze vzor Table-per-Hierarchy (TPH).</span><span class="sxs-lookup"><span data-stu-id="698b4-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="698b4-108">Další běžné vzorce, jako je například tabulka na typ (TPT) a tabulka-podle konkrétního typu (TPC), ještě nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="698b4-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="698b4-109">Konvence</span><span class="sxs-lookup"><span data-stu-id="698b4-109">Conventions</span></span>

<span data-ttu-id="698b4-110">Ve výchozím nastavení bude dědění mapováno pomocí vzoru Table-per-Hierarchy (TPH).</span><span class="sxs-lookup"><span data-stu-id="698b4-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="698b4-111">K ukládání dat pro všechny typy v hierarchii používá typ TPH jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="698b4-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="698b4-112">Sloupec diskriminátoru slouží k identifikaci typu, který každý řádek představuje.</span><span class="sxs-lookup"><span data-stu-id="698b4-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="698b4-113">EF Core bude dědění nastavení pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů (další podrobnosti naleznete v tématu [Dědičnost](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="698b4-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="698b4-114">Níže je příklad znázorňující jednoduchý scénář dědičnosti a data uložená v relační tabulce databáze pomocí vzoru TPH.</span><span class="sxs-lookup"><span data-stu-id="698b4-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="698b4-115">Sloupec *diskriminátor* určuje, který typ *blogu* je uložený v každém řádku.</span><span class="sxs-lookup"><span data-stu-id="698b4-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![obrázek](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="698b4-117">Sloupce databáze se při použití mapování typu TPH automaticky v případě potřeby nepovolují s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="698b4-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="698b4-118">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="698b4-118">Data Annotations</span></span>

<span data-ttu-id="698b4-119">Ke konfiguraci dědičnosti nelze použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="698b4-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="698b4-120">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="698b4-120">Fluent API</span></span>

<span data-ttu-id="698b4-121">Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupce diskriminátoru a hodnot, které se používají k identifikaci každého typu v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="698b4-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="698b4-122">Konfigurace vlastnosti diskriminátoru</span><span class="sxs-lookup"><span data-stu-id="698b4-122">Configuring the discriminator property</span></span>

<span data-ttu-id="698b4-123">V předchozích příkladech je diskriminátor vytvořen jako [vlastnost Shadow](xref:core/modeling/shadow-properties) v základní entitě hierarchie.</span><span class="sxs-lookup"><span data-stu-id="698b4-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="698b4-124">Vzhledem k tomu, že se jedná o vlastnost v modelu, lze ji nakonfigurovat stejně jako jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="698b4-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="698b4-125">Například pro nastavení maximální délky při použití výchozího diskriminátoru podle konvence:</span><span class="sxs-lookup"><span data-stu-id="698b4-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="698b4-126">Diskriminátor lze také namapovat na vlastnost .NET v entitě a nakonfigurovat ji.</span><span class="sxs-lookup"><span data-stu-id="698b4-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="698b4-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="698b4-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="698b4-128">Sdílené sloupce</span><span class="sxs-lookup"><span data-stu-id="698b4-128">Shared columns</span></span>

<span data-ttu-id="698b4-129">Pokud mají dva typy entit stejné úrovně vlastnost se stejným názvem, budou ve výchozím nastavení namapovány na dva samostatné sloupce.</span><span class="sxs-lookup"><span data-stu-id="698b4-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="698b4-130">Pokud jsou ale kompatibilní, můžou být namapované na stejný sloupec:</span><span class="sxs-lookup"><span data-stu-id="698b4-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]