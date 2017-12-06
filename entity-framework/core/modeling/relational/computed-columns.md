---
title: "Počítané sloupce - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="computed-columns"></a><span data-ttu-id="11498-102">Počítané sloupce</span><span class="sxs-lookup"><span data-stu-id="11498-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="11498-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="11498-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="11498-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="11498-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="11498-105">Počítaný sloupec je sloupec, jehož hodnota je vypočítána v databázi.</span><span class="sxs-lookup"><span data-stu-id="11498-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="11498-106">Počítaný sloupec můžete použít jiné sloupce v tabulce pro výpočet jeho hodnoty.</span><span class="sxs-lookup"><span data-stu-id="11498-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="11498-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="11498-107">Conventions</span></span>

<span data-ttu-id="11498-108">Podle konvence počítané sloupce nejsou vytvořené v modelu.</span><span class="sxs-lookup"><span data-stu-id="11498-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="11498-109">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="11498-109">Data Annotations</span></span>

<span data-ttu-id="11498-110">Počítané sloupce nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="11498-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="11498-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="11498-111">Fluent API</span></span>

<span data-ttu-id="11498-112">Rozhraní Fluent API můžete použít k určení, že vlastnost by měla být mapována na počítaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="11498-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
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
