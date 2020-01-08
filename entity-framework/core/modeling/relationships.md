---
title: Relace – EF Core
description: Konfigurace vztahů mezi typy entit při použití Entity Framework Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 6b3e0636bfa266b78baafe1b6e318c9707294560
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502185"
---
# <a name="relationships"></a><span data-ttu-id="04212-103">Relace</span><span class="sxs-lookup"><span data-stu-id="04212-103">Relationships</span></span>

<span data-ttu-id="04212-104">Vztah určuje, jak vzájemně souvisí dvě entity.</span><span class="sxs-lookup"><span data-stu-id="04212-104">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="04212-105">V relační databázi to představuje omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="04212-105">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="04212-106">Většina ukázek v tomto článku používá relaci 1: n k předvedení konceptů.</span><span class="sxs-lookup"><span data-stu-id="04212-106">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="04212-107">Příklady relací 1:1 a m:n najdete v části [Další vzory vztahů](#other-relationship-patterns) na konci článku. "</span><span class="sxs-lookup"><span data-stu-id="04212-107">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="04212-108">Definice pojmů</span><span class="sxs-lookup"><span data-stu-id="04212-108">Definition of terms</span></span>

<span data-ttu-id="04212-109">K popisu vztahů se používá určitý počet výrazů.</span><span class="sxs-lookup"><span data-stu-id="04212-109">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="04212-110">**Závislá entita:** Toto je entita, která obsahuje vlastnosti cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="04212-110">**Dependent entity:** This is the entity that contains the foreign key properties.</span></span> <span data-ttu-id="04212-111">V relaci se někdy označuje jako "podřízený".</span><span class="sxs-lookup"><span data-stu-id="04212-111">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="04212-112">**Hlavní entita:** Toto je entita, která obsahuje vlastnosti primárního nebo alternativního klíče.</span><span class="sxs-lookup"><span data-stu-id="04212-112">**Principal entity:** This is the entity that contains the primary/alternate key properties.</span></span> <span data-ttu-id="04212-113">V relaci se někdy označuje jako "nadřazený".</span><span class="sxs-lookup"><span data-stu-id="04212-113">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="04212-114">**Cizí klíč:** Vlastnosti v závislé entitě, které se používají k ukládání hodnot hlavních klíčů pro související entitu</span><span class="sxs-lookup"><span data-stu-id="04212-114">**Foreign key:** The properties in the dependent entity that are used to store the principal key values for the related entity.</span></span>

* <span data-ttu-id="04212-115">**Hlavní klíč:** Vlastnosti, které jednoznačně identifikují hlavní entitu</span><span class="sxs-lookup"><span data-stu-id="04212-115">**Principal key:** The properties that uniquely identify the principal entity.</span></span> <span data-ttu-id="04212-116">Může se jednat o primární klíč nebo alternativní klíč.</span><span class="sxs-lookup"><span data-stu-id="04212-116">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="04212-117">**Navigační vlastnost:** Vlastnost definovaná na objektu zabezpečení nebo závislé entitě, která odkazuje na související entitu.</span><span class="sxs-lookup"><span data-stu-id="04212-117">**Navigation property:** A property defined on the principal and/or dependent entity that references the related entity.</span></span>

  * <span data-ttu-id="04212-118">**Navigační vlastnost kolekce:** Navigační vlastnost, která obsahuje odkazy na mnoho souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="04212-118">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="04212-119">**Referenční navigační vlastnost:** Navigační vlastnost, která obsahuje odkaz na jednu související entitu.</span><span class="sxs-lookup"><span data-stu-id="04212-119">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="04212-120">**Inverzní navigační vlastnost:** Při diskusi na konkrétní navigační vlastnost se tento termín odkazuje na vlastnost navigace na druhém konci relace.</span><span class="sxs-lookup"><span data-stu-id="04212-120">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>
  
* <span data-ttu-id="04212-121">**Vztah s odkazem na sebe:** Vztah, ve kterém jsou typy závislých a hlavních entit stejné.</span><span class="sxs-lookup"><span data-stu-id="04212-121">**Self-referencing relationship:** A relationship in which the dependent and the principal entity types are the same.</span></span>

<span data-ttu-id="04212-122">Následující kód ukazuje vztah 1: n mezi `Blog` a `Post`</span><span class="sxs-lookup"><span data-stu-id="04212-122">The following code shows a one-to-many relationship between `Blog` and `Post`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Full)]

