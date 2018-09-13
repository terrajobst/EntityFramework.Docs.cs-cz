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
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Rozhraní Fluent API – konfigurace a mapování vlastností a typy
Při práci s Entity Framework Code First k mapování tříd POCO tabulky pomocí sady konvence vloženými do EF je výchozí chování. V některých případech však můžete nelze nebo nechcete, aby tyto zásady pro vytváření a potřebují mít možnost na mapování entit na něco jiného, než co konvencí diktování.  

Existují dva hlavní způsoby, jak můžete nakonfigurovat EF použít něco jiného než konvence, a to [poznámky](~/ef6/modeling/code-first/data-annotations.md) nebo rozhraní API fluent systémem souborů EFs. Poznámky se vztahují pouze na podmnožinu fluent funkcí rozhraní API, takže mapování scénářů, které není možné dosáhnout pomocí poznámek. Tento článek slouží k ukazují, jak použít rozhraní fluent API ke konfiguraci vlastností.  

První fluent API kód přistupuje nejčastěji tak, že přepíšete [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) metodu na vaší odvozené [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Následující ukázky jsou navrženy pro ukazují, jak provádět různé úlohy s rozhraním api fluent a umožňují zkopírovat kód a přizpůsobit ho tak, aby odpovídala modelu, pokud chcete zobrazit model, který je možné použít s jako-je pak najdete na konci tohoto článku.  

## <a name="model-wide-settings"></a>Nastavení pro model  

### <a name="default-schema-ef6-onwards"></a>Výchozí schéma (ef6 nebo novější)  

Počínaje EF6 můžete použít metodu HasDefaultSchema na DbModelBuilder určit schématu databáze pro všechny tabulky, uložené procedury, atd. Toto výchozí nastavení, budou ignorovány pro všechny objekty, které explicitně nakonfigurovat jiné schéma pro.  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a>Vlastní konvence (ef6 nebo novější)  

Spouští se s EF6, můžete vytvořit vlastní zásady odvíjející doplnit těm, které jsou součástí Code First. Další podrobnosti najdete v tématu [první konvence kódu vlastní](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Mapování vlastností  

[Vlastnost](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) metoda se používá ke konfiguraci atributy pro každou vlastnost patřící do entity nebo komplexního typu. Metoda vlastnosti slouží k získání objektu konfigurace pro danou vlastnost. Možnosti na objekt konfigurace jsou specifické pro daný typ, která se právě nastavuje; Například je k dispozici pouze pro vlastnosti řetězce IsUnicode.  

### <a name="configuring-a-primary-key"></a>Konfigurace primární klíč  

Entity Framework konvence pro primární klíče je:  

1. Vaše třída definuje vlastnost, jejíž název je "ID" nebo "Id"  
2. nebo název třídy následovaný "ID" nebo "Id"  

Pokud chcete explicitně nastavit vlastnost, která má být primární klíč, můžete použít metodu HasKey. V následujícím příkladu metoda HasKey slouží ke konfiguraci primární klíč InstructorID OfficeAssignment typu.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Konfigurace složený primární klíč  

Následující příklad nastaví DepartmentID a název vlastnosti, které chcete být složený primární klíč typu oddělení.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Vypínají se Identity pro číselné primární klíče  

Následující příklad nastaví vlastnost DepartmentID System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None k označení, že hodnota nevygeneruje v databázi.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Zadání maximální délku u vlastnosti  

V následujícím příkladu vlastnost Name by měla být delší než 50 znaků. Pokud provedete delší než 50 znaků. hodnota, zobrazí se [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) výjimky. Pokud Code First vytvoří databázi z tohoto modelu také nastaví maximální délka sloupce název až 50 znaků.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Konfigurace vlastnosti jako povinné.  

V následujícím příkladu se vyžaduje vlastnost Name. Pokud název nezadáte, zobrazí se výjimka DbEntityValidationException. Pokud Code First vytvoří databázi z tohoto modelu pak sloupec použitý k uložení této vlastnosti se obvykle mít hodnotu Null.  

> [!NOTE]
> V některých případech nemusí být možné pro sloupec v databázi být null, i když tato vlastnost je vyžadovaná. Například při použití dat TPH dědičnosti strategie pro více typů je uložené v jediné tabulce. Pokud odvozený typ obsahuje požadovanou vlastnost sloupec nelze nastavit jako Null protože ne všechny typy v hierarchii, bude mít tato vlastnost.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Konfigurace na jednu nebo více vlastností indexu  

> [!NOTE]
> **EF6.1 a vyšší pouze** – atribut indexu byla zavedena v Entity Framework 6.1. Pokud používáte starší verzi informace v této části se nevztahují.  

Vytváření indexů nativně nepodporuje rozhraní Fluent API, ale můžete provádět pomocí podpory pro **IndexAttribute** prostřednictvím rozhraní Fluent API. Atributy indexu jsou zpracovány včetně poznámky k modelu na model, který pak bude převedena na Index v databázi později v kanálu. Můžete ručně přidat tyto stejné poznámky pomocí rozhraní Fluent API.  

Nejjednodušší způsob, jak to provést, je vytvoření instance **IndexAttribute** , která obsahuje všechna nastavení pro nový index. Potom můžete vytvořit instanci **IndexAnnotation** což je EF konkrétní typ, který se převede **IndexAttribute** nastavení do poznámky k modelu, který může být uložená na EF modelu. Toto je pak možné předat do **HasColumnAnnotation** metodu na rozhraní Fluent API zadáním názvu **Index** pro poznámku.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Úplný seznam nastavení, které jsou k dispozici v **IndexAttribute**, najdete v článku *Index* část [anotací dat při prvním kód](~/ef6/modeling/code-first/data-annotations.md). To zahrnuje přizpůsobení názvu indexu, vytváření jedinečných indexů a vytváření indexů více sloupci.  

Je-li zadat více poznámek index podle jedné vlastnosti, předá pole **IndexAttribute** konstruktoru **IndexAnnotation**.  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Zadání není pro mapování vlastnosti CLR na sloupec v databázi  

Následující příklad ukazuje, jak určit, že vlastnost na typ CLR není mapováno na sloupec v databázi.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Mapování vlastnosti CLR v určitém sloupci v databázi  

Následující příklad namapuje vlastnost název CLR DepartmentName sloupce databáze.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Přejmenování cizí klíč, který není definován v modelu  

Pokud se rozhodnete definovat cizí klíč pro typ CLR, ale chcete k zadání názvu, jaký by měl mít v databázi, postupujte takto:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Konfiguruje, zda vlastnost řetězce podporuje kódování Unicode obsah  

Ve výchozím nastavení jsou řetězce Unicode (nvarchar v systému SQL Server). Metoda IsUnicode můžete použít k určení, zda řetězec by měl být typu varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Konfigurace datový typ sloupce databáze  

[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) metoda povolí mapování odlišné reprezentace stejného základního typu. Pomocí této metody neumožňuje k provádění jakýchkoli převodů dat za běhu. Upozorňujeme, že IsUnicode upřednostňovaný způsob nastavení sloupce varchar, protože je nezávislá na databázi.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Konfigurace vlastností komplexního typu.  

Existují dva způsoby, jak nakonfigurovat Skalární vlastnosti na komplexního typu.  

Na položku ComplexTypeConfiguration lze volat vlastnost.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Můžete také použít zápisu s tečkou pro přístup k vlastnosti komplexního typu.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Konfiguruje vlastnost, která má sloužit jako Token optimistického řízení souběžnosti  

Chcete-li určit, že vlastnosti v entitě představuje tokenem souběžnosti, můžete použít atribut atribut ConcurrencyCheck nebo metoda IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Metoda IsRowVersion také můžete nakonfigurovat vlastnosti, která má být verze řádku v databázi. Nastavení vlastnosti, která má být, že verze řádku automaticky nakonfiguruje ho, aby se token optimistického řízení souběžnosti.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Typ mapování  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Určení, že třída je komplexního typu.  

Podle konvence typ, který nemá žádný primární klíč zadán je považován za komplexní. Existují některé scénáře, kde Code First nerozpozná komplexní typ (například, pokud mají vlastnost zvanou ID, ale neznamená, aby se primární klíč). V takových případech můžete využít rozhraní fluent API explicitně určit, že typ je komplexní typ.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Zadání není pro mapování typu CLR Entity do tabulky v databázi  

Následující příklad ukazuje, jak vyloučit typ CLR z mapovaný do tabulky v databázi.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mapování typu Entity na určitou tabulku v databázi  

Všechny vlastnosti oddělení budou zmapována do sloupce v tabulce nazvané t_ oddělení.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Můžete také zadat název schématu takto:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Mapování dědičnosti za hierarchii tabulky (TPH)  

Ve scénáři TPH mapování všech typů v hierarchii dědičnosti se mapují na jednu tabulku. Sloupec diskriminátoru se používá k identifikaci typu každý řádek. Při vytváření modelu s Code First, TPH je výchozí strategie pro typy, které jsou součástí hierarchie dědičnosti. Ve výchozím nastavení sloupec diskriminátoru se přidá do tabulky s názvem "Diskriminátoru" a název typu CLR jednotlivých typů v hierarchii se používá pro hodnoty diskriminátoru. Výchozí chování lze upravit pomocí rozhraní API fluent.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Mapování dědičnosti za typ tabulky (TPT)  

Ve scénáři TPT mapování jsou všechny typy mapovány na jednotlivé tabulky. Vlastnosti, které patří výhradně pro základní typ nebo odvozeného typu jsou uložené v tabulce, která se mapuje na tento typ. Tabulky, které se mapují k odvozené typy také uložit cizí klíč, který připojí odvozenou tabulku s základní tabulky.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Mapování tabulek na konkrétní třídy (TPC) dědičnosti  

Ve scénáři testu TPC mapování všechny typy neabstraktní v hierarchii se mapují na jednotlivé tabulky. Tabulky, které se mapují na odvozené třídy nemají žádný vztah k tabulce, která se mapuje na základní třídu v databázi. Všechny vlastnosti třídy, včetně zděděné vlastnosti, jsou mapovány na sloupce příslušné tabulky.  

Volání metody MapInheritedProperties ke konfiguraci jednotlivých odvozeného typu. MapInheritedProperties změní všechny vlastnosti, které byly zděděny ze základní třídy na nové sloupce v tabulce pro odvozenou třídu.  

> [!NOTE]
> Všimněte si, že vzhledem k tomu, že tabulky účasti v hierarchii dědičnosti TPC nesdílejí primární klíč bude duplicitní entita klíče při vkládání do tabulek, které jsou mapovány na podtřídy, pokud máte hodnot v databázi vygeneruje se výchozí hodnota vlastnosti stejné identity. Pro vyřešení tohoto problému můžete zadat hodnotu různých počátečních pro každou tabulku nebo vypnout identitu na vlastnost primárního klíče. Identita je výchozí hodnota vlastnosti integer klíče při práci se službou Code First.  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Mapování vlastnosti typu Entity k několika tabulkám v databázi (rozdělení Entity)  

Entita rozdělení umožňuje vlastnosti typu entity k možné rozdělit do několika tabulek. V následujícím příkladu je entita oddělení rozdělit na dvě tabulky: oddělení a DepartmentDetails. Rozdělení entity pomocí více volání metody mapování mapuje podmnožinu vlastností do určité tabulky.  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Mapování více typů entit na jedné tabulky v databázi (rozdělení tabulky)  

Následující příklad namapuje dva typy entit, které sdílejí primární klíč do jedné tabulky.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Mapování typu Entity na uložené procedury Insert/Update/Delete (ef6 nebo novější)  

Počínaje EF6 můžete mapovat entity pro použití uložené procedury pro vložení aktualizace a odstraňování. Další podrobnosti najdete v tématu, [kód první Insert/Update/Delete uložené procedury](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
