---
title: Rozhraní Fluent API – vztahy - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
caps.latest.revision: 3
ms.openlocfilehash: 7437b395fbed07dcb8c93cd8418317611e14a60b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912661"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="d00e5-102">Rozhraní Fluent API – vztahy</span><span class="sxs-lookup"><span data-stu-id="d00e5-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="d00e5-103">Tato stránka obsahuje informace o nastavení relace v modelu Code First pomocí rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="d00e5-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="d00e5-104">Obecné informace o relacích v EF a jak přistupovat k a manipulaci s daty pomocí relací najdete v tématu [vztahy a navigačních vlastností](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="d00e5-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="d00e5-105">Při práci s Code First, definovat model definováním třídy CLR vaší domény.</span><span class="sxs-lookup"><span data-stu-id="d00e5-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="d00e5-106">Ve výchozím nastavení používá Entity Framework Code First konvence pro mapování na schéma databáze vaší třídy.</span><span class="sxs-lookup"><span data-stu-id="d00e5-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="d00e5-107">Pokud používáte Code First zásady vytváření názvů, ve většině případů můžete spolehnout na Code First pro nastavení relací mezi tabulkami podle cizího klíče a navigačních vlastností, které definujete na třídách.</span><span class="sxs-lookup"><span data-stu-id="d00e5-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="d00e5-108">Pokud při definování třídy neřídí konvence, nebo pokud chcete změnit způsob, jakým konvencí fungují, můžete použít rozhraní fluent API nebo datové poznámky ke konfiguraci třídy, takže Code First můžete mapovat vztahy mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="d00e5-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="d00e5-109">Úvod</span><span class="sxs-lookup"><span data-stu-id="d00e5-109">Introduction</span></span>  

