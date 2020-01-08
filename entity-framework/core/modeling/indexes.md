---
title: Indexy – EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502133"
---
# <a name="indexes"></a><span data-ttu-id="c2252-102">Indexy</span><span class="sxs-lookup"><span data-stu-id="c2252-102">Indexes</span></span>

<span data-ttu-id="c2252-103">Indexy představují společný pojem v mnoha úložištích dat.</span><span class="sxs-lookup"><span data-stu-id="c2252-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="c2252-104">I když se jejich implementace v úložišti dat může lišit, používají se k tomu, aby vyhledávání na základě sloupce (nebo sady sloupců) bylo efektivnější.</span><span class="sxs-lookup"><span data-stu-id="c2252-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

<span data-ttu-id="c2252-105">Indexy nelze vytvořit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="c2252-105">Indexes cannot be created using data annotations.</span></span> <span data-ttu-id="c2252-106">Rozhraní Fluent API můžete použít k určení indexu v jednom sloupci následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c2252-106">You can use the Fluent API to specify an index on a single column as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

<span data-ttu-id="c2252-107">Index můžete zadat také pro více než jeden sloupec:</span><span class="sxs-lookup"><span data-stu-id="c2252-107">You can also specify an index over more than one column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> <span data-ttu-id="c2252-108">Podle konvence se v každé vlastnosti (nebo sadě vlastností) vytvoří index, který se používá jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="c2252-108">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>
>
> <span data-ttu-id="c2252-109">EF Core podporuje pouze jeden index na jednu jedinečnou sadu vlastností.</span><span class="sxs-lookup"><span data-stu-id="c2252-109">EF Core only supports one index per distinct set of properties.</span></span> <span data-ttu-id="c2252-110">Pokud použijete rozhraní Fluent API ke konfiguraci indexu pro sadu vlastností, které už mají definovaný index, buď podle konvence, nebo z předchozí konfigurace, bude změna definice tohoto indexu.</span><span class="sxs-lookup"><span data-stu-id="c2252-110">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="c2252-111">To je užitečné, pokud chcete dále konfigurovat index, který byl vytvořen pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="c2252-111">This is useful if you want to further configure an index that was created by convention.</span></span>

## <a name="index-uniqueness"></a><span data-ttu-id="c2252-112">Jedinečnost indexu</span><span class="sxs-lookup"><span data-stu-id="c2252-112">Index uniqueness</span></span>

<span data-ttu-id="c2252-113">Ve výchozím nastavení nejsou indexy jedinečné: více řádků může mít pro sadu sloupců indexu stejné hodnoty (y).</span><span class="sxs-lookup"><span data-stu-id="c2252-113">By default, indexes aren't unique: multiple rows are allowed to have the same value(s) for the index's column set.</span></span> <span data-ttu-id="c2252-114">Rejstřík můžete označit jedinečným způsobem:</span><span class="sxs-lookup"><span data-stu-id="c2252-114">You can make an index unique as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

<span data-ttu-id="c2252-115">Pokud se pokusíte vložit více než jednu entitu se stejnými hodnotami pro sadu sloupců indexu, vyvolá se výjimka.</span><span class="sxs-lookup"><span data-stu-id="c2252-115">Attempting to insert more than one entity with the same values for the index's column set will cause an exception to be thrown.</span></span>

## <a name="index-name"></a><span data-ttu-id="c2252-116">Název indexu</span><span class="sxs-lookup"><span data-stu-id="c2252-116">Index name</span></span>