* <span data-ttu-id="04212-123">`Post` je závislá entita.</span><span class="sxs-lookup"><span data-stu-id="04212-123">`Post` is the dependent entity</span></span>

* <span data-ttu-id="04212-124">`Blog` je hlavní entitou.</span><span class="sxs-lookup"><span data-stu-id="04212-124">`Blog` is the principal entity</span></span>

* <span data-ttu-id="04212-125">`Post.BlogId` je cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="04212-125">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="04212-126">`Blog.BlogId` je hlavní klíč (v tomto případě je to primární klíč, nikoli alternativní klíč).</span><span class="sxs-lookup"><span data-stu-id="04212-126">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="04212-127">`Post.Blog` je navigační vlastnost odkazu</span><span class="sxs-lookup"><span data-stu-id="04212-127">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="04212-128">`Blog.Posts` je navigační vlastnost kolekce</span><span class="sxs-lookup"><span data-stu-id="04212-128">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="04212-129">`Post.Blog` je inverzní navigační vlastnost `Blog.Posts` (a naopak).</span><span class="sxs-lookup"><span data-stu-id="04212-129">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

## <a name="conventions"></a><span data-ttu-id="04212-130">Konvence</span><span class="sxs-lookup"><span data-stu-id="04212-130">Conventions</span></span>

<span data-ttu-id="04212-131">Ve výchozím nastavení se relace vytvoří, když je u typu zjištěna vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="04212-131">By default, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="04212-132">Vlastnost je považována za navigační vlastnost, pokud typ, na který odkazuje, nemůže být namapován jako skalární typ aktuálním poskytovatelem databáze.</span><span class="sxs-lookup"><span data-stu-id="04212-132">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="04212-133">Relace zjištěné podle konvence budou vždycky cílit na primární klíč hlavní entity.</span><span class="sxs-lookup"><span data-stu-id="04212-133">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="04212-134">Aby bylo možné cílit na alternativní klíč, je nutné provést další konfiguraci pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="04212-134">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="04212-135">Plně definované relace</span><span class="sxs-lookup"><span data-stu-id="04212-135">Fully defined relationships</span></span>

<span data-ttu-id="04212-136">Nejběžnějším vzorem relací je mít vlastnosti navigace definované na obou koncích relace a vlastnost cizího klíče definované v třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="04212-136">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="04212-137">Pokud je mezi dvěma typy nalezen pár vlastností navigace, budou nakonfigurovány jako inverzní navigační vlastnosti stejné relace.</span><span class="sxs-lookup"><span data-stu-id="04212-137">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="04212-138">Pokud závislá entita obsahuje vlastnost s názvem, který odpovídá jednomu z těchto vzorů, pak se nakonfiguruje jako cizí klíč:</span><span class="sxs-lookup"><span data-stu-id="04212-138">If the dependent entity contains a property with a name matching one of these patterns then it will be configured as the foreign key:</span></span>
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Full&highlight=6,15-16)]

<span data-ttu-id="04212-139">V tomto příkladu se pro konfiguraci relace použijí zvýrazněné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="04212-139">In this example the highlighted properties will be used to configure the relationship.</span></span>

> [!NOTE]
> <span data-ttu-id="04212-140">Pokud je vlastnost primárním klíčem nebo je typu, který není kompatibilní s klíčovým objektem zabezpečení, nebude nakonfigurovaný jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="04212-140">If the property is the primary key or is of a type not compatible with the principal key then it won't be configured as the foreign key.</span></span>

