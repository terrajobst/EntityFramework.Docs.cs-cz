---
title: Fluent API – konfigurace a mapování vlastností a typů – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419064"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="18050-102">Rozhraní Fluent API – konfigurace a mapování vlastností a typů</span><span class="sxs-lookup"><span data-stu-id="18050-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="18050-103">Při práci s Entity Framework Code First výchozím chováním je namapovat třídy POCO na tabulky pomocí sady konvencí vloženými na EF.</span><span class="sxs-lookup"><span data-stu-id="18050-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="18050-104">V některých případech však nemůžete ani nebudete chtít tyto konvence namapovat na jinou, než jaké konvence určují.</span><span class="sxs-lookup"><span data-stu-id="18050-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="18050-105">Existují dva hlavní způsoby, jak můžete nakonfigurovat EF, aby používaly jinou než konvenci, konkrétně [poznámky](~/ef6/modeling/code-first/data-annotations.md) nebo rozhraní EFs Fluent API.</span><span class="sxs-lookup"><span data-stu-id="18050-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="18050-106">Poznámky se vztahují pouze na podmnožinu funkcí rozhraní Fluent API, takže existují mapování scénářů, které nelze dosáhnout pomocí poznámek.</span><span class="sxs-lookup"><span data-stu-id="18050-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="18050-107">Tento článek je navržený tak, aby ukázal, jak pomocí rozhraní Fluent API konfigurovat vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18050-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="18050-108">První rozhraní API pro Fluent kódu je nejčastěji dostupné přepsáním metody [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) v odvozeném [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="18050-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="18050-109">Následující ukázky jsou navržené tak, aby ukázaly, jak provádět různé úlohy pomocí rozhraní Fluent API, a umožňuje zkopírovat kód a přizpůsobit ho tak, aby vyhovoval vašemu modelu. Pokud si chcete prohlédnout model, který je možné použít s tak, jak je, je uvedený na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="18050-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="18050-110">Nastavení pro nejrůznější modely</span><span class="sxs-lookup"><span data-stu-id="18050-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="18050-111">Výchozí schéma (EF6 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="18050-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="18050-112">Počínaje EF6 můžete použít metodu HasDefaultSchema na DbModelBuilder k určení schématu databáze pro použití pro všechny tabulky, uložené procedury atd. Toto výchozí nastavení bude potlačeno pro všechny objekty, pro které explicitně nakonfigurujete jiné schéma pro.</span><span class="sxs-lookup"><span data-stu-id="18050-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="18050-113">Vlastní konvence (EF6 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="18050-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="18050-114">Počínaje EF6 můžete vytvořit vlastní konvence, které doplňují ta, která jsou obsažená v Code First.</span><span class="sxs-lookup"><span data-stu-id="18050-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="18050-115">Další podrobnosti najdete v tématu [Vlastní konvence Code First](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="18050-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="18050-116">Mapování vlastností</span><span class="sxs-lookup"><span data-stu-id="18050-116">Property Mapping</span></span>  

<span data-ttu-id="18050-117">Metoda [vlastnosti](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) se používá ke konfiguraci atributů pro každou vlastnost patřící k entitě nebo komplexnímu typu.</span><span class="sxs-lookup"><span data-stu-id="18050-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="18050-118">Metoda Property slouží k získání objektu konfigurace pro danou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18050-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="18050-119">Možnosti objektu konfigurace jsou specifické pro nakonfigurovaný typ. Kódování Unicode je k dispozici pouze v případě vlastností řetězce.</span><span class="sxs-lookup"><span data-stu-id="18050-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="18050-120">Konfigurace primárního klíče</span><span class="sxs-lookup"><span data-stu-id="18050-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="18050-121">Entity Framework konvence pro primární klíče je:</span><span class="sxs-lookup"><span data-stu-id="18050-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="18050-122">Vaše třída definuje vlastnost, jejíž název je "ID" nebo "ID".</span><span class="sxs-lookup"><span data-stu-id="18050-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="18050-123">nebo název třídy následovaný "ID" nebo "ID"</span><span class="sxs-lookup"><span data-stu-id="18050-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="18050-124">Chcete-li explicitně nastavit vlastnost jako primární klíč, můžete použít metodu Haskey –.</span><span class="sxs-lookup"><span data-stu-id="18050-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="18050-125">V následujícím příkladu se metoda Haskey – používá ke konfiguraci primárního klíče InstructorID na typu OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="18050-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="18050-126">Konfigurace složeného primárního klíče</span><span class="sxs-lookup"><span data-stu-id="18050-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="18050-127">Následující příklad konfiguruje vlastnosti DepartmentID a Name jako složený primární klíč typu oddělení.</span><span class="sxs-lookup"><span data-stu-id="18050-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="18050-128">Vypínání identity pro číselné primární klíče</span><span class="sxs-lookup"><span data-stu-id="18050-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="18050-129">Následující příklad nastaví vlastnost DepartmentID na hodnotu System. ComponentModel. DataAnnotations. DatabaseGeneratedOption. None, aby označovala, že tato hodnota nebude vygenerována databází.</span><span class="sxs-lookup"><span data-stu-id="18050-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="18050-130">Určení maximální délky vlastnosti</span><span class="sxs-lookup"><span data-stu-id="18050-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="18050-131">V následujícím příkladu by vlastnost Name neměla být delší než 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="18050-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="18050-132">Pokud nastavíte hodnotu delší než 50 znaků, zobrazí se výjimka [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) .</span><span class="sxs-lookup"><span data-stu-id="18050-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="18050-133">Pokud Code First vytvoří databázi z tohoto modelu, nastaví se také maximální délka sloupce Name na 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="18050-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="18050-134">Konfigurace vlastnosti, která má být požadována</span><span class="sxs-lookup"><span data-stu-id="18050-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="18050-135">V následujícím příkladu je vyžadována vlastnost Name.</span><span class="sxs-lookup"><span data-stu-id="18050-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="18050-136">Pokud název nezadáte, zobrazí se výjimka DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="18050-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="18050-137">Pokud Code First vytvoří databázi z tohoto modelu, sloupec použitý k uložení této vlastnosti obvykle nebude možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="18050-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="18050-138">V některých případech nemusí být možné, aby sloupec v databázi mohl mít hodnotu null, i když je požadovaná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18050-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="18050-139">Pokud například použijete strategii dědičnosti TPH pro více typů, uloží se do jedné tabulky.</span><span class="sxs-lookup"><span data-stu-id="18050-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="18050-140">Pokud odvozený typ obsahuje povinnou vlastnost, sloupec nemůže být nastaven na hodnotu null, protože ne všechny typy v hierarchii budou mít tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18050-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="18050-141">Konfigurace indexu pro jednu nebo více vlastností</span><span class="sxs-lookup"><span data-stu-id="18050-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="18050-142">**EF 6.1 a vyšší pouze** – atribut indexu byl představen v Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="18050-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="18050-143">Pokud používáte starší verzi, informace v této části se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="18050-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="18050-144">Vytváření indexů není v rozhraní API Fluent nativně podporované, ale můžete využít podporu **IndexAttribute** prostřednictvím rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="18050-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="18050-145">Atributy indexu jsou zpracovávány zahrnutím anotace modelu do modelu, který je poté převeden na index v databázi později v kanálu.</span><span class="sxs-lookup"><span data-stu-id="18050-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="18050-146">Stejné Anotace můžete přidat ručně pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="18050-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="18050-147">Nejjednodušší způsob, jak to provést, je vytvořit instanci **IndexAttribute** , která obsahuje všechna nastavení nového indexu.</span><span class="sxs-lookup"><span data-stu-id="18050-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="18050-148">Pak můžete vytvořit instanci **IndexAnnotation** , která je specifickým typem EF, který převede nastavení **IndexAttribute** na anotaci modelu, která může být uložena v modelu EF.</span><span class="sxs-lookup"><span data-stu-id="18050-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="18050-149">Ty pak můžete předat metodě **HasColumnAnnotation** v rozhraní Fluent API a zadat **index** názvu pro anotaci.</span><span class="sxs-lookup"><span data-stu-id="18050-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="18050-150">Úplný seznam nastavení dostupných v **IndexAttribute**najdete v části *Index* [datových poznámek Code First](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="18050-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="18050-151">To zahrnuje přizpůsobení názvu indexu, vytváření jedinečných indexů a vytváření indexů ve více sloupcích.</span><span class="sxs-lookup"><span data-stu-id="18050-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="18050-152">Můžete zadat více indexových poznámek pro jednu vlastnost předáním pole **IndexAttribute** do konstruktoru třídy **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="18050-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="18050-153">Určení, že se má mapovat vlastnost CLR na sloupec v databázi</span><span class="sxs-lookup"><span data-stu-id="18050-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="18050-154">Následující příklad ukazuje, jak určit, že vlastnost u typu CLR není namapována na sloupec v databázi.</span><span class="sxs-lookup"><span data-stu-id="18050-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="18050-155">Mapování vlastnosti CLR na konkrétní sloupec v databázi</span><span class="sxs-lookup"><span data-stu-id="18050-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="18050-156">V následujícím příkladu je mapována vlastnost název CLR na sloupec databáze oddělení.</span><span class="sxs-lookup"><span data-stu-id="18050-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="18050-157">Přejmenování cizího klíče, který není v modelu definován</span><span class="sxs-lookup"><span data-stu-id="18050-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="18050-158">Pokud se rozhodnete nedefinovat cizí klíč pro typ CLR, ale chcete určit, jaký název by měl mít v databázi, udělejte toto:</span><span class="sxs-lookup"><span data-stu-id="18050-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="18050-159">Konfigurace, zda řetězcová vlastnost podporuje obsah v kódování Unicode</span><span class="sxs-lookup"><span data-stu-id="18050-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="18050-160">Ve výchozím nastavení jsou řetězce znakové sady Unicode (nvarchar v SQL Server).</span><span class="sxs-lookup"><span data-stu-id="18050-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="18050-161">Pomocí metody "v kódování Unicode" lze určit, že řetězec by měl být typu varchar.</span><span class="sxs-lookup"><span data-stu-id="18050-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="18050-162">Konfigurace datového typu sloupce databáze</span><span class="sxs-lookup"><span data-stu-id="18050-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="18050-163">Metoda [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) umožňuje mapování na různé reprezentace stejného základního typu.</span><span class="sxs-lookup"><span data-stu-id="18050-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="18050-164">Použití této metody vám neumožňuje provádět převod dat za běhu.</span><span class="sxs-lookup"><span data-stu-id="18050-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="18050-165">Všimněte si, že v kódování Unicode je preferovaný způsob, jak nastavit sloupce na varchar, protože se jedná o nezávislá databáze.</span><span class="sxs-lookup"><span data-stu-id="18050-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="18050-166">Konfigurace vlastností komplexního typu</span><span class="sxs-lookup"><span data-stu-id="18050-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="18050-167">Existují dva způsoby konfigurace skalárních vlastností pro komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="18050-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="18050-168">Můžete volat vlastnost v ComplexTypeConfiguration.</span><span class="sxs-lookup"><span data-stu-id="18050-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="18050-169">Zápis tečky lze také použít pro přístup k vlastnosti komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18050-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="18050-170">Konfigurace vlastnosti, která se má použít jako Optimistická souběžnost tokenu</span><span class="sxs-lookup"><span data-stu-id="18050-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="18050-171">Chcete-li určit, že vlastnost v entitě představuje token souběžnosti, můžete použít buď atribut ConcurrencyCheck, nebo metodu IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="18050-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="18050-172">Můžete také použít metodu IsRowVersion a nakonfigurovat vlastnost tak, aby byla v databázi verze řádku.</span><span class="sxs-lookup"><span data-stu-id="18050-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="18050-173">Nastavením vlastnosti na hodnotu je verze řádku automaticky nakonfigurujete, aby se jedná o optimistický Token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="18050-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="18050-174">Mapování typů</span><span class="sxs-lookup"><span data-stu-id="18050-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="18050-175">Určení, že třída je komplexního typu</span><span class="sxs-lookup"><span data-stu-id="18050-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="18050-176">Podle konvence typ, který nemá zadaný primární klíč, se považuje za komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="18050-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="18050-177">Existují některé scénáře, kdy Code First nezjistí složitý typ (například pokud máte vlastnost s názvem ID, ale nepovažujete to za primární klíč).</span><span class="sxs-lookup"><span data-stu-id="18050-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="18050-178">V takových případech byste pomocí rozhraní Fluent API explicitně určili, že typ je komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="18050-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="18050-179">Určení nemapování typu entity CLR na tabulku v databázi</span><span class="sxs-lookup"><span data-stu-id="18050-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="18050-180">Následující příklad ukazuje, jak vyloučit typ CLR z mapování na tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="18050-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="18050-181">Mapování typu entity na konkrétní tabulku v databázi</span><span class="sxs-lookup"><span data-stu-id="18050-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="18050-182">Všechny vlastnosti oddělení budou namapovány na sloupce v tabulce s názvem t_ oddělení.</span><span class="sxs-lookup"><span data-stu-id="18050-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="18050-183">Název schématu můžete zadat také takto:</span><span class="sxs-lookup"><span data-stu-id="18050-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="18050-184">Mapování dědičnosti typu tabulka na hierarchii (TPH)</span><span class="sxs-lookup"><span data-stu-id="18050-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="18050-185">Ve scénáři mapování TPH jsou všechny typy v hierarchii dědičnosti mapovány na jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="18050-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="18050-186">Sloupec diskriminátoru se používá k identifikaci typu každého řádku.</span><span class="sxs-lookup"><span data-stu-id="18050-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="18050-187">Při vytváření modelu pomocí Code First je pro typy, které se podílejí na hierarchii dědičnosti, výchozí strategie.</span><span class="sxs-lookup"><span data-stu-id="18050-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="18050-188">Ve výchozím nastavení se sloupec diskriminátor přidá do tabulky s názvem "diskriminátor" a název typu CLR každého typu v hierarchii se používá pro hodnoty diskriminátoru.</span><span class="sxs-lookup"><span data-stu-id="18050-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="18050-189">Výchozí chování můžete upravit pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="18050-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="18050-190">Mapování dědičnosti typu tabulka na typ (TPT)</span><span class="sxs-lookup"><span data-stu-id="18050-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="18050-191">Ve scénáři mapování TPT jsou všechny typy namapovány na jednotlivé tabulky.</span><span class="sxs-lookup"><span data-stu-id="18050-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="18050-192">Vlastnosti, které patří výhradně k základnímu typu nebo odvozenému typu, jsou uloženy v tabulce, která je mapována na daný typ.</span><span class="sxs-lookup"><span data-stu-id="18050-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="18050-193">Tabulky, které se mapují na odvozené typy, také uloží cizí klíč, který spojí odvozenou tabulku se základní tabulkou.</span><span class="sxs-lookup"><span data-stu-id="18050-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="18050-194">Mapování dědičnosti třídy TPC (Table-per-beton)</span><span class="sxs-lookup"><span data-stu-id="18050-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="18050-195">Ve scénáři mapování TPC jsou všechny neabstraktní typy v hierarchii namapovány na jednotlivé tabulky.</span><span class="sxs-lookup"><span data-stu-id="18050-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="18050-196">Tabulky, které jsou mapovány na odvozené třídy, nemají žádnou relaci s tabulkou, která je v databázi mapována na základní třídu.</span><span class="sxs-lookup"><span data-stu-id="18050-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="18050-197">Všechny vlastnosti třídy, včetně děděných vlastností, jsou namapovány na sloupce odpovídající tabulky.</span><span class="sxs-lookup"><span data-stu-id="18050-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="18050-198">Pro konfiguraci každého odvozeného typu zavolejte metodu MapInheritedProperties.</span><span class="sxs-lookup"><span data-stu-id="18050-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="18050-199">MapInheritedProperties přemapuje všechny vlastnosti, které byly děděny ze základní třídy na nové sloupce v tabulce pro odvozenou třídu.</span><span class="sxs-lookup"><span data-stu-id="18050-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="18050-200">Všimněte si, že vzhledem k tomu, že tabulky, které se účastní hierarchie dědičnosti TPC, nesdílejí primární klíč, budou při vkládání do tabulek mapovaných na podtřídy duplicitní klíče entit, pokud máte databáze vygenerované hodnotami se stejnou počáteční hodnotou identity.</span><span class="sxs-lookup"><span data-stu-id="18050-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="18050-201">Chcete-li tento problém vyřešit, můžete buď zadat jinou počáteční počáteční hodnotu pro každou tabulku, nebo vypnout identitu u vlastnosti primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="18050-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="18050-202">Identita je výchozí hodnota vlastností celočíselného klíče při práci s Code First.</span><span class="sxs-lookup"><span data-stu-id="18050-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="18050-203">Mapování vlastností typu entity na více tabulek v databázi (rozdělení entity)</span><span class="sxs-lookup"><span data-stu-id="18050-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="18050-204">Rozdělení entit umožňuje rozdělit vlastnosti typu entity napříč více tabulkami.</span><span class="sxs-lookup"><span data-stu-id="18050-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="18050-205">V následujícím příkladu je entita oddělení rozdělená na dvě tabulky: oddělení a DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="18050-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="18050-206">Rozdělování entit používá více volání metody map k mapování podmnožiny vlastností na konkrétní tabulku.</span><span class="sxs-lookup"><span data-stu-id="18050-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="18050-207">Mapování více typů entit na jednu tabulku v databázi (rozdělení tabulky)</span><span class="sxs-lookup"><span data-stu-id="18050-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="18050-208">Následující příklad mapuje dva typy entit, které sdílejí primární klíč s jednou tabulkou.</span><span class="sxs-lookup"><span data-stu-id="18050-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="18050-209">Mapování typu entity na vložení, aktualizaci nebo odstranění uložených procedur (EF6 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="18050-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="18050-210">Počínaje EF6 můžete namapovat entitu na použití uložených procedur pro vložení Update a DELETE.</span><span class="sxs-lookup"><span data-stu-id="18050-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="18050-211">Další podrobnosti najdete v [Code First uložených procedurách INSERT, Update a DELETE](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="18050-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="18050-212">Model používaný v ukázkách</span><span class="sxs-lookup"><span data-stu-id="18050-212">Model Used in Samples</span></span>  

<span data-ttu-id="18050-213">Následující model Code First se používá pro ukázky na této stránce.</span><span class="sxs-lookup"><span data-stu-id="18050-213">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
