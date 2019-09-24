---
title: Počítané sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: da106c94698a202744d7cd465aa84d0d72802833
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197229"
---
# <a name="computed-columns"></a><span data-ttu-id="74dfb-102">Vypočítané sloupce</span><span class="sxs-lookup"><span data-stu-id="74dfb-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="74dfb-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="74dfb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="74dfb-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="74dfb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="74dfb-105">Vypočítaný sloupec je sloupec, jehož hodnota je vypočítána v databázi.</span><span class="sxs-lookup"><span data-stu-id="74dfb-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="74dfb-106">Vypočítaný sloupec může použít jiné sloupce v tabulce k výpočtu jeho hodnoty.</span><span class="sxs-lookup"><span data-stu-id="74dfb-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="74dfb-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="74dfb-107">Conventions</span></span>

<span data-ttu-id="74dfb-108">Podle konvence se v modelu nevytvoří počítané sloupce.</span><span class="sxs-lookup"><span data-stu-id="74dfb-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="74dfb-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="74dfb-109">Data Annotations</span></span>

<span data-ttu-id="74dfb-110">U počítaných sloupců nelze konfigurovat datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="74dfb-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="74dfb-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="74dfb-111">Fluent API</span></span>

<span data-ttu-id="74dfb-112">Rozhraní Fluent API můžete použít k určení, že vlastnost by měla být namapována na vypočítaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="74dfb-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