> [!NOTE]
> <span data-ttu-id="04212-141">Před EF Core 3,0 vlastnost s názvem přesně stejná jako vlastnost hlavního klíče [byla také shodná s cizím klíčem](https://github.com/aspnet/EntityFrameworkCore/issues/13274) .</span><span class="sxs-lookup"><span data-stu-id="04212-141">Before EF Core 3.0 the property named exactly the same as the principal key property [was also matched as the foreign key](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="04212-142">Žádná vlastnost cizího klíče</span><span class="sxs-lookup"><span data-stu-id="04212-142">No foreign key property</span></span>

<span data-ttu-id="04212-143">I když se doporučuje mít vlastnost cizího klíče definovanou ve třídě závislé entity, není to nutné.</span><span class="sxs-lookup"><span data-stu-id="04212-143">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="04212-144">Pokud se nenalezne žádná vlastnost cizího klíče, zavede se [vlastnost stínového cizího klíče](shadow-properties.md) s názvem `<navigation property name><principal key property name>` nebo `<principal entity name><principal key property name>`, pokud se na závislém typu nenachází žádná navigace.</span><span class="sxs-lookup"><span data-stu-id="04212-144">If no foreign key property is found, a [shadow foreign key property](shadow-properties.md) will be introduced with the name `<navigation property name><principal key property name>` or `<principal entity name><principal key property name>` if no navigation is present on the dependent type.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=6,15)]

<span data-ttu-id="04212-145">V tomto příkladu je `BlogId` stín cizího klíče, protože před nedokončeným názvem navigace by byl redundantní.</span><span class="sxs-lookup"><span data-stu-id="04212-145">In this example the shadow foreign key is `BlogId` because prepending the navigation name would be redundant.</span></span>

> [!NOTE]
> <span data-ttu-id="04212-146">Pokud vlastnost se stejným názvem již existuje, bude název vlastnosti stínové kopie zaregistrovaný číslem.</span><span class="sxs-lookup"><span data-stu-id="04212-146">If a property with the same name already exists then the shadow property name will be suffixed with a number.</span></span>

### <a name="single-navigation-property"></a><span data-ttu-id="04212-147">Jednoduchá navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="04212-147">Single navigation property</span></span>

<span data-ttu-id="04212-148">Zahrnutím pouze jedné navigační vlastnosti (bez invertované navigace a žádné vlastnosti cizího klíče) není dostatečná relace definovaná v konvenci.</span><span class="sxs-lookup"><span data-stu-id="04212-148">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="04212-149">Můžete mít také jednu navigační vlastnost a vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="04212-149">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=OneNavigation&highlight=6)]

### <a name="limitations"></a><span data-ttu-id="04212-150">Omezení</span><span class="sxs-lookup"><span data-stu-id="04212-150">Limitations</span></span>

<span data-ttu-id="04212-151">Pokud je mezi dvěma typy definováno více vlastností navigace (to znamená více než pouze jedna dvojice navigačních oblastí, které odkazují na sebe), jsou relace reprezentované navigačními vlastnostmi dvojznačné.</span><span class="sxs-lookup"><span data-stu-id="04212-151">When there are multiple navigation properties defined between two types (that is, more than just one pair of navigations that point to each other) the relationships represented by the navigation properties are ambiguous.</span></span> <span data-ttu-id="04212-152">Budete je muset ručně nakonfigurovat, aby se vyřešila nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="04212-152">You will need to manually configure them to resolve the ambiguity.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="04212-153">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="04212-153">Cascade delete</span></span>

