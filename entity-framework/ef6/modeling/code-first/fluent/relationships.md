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
# <a name="fluent-api---relationships"></a>Rozhraní Fluent API – vztahy
> [!NOTE]
> Tato stránka poskytuje informace o nastavení vztahů v modelu Code First pomocí rozhraní Fluent API. Obecné informace o relacích v EF a o tom, jak přistupovat k datům a pracovat s nimi pomocí vztahů, najdete v tématu [relace & vlastnosti navigace](~/ef6/fundamentals/relationships.md).  

Při práci s Code First definujete model tak, že definujete třídy CLR vaší domény. Ve výchozím nastavení používá Entity Framework k mapování tříd do schématu databáze Code First konvence. Pokud používáte Code First konvence pojmenování, ve většině případů můžete spoléhat na Code First a nastavit vztahy mezi tabulkami na základě cizích klíčů a navigačních vlastností, které definujete u tříd. Pokud nedodržujete konvence při definování tříd nebo pokud chcete změnit způsob fungování konvencí, můžete použít rozhraní Fluent API nebo datové poznámky ke konfiguraci tříd tak, aby Code First mohli namapovat relace mezi tabulkami.  

## <a name="introduction"></a>Úvod  

Když konfigurujete relaci s rozhraním API Fluent, začnete s instancí EntityTypeConfiguration a pak pomocí metody HasRequired, HasOptional nebo HasMany určíte typ vztahu, ve kterém se tato entita účastní. Metody HasRequired a HasOptional přebírají výraz lambda, který představuje navigační vlastnost odkazu. Metoda HasMany přebírá výraz lambda, který představuje navigační vlastnost kolekce. Pak můžete nakonfigurovat vlastnost inverzní navigace pomocí metod WithRequired, WithOptional a WithMany. Tyto metody mají přetížení, která neberou argumenty a dají se použít k určení mohutnosti s jednosměrnými navigacemi.  

Pak můžete nakonfigurovat vlastnosti cizího klíče pomocí metody HasForeignKey. Tato metoda přebírá lambda výraz, který představuje vlastnost, která má být použita jako cizí klíč.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Konfigurace požadavku na nepovinnou relaci (1:1 nebo jedna)  

Následující příklad konfiguruje relaci 1:1 nebo 1:1. OfficeAssignment má vlastnost InstructorID, která je primárním klíčem a cizím klíčem, protože název vlastnosti nedodržuje konvenci. metoda Haskey – se používá ke konfiguraci primárního klíče.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Konfigurace relace, kde jsou vyžadovány oba konce (1:1)  

Ve většině případů Entity Framework může odvodit, který typ je závislý a který je objektem zabezpečení v relaci. Pokud jsou však požadovány oba konce relace nebo jsou obě strany volitelné Entity Framework nemůžou identifikovat závislé objekty a objekty zabezpečení. V případě potřeby obou konců relace použijte WithRequiredPrincipal nebo WithRequiredDependent za metodou HasRequired. Pokud jsou oba konce relace volitelné, použijte WithOptionalPrincipal nebo WithOptionalDependent za metodou HasOptional.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Konfigurace relace M:n  

Následující kód konfiguruje vztah n:n mezi typy kurzů a instruktorů. V následujícím příkladu jsou použity výchozí konvence Code First k vytvoření tabulky JOIN. Výsledkem je, že je Tabulka CourseInstructor vytvořená pomocí sloupců Course_CourseID a Instructor_InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Pokud chcete zadat název spojovací tabulky a názvy sloupců v tabulce, musíte pomocí metody map provést další konfiguraci. Následující kód vygeneruje tabulku CourseInstructor se sloupci CourseID a InstructorID.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Konfigurace relace s jednou navigační vlastností  

Jednosměrná relace (označovaná také jako jednosměrný) je v případě, že je vlastnost navigace definována pouze v jednom z relací a nikoli v obou. Podle konvence Code First vždy interpretovat jednosměrný vztah jako jeden k mnoha. Například pokud chcete, aby byl vztah 1:1 mezi instruktorem a OfficeAssignment, kde máte navigační vlastnost pouze pro typ instruktora, je nutné použít rozhraní Fluent API ke konfiguraci tohoto vztahu.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Povolení kaskádového odstraňování  

Kaskádové odstranění v relaci můžete nakonfigurovat pomocí metody WillCascadeOnDelete. Pokud cizímu klíči na závislé entitě není nabývat hodnoty null, Code First nastaví kaskádové odstranění v relaci. Pokud je cizí klíč na závislé entitě Nullable, Code First v relaci nenastavuje kaskádové odstranění a při odstranění objektu zabezpečení se cizí klíč nastaví na hodnotu null.  

Tyto konvence kaskádové odstranění můžete odebrat pomocí:  

modelBuilder. Conventions. Remove\<OneToManyCascadeDeleteConvention\>()  
modelBuilder. Conventions. Remove\<ManyToManyCascadeDeleteConvention\>()  

Následující kód nakonfiguruje vztah, který má být požadován, a poté zakáže kaskádové odstranění.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Konfigurace složeného cizího klíče  

Pokud se primární klíč na typu oddělení skládá z DepartmentID a vlastností názvu, můžete nakonfigurovat primární klíč pro oddělení a cizí klíč na typech kurzů následujícím způsobem:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Přejmenování cizího klíče, který není v modelu definován  

Pokud se rozhodnete nedefinovat cizí klíč pro typ CLR, ale chcete určit, jaký název by měl mít v databázi, udělejte toto:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Konfigurace názvu cizího klíče, který nedodržuje konvenci Code First  

Pokud byla vlastnost cizího klíče ve třídě Course SomeDepartmentID namísto DepartmentID, je třeba provést následující kroky, abyste určili, že má SomeDepartmentID být cizí klíč:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Model používaný v ukázkách  

Následující model Code First se používá pro ukázky na této stránce.  

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
