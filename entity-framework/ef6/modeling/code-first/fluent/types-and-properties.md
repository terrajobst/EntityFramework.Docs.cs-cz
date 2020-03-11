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
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Rozhraní Fluent API – konfigurace a mapování vlastností a typů
Při práci s Entity Framework Code First výchozím chováním je namapovat třídy POCO na tabulky pomocí sady konvencí vloženými na EF. V některých případech však nemůžete ani nebudete chtít tyto konvence namapovat na jinou, než jaké konvence určují.  

Existují dva hlavní způsoby, jak můžete nakonfigurovat EF, aby používaly jinou než konvenci, konkrétně [poznámky](~/ef6/modeling/code-first/data-annotations.md) nebo rozhraní EFs Fluent API. Poznámky se vztahují pouze na podmnožinu funkcí rozhraní Fluent API, takže existují mapování scénářů, které nelze dosáhnout pomocí poznámek. Tento článek je navržený tak, aby ukázal, jak pomocí rozhraní Fluent API konfigurovat vlastnosti.  

První rozhraní API pro Fluent kódu je nejčastěji dostupné přepsáním metody [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) v odvozeném [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Následující ukázky jsou navržené tak, aby ukázaly, jak provádět různé úlohy pomocí rozhraní Fluent API, a umožňuje zkopírovat kód a přizpůsobit ho tak, aby vyhovoval vašemu modelu. Pokud si chcete prohlédnout model, který je možné použít s tak, jak je, je uvedený na konci tohoto článku.  

## <a name="model-wide-settings"></a>Nastavení pro nejrůznější modely  

### <a name="default-schema-ef6-onwards"></a>Výchozí schéma (EF6 a vyšší)  

Počínaje EF6 můžete použít metodu HasDefaultSchema na DbModelBuilder k určení schématu databáze pro použití pro všechny tabulky, uložené procedury atd. Toto výchozí nastavení bude potlačeno pro všechny objekty, pro které explicitně nakonfigurujete jiné schéma pro.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Vlastní konvence (EF6 a vyšší)  

Počínaje EF6 můžete vytvořit vlastní konvence, které doplňují ta, která jsou obsažená v Code First. Další podrobnosti najdete v tématu [Vlastní konvence Code First](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Mapování vlastností  

Metoda [vlastnosti](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) se používá ke konfiguraci atributů pro každou vlastnost patřící k entitě nebo komplexnímu typu. Metoda Property slouží k získání objektu konfigurace pro danou vlastnost. Možnosti objektu konfigurace jsou specifické pro nakonfigurovaný typ. Kódování Unicode je k dispozici pouze v případě vlastností řetězce.  

### <a name="configuring-a-primary-key"></a>Konfigurace primárního klíče  

Entity Framework konvence pro primární klíče je:  

1. Vaše třída definuje vlastnost, jejíž název je "ID" nebo "ID".  
2. nebo název třídy následovaný "ID" nebo "ID"  

Chcete-li explicitně nastavit vlastnost jako primární klíč, můžete použít metodu Haskey –. V následujícím příkladu se metoda Haskey – používá ke konfiguraci primárního klíče InstructorID na typu OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Konfigurace složeného primárního klíče  

Následující příklad konfiguruje vlastnosti DepartmentID a Name jako složený primární klíč typu oddělení.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Vypínání identity pro číselné primární klíče  

Následující příklad nastaví vlastnost DepartmentID na hodnotu System. ComponentModel. DataAnnotations. DatabaseGeneratedOption. None, aby označovala, že tato hodnota nebude vygenerována databází.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Určení maximální délky vlastnosti  

V následujícím příkladu by vlastnost Name neměla být delší než 50 znaků. Pokud nastavíte hodnotu delší než 50 znaků, zobrazí se výjimka [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) . Pokud Code First vytvoří databázi z tohoto modelu, nastaví se také maximální délka sloupce Name na 50 znaků.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Konfigurace vlastnosti, která má být požadována  

V následujícím příkladu je vyžadována vlastnost Name. Pokud název nezadáte, zobrazí se výjimka DbEntityValidationException. Pokud Code First vytvoří databázi z tohoto modelu, sloupec použitý k uložení této vlastnosti obvykle nebude možnou hodnotou null.  

> [!NOTE]
> V některých případech nemusí být možné, aby sloupec v databázi mohl mít hodnotu null, i když je požadovaná vlastnost. Pokud například použijete strategii dědičnosti TPH pro více typů, uloží se do jedné tabulky. Pokud odvozený typ obsahuje povinnou vlastnost, sloupec nemůže být nastaven na hodnotu null, protože ne všechny typy v hierarchii budou mít tuto vlastnost.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Konfigurace indexu pro jednu nebo více vlastností  

> [!NOTE]
> **EF 6.1 a vyšší pouze** – atribut indexu byl představen v Entity Framework 6,1. Pokud používáte starší verzi, informace v této části se nevztahují.  

Vytváření indexů není v rozhraní API Fluent nativně podporované, ale můžete využít podporu **IndexAttribute** prostřednictvím rozhraní Fluent API. Atributy indexu jsou zpracovávány zahrnutím anotace modelu do modelu, který je poté převeden na index v databázi později v kanálu. Stejné Anotace můžete přidat ručně pomocí rozhraní Fluent API.  

Nejjednodušší způsob, jak to provést, je vytvořit instanci **IndexAttribute** , která obsahuje všechna nastavení nového indexu. Pak můžete vytvořit instanci **IndexAnnotation** , která je specifickým typem EF, který převede nastavení **IndexAttribute** na anotaci modelu, která může být uložena v modelu EF. Ty pak můžete předat metodě **HasColumnAnnotation** v rozhraní Fluent API a zadat **index** názvu pro anotaci.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Úplný seznam nastavení dostupných v **IndexAttribute**najdete v části *Index* [datových poznámek Code First](~/ef6/modeling/code-first/data-annotations.md). To zahrnuje přizpůsobení názvu indexu, vytváření jedinečných indexů a vytváření indexů ve více sloupcích.  

Můžete zadat více indexových poznámek pro jednu vlastnost předáním pole **IndexAttribute** do konstruktoru třídy **IndexAnnotation**.  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Určení, že se má mapovat vlastnost CLR na sloupec v databázi  

Následující příklad ukazuje, jak určit, že vlastnost u typu CLR není namapována na sloupec v databázi.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Mapování vlastnosti CLR na konkrétní sloupec v databázi  

V následujícím příkladu je mapována vlastnost název CLR na sloupec databáze oddělení.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Přejmenování cizího klíče, který není v modelu definován  

Pokud se rozhodnete nedefinovat cizí klíč pro typ CLR, ale chcete určit, jaký název by měl mít v databázi, udělejte toto:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Konfigurace, zda řetězcová vlastnost podporuje obsah v kódování Unicode  

Ve výchozím nastavení jsou řetězce znakové sady Unicode (nvarchar v SQL Server). Pomocí metody "v kódování Unicode" lze určit, že řetězec by měl být typu varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Konfigurace datového typu sloupce databáze  

Metoda [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) umožňuje mapování na různé reprezentace stejného základního typu. Použití této metody vám neumožňuje provádět převod dat za běhu. Všimněte si, že v kódování Unicode je preferovaný způsob, jak nastavit sloupce na varchar, protože se jedná o nezávislá databáze.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Konfigurace vlastností komplexního typu  

Existují dva způsoby konfigurace skalárních vlastností pro komplexní typ.  

Můžete volat vlastnost v ComplexTypeConfiguration.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Zápis tečky lze také použít pro přístup k vlastnosti komplexního typu.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Konfigurace vlastnosti, která se má použít jako Optimistická souběžnost tokenu  

Chcete-li určit, že vlastnost v entitě představuje token souběžnosti, můžete použít buď atribut ConcurrencyCheck, nebo metodu IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Můžete také použít metodu IsRowVersion a nakonfigurovat vlastnost tak, aby byla v databázi verze řádku. Nastavením vlastnosti na hodnotu je verze řádku automaticky nakonfigurujete, aby se jedná o optimistický Token souběžnosti.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Mapování typů  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Určení, že třída je komplexního typu  

Podle konvence typ, který nemá zadaný primární klíč, se považuje za komplexní typ. Existují některé scénáře, kdy Code First nezjistí složitý typ (například pokud máte vlastnost s názvem ID, ale nepovažujete to za primární klíč). V takových případech byste pomocí rozhraní Fluent API explicitně určili, že typ je komplexní typ.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Určení nemapování typu entity CLR na tabulku v databázi  

Následující příklad ukazuje, jak vyloučit typ CLR z mapování na tabulku v databázi.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mapování typu entity na konkrétní tabulku v databázi  

Všechny vlastnosti oddělení budou namapovány na sloupce v tabulce s názvem t_ oddělení.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Název schématu můžete zadat také takto:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Mapování dědičnosti typu tabulka na hierarchii (TPH)  

Ve scénáři mapování TPH jsou všechny typy v hierarchii dědičnosti mapovány na jednu tabulku. Sloupec diskriminátoru se používá k identifikaci typu každého řádku. Při vytváření modelu pomocí Code First je pro typy, které se podílejí na hierarchii dědičnosti, výchozí strategie. Ve výchozím nastavení se sloupec diskriminátor přidá do tabulky s názvem "diskriminátor" a název typu CLR každého typu v hierarchii se používá pro hodnoty diskriminátoru. Výchozí chování můžete upravit pomocí rozhraní Fluent API.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Mapování dědičnosti typu tabulka na typ (TPT)  

Ve scénáři mapování TPT jsou všechny typy namapovány na jednotlivé tabulky. Vlastnosti, které patří výhradně k základnímu typu nebo odvozenému typu, jsou uloženy v tabulce, která je mapována na daný typ. Tabulky, které se mapují na odvozené typy, také uloží cizí klíč, který spojí odvozenou tabulku se základní tabulkou.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Mapování dědičnosti třídy TPC (Table-per-beton)  

Ve scénáři mapování TPC jsou všechny neabstraktní typy v hierarchii namapovány na jednotlivé tabulky. Tabulky, které jsou mapovány na odvozené třídy, nemají žádnou relaci s tabulkou, která je v databázi mapována na základní třídu. Všechny vlastnosti třídy, včetně děděných vlastností, jsou namapovány na sloupce odpovídající tabulky.  

Pro konfiguraci každého odvozeného typu zavolejte metodu MapInheritedProperties. MapInheritedProperties přemapuje všechny vlastnosti, které byly děděny ze základní třídy na nové sloupce v tabulce pro odvozenou třídu.  

> [!NOTE]
> Všimněte si, že vzhledem k tomu, že tabulky, které se účastní hierarchie dědičnosti TPC, nesdílejí primární klíč, budou při vkládání do tabulek mapovaných na podtřídy duplicitní klíče entit, pokud máte databáze vygenerované hodnotami se stejnou počáteční hodnotou identity. Chcete-li tento problém vyřešit, můžete buď zadat jinou počáteční počáteční hodnotu pro každou tabulku, nebo vypnout identitu u vlastnosti primárního klíče. Identita je výchozí hodnota vlastností celočíselného klíče při práci s Code First.  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Mapování vlastností typu entity na více tabulek v databázi (rozdělení entity)  

Rozdělení entit umožňuje rozdělit vlastnosti typu entity napříč více tabulkami. V následujícím příkladu je entita oddělení rozdělená na dvě tabulky: oddělení a DepartmentDetails. Rozdělování entit používá více volání metody map k mapování podmnožiny vlastností na konkrétní tabulku.  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Mapování více typů entit na jednu tabulku v databázi (rozdělení tabulky)  

Následující příklad mapuje dva typy entit, které sdílejí primární klíč s jednou tabulkou.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Mapování typu entity na vložení, aktualizaci nebo odstranění uložených procedur (EF6 a vyšší)  

Počínaje EF6 můžete namapovat entitu na použití uložených procedur pro vložení Update a DELETE. Další podrobnosti najdete v [Code First uložených procedurách INSERT, Update a DELETE](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
