---
title: Maximální délka - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054163"
---
# <a name="maximum-length"></a><span data-ttu-id="872cc-102">Maximální délka</span><span class="sxs-lookup"><span data-stu-id="872cc-102">Maximum Length</span></span>

<span data-ttu-id="872cc-103">Konfigurace maximální délku poskytuje nápovědu k úložišti dat o příslušný datový typ chcete použít pro danou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="872cc-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="872cc-104">Maximální délka se vztahuje pouze na pole datové typy, jako například `string` a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="872cc-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="872cc-105">Rozhraní Entity Framework neprovádí žádné ověření maximální délku před předávání dat zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="872cc-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="872cc-106">Je zprostředkovatele nebo datového úložiště, ověřte, jestli je to vhodné.</span><span class="sxs-lookup"><span data-stu-id="872cc-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="872cc-107">Například pokud cílení na SQL Server, přesahuje maximální délku bude mít za následek výjimku jako datový typ sloupce základní neumožní nadbytečná data k uložení.</span><span class="sxs-lookup"><span data-stu-id="872cc-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="872cc-108">Konvence</span><span class="sxs-lookup"><span data-stu-id="872cc-108">Conventions</span></span>

<span data-ttu-id="872cc-109">Podle konvence je ponechán až zprostředkovatel databáze k výběru typu příslušná data pro vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="872cc-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="872cc-110">Pro vlastnosti, které mají délku zprostředkovatel databáze obvykle vyberte datový typ, který umožňuje nejdelší dobu data.</span><span class="sxs-lookup"><span data-stu-id="872cc-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="872cc-111">Například Microsoft SQL Server použije `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` Pokud sloupec slouží jako klíč).</span><span class="sxs-lookup"><span data-stu-id="872cc-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="872cc-112">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="872cc-112">Data Annotations</span></span>

<span data-ttu-id="872cc-113">Můžete nakonfigurovat maximální délku pro vlastnost datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="872cc-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="872cc-114">V tomto příkladu cílení na SQL Server, které jsou výsledkem by `nvarchar(500)` datový typ používaný.</span><span class="sxs-lookup"><span data-stu-id="872cc-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="872cc-115">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="872cc-115">Fluent API</span></span>

<span data-ttu-id="872cc-116">Rozhraní Fluent API můžete použít ke konfiguraci maximální délka pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="872cc-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="872cc-117">V tomto příkladu cílení na SQL Server, které jsou výsledkem by `nvarchar(500)` datový typ používaný.</span><span class="sxs-lookup"><span data-stu-id="872cc-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