<span data-ttu-id="04212-154">Podle konvence se kaskádové odstranění nastaví na *kaskády* pro požadované relace a *ClientSetNull* pro volitelné vztahy.</span><span class="sxs-lookup"><span data-stu-id="04212-154">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="04212-155">*Cascade* znamená, že jsou také odstraněny závislé entity.</span><span class="sxs-lookup"><span data-stu-id="04212-155">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="04212-156">*ClientSetNull* znamená, že závislé entity, které nejsou načteny do paměti, zůstanou beze změny a je nutné je ručně odstranit nebo aktualizovat, aby odkazovaly na platnou objektovou entitu.</span><span class="sxs-lookup"><span data-stu-id="04212-156">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="04212-157">V případě entit, které jsou načteny do paměti, se EF Core pokusí nastavit vlastnosti cizího klíče na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="04212-157">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="04212-158">Rozdíl mezi požadovanými a volitelnými vztahy najdete v části [vyžadované a volitelné relace](#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="04212-158">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="04212-159">Další podrobnosti o různých chováních při odstraňování a výchozích hodnotách, které používá konvence, najdete v tématu [kaskádová odstranění](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="04212-159">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="manual-configuration"></a><span data-ttu-id="04212-160">Ruční konfigurace</span><span class="sxs-lookup"><span data-stu-id="04212-160">Manual configuration</span></span>

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="04212-161">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="04212-161">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="04212-162">Pokud chcete nakonfigurovat relaci v rozhraní Fluent API, začněte tím, že určíte navigační vlastnosti, které tvoří relaci.</span><span class="sxs-lookup"><span data-stu-id="04212-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="04212-163">`HasOne` nebo `HasMany` identifikuje navigační vlastnost u typu entity, na které začínáte konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="04212-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="04212-164">Pak řetězení volání `WithOne` nebo `WithMany` k identifikaci invertované navigace.</span><span class="sxs-lookup"><span data-stu-id="04212-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="04212-165">`HasOne`/`WithOne` se používají pro referenční vlastnosti navigace a `HasMany`/`WithMany` se používají pro navigační vlastnosti kolekce.</span><span class="sxs-lookup"><span data-stu-id="04212-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=8-10)]

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="04212-166">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="04212-166">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="04212-167">Pomocí datových poznámek můžete nakonfigurovat, jak se mají spárovat vlastnosti navigace na závislých a hlavních entitách.</span><span class="sxs-lookup"><span data-stu-id="04212-167">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="04212-168">To se obvykle provádí, pokud existuje více než jedna dvojice vlastností navigace mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="04212-168">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?name=InverseProperty&highlight=20,23)]

> [!NOTE]
> <span data-ttu-id="04212-169">Můžete použít pouze [povinné] u vlastností závislé entity, aby se ovlivnila požadovaná vlastnost vztahu.</span><span class="sxs-lookup"><span data-stu-id="04212-169">You can only use [Required] on properties on the dependent entity to impact the requiredness of the relationship.</span></span> <span data-ttu-id="04212-170">[Požadováno] při navigaci z hlavní entity je obvykle ignorováno, ale může způsobit, že entita bude závislá na sobě.</span><span class="sxs-lookup"><span data-stu-id="04212-170">[Required] on the navigation from the principal entity is usually ignored, but it may cause the entity to become the dependent one.</span></span>

