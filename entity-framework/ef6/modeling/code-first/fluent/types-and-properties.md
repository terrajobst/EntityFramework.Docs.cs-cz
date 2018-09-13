---
title: Rozhraní API Fluent – konfigurace a mapování vlastností a typy - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 031376d2fc4778e6f0fa2434ab7ccfd45d436c4a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490179"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="dcc1e-102">Rozhraní Fluent API – konfigurace a mapování vlastností a typy</span><span class="sxs-lookup"><span data-stu-id="dcc1e-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="dcc1e-103">Při práci s Entity Framework Code First k mapování tříd POCO tabulky pomocí sady konvence vloženými do EF je výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="dcc1e-104">V některých případech však můžete nelze nebo nechcete, aby tyto zásady pro vytváření a potřebují mít možnost na mapování entit na něco jiného, než co konvencí diktování.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="dcc1e-105">Existují dva hlavní způsoby, jak můžete nakonfigurovat EF použít něco jiného než konvence, a to [poznámky](~/ef6/modeling/code-first/data-annotations.md) nebo rozhraní API fluent systémem souborů EFs.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="dcc1e-106">Poznámky se vztahují pouze na podmnožinu fluent funkcí rozhraní API, takže mapování scénářů, které není možné dosáhnout pomocí poznámek.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="dcc1e-107">Tento článek slouží k ukazují, jak použít rozhraní fluent API ke konfiguraci vlastností.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="dcc1e-108">První fluent API kód přistupuje nejčastěji tak, že přepíšete [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) metodu na vaší odvozené [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="dcc1e-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="dcc1e-109">Následující ukázky jsou navrženy pro ukazují, jak provádět různé úlohy s rozhraním api fluent a umožňují zkopírovat kód a přizpůsobit ho tak, aby odpovídala modelu, pokud chcete zobrazit model, který je možné použít s jako-je pak najdete na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="dcc1e-110">Nastavení pro model</span><span class="sxs-lookup"><span data-stu-id="dcc1e-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="dcc1e-111">Výchozí schéma (ef6 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="dcc1e-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="dcc1e-112">Počínaje EF6 můžete použít metodu HasDefaultSchema na DbModelBuilder určit schématu databáze pro všechny tabulky, uložené procedury, atd. Toto výchozí nastavení, budou ignorovány pro všechny objekty, které explicitně nakonfigurovat jiné schéma pro.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="dcc1e-113">Vlastní konvence (ef6 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="dcc1e-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="dcc1e-114">Spouští se s EF6, můžete vytvořit vlastní zásady odvíjející doplnit těm, které jsou součástí Code First.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="dcc1e-115">Další podrobnosti najdete v tématu [první konvence kódu vlastní](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="dcc1e-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="dcc1e-116">Mapování vlastností</span><span class="sxs-lookup"><span data-stu-id="dcc1e-116">Property Mapping</span></span>  

<span data-ttu-id="dcc1e-117">[Vlastnost](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) metoda se používá ke konfiguraci atributy pro každou vlastnost patřící do entity nebo komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="dcc1e-118">Metoda vlastnosti slouží k získání objektu konfigurace pro danou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="dcc1e-119">Možnosti na objekt konfigurace jsou specifické pro daný typ, která se právě nastavuje; Například je k dispozici pouze pro vlastnosti řetězce IsUnicode.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="dcc1e-120">Konfigurace primární klíč</span><span class="sxs-lookup"><span data-stu-id="dcc1e-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="dcc1e-121">Entity Framework konvence pro primární klíče je:</span><span class="sxs-lookup"><span data-stu-id="dcc1e-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="dcc1e-122">Vaše třída definuje vlastnost, jejíž název je "ID" nebo "Id"</span><span class="sxs-lookup"><span data-stu-id="dcc1e-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="dcc1e-123">nebo název třídy následovaný "ID" nebo "Id"</span><span class="sxs-lookup"><span data-stu-id="dcc1e-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="dcc1e-124">Pokud chcete explicitně nastavit vlastnost, která má být primární klíč, můžete použít metodu HasKey.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="dcc1e-125">V následujícím příkladu metoda HasKey slouží ke konfiguraci primární klíč InstructorID OfficeAssignment typu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="dcc1e-126">Konfigurace složený primární klíč</span><span class="sxs-lookup"><span data-stu-id="dcc1e-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="dcc1e-127">Následující příklad nastaví DepartmentID a název vlastnosti, které chcete být složený primární klíč typu oddělení.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="dcc1e-128">Vypínají se Identity pro číselné primární klíče</span><span class="sxs-lookup"><span data-stu-id="dcc1e-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="dcc1e-129">Následující příklad nastaví vlastnost DepartmentID System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None k označení, že hodnota nevygeneruje v databázi.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="dcc1e-130">Zadání maximální délku u vlastnosti</span><span class="sxs-lookup"><span data-stu-id="dcc1e-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="dcc1e-131">V následujícím příkladu vlastnost Name by měla být delší než 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="dcc1e-132">Pokud provedete delší než 50 znaků. hodnota, zobrazí se [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) výjimky.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="dcc1e-133">Pokud Code First vytvoří databázi z tohoto modelu také nastaví maximální délka sloupce název až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="dcc1e-134">Konfigurace vlastnosti jako povinné.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="dcc1e-135">V následujícím příkladu se vyžaduje vlastnost Name.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="dcc1e-136">Pokud název nezadáte, zobrazí se výjimka DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="dcc1e-137">Pokud Code First vytvoří databázi z tohoto modelu pak sloupec použitý k uložení této vlastnosti se obvykle mít hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="dcc1e-138">V některých případech nemusí být možné pro sloupec v databázi být null, i když tato vlastnost je vyžadovaná.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="dcc1e-139">Například při použití dat TPH dědičnosti strategie pro více typů je uložené v jediné tabulce.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="dcc1e-140">Pokud odvozený typ obsahuje požadovanou vlastnost sloupec nelze nastavit jako Null protože ne všechny typy v hierarchii, bude mít tato vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="dcc1e-141">Konfigurace na jednu nebo více vlastností indexu</span><span class="sxs-lookup"><span data-stu-id="dcc1e-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="dcc1e-142">**EF6.1 a vyšší pouze** – atribut indexu byla zavedena v Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="dcc1e-143">Pokud používáte starší verzi informace v této části se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="dcc1e-144">Vytváření indexů nativně nepodporuje rozhraní Fluent API, ale můžete provádět pomocí podpory pro **IndexAttribute** prostřednictvím rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="dcc1e-145">Atributy indexu jsou zpracovány včetně poznámky k modelu na model, který pak bude převedena na Index v databázi později v kanálu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="dcc1e-146">Můžete ručně přidat tyto stejné poznámky pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="dcc1e-147">Nejjednodušší způsob, jak to provést, je vytvoření instance **IndexAttribute** , která obsahuje všechna nastavení pro nový index.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="dcc1e-148">Potom můžete vytvořit instanci **IndexAnnotation** což je EF konkrétní typ, který se převede **IndexAttribute** nastavení do poznámky k modelu, který může být uložená na EF modelu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="dcc1e-149">Toto je pak možné předat do **HasColumnAnnotation** metodu na rozhraní Fluent API zadáním názvu **Index** pro poznámku.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="dcc1e-150">Úplný seznam nastavení, které jsou k dispozici v **IndexAttribute**, najdete v článku *Index* část [anotací dat při prvním kód](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="dcc1e-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="dcc1e-151">To zahrnuje přizpůsobení názvu indexu, vytváření jedinečných indexů a vytváření indexů více sloupci.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="dcc1e-152">Je-li zadat více poznámek index podle jedné vlastnosti, předá pole **IndexAttribute** konstruktoru **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="dcc1e-153">Zadání není pro mapování vlastnosti CLR na sloupec v databázi</span><span class="sxs-lookup"><span data-stu-id="dcc1e-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="dcc1e-154">Následující příklad ukazuje, jak určit, že vlastnost na typ CLR není mapováno na sloupec v databázi.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="dcc1e-155">Mapování vlastnosti CLR v určitém sloupci v databázi</span><span class="sxs-lookup"><span data-stu-id="dcc1e-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="dcc1e-156">Následující příklad namapuje vlastnost název CLR DepartmentName sloupce databáze.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="dcc1e-157">Přejmenování cizí klíč, který není definován v modelu</span><span class="sxs-lookup"><span data-stu-id="dcc1e-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="dcc1e-158">Pokud se rozhodnete definovat cizí klíč pro typ CLR, ale chcete k zadání názvu, jaký by měl mít v databázi, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="dcc1e-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="dcc1e-159">Konfiguruje, zda vlastnost řetězce podporuje kódování Unicode obsah</span><span class="sxs-lookup"><span data-stu-id="dcc1e-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="dcc1e-160">Ve výchozím nastavení jsou řetězce Unicode (nvarchar v systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="dcc1e-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="dcc1e-161">Metoda IsUnicode můžete použít k určení, zda řetězec by měl být typu varchar.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="dcc1e-162">Konfigurace datový typ sloupce databáze</span><span class="sxs-lookup"><span data-stu-id="dcc1e-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="dcc1e-163">[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) metoda povolí mapování odlišné reprezentace stejného základního typu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="dcc1e-164">Pomocí této metody neumožňuje k provádění jakýchkoli převodů dat za běhu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="dcc1e-165">Upozorňujeme, že IsUnicode upřednostňovaný způsob nastavení sloupce varchar, protože je nezávislá na databázi.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="dcc1e-166">Konfigurace vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="dcc1e-167">Existují dva způsoby, jak nakonfigurovat Skalární vlastnosti na komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="dcc1e-168">Na položku ComplexTypeConfiguration lze volat vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="dcc1e-169">Můžete také použít zápisu s tečkou pro přístup k vlastnosti komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="dcc1e-170">Konfiguruje vlastnost, která má sloužit jako Token optimistického řízení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="dcc1e-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="dcc1e-171">Chcete-li určit, že vlastnosti v entitě představuje tokenem souběžnosti, můžete použít atribut atribut ConcurrencyCheck nebo metoda IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="dcc1e-172">Metoda IsRowVersion také můžete nakonfigurovat vlastnosti, která má být verze řádku v databázi.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="dcc1e-173">Nastavení vlastnosti, která má být, že verze řádku automaticky nakonfiguruje ho, aby se token optimistického řízení souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="dcc1e-174">Typ mapování</span><span class="sxs-lookup"><span data-stu-id="dcc1e-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="dcc1e-175">Určení, že třída je komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="dcc1e-176">Podle konvence typ, který nemá žádný primární klíč zadán je považován za komplexní.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="dcc1e-177">Existují některé scénáře, kde Code First nerozpozná komplexní typ (například, pokud mají vlastnost zvanou ID, ale neznamená, aby se primární klíč).</span><span class="sxs-lookup"><span data-stu-id="dcc1e-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="dcc1e-178">V takových případech můžete využít rozhraní fluent API explicitně určit, že typ je komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="dcc1e-179">Zadání není pro mapování typu CLR Entity do tabulky v databázi</span><span class="sxs-lookup"><span data-stu-id="dcc1e-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="dcc1e-180">Následující příklad ukazuje, jak vyloučit typ CLR z mapovaný do tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="dcc1e-181">Mapování typu Entity na určitou tabulku v databázi</span><span class="sxs-lookup"><span data-stu-id="dcc1e-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="dcc1e-182">Všechny vlastnosti oddělení budou zmapována do sloupce v tabulce nazvané t_ oddělení.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="dcc1e-183">Můžete také zadat název schématu takto:</span><span class="sxs-lookup"><span data-stu-id="dcc1e-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="dcc1e-184">Mapování dědičnosti za hierarchii tabulky (TPH)</span><span class="sxs-lookup"><span data-stu-id="dcc1e-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="dcc1e-185">Ve scénáři TPH mapování všech typů v hierarchii dědičnosti se mapují na jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="dcc1e-186">Sloupec diskriminátoru se používá k identifikaci typu každý řádek.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="dcc1e-187">Při vytváření modelu s Code First, TPH je výchozí strategie pro typy, které jsou součástí hierarchie dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="dcc1e-188">Ve výchozím nastavení sloupec diskriminátoru se přidá do tabulky s názvem "Diskriminátoru" a název typu CLR jednotlivých typů v hierarchii se používá pro hodnoty diskriminátoru.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="dcc1e-189">Výchozí chování lze upravit pomocí rozhraní API fluent.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="dcc1e-190">Mapování dědičnosti za typ tabulky (TPT)</span><span class="sxs-lookup"><span data-stu-id="dcc1e-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="dcc1e-191">Ve scénáři TPT mapování jsou všechny typy mapovány na jednotlivé tabulky.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="dcc1e-192">Vlastnosti, které patří výhradně pro základní typ nebo odvozeného typu jsou uložené v tabulce, která se mapuje na tento typ.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="dcc1e-193">Tabulky, které se mapují k odvozené typy také uložit cizí klíč, který připojí odvozenou tabulku s základní tabulky.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="dcc1e-194">Mapování tabulek na konkrétní třídy (TPC) dědičnosti</span><span class="sxs-lookup"><span data-stu-id="dcc1e-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="dcc1e-195">Ve scénáři testu TPC mapování všechny typy neabstraktní v hierarchii se mapují na jednotlivé tabulky.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="dcc1e-196">Tabulky, které se mapují na odvozené třídy nemají žádný vztah k tabulce, která se mapuje na základní třídu v databázi.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="dcc1e-197">Všechny vlastnosti třídy, včetně zděděné vlastnosti, jsou mapovány na sloupce příslušné tabulky.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="dcc1e-198">Volání metody MapInheritedProperties ke konfiguraci jednotlivých odvozeného typu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="dcc1e-199">MapInheritedProperties změní všechny vlastnosti, které byly zděděny ze základní třídy na nové sloupce v tabulce pro odvozenou třídu.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="dcc1e-200">Všimněte si, že vzhledem k tomu, že tabulky účasti v hierarchii dědičnosti TPC nesdílejí primární klíč bude duplicitní entita klíče při vkládání do tabulek, které jsou mapovány na podtřídy, pokud máte hodnot v databázi vygeneruje se výchozí hodnota vlastnosti stejné identity.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="dcc1e-201">Pro vyřešení tohoto problému můžete zadat hodnotu různých počátečních pro každou tabulku nebo vypnout identitu na vlastnost primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="dcc1e-202">Identita je výchozí hodnota vlastnosti integer klíče při práci se službou Code First.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="dcc1e-203">Mapování vlastnosti typu Entity k několika tabulkám v databázi (rozdělení Entity)</span><span class="sxs-lookup"><span data-stu-id="dcc1e-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="dcc1e-204">Entita rozdělení umožňuje vlastnosti typu entity k možné rozdělit do několika tabulek.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="dcc1e-205">V následujícím příkladu je entita oddělení rozdělit na dvě tabulky: oddělení a DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="dcc1e-206">Rozdělení entity pomocí více volání metody mapování mapuje podmnožinu vlastností do určité tabulky.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="dcc1e-207">Mapování více typů entit na jedné tabulky v databázi (rozdělení tabulky)</span><span class="sxs-lookup"><span data-stu-id="dcc1e-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="dcc1e-208">Následující příklad namapuje dva typy entit, které sdílejí primární klíč do jedné tabulky.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="dcc1e-209">Mapování typu Entity na uložené procedury Insert/Update/Delete (ef6 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="dcc1e-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="dcc1e-210">Počínaje EF6 můžete mapovat entity pro použití uložené procedury pro vložení aktualizace a odstraňování.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="dcc1e-211">Další podrobnosti najdete v tématu, [kód první Insert/Update/Delete uložené procedury](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="dcc1e-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="dcc1e-212">Model použitý v ukázky</span><span class="sxs-lookup"><span data-stu-id="dcc1e-212">Model Used in Samples</span></span>  

<span data-ttu-id="dcc1e-213">Následující kód první model se používá pro ukázky na této stránce.</span><span class="sxs-lookup"><span data-stu-id="dcc1e-213">The following Code First model is used for the samples on this page.</span></span>  

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
