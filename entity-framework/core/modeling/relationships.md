---
title: "Vztahy - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="relationships"></a><span data-ttu-id="5fdb4-102">Vztahy</span><span class="sxs-lookup"><span data-stu-id="5fdb4-102">Relationships</span></span>

<span data-ttu-id="5fdb4-103">Relaci definuje, jak dvě entity vzájemně souvisejí.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="5fdb4-104">V relační databázi to je reprezentována omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="5fdb4-105">Většina ukázky v tomto článku používá vztah jeden mnoho k předvedení konceptů.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="5fdb4-106">Příkladem relace 1: 1 a m: n, najdete v tématu [další relace vzory](#other-relationship-patterns) části na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="5fdb4-107">Definice pojmů</span><span class="sxs-lookup"><span data-stu-id="5fdb4-107">Definition of Terms</span></span>

<span data-ttu-id="5fdb4-108">Existuje několik termínů používaných k popisu vztahů</span><span class="sxs-lookup"><span data-stu-id="5fdb4-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="5fdb4-109">**Závislé entity:** Toto je typ entity, která obsahuje cizí klíčové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="5fdb4-110">Někdy označuje jako "podřízený" relace.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="5fdb4-111">**Hlavní entity:** Toto je typ entity, která obsahuje primární nebo alternativní klíčové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="5fdb4-112">Někdy označuje jako "nadřazeného" relace.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="5fdb4-113">**Cizí klíč:** vlastností v závislé entity, který se používá k ukládání hodnot hlavní vlastnost klíče, která entita má vztah k.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="5fdb4-114">**Hlavní klíč:** vlastností, která jednoznačně identifikuje hlavní entity.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="5fdb4-115">To může být primární klíč nebo alternativního klíče.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="5fdb4-116">**Navigační vlastnost:** vlastnost definovanou u hlavní nebo závislé entity, který obsahuje odkazy na související entity(s).</span><span class="sxs-lookup"><span data-stu-id="5fdb4-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="5fdb4-117">**Navigační vlastnost kolekce:** navigační vlastnost, která obsahuje odkazy na související entity.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="5fdb4-118">**Odkazovat na navigační vlastnost:** navigační vlastnost, která obsahuje odkaz na jedné související entity.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="5fdb4-119">**Inverzní navigační vlastnost:** když hovoříte o konkrétní navigační vlastnost, tento termín se vztahuje k navigační vlastnosti na druhém konci vztahu.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="5fdb4-120">Následující výpis kódu ukazuje vztah jeden mnoho mezi `Blog` a`Post`</span><span class="sxs-lookup"><span data-stu-id="5fdb4-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="5fdb4-121">`Post`je závislé entity</span><span class="sxs-lookup"><span data-stu-id="5fdb4-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="5fdb4-122">`Blog`je hlavní entity</span><span class="sxs-lookup"><span data-stu-id="5fdb4-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="5fdb4-123">`Post.BlogId`je cizí klíč</span><span class="sxs-lookup"><span data-stu-id="5fdb4-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="5fdb4-124">`Blog.BlogId`je hlavní klíč (v tomto případě je primární klíč, nikoli alternativní klíč)</span><span class="sxs-lookup"><span data-stu-id="5fdb4-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="5fdb4-125">`Post.Blog`je navigační vlastností odkazu</span><span class="sxs-lookup"><span data-stu-id="5fdb4-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="5fdb4-126">`Blog.Posts`je navigační vlastnost kolekce</span><span class="sxs-lookup"><span data-stu-id="5fdb4-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="5fdb4-127">`Post.Blog`inverzní navigační vlastnost třídy `Blog.Posts` (a naopak)</span><span class="sxs-lookup"><span data-stu-id="5fdb4-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="5fdb4-128">Konvence</span><span class="sxs-lookup"><span data-stu-id="5fdb4-128">Conventions</span></span>

<span data-ttu-id="5fdb4-129">Podle konvence vztah vytvoří při navigační vlastnost zjištěných v typu.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="5fdb4-130">Vlastnost je považován za navigační vlastnost, pokud nemůže být namapován na typ, který odkazuje na jako skalární typ. aktuálním zprostředkovatelem databáze.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="5fdb4-131">Relace, která jsou vyhledána pomocí konvence vždy se zaměří na primární klíč objektu entity.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="5fdb4-132">Pokud chcete zacílit alternativní klíč, musí být provedeno další konfiguraci, pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="5fdb4-133">Plně definované relace</span><span class="sxs-lookup"><span data-stu-id="5fdb4-133">Fully Defined Relationships</span></span>

<span data-ttu-id="5fdb4-134">Nejběžnější vzor pro relace je tak, aby měl navigační vlastnosti definované na obou konců relace a vlastností cizího klíče definovaný ve třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="5fdb4-135">Pokud se najde pár navigační vlastnosti mezi dvěma typy, bude nakonfigurován jako inverzní navigační vlastnosti stejné relace.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="5fdb4-136">Pokud závislé entity obsahuje vlastnost s názvem `<primary key property name>`, `<navigation property name><primary key property name>`, nebo `<principal entity name><primary key property name>` pak bude nakonfigurován jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="5fdb4-137">Pokud nejsou k dispozici více navigační vlastnosti definované mezi dvěma typy (tj. více než jednu dvojici odlišné navigace, které odkazují na sobě navzájem), pak se vytvoří žádné relace podle konvence a budete muset ručně nakonfigurovat je pro identifikaci jak navigační vlastnosti spárovat.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-137">If there are multiple navigation properties defined between two types (i.e. more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="5fdb4-138">Žádné vlastností cizího klíče</span><span class="sxs-lookup"><span data-stu-id="5fdb4-138">No Foreign Key Property</span></span>

<span data-ttu-id="5fdb4-139">Když se doporučuje mít vlastností cizího klíče definovaný ve třídě závislé entity, není nutné.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="5fdb4-140">Pokud se nenajde žádný vlastností cizího klíče, bude nutné zavést stínové vlastností cizího klíče s názvem `<navigation property name><principal key property name>` (viz [vlastnosti stínové](shadow-properties.md) informace).</span><span class="sxs-lookup"><span data-stu-id="5fdb4-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="5fdb4-141">Jedné navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="5fdb4-141">Single Navigation Property</span></span>

<span data-ttu-id="5fdb4-142">Včetně právě jedné navigační vlastnosti (žádné inverzní navigační a žádné vlastností cizího klíče) je dost má vztahy definované konvence.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="5fdb4-143">Můžete taky nechat jedné navigační vlastnosti kolekce a vlastností cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="5fdb4-144">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="5fdb4-144">Cascade Delete</span></span>

<span data-ttu-id="5fdb4-145">Podle konvence je možnost kaskádové odstranění *Cascade* pro požadované relace a *ClientSetNull* pro volitelné relace.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="5fdb4-146">*CASCADE* znamená závislé entity budou také odstraněny.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="5fdb4-147">*ClientSetNull* znamená, že závislé entity, které nejsou načten do paměti zůstane beze změny a musí být ručně odstraněn nebo aktualizovat tak, aby odkazoval na platný hlavní entitu.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="5fdb4-148">Pro entity, která jsou načtena do paměti se pokusí EF základní vlastnosti cizího klíče nastavena na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="5fdb4-149">Najdete v článku [požadované a volitelné vztahy](#required-and-optional-relationships) části rozdíl mezi požadované a volitelné vztahy.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="5fdb4-150">V tématu [Cascade Delete](../saving/cascade-delete.md) pro další podrobnosti o různých odstranit chování a výchozí hodnoty používané konvence.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5fdb4-151">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="5fdb4-151">Data Annotations</span></span>

<span data-ttu-id="5fdb4-152">Existují dvě poznámky dat, které můžete použít ke konfiguraci relace, `[ForeignKey]` a `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="5fdb4-153">[Cizí klíč]</span><span class="sxs-lookup"><span data-stu-id="5fdb4-153">[ForeignKey]</span></span>

<span data-ttu-id="5fdb4-154">Anotací dat můžete použít ke konfiguraci vlastností, které slouží jako vlastnost cizího klíče pro daný vztah.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="5fdb4-155">To se obvykle provádí, pokud není názvů vlastností cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="5fdb4-156">`[ForeignKey]` Poznámky je možné použít u buď navigační vlastnost v atributu relationship.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="5fdb4-157">Není nutné přejít na navigační vlastnost ve třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="5fdb4-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="5fdb4-158">[InverseProperty]</span></span>

<span data-ttu-id="5fdb4-159">Můžete nakonfigurovat, jak navigační vlastnosti na entity dependent a principal spárovat datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="5fdb4-160">To se obvykle provádí po více než jednu dvojici vlastností navigace mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="5fdb4-161">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="5fdb4-161">Fluent API</span></span>

<span data-ttu-id="5fdb4-162">Konfigurace vztahu v rozhraní Fluent API, spusťte určením navigační vlastnosti, které tvoří relace.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="5fdb4-163">`HasOne`nebo `HasMany` identifikuje navigační vlastnost u zahájíte konfiguraci na typ entity.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="5fdb4-164">Potom tvoří řetěz volání `WithOne` nebo `WithMany` k identifikaci inverzní navigační.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="5fdb4-165">`HasOne`/`WithOne`slouží k navigační vlastnosti odkazu a `HasMany` / `WithMany` se používají pro navigační vlastnosti kolekce.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="5fdb4-166">Jedné navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="5fdb4-166">Single Navigation Property</span></span>

<span data-ttu-id="5fdb4-167">Pokud máte pouze jednu navigační vlastnost pak existují bez parametrů přetížení `WithOne` a `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="5fdb4-168">To znamená, že je koncepčně reference nebo collection na druhém konci relace, ale není součástí třídy entita navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="5fdb4-169">Cizí klíč</span><span class="sxs-lookup"><span data-stu-id="5fdb4-169">Foreign Key</span></span>

<span data-ttu-id="5fdb4-170">Rozhraní Fluent API můžete použít ke konfiguraci vlastností, které slouží jako vlastnost cizího klíče pro daný vztah.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="5fdb4-171">Výpis následující kód ukazuje, jak nakonfigurovat složeného cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="5fdb4-172">Můžete použít přetížení řetězec `HasForeignKey(...)` konfigurovat vlastnosti stínové jako cizí klíč (viz [vlastnosti stínové](shadow-properties.md) Další informace).</span><span class="sxs-lookup"><span data-stu-id="5fdb4-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="5fdb4-173">Doporučujeme, abyste výslovně přidávání vlastnost stínové do modelu před jeho použitím jako cizí klíč (jak je znázorněno níže).</span><span class="sxs-lookup"><span data-stu-id="5fdb4-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="5fdb4-174">Hlavní klíč</span><span class="sxs-lookup"><span data-stu-id="5fdb4-174">Principal Key</span></span>

<span data-ttu-id="5fdb4-175">Pokud chcete cizí klíč k odkazování vlastnost než primární klíč, můžete nakonfigurovat vlastnost hlavní klíče pro relaci rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="5fdb4-176">Vlastnost, která se konfiguruje automaticky jako hlavní klíče budou se instalační program jako alternativní klíč (viz [alternativní klíče](alternate-keys.md) Další informace).</span><span class="sxs-lookup"><span data-stu-id="5fdb4-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="5fdb4-177">Výpis následující kód ukazuje, jak nakonfigurovat složený hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-177">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="5fdb4-178">Pořadí, ve kterém můžete zadat vlastnosti hlavního klíče musí odpovídat pořadí, ve kterém jsou uvedené pro cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="5fdb4-179">Povinné a nepovinné relace</span><span class="sxs-lookup"><span data-stu-id="5fdb4-179">Required and Optional Relationships</span></span>

<span data-ttu-id="5fdb4-180">Rozhraní Fluent API můžete nakonfigurovat, jestli je vztah požadované nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="5fdb4-181">Nakonec tato volba určuje, zda je vlastnost cizího klíče požadované nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="5fdb4-182">Toto je velmi užitečné, pokud používáte cizí klíč, který stínové stavu.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="5fdb4-183">Pokud máte vlastností cizího klíče v třídě entity, pak requiredness relace je určen na základě toho, jestli vlastnost cizího klíče požadované nebo volitelné (viz [požadované a volitelné vlastnosti](required-optional.md) Další informace informace o).</span><span class="sxs-lookup"><span data-stu-id="5fdb4-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="5fdb4-184">Kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="5fdb4-184">Cascade Delete</span></span>

<span data-ttu-id="5fdb4-185">Rozhraní Fluent API můžete explicitně nakonfigurovat chování cascade delete pro daný vztah.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="5fdb4-186">V tématu [Cascade Delete](../saving/cascade-delete.md) v oddílu ukládání dat pro podrobnou diskuzi o jednotlivých možností.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="5fdb4-187">Dalšími vzory relace</span><span class="sxs-lookup"><span data-stu-id="5fdb4-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="5fdb4-188">1: 1</span><span class="sxs-lookup"><span data-stu-id="5fdb4-188">One-to-one</span></span>

<span data-ttu-id="5fdb4-189">Relace 1: 1 mít navigační vlastnosti odkazu na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="5fdb4-190">Se řídí stejnými názvů jako jeden mnoho relace, ale jedinečný index byla zavedená v vlastností cizího klíče k zajištění, že pouze jeden závislé souvisí se každý objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="5fdb4-191">EF vybere jeden z entity jako závislé položky podle jeho schopnost rozpoznat vlastností cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="5fdb4-192">Pokud jste vybrali nesprávný entity jako závislé položky, můžete tuto chybu opravit rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="5fdb4-193">Při konfiguraci pomocí rozhraní Fluent API relace, je použít `HasOne` a `WithOne` metody.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="5fdb4-194">Při konfiguraci cizí klíč, je třeba zadat typ závislé entity - Všimněte si obecný parametr poskytované `HasForeignKey` v seznamu níže.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="5fdb4-195">V vztah jeden mnoho je zřejmé, že entity s navigační odkaz je závislé a ten se kolekce je objekt zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="5fdb4-196">Není to Ano v relaci 1: 1 - proto potřeba explicitně definovat.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="5fdb4-197">M m</span><span class="sxs-lookup"><span data-stu-id="5fdb4-197">Many-to-many</span></span>

<span data-ttu-id="5fdb4-198">Relace m: n bez třídu entity k připojení k tabulce představují ještě nejsou podporované.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="5fdb4-199">Ale může představovat relace m: n zahrnutím třídu entity pro připojení k tabulce a mapování dvě samostatné 1 n relace.</span><span class="sxs-lookup"><span data-stu-id="5fdb4-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

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
