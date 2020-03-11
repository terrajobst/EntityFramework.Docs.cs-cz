---
title: Rozhraní Fluent API – vztahy – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419071"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="3003d-102">Rozhraní Fluent API – vztahy</span><span class="sxs-lookup"><span data-stu-id="3003d-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="3003d-103">Tato stránka poskytuje informace o nastavení vztahů v modelu Code First pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="3003d-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="3003d-104">Obecné informace o relacích v EF a o tom, jak přistupovat k datům a pracovat s nimi pomocí vztahů, najdete v tématu [relace & vlastnosti navigace](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="3003d-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="3003d-105">Při práci s Code First definujete model tak, že definujete třídy CLR vaší domény.</span><span class="sxs-lookup"><span data-stu-id="3003d-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="3003d-106">Ve výchozím nastavení používá Entity Framework k mapování tříd do schématu databáze Code First konvence.</span><span class="sxs-lookup"><span data-stu-id="3003d-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="3003d-107">Pokud používáte Code First konvence pojmenování, ve většině případů můžete spoléhat na Code First a nastavit vztahy mezi tabulkami na základě cizích klíčů a navigačních vlastností, které definujete u tříd.</span><span class="sxs-lookup"><span data-stu-id="3003d-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="3003d-108">Pokud nedodržujete konvence při definování tříd nebo pokud chcete změnit způsob fungování konvencí, můžete použít rozhraní Fluent API nebo datové poznámky ke konfiguraci tříd tak, aby Code First mohli namapovat relace mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="3003d-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="3003d-109">Úvod</span><span class="sxs-lookup"><span data-stu-id="3003d-109">Introduction</span></span>  

