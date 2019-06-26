---
title: Relace – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 3731d30a15222a18ad6c729e010b9bf0994c82b2
ms.sourcegitcommit: 83c1e2fc034e5eb1fec1ebabc8d629ffcc7c0632
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2019
ms.locfileid: "67351351"
---
# <a name="relationships"></a><span data-ttu-id="8f185-102">Relace</span><span class="sxs-lookup"><span data-stu-id="8f185-102">Relationships</span></span>

<span data-ttu-id="8f185-103">Jak dvě entity, které definuje vztah vzájemně souvisí.</span><span class="sxs-lookup"><span data-stu-id="8f185-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="8f185-104">V relační databázi to je reprezentován omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="8f185-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="8f185-105">Většina ukázky v tomto článku použijte vztah jeden mnoho k předvedení konceptů.</span><span class="sxs-lookup"><span data-stu-id="8f185-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="8f185-106">Příklady relace 1: 1 a many-to-many viz [jiné vzory vztah](#other-relationship-patterns) na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="8f185-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="8f185-107">Definice pojmů</span><span class="sxs-lookup"><span data-stu-id="8f185-107">Definition of Terms</span></span>

<span data-ttu-id="8f185-108">Existuje několik termínů používaných k popisu relací</span><span class="sxs-lookup"><span data-stu-id="8f185-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="8f185-109">**Závislé entity:** Toto je entita, která obsahuje cizí klíčové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8f185-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="8f185-110">Někdy označovány jako "podřízený" relace.</span><span class="sxs-lookup"><span data-stu-id="8f185-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="8f185-111">**Hlavní entita:** Toto je entita, která obsahuje primární a alternativní klíčové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8f185-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="8f185-112">Někdy označovány jako "nadřazený" relace.</span><span class="sxs-lookup"><span data-stu-id="8f185-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="8f185-113">**Cizí klíč:** Vlastností v závislé entity, který se používá k ukládání hodnot hlavní klíčová vlastnost, která entita souvisí s.</span><span class="sxs-lookup"><span data-stu-id="8f185-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="8f185-114">**Klíč instančního objektu:** Vlastnost, která jednoznačně identifikuje instančního objektu entity.</span><span class="sxs-lookup"><span data-stu-id="8f185-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="8f185-115">To může být primární klíč nebo alternativního klíče.</span><span class="sxs-lookup"><span data-stu-id="8f185-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="8f185-116">**Navigační vlastnost:** Vlastnost definovaná pro entitu instančního objektu a/nebo závislé, který obsahuje odkazy na související entity(s).</span><span class="sxs-lookup"><span data-stu-id="8f185-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="8f185-117">**Navigační vlastnost kolekce:** Navigační vlastnost, která obsahuje odkazy na související entity.</span><span class="sxs-lookup"><span data-stu-id="8f185-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="8f185-118">**Vlastnost navigace odkaz:** Navigační vlastnost, která obsahuje odkaz na jednu související entity.</span><span class="sxs-lookup"><span data-stu-id="8f185-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="8f185-119">**Inverzní navigační vlastnost:** Když hovoříte o konkrétní navigační vlastnost, tento výraz odkazuje na vlastnost navigace na druhém konci vztahu.</span><span class="sxs-lookup"><span data-stu-id="8f185-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="8f185-120">Následující výpis kódu ukazuje vztah jeden mnoho mezi `Blog` a `Post`</span><span class="sxs-lookup"><span data-stu-id="8f185-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="8f185-121">`Post` je závislé entity</span><span class="sxs-lookup"><span data-stu-id="8f185-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="8f185-122">`Blog` je entita, instančního objektu</span><span class="sxs-lookup"><span data-stu-id="8f185-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="8f185-123">`Post.BlogId` je cizí klíč</span><span class="sxs-lookup"><span data-stu-id="8f185-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="8f185-124">`Blog.BlogId` je klíč instančního objektu (v tomto případě je primární klíč, místo alternativního klíče)</span><span class="sxs-lookup"><span data-stu-id="8f185-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="8f185-125">`Post.Blog` je odkaz na vlastnost navigace</span><span class="sxs-lookup"><span data-stu-id="8f185-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="8f185-126">`Blog.Posts` je navigační vlastnost kolekce</span><span class="sxs-lookup"><span data-stu-id="8f185-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="8f185-127">`Post.Blog` je inverzní navigační vlastnost `Blog.Posts` (a naopak)</span><span class="sxs-lookup"><span data-stu-id="8f185-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="8f185-128">Konvence</span><span class="sxs-lookup"><span data-stu-id="8f185-128">Conventions</span></span>

