---
title: Konvence Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419288"
---
# <a name="code-first-conventions"></a><span data-ttu-id="c4fb6-102">Code First konvence</span><span class="sxs-lookup"><span data-stu-id="c4fb6-102">Code First Conventions</span></span>
<span data-ttu-id="c4fb6-103">Code First umožňuje popsat model pomocí C# nebo Visual Basic třídy .NET.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="c4fb6-104">Základní tvar modelu se detekuje pomocí konvencí.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="c4fb6-105">Konvence jsou sady pravidel, které se používají k automatické konfiguraci koncepčního modelu na základě definic třídy při práci s Code First.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="c4fb6-106">Konvence jsou definovány v oboru názvů System. data. entity. ModelConfiguration. Conventions.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="c4fb6-107">Model můžete dále konfigurovat pomocí datových poznámek nebo rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="c4fb6-108">Přednost je dána konfiguraci prostřednictvím rozhraní Fluent API, po kterém následují datové poznámky a potom konvence.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="c4fb6-109">Další informace najdete v tématu [datové poznámky](~/ef6/modeling/code-first/data-annotations.md), [Fluent API-Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [typy rozhraní API Fluent & vlastnosti](~/ef6/modeling/code-first/fluent/types-and-properties.md) a [Fluent API pomocí VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb6-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="c4fb6-110">Podrobný seznam konvencí Code First najdete v [dokumentaci k rozhraní API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4fb6-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="c4fb6-111">Toto téma poskytuje přehled konvencí používaných nástrojem Code First.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="c4fb6-112">Zjišťování typů</span><span class="sxs-lookup"><span data-stu-id="c4fb6-112">Type Discovery</span></span>  

<span data-ttu-id="c4fb6-113">Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="c4fb6-114">Kromě definování tříd musíte také dát **DbContext** informace o tom, které typy chcete do modelu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="c4fb6-115">Chcete-li to provést, Definujte třídu kontextu, která je odvozena z **DbContext** a zpřístupňuje vlastnosti **negenerickými** pro typy, které mají být součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="c4fb6-116">Code First budou tyto typy zahrnovat a budou také vyžádané v jakémkoli odkazovaném typu, a to i v případě, že jsou odkazované typy definovány v jiném sestavení.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="c4fb6-117">Pokud se vaše typy účastní hierarchie dědičnosti, je dostačující definovat vlastnost **negenerickými** pro základní třídu a odvozené typy budou automaticky zahrnuty, pokud jsou ve stejném sestavení jako základní třída.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="c4fb6-118">V následujícím příkladu je definována pouze jedna vlastnost **negenerickými** ve třídě **SchoolEntities** (**departments**).</span><span class="sxs-lookup"><span data-stu-id="c4fb6-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="c4fb6-119">Code First tuto vlastnost používá ke zjišťování a vyžádání v jakémkoli odkazovaném typu.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="c4fb6-120">Pokud chcete vyloučit typ z modelu, použijte atribut **NotMapped** nebo **DbModelBuilder. Ignore** Fluent API.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="c4fb6-121">Konvence primárního klíče</span><span class="sxs-lookup"><span data-stu-id="c4fb6-121">Primary Key Convention</span></span>  

<span data-ttu-id="c4fb6-122">Code First odvodí, že vlastnost je primární klíč, pokud je vlastnost u třídy pojmenována "ID" (nerozlišuje malá a velká písmena) nebo název třídy následovaný "ID".</span><span class="sxs-lookup"><span data-stu-id="c4fb6-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="c4fb6-123">Pokud je typ vlastnosti primárního klíče číslo nebo GUID, bude nakonfigurovaný jako sloupec identity.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="c4fb6-124">Konvence vztahů</span><span class="sxs-lookup"><span data-stu-id="c4fb6-124">Relationship Convention</span></span>  

