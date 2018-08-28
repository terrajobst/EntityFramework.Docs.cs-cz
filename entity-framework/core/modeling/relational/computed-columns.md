---
title: Vypočítané sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993950"
---
# <a name="computed-columns"></a><span data-ttu-id="ac680-102">Vypočítané sloupce</span><span class="sxs-lookup"><span data-stu-id="ac680-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="ac680-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="ac680-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ac680-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="ac680-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ac680-105">Počítaný sloupec je sloupec, jehož hodnota je vypočítána v databázi.</span><span class="sxs-lookup"><span data-stu-id="ac680-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="ac680-106">Počítaný sloupec můžete použít jiné sloupce v tabulce k výpočtu jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ac680-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="ac680-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="ac680-107">Conventions</span></span>

<span data-ttu-id="ac680-108">Podle konvence nejsou počítané sloupce vytvořené v modelu.</span><span class="sxs-lookup"><span data-stu-id="ac680-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ac680-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="ac680-109">Data Annotations</span></span>

<span data-ttu-id="ac680-110">S anotacemi dat nelze konfigurovat vypočítané sloupce.</span><span class="sxs-lookup"><span data-stu-id="ac680-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ac680-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="ac680-111">Fluent API</span></span>

<span data-ttu-id="ac680-112">Rozhraní Fluent API můžete použít k určení, že vlastnost by měla být mapována k počítanému sloupci.</span><span class="sxs-lookup"><span data-stu-id="ac680-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