<span data-ttu-id="d00e5-110">Při konfiguraci relace s rozhraním API fluent, začněte s EntityTypeConfiguration instance a potom použít metodu HasRequired HasOptional či HasMany k určení typu vztahu, který se účastní tuto entitu.</span><span class="sxs-lookup"><span data-stu-id="d00e5-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="d00e5-111">Metody HasRequired a HasOptional trvat výraz lambda reprezentující navigační vlastnost odkaz.</span><span class="sxs-lookup"><span data-stu-id="d00e5-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="d00e5-112">Metoda HasMany má výraz lambda reprezentující navigační vlastnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="d00e5-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="d00e5-113">Potom můžete nakonfigurovat inverzní navigační vlastnost s použitím metody WithRequired WithOptional a WithMany.</span><span class="sxs-lookup"><span data-stu-id="d00e5-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="d00e5-114">Tyto metody mají přetížení, které nepřebírají argumenty a je možné určit Kardinalita se jednosměrný navigaci.</span><span class="sxs-lookup"><span data-stu-id="d00e5-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="d00e5-115">Potom můžete nakonfigurovat vlastnosti cizího klíče pomocí metody HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="d00e5-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="d00e5-116">Tato metoda přebírá výraz lambda reprezentující vlastnost, která má být použit jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="d00e5-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="d00e5-117">Konfigurace požadované na volitelný vztah (jeden – nulu nebo m)</span><span class="sxs-lookup"><span data-stu-id="d00e5-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="d00e5-118">Následující příklad nakonfiguruje vztah jeden: nula nebo 1.</span><span class="sxs-lookup"><span data-stu-id="d00e5-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="d00e5-119">OfficeAssignment má InstructorID vlastnost, která je primární klíč a cizí klíče, protože název vlastnosti není postupujte podle úmluvy HasKey metoda se používá ke konfiguraci primární klíč.</span><span class="sxs-lookup"><span data-stu-id="d00e5-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="d00e5-120">Konfigurace relace, ve kterém jsou nutné obou koncích (1: 1)</span><span class="sxs-lookup"><span data-stu-id="d00e5-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="d00e5-121">Ve většině případů lze odvodit Entity Framework typů, které se je rolích dependent a který instanční objekt v relaci.</span><span class="sxs-lookup"><span data-stu-id="d00e5-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="d00e5-122">Ale při jak elementy end vztahu jsou povinné nebo obě strany jsou volitelné Entity Framework nemůže identifikovat závislé a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d00e5-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="d00e5-123">Když oba elementy end vztahu jsou požadovány, použijte WithRequiredPrincipal nebo WithRequiredDependent po metodě HasRequired.</span><span class="sxs-lookup"><span data-stu-id="d00e5-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="d00e5-124">Když oba elementy end vztahu jsou volitelné, použití WithOptionalPrincipal nebo WithOptionalDependent po HasOptional metody.</span><span class="sxs-lookup"><span data-stu-id="d00e5-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="d00e5-125">Konfigurace vztah mnoho mnoho</span><span class="sxs-lookup"><span data-stu-id="d00e5-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="d00e5-126">Následující kód konfiguruje many-to-many vztah mezi typy kurzu a instruktorem.</span><span class="sxs-lookup"><span data-stu-id="d00e5-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="d00e5-127">V následujícím příkladu se používají výchozích konvencí Code First a vytvořit tabulku spojení.</span><span class="sxs-lookup"><span data-stu-id="d00e5-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="d00e5-128">V důsledku CourseInstructor tabulky se vytvoří s Course_CourseID a Instructor_InstructorID sloupce.</span><span class="sxs-lookup"><span data-stu-id="d00e5-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="d00e5-129">Pokud chcete zadat název tabulky spojení a názvy sloupců v tabulce, je potřeba provést další konfiguraci, pomocí metody mapování.</span><span class="sxs-lookup"><span data-stu-id="d00e5-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="d00e5-130">Následující kód vygeneruje CourseInstructor tabulku se sloupci CourseID a InstructorID.</span><span class="sxs-lookup"><span data-stu-id="d00e5-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="d00e5-131">Konfigurace relace s jednu navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="d00e5-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="d00e5-132">Jednosměrnou (nazývané také jednosměrnou) je při navigační vlastnost je definována pouze na jednom z konců relace a ne na obě relace.</span><span class="sxs-lookup"><span data-stu-id="d00e5-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="d00e5-133">Podle konvence Code First vždy interpretuje jednosměrnou relaci jako jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="d00e5-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="d00e5-134">Například pokud chcete, aby vztah 1: 1 mezi instruktorem a OfficeAssignment, kde mají vlastnost navigace na pouze typ instruktorem, budete muset použít rozhraní fluent API nakonfigurovat tuto relaci.</span><span class="sxs-lookup"><span data-stu-id="d00e5-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="d00e5-135">Povolení kaskádové odstranění</span><span class="sxs-lookup"><span data-stu-id="d00e5-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="d00e5-136">Kaskádové odstranění můžete nakonfigurovat na vztahu WillCascadeOnDelete metodou.</span><span class="sxs-lookup"><span data-stu-id="d00e5-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="d00e5-137">Pokud není cizí klíč na závislé entity s možnou hodnotou Null, pak Code First nastaví kaskádové odstranění relace.</span><span class="sxs-lookup"><span data-stu-id="d00e5-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="d00e5-138">Pokud cizí klíč na závislé entita může mít hodnotu Null, Code First nenastaví kaskádové odstranění na relaci a při odstranění objektu zabezpečení cizí klíč se nastaví na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d00e5-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="d00e5-139">Tato konvence cascade delete můžete odebrat pomocí:</span><span class="sxs-lookup"><span data-stu-id="d00e5-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="d00e5-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="d00e5-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="d00e5-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="d00e5-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="d00e5-142">Následující kód nakonfiguruje vztah jako povinné a zakáže kaskádové odstranění.</span><span class="sxs-lookup"><span data-stu-id="d00e5-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="d00e5-143">Konfigurace složený cizí klíč</span><span class="sxs-lookup"><span data-stu-id="d00e5-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="d00e5-144">Pokud se primární klíč pro typ oddělení DepartmentID a název vlastnosti, by nakonfigurujete primární klíč pro oddělení a cizí klíč pro typy kurzu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d00e5-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="d00e5-145">Přejmenování cizí klíč, který není definován v modelu</span><span class="sxs-lookup"><span data-stu-id="d00e5-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="d00e5-146">Pokud se rozhodnete definovat cizího klíče v typu modulu CLR, ale chcete k zadání názvu, jaký by měl mít v databázi, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d00e5-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="d00e5-147">Konfigurace název cizí klíče, který není podle úmluvy první kódu</span><span class="sxs-lookup"><span data-stu-id="d00e5-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="d00e5-148">Pokud vlastnost cizího klíče ve třídě kurzu byla volána SomeDepartmentID místo DepartmentID, musíte následujícím postupem určete, jestli má SomeDepartmentID jako cizí klíč:</span><span class="sxs-lookup"><span data-stu-id="d00e5-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="d00e5-149">Model použitý v ukázky</span><span class="sxs-lookup"><span data-stu-id="d00e5-149">Model Used in Samples</span></span>  

<span data-ttu-id="d00e5-150">Následující kód první model se používá pro ukázky na této stránce.</span><span class="sxs-lookup"><span data-stu-id="d00e5-150">The following Code First model is used for the samples on this page.</span></span>  

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