<span data-ttu-id="c4fb6-125">V Entity Framework navigační vlastnosti poskytují způsob, jak procházet relaci mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="c4fb6-126">Každý objekt může mít navigační vlastnost pro každý vztah, ve kterém se účastní.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="c4fb6-127">Navigační vlastnosti umožňují procházet a spravovat relace v obou směrech, vracet buď Referenční objekt (Pokud je násobnost buď jedna, nebo nula, nebo-jedna), nebo kolekci (Pokud je násobnost mnoho).</span><span class="sxs-lookup"><span data-stu-id="c4fb6-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="c4fb6-128">Code First odvodí vztahy na základě navigačních vlastností definovaných ve vašich typech.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="c4fb6-129">Kromě navigačních vlastností doporučujeme, abyste do typů, které reprezentují závislé objekty, zahrnuli vlastnosti cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="c4fb6-130">Jakákoli vlastnost se stejným datovým typem jako primární vlastnost primárního klíče a s názvem, který následuje jeden z následujících formátů, představuje cizí klíč pro relaci: '\<název vlastnosti navigace\>\<hlavní název vlastnosti primárního klíče\>', '\<název třídy zabezpečení\>\<primární klíč vlastnosti název\>', nebo '\<hlavní primární klíč vlastnosti\>'.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="c4fb6-131">Pokud je nalezeno více shod, bude přednost uvedena v pořadí uvedeném výše.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="c4fb6-132">Detekce cizího klíče nerozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="c4fb6-133">Když je zjištěna vlastnost cizího klíče, Code First odvodí násobnost relace na základě hodnoty null cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="c4fb6-134">Pokud je vlastnost nastavena na hodnotu null, je relace registrována jako volitelná; v opačném případě bude relace registrována podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="c4fb6-135">Pokud cizímu klíči na závislé entitě není nabývat hodnoty null, Code First nastaví kaskádové odstranění v relaci.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="c4fb6-136">Pokud je cizí klíč na závislé entitě Nullable, Code First v relaci nenastavuje kaskádové odstranění a při odstranění objektu zabezpečení se cizí klíč nastaví na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="c4fb6-137">Chování násobnosti a kaskádového odstranění zjištěné konvencí lze přepsat pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="c4fb6-138">V následujícím příkladu jsou k definování vztahu mezi třídami oddělení a kurzu použity navigační vlastnosti a cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="c4fb6-139">Pokud máte více vztahů mezi stejnými typy (například Předpokládejme, že definujete třídy **Person** a **Book** , kde třída **Person** obsahuje navigační vlastnosti **ReviewedBooks** a **AuthoredBooks** a třída **Book** obsahuje navigační vlastnosti **autora** a **kontrolora** ), je nutné ručně nakonfigurovat relace pomocí datových poznámek nebo rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="c4fb6-140">Další informace najdete v tématech [datové poznámky – vztahy](~/ef6/modeling/code-first/data-annotations.md) a [Fluent API-Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb6-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="c4fb6-141">Konvence komplexních typů</span><span class="sxs-lookup"><span data-stu-id="c4fb6-141">Complex Types Convention</span></span>  

<span data-ttu-id="c4fb6-142">Když Code First zjistí definici třídy, kde nelze odvodit primární klíč, a žádný primární klíč není zaregistrován prostřednictvím datových poznámek nebo rozhraní API Fluent, pak je typ automaticky registrován jako složitý typ.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="c4fb6-143">Detekce komplexního typu také vyžaduje, aby typ neměl vlastnosti, které odkazují na typy entit a neodkazuje se na vlastnost kolekce na jiném typu.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="c4fb6-144">S ohledem na následující definice tříd Code First by byly odvodit, že **Podrobnosti** jsou komplexní typ, protože nemá žádný primární klíč.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
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
```  

## <a name="connection-string-convention"></a><span data-ttu-id="c4fb6-145">Konvence připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="c4fb6-145">Connection String Convention</span></span>  

<span data-ttu-id="c4fb6-146">Další informace o konvencích, které DbContext používá ke zjištění připojení pro použití, najdete v tématu [připojení a modely](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb6-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="c4fb6-147">Odebírají se konvence.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-147">Removing Conventions</span></span>  

<span data-ttu-id="c4fb6-148">Můžete odebrat libovolné konvence definované v oboru názvů System. data. entity. ModelConfiguration. Conventions.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="c4fb6-149">Následující příklad odebere **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="c4fb6-150">Vlastní konvence</span><span class="sxs-lookup"><span data-stu-id="c4fb6-150">Custom Conventions</span></span>  

<span data-ttu-id="c4fb6-151">Vlastní konvence jsou podporované v EF6 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="c4fb6-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="c4fb6-152">Další informace najdete v tématu [Vlastní konvence Code First](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="c4fb6-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