<span data-ttu-id="8f185-129">Podle konvence se vytvořit relaci po navigační vlastnost nalezených v typu.</span><span class="sxs-lookup"><span data-stu-id="8f185-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="8f185-130">Vlastnost je považován za vlastnost navigace, pokud jako skalární typ. nelze mapovat na typ, který odkazuje na aktuálním zprostředkovatelem databáze.</span><span class="sxs-lookup"><span data-stu-id="8f185-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="8f185-131">Vztahy, které jsou zjištěny podle konvence se vždy cílit na primární klíč instančního objektu entity.</span><span class="sxs-lookup"><span data-stu-id="8f185-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="8f185-132">Cílit na alternativního klíče, musí provést další konfigurace pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="8f185-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="8f185-133">Plně definované relace</span><span class="sxs-lookup"><span data-stu-id="8f185-133">Fully Defined Relationships</span></span>

<span data-ttu-id="8f185-134">Nejběžnější vzor pro relace je definovaný na obou stranách relace a vlastnost cizího klíče definovaná ve třídě závislé entit navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8f185-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="8f185-135">Pokud se najde dvojici vlastností navigace mezi dvěma typy, bude nakonfigurován jako inverzní navigační vlastnosti stejné relace.</span><span class="sxs-lookup"><span data-stu-id="8f185-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="8f185-136">Pokud subjektem závislé obsahuje vlastnost s názvem `<primary key property name>`, `<navigation property name><primary key property name>`, nebo `<principal entity name><primary key property name>` pak bude nakonfigurován jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="8f185-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="8f185-137">Pokud jsou definované mezi dvěma typy více navigačních vlastností (to znamená, více než jeden odlišné pár navigaci, které odkazují na sobě navzájem), pak žádné relace, bude vytvořen pomocí konvence a budete muset ručně nakonfigurovat k identifikaci jak spojením navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8f185-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="8f185-138">Žádná vlastnost cizího klíče</span><span class="sxs-lookup"><span data-stu-id="8f185-138">No Foreign Key Property</span></span>

<span data-ttu-id="8f185-139">Doporučuje se mít vlastnost cizího klíče definovaná ve třídě závislé entity, není povinné.</span><span class="sxs-lookup"><span data-stu-id="8f185-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="8f185-140">Pokud se nenajde žádná vlastnost cizího klíče, se bude zavádět stínové vlastnost cizího klíče s názvem `<navigation property name><principal key property name>` (viz [stínové vlastnosti](shadow-properties.md) Další informace).</span><span class="sxs-lookup"><span data-stu-id="8f185-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="8f185-141">Jednu navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="8f185-141">Single Navigation Property</span></span>

<span data-ttu-id="8f185-142">Včetně pouze jednu vlastnost navigace (žádné inverzní navigace a žádná vlastnost cizího klíče), stačí vztah definovaný podle konvence.</span><span class="sxs-lookup"><span data-stu-id="8f185-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="8f185-143">Můžete mít také jednu navigační vlastnost a vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="8f185-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="8f185-144">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="8f185-144">Cascade Delete</span></span>