> [!NOTE]
> <span data-ttu-id="04212-171">Poznámky k datům `[ForeignKey]` a `[InverseProperty]` jsou k dispozici v oboru názvů `System.ComponentModel.DataAnnotations.Schema`.</span><span class="sxs-lookup"><span data-stu-id="04212-171">The data annotations `[ForeignKey]` and `[InverseProperty]` are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span> <span data-ttu-id="04212-172">`[Required]` je k dispozici v oboru názvů `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="04212-172">`[Required]` is available in the `System.ComponentModel.DataAnnotations` namespace.</span></span>

---

### <a name="single-navigation-property"></a><span data-ttu-id="04212-173">Jednoduchá navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="04212-173">Single navigation property</span></span>

<span data-ttu-id="04212-174">Pokud máte pouze jednu navigační vlastnost, existují přetížení `WithOne` a `WithMany`bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="04212-174">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="04212-175">To znamená, že na konci relace existuje koncepční odkaz nebo kolekce, ale ve třídě entity není obsažena žádná vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="04212-175">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?name=OneNavigation&highlight=8-10)]

### <a name="foreign-key"></a><span data-ttu-id="04212-176">Cizí klíč</span><span class="sxs-lookup"><span data-stu-id="04212-176">Foreign key</span></span>

#### <a name="fluent-api-simple-keytabfluent-api-simple-key"></a>[<span data-ttu-id="04212-177">Rozhraní Fluent API (jednoduchý klíč)</span><span class="sxs-lookup"><span data-stu-id="04212-177">Fluent API (simple key)</span></span>](#tab/fluent-api-simple-key)

<span data-ttu-id="04212-178">Pomocí rozhraní Fluent API můžete nakonfigurovat, která vlastnost se má použít jako vlastnost cizího klíče pro daný vztah:</span><span class="sxs-lookup"><span data-stu-id="04212-178">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?name=ForeignKey&highlight=11)]

#### <a name="fluent-api-composite-keytabfluent-api-composite-key"></a>[<span data-ttu-id="04212-179">Rozhraní Fluent API (složený klíč)</span><span class="sxs-lookup"><span data-stu-id="04212-179">Fluent API (composite key)</span></span>](#tab/fluent-api-composite-key)

<span data-ttu-id="04212-180">Pomocí rozhraní Fluent API můžete nakonfigurovat, které vlastnosti se mají použít jako vlastnosti složeného cizího klíče pro danou relaci:</span><span class="sxs-lookup"><span data-stu-id="04212-180">You can use the Fluent API to configure which properties should be used as the composite foreign key properties for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?name=CompositeForeignKey&highlight=13)]

#### <a name="data-annotations-simple-keytabdata-annotations-simple-key"></a>[<span data-ttu-id="04212-181">Datové poznámky (jednoduchý klíč)</span><span class="sxs-lookup"><span data-stu-id="04212-181">Data annotations (simple key)</span></span>](#tab/data-annotations-simple-key)

<span data-ttu-id="04212-182">Pomocí datových poznámek můžete nakonfigurovat, která vlastnost má být použita jako vlastnost cizího klíče pro danou relaci.</span><span class="sxs-lookup"><span data-stu-id="04212-182">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="04212-183">To se obvykle provádí v případě, že vlastnost cizího klíče není zjištěna podle konvence:</span><span class="sxs-lookup"><span data-stu-id="04212-183">This is typically done when the foreign key property is not discovered by convention:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?name=ForeignKey&highlight=17)]

> [!TIP]  
> <span data-ttu-id="04212-184">Anotaci `[ForeignKey]` lze umístit buď do vlastnosti navigace v relaci.</span><span class="sxs-lookup"><span data-stu-id="04212-184">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="04212-185">Nepotřebujete přejít na navigační vlastnost v třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="04212-185">It does not need to go on the navigation property in the dependent entity class.</span></span>

> [!NOTE]
> <span data-ttu-id="04212-186">Vlastnost zadaná pomocí `[ForeignKey]` pro vlastnost navigace nemusí existovat závislého typu.</span><span class="sxs-lookup"><span data-stu-id="04212-186">The property specified using `[ForeignKey]` on a navigation property doesn't need to exist the the dependent type.</span></span> <span data-ttu-id="04212-187">V tomto případě se pro vytvoření stínového cizího klíče použije zadaný název.</span><span class="sxs-lookup"><span data-stu-id="04212-187">In this case the specified name will be used to create a shadow foreign key.</span></span>

---

#### <a name="shadow-foreign-key"></a><span data-ttu-id="04212-188">Stínový cizí klíč</span><span class="sxs-lookup"><span data-stu-id="04212-188">Shadow foreign key</span></span>

<span data-ttu-id="04212-189">Pomocí přetížení řetězců `HasForeignKey(...)` můžete nakonfigurovat vlastnost Shadow jako cizí klíč (Další informace najdete v tématu [vlastnosti stínu](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="04212-189">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="04212-190">Do modelu doporučujeme explicitně přidat vlastnost Shadow, než ji použijete jako cizí klíč (jak je vidět níže).</span><span class="sxs-lookup"><span data-stu-id="04212-190">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs?name=ShadowForeignKey&highlight=10,16)]

#### <a name="foreign-key-constraint-name"></a><span data-ttu-id="04212-191">Název omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="04212-191">Foreign key constraint name</span></span>

<span data-ttu-id="04212-192">Podle konvence při cílení na relační databázi jsou omezení cizího klíče pojmenována FK_<dependent type name>_<principal type name>_<foreign key property name>.</span><span class="sxs-lookup"><span data-stu-id="04212-192">By convention, when targeting a relational database, foreign key constraints are named FK_<dependent type name>_<principal type name>_<foreign key property name>.</span></span> <span data-ttu-id="04212-193">Pro složené cizí klíče se <foreign key property name> jako podtržítko oddělený seznam názvů vlastností cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="04212-193">For composite foreign keys <foreign key property name> becomes an underscore separated list of foreign key property names.</span></span>

<span data-ttu-id="04212-194">Název omezení můžete také nakonfigurovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="04212-194">You can also configure the constraint name as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ConstraintName.cs?name=ConstraintName&highlight=6-7)]

### <a name="without-navigation-property"></a><span data-ttu-id="04212-195">Bez navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="04212-195">Without navigation property</span></span>

<span data-ttu-id="04212-196">Nemusíte nutně zadávat navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="04212-196">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="04212-197">Můžete jednoduše zadat cizí klíč na jednu stranu relace.</span><span class="sxs-lookup"><span data-stu-id="04212-197">You can simply provide a foreign key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?name=NoNavigation&highlight=8-11)]

### <a name="principal-key"></a><span data-ttu-id="04212-198">Hlavní klíč</span><span class="sxs-lookup"><span data-stu-id="04212-198">Principal key</span></span>

<span data-ttu-id="04212-199">Pokud chcete, aby cizí klíč odkazoval na jinou vlastnost než na primární klíč, můžete pro relaci nakonfigurovat vlastnost klíče zabezpečení pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="04212-199">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="04212-200">Vlastnost, kterou nakonfigurujete jako hlavní klíč, se automaticky nastaví jako [alternativní klíč](alternate-keys.md).</span><span class="sxs-lookup"><span data-stu-id="04212-200">The property that you configure as the principal key will automatically be setup as an [alternate key](alternate-keys.md).</span></span>

#### <a name="simple-keytabsimple-key"></a>[<span data-ttu-id="04212-201">Jednoduchý klíč</span><span class="sxs-lookup"><span data-stu-id="04212-201">Simple key</span></span>](#tab/simple-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

#### <a name="composite-keytabcomposite-key"></a>[<span data-ttu-id="04212-202">Složený klíč</span><span class="sxs-lookup"><span data-stu-id="04212-202">Composite key</span></span>](#tab/composite-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=CompositePrincipalKey&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="04212-203">Pořadí, ve kterém určíte vlastnosti hlavního klíče, musí odpovídat pořadí, ve kterém jsou zadané pro cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="04212-203">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

---

### <a name="required-and-optional-relationships"></a><span data-ttu-id="04212-204">Povinné a volitelné relace</span><span class="sxs-lookup"><span data-stu-id="04212-204">Required and optional relationships</span></span>

<span data-ttu-id="04212-205">Pomocí rozhraní Fluent API můžete nakonfigurovat, jestli je relace povinná nebo volitelná.</span><span class="sxs-lookup"><span data-stu-id="04212-205">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="04212-206">Nakonec tato možnost určuje, jestli je vlastnost cizího klíče povinná nebo volitelná.</span><span class="sxs-lookup"><span data-stu-id="04212-206">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="04212-207">To je nejužitečnější při použití cizího klíče stavu stínové kopie.</span><span class="sxs-lookup"><span data-stu-id="04212-207">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="04212-208">Máte-li ve třídě entity vlastnost cizího klíče, je vyžadována požadovaná vlastnost vztahu na základě toho, zda je vlastnost cizího klíče povinná nebo volitelná (Další informace naleznete v části [povinné a volitelné vlastnosti](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="04212-208">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=6)]

> [!NOTE]
> <span data-ttu-id="04212-209">Volání `IsRequired(false)` také nastaví vlastnost cizího klíče jako volitelnou, pokud není konfigurována jinak.</span><span class="sxs-lookup"><span data-stu-id="04212-209">Calling `IsRequired(false)` also makes the foreign key property optional unless it's configured otherwise.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="04212-210">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="04212-210">Cascade delete</span></span>

<span data-ttu-id="04212-211">Rozhraní Fluent API můžete použít ke konfiguraci chování kaskádového odstranění pro daný vztah explicitně.</span><span class="sxs-lookup"><span data-stu-id="04212-211">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="04212-212">Podrobné informace o jednotlivých možnostech najdete v části [kaskádové odstranění](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="04212-212">See [Cascade Delete](../saving/cascade-delete.md) for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=6)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="04212-213">Další vzory vztahů</span><span class="sxs-lookup"><span data-stu-id="04212-213">Other relationship patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="04212-214">Relace jednoho k jednomu jinému</span><span class="sxs-lookup"><span data-stu-id="04212-214">One-to-one</span></span>

<span data-ttu-id="04212-215">Jedna až jedna relace má referenční navigační vlastnost na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="04212-215">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="04212-216">Dodržují stejné konvence jako relace 1: n, ale do vlastnosti cizího klíče se zavedl jedinečný index, který zaručí, že k jednotlivým objektům zabezpečení souvisí jenom jedna závislá hodnota.</span><span class="sxs-lookup"><span data-stu-id="04212-216">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=OneToOne&highlight=6,15-16)]

> [!NOTE]  
> <span data-ttu-id="04212-217">EF vybere jednu z entit, které budou závislé na závislosti na schopnosti detekovat vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="04212-217">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="04212-218">Pokud je jako závislá vybraná nesprávná entita, můžete ji opravit pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="04212-218">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="04212-219">Když konfigurujete relaci s rozhraním API Fluent, použijete metody `HasOne` a `WithOne`.</span><span class="sxs-lookup"><span data-stu-id="04212-219">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="04212-220">Při konfiguraci cizího klíče musíte zadat typ závislé entity – Všimněte si obecného parametru poskytnutého `HasForeignKey` v níže uvedeném seznamu.</span><span class="sxs-lookup"><span data-stu-id="04212-220">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="04212-221">V relaci 1: n je jasné, že entita s referenční navigací je závislá a ta, která je v kolekci, je objektem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="04212-221">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="04212-222">Nejedná se ale o relaci 1:1, takže je potřeba explicitně definovat.</span><span class="sxs-lookup"><span data-stu-id="04212-222">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="04212-223">Many-to-many</span><span class="sxs-lookup"><span data-stu-id="04212-223">Many-to-many</span></span>

<span data-ttu-id="04212-224">Relace m:n bez třídy entity představující tabulku JOIN se ještě nepodporují.</span><span class="sxs-lookup"><span data-stu-id="04212-224">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="04212-225">Můžete však znázornit relaci n:n zahrnutím třídy entity pro tabulku JOIN a mapováním dvou samostatných vztahů 1: n.</span><span class="sxs-lookup"><span data-stu-id="04212-225">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11-14,16-19,39-46)]
