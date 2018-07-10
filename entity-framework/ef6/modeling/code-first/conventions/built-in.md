---
title: První vytváření – EF6 kódu
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912061"
---
# <a name="code-first-conventions"></a><span data-ttu-id="2f000-102">První konvence kódu</span><span class="sxs-lookup"><span data-stu-id="2f000-102">Code First Conventions</span></span>
<span data-ttu-id="2f000-103">Code First umožňuje popisuje model s použitím tříd jazyka C# nebo Visual Basic .NET.</span><span class="sxs-lookup"><span data-stu-id="2f000-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="2f000-104">Základní obrazec modelu je zjišťován pomocí konvencí.</span><span class="sxs-lookup"><span data-stu-id="2f000-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="2f000-105">Konvence jsou sady pravidel, která se používají pro automatickou konfiguraci konceptuálního modelu podle definice tříd při práci se službou Code First.</span><span class="sxs-lookup"><span data-stu-id="2f000-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="2f000-106">Konvence jsou definovány v oboru názvů System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="2f000-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="2f000-107">Model lze dále konfigurovat pomocí anotacemi dat nebo rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="2f000-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="2f000-108">Prioritu konfiguraci prostřednictvím rozhraní fluent API následovat anotacemi dat a vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="2f000-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="2f000-109">Další informace najdete v části [anotacemi dat](~/ef6/modeling/code-first/data-annotations.md), [rozhraní Fluent API – vztahy](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API – typy a vlastnosti](~/ef6/modeling/code-first/fluent/types-and-properties.md) a [Fluent API s využitím VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="2f000-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="2f000-110">Podrobný seznam Code First konvence je k dispozici v [dokumentace k rozhraní API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f000-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="2f000-111">Toto téma obsahuje základní informace o konvencích použitých ji Code First.</span><span class="sxs-lookup"><span data-stu-id="2f000-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="2f000-112">Typ zjišťování</span><span class="sxs-lookup"><span data-stu-id="2f000-112">Type Discovery</span></span>  

<span data-ttu-id="2f000-113">Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény).</span><span class="sxs-lookup"><span data-stu-id="2f000-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="2f000-114">Kromě definování třídy, je také potřeba nechat **DbContext** vědět, jaké typy, které chcete zahrnout do modelu.</span><span class="sxs-lookup"><span data-stu-id="2f000-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="2f000-115">K tomuto účelu můžete definovat, která je odvozena z třídy kontextu **DbContext** a zveřejňuje **DbSet** vlastnosti pro typy, které mají být součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="2f000-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="2f000-116">Kód nejprve bude obsahovat tyto typy a také se o přijetí změn v odkazovaných typů, i v případě, že odkazované typy jsou definovány v jiném sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f000-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="2f000-117">Pokud vaše typy účastnit v hierarchii dědičnosti, stačí definovat **DbSet** vlastnost pro základní třídu a odvozené typy budou přidány automaticky, pokud jsou ve stejném sestavení jako základní třídy.</span><span class="sxs-lookup"><span data-stu-id="2f000-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="2f000-118">V následujícím příkladu je zaznamenána pouze jedna **DbSet** vlastnosti definované v **SchoolEntities** třídy (**oddělení**).</span><span class="sxs-lookup"><span data-stu-id="2f000-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="2f000-119">Tato vlastnost kód nejprve používá ke zjišťování a získává všechny odkazované typy.</span><span class="sxs-lookup"><span data-stu-id="2f000-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

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

<span data-ttu-id="2f000-120">Pokud chcete vyloučit z modelu typ, použijte **NotMapped** atribut nebo **DbModelBuilder.Ignore** rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="2f000-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="2f000-121">Primární klíče konvence</span><span class="sxs-lookup"><span data-stu-id="2f000-121">Primary Key Convention</span></span>  

<span data-ttu-id="2f000-122">Kód nejprve odvodí, že vlastnost je primární klíč, pokud je vlastnost třídy s názvem "ID" (nerozlišuje velikost písmen) nebo název třídy následovaný "ID".</span><span class="sxs-lookup"><span data-stu-id="2f000-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="2f000-123">Pokud je číselného typu vlastnost primárního klíče nebo identifikátor GUID bude nakonfigurován jako sloupec identity.</span><span class="sxs-lookup"><span data-stu-id="2f000-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="2f000-124">Vztah konvence</span><span class="sxs-lookup"><span data-stu-id="2f000-124">Relationship Convention</span></span>  