<span data-ttu-id="3003d-110">Když konfigurujete relaci s rozhraním API Fluent, začnete s instancí EntityTypeConfiguration a pak pomocí metody HasRequired, HasOptional nebo HasMany určíte typ vztahu, ve kterém se tato entita účastní.</span><span class="sxs-lookup"><span data-stu-id="3003d-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="3003d-111">Metody HasRequired a HasOptional přebírají výraz lambda, který představuje navigační vlastnost odkazu.</span><span class="sxs-lookup"><span data-stu-id="3003d-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="3003d-112">Metoda HasMany přebírá výraz lambda, který představuje navigační vlastnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="3003d-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="3003d-113">Pak můžete nakonfigurovat vlastnost inverzní navigace pomocí metod WithRequired, WithOptional a WithMany.</span><span class="sxs-lookup"><span data-stu-id="3003d-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="3003d-114">Tyto metody mají přetížení, která neberou argumenty a dají se použít k určení mohutnosti s jednosměrnými navigacemi.</span><span class="sxs-lookup"><span data-stu-id="3003d-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="3003d-115">Pak můžete nakonfigurovat vlastnosti cizího klíče pomocí metody HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="3003d-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="3003d-116">Tato metoda přebírá lambda výraz, který představuje vlastnost, která má být použita jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="3003d-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="3003d-117">Konfigurace požadavku na nepovinnou relaci (1:1 nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="3003d-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="3003d-118">Následující příklad konfiguruje relaci 1:1 nebo 1:1.</span><span class="sxs-lookup"><span data-stu-id="3003d-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="3003d-119">OfficeAssignment má vlastnost InstructorID, která je primárním klíčem a cizím klíčem, protože název vlastnosti nedodržuje konvenci. metoda Haskey – se používá ke konfiguraci primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="3003d-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="3003d-120">Konfigurace relace, kde jsou vyžadovány oba konce (1:1)</span><span class="sxs-lookup"><span data-stu-id="3003d-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="3003d-121">Ve většině případů Entity Framework může odvodit, který typ je závislý a který je objektem zabezpečení v relaci.</span><span class="sxs-lookup"><span data-stu-id="3003d-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="3003d-122">Pokud jsou však požadovány oba konce relace nebo jsou obě strany volitelné Entity Framework nemůžou identifikovat závislé objekty a objekty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3003d-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="3003d-123">V případě potřeby obou konců relace použijte WithRequiredPrincipal nebo WithRequiredDependent za metodou HasRequired.</span><span class="sxs-lookup"><span data-stu-id="3003d-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="3003d-124">Pokud jsou oba konce relace volitelné, použijte WithOptionalPrincipal nebo WithOptionalDependent za metodou HasOptional.</span><span class="sxs-lookup"><span data-stu-id="3003d-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="3003d-125">Konfigurace relace M:n</span><span class="sxs-lookup"><span data-stu-id="3003d-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="3003d-126">Následující kód konfiguruje vztah n:n mezi typy kurzů a instruktorů.</span><span class="sxs-lookup"><span data-stu-id="3003d-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="3003d-127">V následujícím příkladu jsou použity výchozí konvence Code First k vytvoření tabulky JOIN.</span><span class="sxs-lookup"><span data-stu-id="3003d-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="3003d-128">Výsledkem je, že je Tabulka CourseInstructor vytvořená pomocí sloupců Course_CourseID a Instructor_InstructorID.</span><span class="sxs-lookup"><span data-stu-id="3003d-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="3003d-129">Pokud chcete zadat název spojovací tabulky a názvy sloupců v tabulce, musíte pomocí metody map provést další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3003d-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="3003d-130">Následující kód vygeneruje tabulku CourseInstructor se sloupci CourseID a InstructorID.</span><span class="sxs-lookup"><span data-stu-id="3003d-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="3003d-131">Konfigurace relace s jednou navigační vlastností</span><span class="sxs-lookup"><span data-stu-id="3003d-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="3003d-132">Jednosměrná relace (označovaná také jako jednosměrný) je v případě, že je vlastnost navigace definována pouze v jednom z relací a nikoli v obou.</span><span class="sxs-lookup"><span data-stu-id="3003d-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="3003d-133">Podle konvence Code First vždy interpretovat jednosměrný vztah jako jeden k mnoha.</span><span class="sxs-lookup"><span data-stu-id="3003d-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="3003d-134">Například pokud chcete, aby byl vztah 1:1 mezi instruktorem a OfficeAssignment, kde máte navigační vlastnost pouze pro typ instruktora, je nutné použít rozhraní Fluent API ke konfiguraci tohoto vztahu.</span><span class="sxs-lookup"><span data-stu-id="3003d-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="3003d-135">Povolení kaskádového odstraňování</span><span class="sxs-lookup"><span data-stu-id="3003d-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="3003d-136">Kaskádové odstranění v relaci můžete nakonfigurovat pomocí metody WillCascadeOnDelete.</span><span class="sxs-lookup"><span data-stu-id="3003d-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="3003d-137">Pokud cizímu klíči na závislé entitě není nabývat hodnoty null, Code First nastaví kaskádové odstranění v relaci.</span><span class="sxs-lookup"><span data-stu-id="3003d-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="3003d-138">Pokud je cizí klíč na závislé entitě Nullable, Code First v relaci nenastavuje kaskádové odstranění a při odstranění objektu zabezpečení se cizí klíč nastaví na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="3003d-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="3003d-139">Tyto konvence kaskádové odstranění můžete odebrat pomocí:</span><span class="sxs-lookup"><span data-stu-id="3003d-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="3003d-140">modelBuilder. Conventions. Remove\<OneToManyCascadeDeleteConvention\>()</span><span class="sxs-lookup"><span data-stu-id="3003d-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="3003d-141">modelBuilder. Conventions. Remove\<ManyToManyCascadeDeleteConvention\>()</span><span class="sxs-lookup"><span data-stu-id="3003d-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="3003d-142">Následující kód nakonfiguruje vztah, který má být požadován, a poté zakáže kaskádové odstranění.</span><span class="sxs-lookup"><span data-stu-id="3003d-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="3003d-143">Konfigurace složeného cizího klíče</span><span class="sxs-lookup"><span data-stu-id="3003d-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="3003d-144">Pokud se primární klíč na typu oddělení skládá z DepartmentID a vlastností názvu, můžete nakonfigurovat primární klíč pro oddělení a cizí klíč na typech kurzů následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3003d-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="3003d-145">Přejmenování cizího klíče, který není v modelu definován</span><span class="sxs-lookup"><span data-stu-id="3003d-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="3003d-146">Pokud se rozhodnete nedefinovat cizí klíč pro typ CLR, ale chcete určit, jaký název by měl mít v databázi, udělejte toto:</span><span class="sxs-lookup"><span data-stu-id="3003d-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="3003d-147">Konfigurace názvu cizího klíče, který nedodržuje konvenci Code First</span><span class="sxs-lookup"><span data-stu-id="3003d-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="3003d-148">Pokud byla vlastnost cizího klíče ve třídě Course SomeDepartmentID namísto DepartmentID, je třeba provést následující kroky, abyste určili, že má SomeDepartmentID být cizí klíč:</span><span class="sxs-lookup"><span data-stu-id="3003d-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="3003d-149">Model používaný v ukázkách</span><span class="sxs-lookup"><span data-stu-id="3003d-149">Model Used in Samples</span></span>  

<span data-ttu-id="3003d-150">Následující model Code First se používá pro ukázky na této stránce.</span><span class="sxs-lookup"><span data-stu-id="3003d-150">The following Code First model is used for the samples on this page.</span></span>  

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
