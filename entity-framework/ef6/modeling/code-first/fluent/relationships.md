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
# <a name="fluent-api---relationships"></a>Rozhraní Fluent API – vztahy
> [!NOTE]
> Tato stránka obsahuje informace o nastavení relace v modelu Code First pomocí rozhraní fluent API. Obecné informace o relacích v EF a jak přistupovat k a manipulaci s daty pomocí relací najdete v tématu [vztahy a navigačních vlastností](~/ef6/fundamentals/relationships.md).  

Při práci s Code First, definovat model definováním třídy CLR vaší domény. Ve výchozím nastavení používá Entity Framework Code First konvence pro mapování na schéma databáze vaší třídy. Pokud používáte Code First zásady vytváření názvů, ve většině případů můžete spolehnout na Code First pro nastavení relací mezi tabulkami podle cizího klíče a navigačních vlastností, které definujete na třídách. Pokud při definování třídy neřídí konvence, nebo pokud chcete změnit způsob, jakým konvencí fungují, můžete použít rozhraní fluent API nebo datové poznámky ke konfiguraci třídy, takže Code First můžete mapovat vztahy mezi tabulkami.  

## <a name="introduction"></a>Úvod  

Při konfiguraci relace s rozhraním API fluent, začněte s EntityTypeConfiguration instance a potom použít metodu HasRequired HasOptional či HasMany k určení typu vztahu, který se účastní tuto entitu. Metody HasRequired a HasOptional trvat výraz lambda reprezentující navigační vlastnost odkaz. Metoda HasMany má výraz lambda reprezentující navigační vlastnost kolekce. Potom můžete nakonfigurovat inverzní navigační vlastnost s použitím metody WithRequired WithOptional a WithMany. Tyto metody mají přetížení, které nepřebírají argumenty a je možné určit Kardinalita se jednosměrný navigaci.  

Potom můžete nakonfigurovat vlastnosti cizího klíče pomocí metody HasForeignKey. Tato metoda přebírá výraz lambda reprezentující vlastnost, která má být použit jako cizí klíč.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Konfigurace požadované na volitelný vztah (jeden – nulu nebo m)  

Následující příklad nakonfiguruje vztah jeden: nula nebo 1. OfficeAssignment má InstructorID vlastnost, která je primární klíč a cizí klíče, protože název vlastnosti není postupujte podle úmluvy HasKey metoda se používá ke konfiguraci primární klíč.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Konfigurace relace, ve kterém jsou nutné obou koncích (1: 1)  

Ve většině případů lze odvodit Entity Framework typů, které se je rolích dependent a který instanční objekt v relaci. Ale při jak elementy end vztahu jsou povinné nebo obě strany jsou volitelné Entity Framework nemůže identifikovat závislé a zabezpečení. Když oba elementy end vztahu jsou požadovány, použijte WithRequiredPrincipal nebo WithRequiredDependent po metodě HasRequired. Když oba elementy end vztahu jsou volitelné, použití WithOptionalPrincipal nebo WithOptionalDependent po HasOptional metody.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Konfigurace vztah mnoho mnoho  

Následující kód konfiguruje many-to-many vztah mezi typy kurzu a instruktorem. V následujícím příkladu se používají výchozích konvencí Code First a vytvořit tabulku spojení. V důsledku CourseInstructor tabulky se vytvoří s Course_CourseID a Instructor_InstructorID sloupce.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Pokud chcete zadat název tabulky spojení a názvy sloupců v tabulce, je potřeba provést další konfiguraci, pomocí metody mapování. Následující kód vygeneruje CourseInstructor tabulku se sloupci CourseID a InstructorID.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Konfigurace relace s jednu navigační vlastnost  

Jednosměrnou (nazývané také jednosměrnou) je při navigační vlastnost je definována pouze na jednom z konců relace a ne na obě relace. Podle konvence Code First vždy interpretuje jednosměrnou relaci jako jeden mnoho. Například pokud chcete, aby vztah 1: 1 mezi instruktorem a OfficeAssignment, kde mají vlastnost navigace na pouze typ instruktorem, budete muset použít rozhraní fluent API nakonfigurovat tuto relaci.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Povolení kaskádové odstranění  

Kaskádové odstranění můžete nakonfigurovat na vztahu WillCascadeOnDelete metodou. Pokud není cizí klíč na závislé entity s možnou hodnotou Null, pak Code First nastaví kaskádové odstranění relace. Pokud cizí klíč na závislé entita může mít hodnotu Null, Code First nenastaví kaskádové odstranění na relaci a při odstranění objektu zabezpečení cizí klíč se nastaví na hodnotu null.  

Tato konvence cascade delete můžete odebrat pomocí:  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

Následující kód nakonfiguruje vztah jako povinné a zakáže kaskádové odstranění.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Konfigurace složený cizí klíč  

Pokud se primární klíč pro typ oddělení DepartmentID a název vlastnosti, by nakonfigurujete primární klíč pro oddělení a cizí klíč pro typy kurzu následujícím způsobem:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Přejmenování cizí klíč, který není definován v modelu  

Pokud se rozhodnete definovat cizího klíče v typu modulu CLR, ale chcete k zadání názvu, jaký by měl mít v databázi, postupujte takto:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Konfigurace název cizí klíče, který není podle úmluvy první kódu  

Pokud vlastnost cizího klíče ve třídě kurzu byla volána SomeDepartmentID místo DepartmentID, musíte následujícím postupem určete, jestli má SomeDepartmentID jako cizí klíč:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Model použitý v ukázky  

Následující kód první model se používá pro ukázky na této stránce.  

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