<span data-ttu-id="2f000-125">Navigační vlastnosti v Entity Framework, poskytují způsob, jak procházet vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="2f000-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="2f000-126">Každý objekt může mít navigační vlastnost pro každý vztah, ve kterém se podílí.</span><span class="sxs-lookup"><span data-stu-id="2f000-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="2f000-127">Navigační vlastnosti umožňují přejít a spravovat vztahy v obou směrech, vrací buď odkaz na objekt (Pokud je násobnost buď jeden nebo nula nebo jedna) nebo celé kolekci (Pokud je násobnost mnoho).</span><span class="sxs-lookup"><span data-stu-id="2f000-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="2f000-128">Kód nejprve odvodí relace založené na navigační vlastnosti definované ve vašich typů.</span><span class="sxs-lookup"><span data-stu-id="2f000-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="2f000-129">Kromě vlastnosti navigace doporučujeme vám, že složku zahrnujete vlastnosti cizího klíče na typy, které představují závislé objekty.</span><span class="sxs-lookup"><span data-stu-id="2f000-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="2f000-130">Cizí klíč pro daný vztah představuje jakoukoli vlastnost se stejným typem dat jako hlavní vlastnost primárního klíče a s názvem, který následuje jedna z následujících formátů: "\<název navigační vlastnosti\>\<instančního objektu Název vlastnost primárního klíče\>","\<název instančního objektu třídy\>\<vlastnost primárního klíče název\>", nebo"\<název instančního objektu vlastnost primárního klíče\>".</span><span class="sxs-lookup"><span data-stu-id="2f000-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="2f000-131">Pokud se nenajde více shod prioritu v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2f000-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="2f000-132">Detekce cizích klíčů se nerozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="2f000-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="2f000-133">Pokud vlastnost cizího klíče se zjistí, Code First odvodí násobnost relací podle možnosti použití hodnoty Null cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="2f000-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="2f000-134">Pokud je vlastnost s možnou hodnotou Null vztah je registrované jako volitelné. předávala relace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="2f000-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="2f000-135">Pokud není cizí klíč na závislé entity s možnou hodnotou Null, pak Code First nastaví kaskádové odstranění relace.</span><span class="sxs-lookup"><span data-stu-id="2f000-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="2f000-136">Pokud cizí klíč na závislé entita může mít hodnotu Null, Code First nenastaví kaskádové odstranění na relaci a při odstranění objektu zabezpečení cizí klíč se nastaví na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2f000-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="2f000-137">Násobnost a kaskádové odstranění chování detekoval konvence lze přepsat pomocí rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="2f000-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="2f000-138">V následujícím příkladu navigačních vlastností a cizí klíče slouží k definování vztahů mezi třídami oddělení a kurzu.</span><span class="sxs-lookup"><span data-stu-id="2f000-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

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
> <span data-ttu-id="2f000-139">Pokud máte více vztahů mezi stejné typy (Předpokládejme například, že definujete **osoba** a **knihy** třídy, kde **osoba** třída obsahuje  **ReviewedBooks** a **AuthoredBooks** navigačních vlastností a **knihy** třída obsahuje **Autor** a  **Kontrolor** navigační vlastnosti) budete muset ručně konfigurovat vztahy pomocí anotacemi dat nebo rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="2f000-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="2f000-140">Další informace najdete v tématu [anotacemi dat – vztahy](~/ef6/modeling/code-first/data-annotations.md) a [rozhraní Fluent API – vztahy](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="2f000-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="2f000-141">Konvence komplexní typy</span><span class="sxs-lookup"><span data-stu-id="2f000-141">Complex Types Convention</span></span>  

<span data-ttu-id="2f000-142">Při Code First umožňuje zjistit primární klíč nelze odvodit, kde žádný primární klíč je zaregistrované prostřednictvím anotací dat nebo rozhraní fluent API definice třídy, pak typ automaticky zaregistrována jako komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="2f000-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="2f000-143">Komplexní typ detekce také vyžaduje, typ nebude mít vlastnosti, které odkazují na typy entit a neodkazuje na jiný typ vlastnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="2f000-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="2f000-144">Daný následující definice tříd Code First by odvodit, který **podrobnosti** je komplexní typ, protože nemá primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2f000-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

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

## <a name="connection-string-convention"></a><span data-ttu-id="2f000-145">Připojovací řetězec konvence</span><span class="sxs-lookup"><span data-stu-id="2f000-145">Connection String Convention</span></span>  

<span data-ttu-id="2f000-146">Další informace o konvence, DbContext používá ke zjišťování připojení pro použití podívejte [připojení a modely](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="2f000-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="2f000-147">Odebrání konvence</span><span class="sxs-lookup"><span data-stu-id="2f000-147">Removing Conventions</span></span>  

<span data-ttu-id="2f000-148">Můžete odebrat všechny konvence definovaný v oboru názvů System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="2f000-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="2f000-149">Následující příklad odebere **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="2f000-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

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

## <a name="custom-conventions"></a><span data-ttu-id="2f000-150">Vlastní konvence</span><span class="sxs-lookup"><span data-stu-id="2f000-150">Custom Conventions</span></span>  

<span data-ttu-id="2f000-151">Vlastní konvence jsou podporovány v EF6 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="2f000-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="2f000-152">Další informace najdete v části [první konvence kódu vlastní](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="2f000-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