<span data-ttu-id="c2252-117">Podle konvence jsou indexy vytvořené v relační databázi pojmenované `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="c2252-117">By convention, indexes created in a relational database are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="c2252-118">U složených indexů se `<property name>` jako podtržítko oddělený seznam názvů vlastností.</span><span class="sxs-lookup"><span data-stu-id="c2252-118">For composite indexes, `<property name>` becomes an underscore separated list of property names.</span></span>

<span data-ttu-id="c2252-119">Rozhraní Fluent API můžete použít k nastavení názvu indexu vytvořeného v databázi:</span><span class="sxs-lookup"><span data-stu-id="c2252-119">You can use the Fluent API to set the name of the index created in the database:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a><span data-ttu-id="c2252-120">Filtr indexu</span><span class="sxs-lookup"><span data-stu-id="c2252-120">Index filter</span></span>

<span data-ttu-id="c2252-121">Některé relační databáze umožňují zadat filtrovaný nebo částečný index.</span><span class="sxs-lookup"><span data-stu-id="c2252-121">Some relational databases allow you to specify a filtered or partial index.</span></span> <span data-ttu-id="c2252-122">To umožňuje indexovat pouze podmnožinu hodnot sloupců, zmenšení velikosti indexu a zlepšení využití výkonu i místa na disku.</span><span class="sxs-lookup"><span data-stu-id="c2252-122">This allows you to index only a subset of a column's values, reducing the index's size and improving both performance and disk space usage.</span></span> <span data-ttu-id="c2252-123">Další informace o SQL Server filtrovaných indexech [najdete v dokumentaci](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).</span><span class="sxs-lookup"><span data-stu-id="c2252-123">For more information on SQL Server filtered indexes, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).</span></span>

<span data-ttu-id="c2252-124">Rozhraní Fluent API můžete použít k určení filtru pro index, který je k dispozici jako výraz SQL:</span><span class="sxs-lookup"><span data-stu-id="c2252-124">You can use the Fluent API to specify a filter on an index, provided as a SQL expression:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

<span data-ttu-id="c2252-125">Při použití poskytovatele SQL Server EF přidá `'IS NOT NULL'` filtr pro všechny sloupce s možnou hodnotou null, které jsou součástí jedinečného indexu.</span><span class="sxs-lookup"><span data-stu-id="c2252-125">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="c2252-126">K přepsání této konvence můžete uvést `null`ou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c2252-126">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a><span data-ttu-id="c2252-127">Zahrnuté sloupce</span><span class="sxs-lookup"><span data-stu-id="c2252-127">Included columns</span></span>

<span data-ttu-id="c2252-128">Některé relační databáze umožňují nakonfigurovat sadu sloupců, které jsou zahrnuté do indexu, ale nejsou součástí jeho "klíče".</span><span class="sxs-lookup"><span data-stu-id="c2252-128">Some relational databases allow you to configure a set of columns which get included in the index, but aren't part of its "key".</span></span> <span data-ttu-id="c2252-129">To může významně zlepšit výkon dotazů, když jsou všechny sloupce v dotazu zahrnuté do indexu jako sloupce Key nebo nonkey, protože k samotné tabulce nepotřebujete mít pøístup.</span><span class="sxs-lookup"><span data-stu-id="c2252-129">This can significantly improve query performance when all columns in the query are included in the index either as key or nonkey columns, as the table itself doesn't need to be accessed.</span></span> <span data-ttu-id="c2252-130">Další informace o zahrnutých sloupcích SQL Server [najdete v dokumentaci](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).</span><span class="sxs-lookup"><span data-stu-id="c2252-130">For more information on SQL Server included columns, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).</span></span>

<span data-ttu-id="c2252-131">V následujícím příkladu je sloupec `Url` součástí klíče indexu, takže jakékoli filtrování dotazů v tomto sloupci může index použít.</span><span class="sxs-lookup"><span data-stu-id="c2252-131">In the following example, the `Url` column is part of the index key, so any query filtering on that column can use the index.</span></span> <span data-ttu-id="c2252-132">Dotazy, které přistupují pouze k `Title` a `PublishedOn` sloupců, ale navíc nebudou potřebovat přístup k tabulce a budou fungovat efektivněji:</span><span class="sxs-lookup"><span data-stu-id="c2252-132">But in addition, queries accessing only the `Title` and `PublishedOn` columns will not need to access the table and will run more efficiently:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