<span data-ttu-id="8f185-145">Podle konvence se nastaví kaskádové odstranění *Cascade* pro požadované relace a *ClientSetNull* pro volitelné relace.</span><span class="sxs-lookup"><span data-stu-id="8f185-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="8f185-146">*Kaskádové* znamená, že se odstraní také závislých položek.</span><span class="sxs-lookup"><span data-stu-id="8f185-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="8f185-147">*ClientSetNull* znamená, že závislých položek, které nejsou načtené do paměti zůstane beze změny a musí být ručně odstranit nebo aktualizovat tak, aby odkazovala na platný hlavní entitu.</span><span class="sxs-lookup"><span data-stu-id="8f185-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="8f185-148">Pro entity, která jsou načtena do paměti EF Core se pokus o nastavení vlastnosti cizího klíče na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="8f185-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="8f185-149">Najdete v článku [povinné a nepovinné vztahy](#required-and-optional-relationships) části rozdíl mezi požadované a volitelné vztahy.</span><span class="sxs-lookup"><span data-stu-id="8f185-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="8f185-150">Zobrazit [Kaskádové odstraňování](../saving/cascade-delete.md) pro další podrobnosti o různých odstranit chování a výchozí hodnoty, které používají konvence.</span><span class="sxs-lookup"><span data-stu-id="8f185-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8f185-151">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="8f185-151">Data Annotations</span></span>

<span data-ttu-id="8f185-152">Existují dva anotacemi dat, které lze použít ke konfiguraci relace, `[ForeignKey]` a `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="8f185-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="8f185-153">Tyto jsou dostupné v `System.ComponentModel.DataAnnotations.Schema` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="8f185-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="8f185-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="8f185-154">[ForeignKey]</span></span>

<span data-ttu-id="8f185-155">Datové poznámky můžete použít ke konfiguraci vlastností, které slouží jako vlastnost cizího klíče pro daný vztah.</span><span class="sxs-lookup"><span data-stu-id="8f185-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="8f185-156">To se obvykle provádí při vlastnost cizího klíče nejde zjistit konvencí.</span><span class="sxs-lookup"><span data-stu-id="8f185-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="8f185-157">`[ForeignKey]` Poznámky můžete umístit na žádnou vlastnost navigace v relaci.</span><span class="sxs-lookup"><span data-stu-id="8f185-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="8f185-158">Není nutné přejít na navigační vlastnost ve třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="8f185-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="8f185-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="8f185-159">[InverseProperty]</span></span>

<span data-ttu-id="8f185-160">Datové poznámky můžete nakonfigurovat jak vlastnosti navigace entit dependent a principal spárovat.</span><span class="sxs-lookup"><span data-stu-id="8f185-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="8f185-161">To se obvykle provádí po více než jednu dvojici vlastností navigace mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="8f185-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="8f185-162">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="8f185-162">Fluent API</span></span>

<span data-ttu-id="8f185-163">Ke konfiguraci vztahu v rozhraní Fluent API, spusťte určením navigační vlastnosti, které tvoří relace.</span><span class="sxs-lookup"><span data-stu-id="8f185-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="8f185-164">`HasOne` nebo `HasMany` určuje vlastnost navigace typu entity jsou od konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8f185-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="8f185-165">Potom řetězit volání `WithOne` nebo `WithMany` k identifikaci inverzní navigace.</span><span class="sxs-lookup"><span data-stu-id="8f185-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="8f185-166">`HasOne`/`WithOne` se používají pro vlastnosti navigačního odkazu a `HasMany` / `WithMany` se používají pro navigační vlastnosti kolekce.</span><span class="sxs-lookup"><span data-stu-id="8f185-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="8f185-167">Jednu navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="8f185-167">Single Navigation Property</span></span>

<span data-ttu-id="8f185-168">Pokud máte jenom jednu navigační vlastnost, existují konstruktor bez parametrů přetížení `WithOne` a `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="8f185-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="8f185-169">To znamená, že je koncepčně referencí nebo kolekcí na druhém konci vztahu, ale neexistuje žádné navigační vlastnost zahrnuta do entity třídy.</span><span class="sxs-lookup"><span data-stu-id="8f185-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="8f185-170">Cizí klíč</span><span class="sxs-lookup"><span data-stu-id="8f185-170">Foreign Key</span></span>

<span data-ttu-id="8f185-171">Rozhraní Fluent API můžete použít ke konfiguraci vlastností, které slouží jako vlastnost cizího klíče pro daný vztah.</span><span class="sxs-lookup"><span data-stu-id="8f185-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="8f185-172">Následující výpis kódu ukazuje, jak nakonfigurovat složený cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="8f185-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="8f185-173">Můžete použít přetížení řetězce `HasForeignKey(...)` pro konfiguraci vlastnosti stínové jako cizí klíč (viz [stínové vlastnosti](shadow-properties.md) Další informace).</span><span class="sxs-lookup"><span data-stu-id="8f185-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="8f185-174">Doporučujeme explicitně přidat vlastnost stínové modelu před jeho použitím jako cizí klíč (jak je vidět níže).</span><span class="sxs-lookup"><span data-stu-id="8f185-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="8f185-175">Bez navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="8f185-175">Without Navigation Property</span></span>

<span data-ttu-id="8f185-176">Není nutné nemusí poskytnout navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8f185-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="8f185-177">Cizí klíč můžete jednoduše zadat na jedné straně relace.</span><span class="sxs-lookup"><span data-stu-id="8f185-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="8f185-178">Klíč objektu</span><span class="sxs-lookup"><span data-stu-id="8f185-178">Principal Key</span></span>

<span data-ttu-id="8f185-179">Pokud chcete cizí klíč k odkazování vlastnosti jiné než primární klíč, můžete použít rozhraní Fluent API nakonfigurovat vlastnost hlavního klíče pro daný vztah.</span><span class="sxs-lookup"><span data-stu-id="8f185-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="8f185-180">Vlastnost, která se konfiguruje automaticky jako hlavní klíče se být nastaveny jako alternativního klíče (viz [alternativní klíče](alternate-keys.md) Další informace).</span><span class="sxs-lookup"><span data-stu-id="8f185-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="8f185-181">Následující výpis kódu ukazuje, jak nakonfigurovat složený klíč instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="8f185-181">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="8f185-182">Pořadí, ve kterém můžete zadat hlavní klíčové vlastnosti musí odpovídat pořadí, ve kterém jsou uvedeny pro cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="8f185-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="8f185-183">Povinné a nepovinné relace</span><span class="sxs-lookup"><span data-stu-id="8f185-183">Required and Optional Relationships</span></span>

<span data-ttu-id="8f185-184">Rozhraní Fluent API můžete konfigurovat, jestli je vztah je povinná nebo volitelná.</span><span class="sxs-lookup"><span data-stu-id="8f185-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="8f185-185">Takže v konečném důsledku Určuje, zda vlastnost cizího klíče je povinná nebo volitelná.</span><span class="sxs-lookup"><span data-stu-id="8f185-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="8f185-186">To je nejužitečnější, pokud používáte cizí klíč stínové stavu.</span><span class="sxs-lookup"><span data-stu-id="8f185-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="8f185-187">Pokud máte ve své třídě entity vlastnost cizího klíče pak requiredness relace je určen podle toho, jestli vlastnost cizího klíče požadované nebo volitelné (viz [požadované a volitelné vlastnosti](required-optional.md) Další informace informace o).</span><span class="sxs-lookup"><span data-stu-id="8f185-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="8f185-188">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="8f185-188">Cascade Delete</span></span>

<span data-ttu-id="8f185-189">Rozhraní Fluent API můžete explicitně nakonfigurovat chování kaskádové odstranění pro daný vztah.</span><span class="sxs-lookup"><span data-stu-id="8f185-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="8f185-190">Zobrazit [Kaskádové odstraňování](../saving/cascade-delete.md) v části ukládání dat pro podrobnou diskuzi o jednotlivých možností.</span><span class="sxs-lookup"><span data-stu-id="8f185-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="8f185-191">Další způsoby vztah</span><span class="sxs-lookup"><span data-stu-id="8f185-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="8f185-192">1: 1</span><span class="sxs-lookup"><span data-stu-id="8f185-192">One-to-one</span></span>

<span data-ttu-id="8f185-193">Relace mít vlastnost navigace odkaz na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="8f185-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="8f185-194">Se řídí stejnými konvencemi jako jeden mnoho vztahů, ale je zaveden jedinečný index u vlastnosti cizího klíče k zajištění, že pouze jeden závislé souvisí s každý instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="8f185-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="8f185-195">EF zvolí jednu z entity, které chcete být závislé podle jeho schopnost detekovat vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="8f185-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="8f185-196">Pokud je zvolená nesprávné entity jako závislé, můžete použít rozhraní Fluent API to pokud chcete opravit.</span><span class="sxs-lookup"><span data-stu-id="8f185-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="8f185-197">Při konfiguraci relace s rozhraním API Fluent, můžete použít `HasOne` a `WithOne` metody.</span><span class="sxs-lookup"><span data-stu-id="8f185-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="8f185-198">Při konfiguraci cizího klíče je třeba zadat typ entity závislé – Všimněte si, že obecný parametr předán `HasForeignKey` v seznamu níže.</span><span class="sxs-lookup"><span data-stu-id="8f185-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="8f185-199">Ve vztahu jednoho k několika je jasné, že entity s navigační odkaz závisí a ten se shromažďováním je objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8f185-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="8f185-200">Ale to není tak v relaci 1: 1 - proto nutné explicitně definovat.</span><span class="sxs-lookup"><span data-stu-id="8f185-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="8f185-201">Many-to-many</span><span class="sxs-lookup"><span data-stu-id="8f185-201">Many-to-many</span></span>

<span data-ttu-id="8f185-202">Vztahy many-to-many bez třídu entity k reprezentaci tabulky spojení se zatím nepodporují.</span><span class="sxs-lookup"><span data-stu-id="8f185-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="8f185-203">Můžete však představují vztah many-to-many zahrnutím třídu entity pro tabulku spojení a mapování dvě samostatné 1 n relace.</span><span class="sxs-lookup"><span data-stu-id="8f185-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
