---
title: Relace – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197653"
---
# <a name="relationships"></a><span data-ttu-id="5a094-102">Relace</span><span class="sxs-lookup"><span data-stu-id="5a094-102">Relationships</span></span>

<span data-ttu-id="5a094-103">Vztah určuje, jak vzájemně souvisí dvě entity.</span><span class="sxs-lookup"><span data-stu-id="5a094-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="5a094-104">V relační databázi to představuje omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5a094-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="5a094-105">Většina ukázek v tomto článku používá relaci 1: n k předvedení konceptů.</span><span class="sxs-lookup"><span data-stu-id="5a094-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="5a094-106">Příklady relací 1:1 a m:n najdete v části [Další vzory vztahů](#other-relationship-patterns) na konci článku. "</span><span class="sxs-lookup"><span data-stu-id="5a094-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="5a094-107">Definice pojmů</span><span class="sxs-lookup"><span data-stu-id="5a094-107">Definition of Terms</span></span>

<span data-ttu-id="5a094-108">K popisu vztahů se používá určitý počet výrazů.</span><span class="sxs-lookup"><span data-stu-id="5a094-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="5a094-109">**Závislá entita:** Toto je entita, která obsahuje vlastnosti cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5a094-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="5a094-110">V relaci se někdy označuje jako "podřízený".</span><span class="sxs-lookup"><span data-stu-id="5a094-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="5a094-111">**Hlavní entita:** Toto je entita, která obsahuje vlastnosti primárního a alternativního klíče.</span><span class="sxs-lookup"><span data-stu-id="5a094-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="5a094-112">V relaci se někdy označuje jako "nadřazený".</span><span class="sxs-lookup"><span data-stu-id="5a094-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="5a094-113">**Cizí klíč:** Vlastnosti v závislé entitě, která se používá k uložení hodnot vlastnosti hlavního klíče, se kterou entita souvisí</span><span class="sxs-lookup"><span data-stu-id="5a094-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="5a094-114">**Hlavní klíč:** Vlastnosti, které jedinečně identifikují hlavní entitu.</span><span class="sxs-lookup"><span data-stu-id="5a094-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="5a094-115">Může se jednat o primární klíč nebo alternativní klíč.</span><span class="sxs-lookup"><span data-stu-id="5a094-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="5a094-116">**Navigační vlastnost:** Vlastnost definovaná na objektu zabezpečení nebo závislé entitě, která obsahuje odkazy na související entity (y).</span><span class="sxs-lookup"><span data-stu-id="5a094-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="5a094-117">**Navigační vlastnost kolekce:** Navigační vlastnost, která obsahuje odkazy na mnoho souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="5a094-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="5a094-118">**Referenční navigační vlastnost:** Navigační vlastnost, která obsahuje odkaz na jednu související entitu.</span><span class="sxs-lookup"><span data-stu-id="5a094-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="5a094-119">**Inverzní navigační vlastnost:** Při diskusi na konkrétní navigační vlastnost se tento termín odkazuje na vlastnost navigace na druhém konci relace.</span><span class="sxs-lookup"><span data-stu-id="5a094-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="5a094-120">Následující výpis kódu ukazuje vztah 1: n mezi `Blog` a`Post`</span><span class="sxs-lookup"><span data-stu-id="5a094-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="5a094-121">`Post`je závislá entita</span><span class="sxs-lookup"><span data-stu-id="5a094-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="5a094-122">`Blog`je hlavní entitou</span><span class="sxs-lookup"><span data-stu-id="5a094-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="5a094-123">`Post.BlogId`je cizí klíč</span><span class="sxs-lookup"><span data-stu-id="5a094-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="5a094-124">`Blog.BlogId`je hlavní klíč (v tomto případě je to primární klíč, nikoli alternativní klíč).</span><span class="sxs-lookup"><span data-stu-id="5a094-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="5a094-125">`Post.Blog`je referenční navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="5a094-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="5a094-126">`Blog.Posts`je navigační vlastnost kolekce</span><span class="sxs-lookup"><span data-stu-id="5a094-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="5a094-127">`Post.Blog`je inverzní vlastnost `Blog.Posts` navigace (a naopak).</span><span class="sxs-lookup"><span data-stu-id="5a094-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="5a094-128">Konvence</span><span class="sxs-lookup"><span data-stu-id="5a094-128">Conventions</span></span>

<span data-ttu-id="5a094-129">Podle konvence se vytvoří relace, když je u typu zjištěna vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="5a094-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="5a094-130">Vlastnost je považována za navigační vlastnost, pokud typ, na který odkazuje, nemůže být namapován jako skalární typ aktuálním poskytovatelem databáze.</span><span class="sxs-lookup"><span data-stu-id="5a094-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="5a094-131">Relace zjištěné podle konvence budou vždycky cílit na primární klíč hlavní entity.</span><span class="sxs-lookup"><span data-stu-id="5a094-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="5a094-132">Aby bylo možné cílit na alternativní klíč, je nutné provést další konfiguraci pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="5a094-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="5a094-133">Plně definované relace</span><span class="sxs-lookup"><span data-stu-id="5a094-133">Fully Defined Relationships</span></span>

<span data-ttu-id="5a094-134">Nejběžnějším vzorem relací je mít vlastnosti navigace definované na obou koncích relace a vlastnost cizího klíče definované v třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="5a094-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="5a094-135">Pokud je mezi dvěma typy nalezen pár vlastností navigace, budou nakonfigurovány jako inverzní navigační vlastnosti stejné relace.</span><span class="sxs-lookup"><span data-stu-id="5a094-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="5a094-136">Pokud závislá entita obsahuje vlastnost s názvem `<primary key property name>`, `<navigation property name><primary key property name>`, nebo `<principal entity name><primary key property name>` se nakonfiguruje jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="5a094-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="5a094-137">Pokud je mezi dvěma typy definováno více vlastností navigace (tj. více než jeden odlišný pár navigace, které navzájem odkazují), nebudou vytvořeny žádné vztahy podle konvence a bude nutné je ručně nakonfigurovat, abyste identifikovali, jak párování vlastností navigace nahoru.</span><span class="sxs-lookup"><span data-stu-id="5a094-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="5a094-138">Žádná vlastnost cizího klíče</span><span class="sxs-lookup"><span data-stu-id="5a094-138">No Foreign Key Property</span></span>

<span data-ttu-id="5a094-139">I když se doporučuje mít vlastnost cizího klíče definovanou ve třídě závislé entity, není to nutné.</span><span class="sxs-lookup"><span data-stu-id="5a094-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="5a094-140">Pokud se nenalezne žádná vlastnost cizího klíče, zavedený s názvem `<navigation property name><principal key property name>` se vytvoří vlastnost stínového cizího klíče (Další informace najdete v tématu [vlastnosti stínu](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="5a094-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="5a094-141">Jednoduchá navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="5a094-141">Single Navigation Property</span></span>

<span data-ttu-id="5a094-142">Zahrnutím pouze jedné navigační vlastnosti (bez invertované navigace a žádné vlastnosti cizího klíče) není dostatečná relace definovaná v konvenci.</span><span class="sxs-lookup"><span data-stu-id="5a094-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="5a094-143">Můžete mít také jednu navigační vlastnost a vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5a094-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="5a094-144">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="5a094-144">Cascade Delete</span></span>

<span data-ttu-id="5a094-145">Podle konvence se kaskádové odstranění nastaví na *kaskády* pro požadované relace a *ClientSetNull* pro volitelné vztahy.</span><span class="sxs-lookup"><span data-stu-id="5a094-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="5a094-146">*Cascade* znamená, že jsou také odstraněny závislé entity.</span><span class="sxs-lookup"><span data-stu-id="5a094-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="5a094-147">*ClientSetNull* znamená, že závislé entity, které nejsou načteny do paměti, zůstanou beze změny a je nutné je ručně odstranit nebo aktualizovat, aby odkazovaly na platnou objektovou entitu.</span><span class="sxs-lookup"><span data-stu-id="5a094-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="5a094-148">V případě entit, které jsou načteny do paměti, se EF Core pokusí nastavit vlastnosti cizího klíče na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="5a094-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="5a094-149">Rozdíl mezi požadovanými a volitelnými vztahy najdete v části [vyžadované a volitelné relace](#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="5a094-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="5a094-150">Další podrobnosti o různých chováních při odstraňování a výchozích hodnotách, které používá konvence, najdete v tématu [kaskádová odstranění](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="5a094-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5a094-151">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="5a094-151">Data Annotations</span></span>

<span data-ttu-id="5a094-152">K dispozici jsou dva datové poznámky, které lze použít ke konfiguraci `[ForeignKey]` relací `[InverseProperty]`a.</span><span class="sxs-lookup"><span data-stu-id="5a094-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="5a094-153">Ty jsou k dispozici `System.ComponentModel.DataAnnotations.Schema` v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5a094-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="5a094-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="5a094-154">[ForeignKey]</span></span>

<span data-ttu-id="5a094-155">Pomocí datových poznámek můžete nakonfigurovat, která vlastnost má být použita jako vlastnost cizího klíče pro danou relaci.</span><span class="sxs-lookup"><span data-stu-id="5a094-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="5a094-156">To se obvykle provádí v případě, že není zjištěna vlastnost cizího klíče podle konvence.</span><span class="sxs-lookup"><span data-stu-id="5a094-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="5a094-157">`[ForeignKey]` Anotace lze umístit buď do vlastnosti navigace v relaci.</span><span class="sxs-lookup"><span data-stu-id="5a094-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="5a094-158">Nepotřebujete přejít na navigační vlastnost v třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="5a094-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="5a094-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="5a094-159">[InverseProperty]</span></span>

<span data-ttu-id="5a094-160">Pomocí datových poznámek můžete nakonfigurovat, jak se mají spárovat vlastnosti navigace na závislých a hlavních entitách.</span><span class="sxs-lookup"><span data-stu-id="5a094-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="5a094-161">To se obvykle provádí, pokud existuje více než jedna dvojice vlastností navigace mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="5a094-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="5a094-162">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="5a094-162">Fluent API</span></span>

<span data-ttu-id="5a094-163">Pokud chcete nakonfigurovat relaci v rozhraní Fluent API, začněte tím, že určíte navigační vlastnosti, které tvoří relaci.</span><span class="sxs-lookup"><span data-stu-id="5a094-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="5a094-164">`HasOne`nebo `HasMany` identifikuje navigační vlastnost u typu entity, na které začínáte konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="5a094-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="5a094-165">Pak řetězení volání `WithOne` nebo `WithMany` k identifikaci invertované navigace.</span><span class="sxs-lookup"><span data-stu-id="5a094-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="5a094-166">`HasOne`/`WithOne`slouží k odkazu na navigační vlastnosti a `HasMany` / `WithMany` používají se pro navigační vlastnosti kolekce.</span><span class="sxs-lookup"><span data-stu-id="5a094-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="5a094-167">Jednoduchá navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="5a094-167">Single Navigation Property</span></span>

<span data-ttu-id="5a094-168">Pokud máte pouze jednu navigační vlastnost, existují přetížení `WithOne` parametrů a. `WithMany`</span><span class="sxs-lookup"><span data-stu-id="5a094-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="5a094-169">To znamená, že na konci relace existuje koncepční odkaz nebo kolekce, ale ve třídě entity není obsažena žádná vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="5a094-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="5a094-170">Cizí klíč</span><span class="sxs-lookup"><span data-stu-id="5a094-170">Foreign Key</span></span>

<span data-ttu-id="5a094-171">Rozhraní Fluent API můžete použít ke konfiguraci, které vlastnosti by se měly používat jako vlastnost cizího klíče pro danou relaci.</span><span class="sxs-lookup"><span data-stu-id="5a094-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="5a094-172">Následující výpis kódu ukazuje, jak nakonfigurovat složený cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="5a094-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="5a094-173">Můžete použít přetížení `HasForeignKey(...)` řetězce pro ke konfiguraci vlastnosti Shadow jako cizího klíče (Další informace najdete v tématu [vlastnosti stínu](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="5a094-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="5a094-174">Do modelu doporučujeme explicitně přidat vlastnost Shadow, než ji použijete jako cizí klíč (jak je vidět níže).</span><span class="sxs-lookup"><span data-stu-id="5a094-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="5a094-175">Bez navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="5a094-175">Without Navigation Property</span></span>

<span data-ttu-id="5a094-176">Nemusíte nutně zadávat navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5a094-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="5a094-177">Můžete jednoduše zadat cizí klíč na jednu stranu relace.</span><span class="sxs-lookup"><span data-stu-id="5a094-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="5a094-178">Hlavní klíč</span><span class="sxs-lookup"><span data-stu-id="5a094-178">Principal Key</span></span>

<span data-ttu-id="5a094-179">Pokud chcete, aby cizí klíč odkazoval na jinou vlastnost než na primární klíč, můžete pro relaci nakonfigurovat vlastnost klíče zabezpečení pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="5a094-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="5a094-180">Vlastnost, kterou nakonfigurujete jako hlavní klíč, se automaticky nastaví jako alternativní klíč (Další informace najdete v tématu [alternativní klíče](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="5a094-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
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

<span data-ttu-id="5a094-181">Následující výpis kódu ukazuje, jak nakonfigurovat složený klíč zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5a094-181">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> <span data-ttu-id="5a094-182">Pořadí, ve kterém určíte vlastnosti hlavního klíče, musí odpovídat pořadí, ve kterém jsou zadané pro cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="5a094-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="5a094-183">Povinné a volitelné relace</span><span class="sxs-lookup"><span data-stu-id="5a094-183">Required and Optional Relationships</span></span>

<span data-ttu-id="5a094-184">Pomocí rozhraní Fluent API můžete nakonfigurovat, jestli je relace povinná nebo volitelná.</span><span class="sxs-lookup"><span data-stu-id="5a094-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="5a094-185">Nakonec tato možnost určuje, jestli je vlastnost cizího klíče povinná nebo volitelná.</span><span class="sxs-lookup"><span data-stu-id="5a094-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="5a094-186">To je nejužitečnější při použití cizího klíče stavu stínové kopie.</span><span class="sxs-lookup"><span data-stu-id="5a094-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="5a094-187">Máte-li ve třídě entity vlastnost cizího klíče, je vyžadována požadovaná vlastnost vztahu na základě toho, zda je vlastnost cizího klíče povinná nebo volitelná (Další informace naleznete v části [povinné a volitelné vlastnosti](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="5a094-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
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

### <a name="cascade-delete"></a><span data-ttu-id="5a094-188">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="5a094-188">Cascade Delete</span></span>

<span data-ttu-id="5a094-189">Rozhraní Fluent API můžete použít ke konfiguraci chování kaskádového odstranění pro daný vztah explicitně.</span><span class="sxs-lookup"><span data-stu-id="5a094-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="5a094-190">Podrobné informace o jednotlivých možnostech najdete v části [kaskádová odstranění](../saving/cascade-delete.md) v části ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="5a094-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
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

## <a name="other-relationship-patterns"></a><span data-ttu-id="5a094-191">Další vzory vztahů</span><span class="sxs-lookup"><span data-stu-id="5a094-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="5a094-192">Jeden k jednomu</span><span class="sxs-lookup"><span data-stu-id="5a094-192">One-to-one</span></span>

<span data-ttu-id="5a094-193">Jedna až jedna relace má referenční navigační vlastnost na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="5a094-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="5a094-194">Dodržují stejné konvence jako relace 1: n, ale do vlastnosti cizího klíče se zavedl jedinečný index, který zaručí, že k jednotlivým objektům zabezpečení souvisí jenom jedna závislá hodnota.</span><span class="sxs-lookup"><span data-stu-id="5a094-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> <span data-ttu-id="5a094-195">EF vybere jednu z entit, které budou závislé na závislosti na schopnosti detekovat vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5a094-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="5a094-196">Pokud je jako závislá vybraná nesprávná entita, můžete ji opravit pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="5a094-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="5a094-197">Při konfiguraci relace s rozhraním API Fluent použijte `HasOne` metody a. `WithOne`</span><span class="sxs-lookup"><span data-stu-id="5a094-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="5a094-198">Při konfiguraci cizího klíče je nutné zadat typ závislé entity – Všimněte si obecného parametru, který `HasForeignKey` je uveden v níže uvedeném seznamu.</span><span class="sxs-lookup"><span data-stu-id="5a094-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="5a094-199">V relaci 1: n je jasné, že entita s referenční navigací je závislá a ta, která je v kolekci, je objektem zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5a094-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="5a094-200">Nejedná se ale o relaci 1:1, takže je potřeba explicitně definovat.</span><span class="sxs-lookup"><span data-stu-id="5a094-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
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

### <a name="many-to-many"></a><span data-ttu-id="5a094-201">Many-to-many</span><span class="sxs-lookup"><span data-stu-id="5a094-201">Many-to-many</span></span>

<span data-ttu-id="5a094-202">Relace m:n bez třídy entity představující tabulku JOIN se ještě nepodporují.</span><span class="sxs-lookup"><span data-stu-id="5a094-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="5a094-203">Můžete však znázornit relaci n:n zahrnutím třídy entity pro tabulku JOIN a mapováním dvou samostatných vztahů 1: n.</span><span class="sxs-lookup"><span data-stu-id="5a094-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
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
