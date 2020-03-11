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
# <a name="code-first-conventions"></a>Code First konvence
Code First umožňuje popsat model pomocí C# nebo Visual Basic třídy .NET. Základní tvar modelu se detekuje pomocí konvencí. Konvence jsou sady pravidel, které se používají k automatické konfiguraci koncepčního modelu na základě definic třídy při práci s Code First. Konvence jsou definovány v oboru názvů System. data. entity. ModelConfiguration. Conventions.  

Model můžete dále konfigurovat pomocí datových poznámek nebo rozhraní Fluent API. Přednost je dána konfiguraci prostřednictvím rozhraní Fluent API, po kterém následují datové poznámky a potom konvence. Další informace najdete v tématu [datové poznámky](~/ef6/modeling/code-first/data-annotations.md), [Fluent API-Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [typy rozhraní API Fluent & vlastnosti](~/ef6/modeling/code-first/fluent/types-and-properties.md) a [Fluent API pomocí VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Podrobný seznam konvencí Code First najdete v [dokumentaci k rozhraní API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Toto téma poskytuje přehled konvencí používaných nástrojem Code First.  

## <a name="type-discovery"></a>Zjišťování typů  

Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model. Kromě definování tříd musíte také dát **DbContext** informace o tom, které typy chcete do modelu zahrnout. Chcete-li to provést, Definujte třídu kontextu, která je odvozena z **DbContext** a zpřístupňuje vlastnosti **negenerickými** pro typy, které mají být součástí modelu. Code First budou tyto typy zahrnovat a budou také vyžádané v jakémkoli odkazovaném typu, a to i v případě, že jsou odkazované typy definovány v jiném sestavení.  

Pokud se vaše typy účastní hierarchie dědičnosti, je dostačující definovat vlastnost **negenerickými** pro základní třídu a odvozené typy budou automaticky zahrnuty, pokud jsou ve stejném sestavení jako základní třída.  

V následujícím příkladu je definována pouze jedna vlastnost **negenerickými** ve třídě **SchoolEntities** (**departments**). Code First tuto vlastnost používá ke zjišťování a vyžádání v jakémkoli odkazovaném typu.  

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

Pokud chcete vyloučit typ z modelu, použijte atribut **NotMapped** nebo **DbModelBuilder. Ignore** Fluent API.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Konvence primárního klíče  

Code First odvodí, že vlastnost je primární klíč, pokud je vlastnost u třídy pojmenována "ID" (nerozlišuje malá a velká písmena) nebo název třídy následovaný "ID". Pokud je typ vlastnosti primárního klíče číslo nebo GUID, bude nakonfigurovaný jako sloupec identity.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Konvence vztahů  

V Entity Framework navigační vlastnosti poskytují způsob, jak procházet relaci mezi dvěma typy entit. Každý objekt může mít navigační vlastnost pro každý vztah, ve kterém se účastní. Navigační vlastnosti umožňují procházet a spravovat relace v obou směrech, vracet buď Referenční objekt (Pokud je násobnost buď jedna, nebo nula, nebo-jedna), nebo kolekci (Pokud je násobnost mnoho). Code First odvodí vztahy na základě navigačních vlastností definovaných ve vašich typech.  

Kromě navigačních vlastností doporučujeme, abyste do typů, které reprezentují závislé objekty, zahrnuli vlastnosti cizího klíče. Jakákoli vlastnost se stejným datovým typem jako primární vlastnost primárního klíče a s názvem, který následuje jeden z následujících formátů, představuje cizí klíč pro relaci: '\<název vlastnosti navigace\>\<hlavní název vlastnosti primárního klíče\>', '\<název třídy zabezpečení\>\<primární klíč vlastnosti název\>', nebo '\<hlavní primární klíč vlastnosti\>'. Pokud je nalezeno více shod, bude přednost uvedena v pořadí uvedeném výše. Detekce cizího klíče nerozlišuje velká a malá písmena. Když je zjištěna vlastnost cizího klíče, Code First odvodí násobnost relace na základě hodnoty null cizího klíče. Pokud je vlastnost nastavena na hodnotu null, je relace registrována jako volitelná; v opačném případě bude relace registrována podle potřeby.  

Pokud cizímu klíči na závislé entitě není nabývat hodnoty null, Code First nastaví kaskádové odstranění v relaci. Pokud je cizí klíč na závislé entitě Nullable, Code First v relaci nenastavuje kaskádové odstranění a při odstranění objektu zabezpečení se cizí klíč nastaví na hodnotu null. Chování násobnosti a kaskádového odstranění zjištěné konvencí lze přepsat pomocí rozhraní Fluent API.  

V následujícím příkladu jsou k definování vztahu mezi třídami oddělení a kurzu použity navigační vlastnosti a cizí klíč.  

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
> Pokud máte více vztahů mezi stejnými typy (například Předpokládejme, že definujete třídy **Person** a **Book** , kde třída **Person** obsahuje navigační vlastnosti **ReviewedBooks** a **AuthoredBooks** a třída **Book** obsahuje navigační vlastnosti **autora** a **kontrolora** ), je nutné ručně nakonfigurovat relace pomocí datových poznámek nebo rozhraní Fluent API. Další informace najdete v tématech [datové poznámky – vztahy](~/ef6/modeling/code-first/data-annotations.md) a [Fluent API-Relationships](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Konvence komplexních typů  

Když Code First zjistí definici třídy, kde nelze odvodit primární klíč, a žádný primární klíč není zaregistrován prostřednictvím datových poznámek nebo rozhraní API Fluent, pak je typ automaticky registrován jako složitý typ. Detekce komplexního typu také vyžaduje, aby typ neměl vlastnosti, které odkazují na typy entit a neodkazuje se na vlastnost kolekce na jiném typu. S ohledem na následující definice tříd Code First by byly odvodit, že **Podrobnosti** jsou komplexní typ, protože nemá žádný primární klíč.  

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

## <a name="connection-string-convention"></a>Konvence připojovacího řetězce  

Další informace o konvencích, které DbContext používá ke zjištění připojení pro použití, najdete v tématu [připojení a modely](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Odebírají se konvence.  

Můžete odebrat libovolné konvence definované v oboru názvů System. data. entity. ModelConfiguration. Conventions. Následující příklad odebere **PluralizingTableNameConvention**.  

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

## <a name="custom-conventions"></a>Vlastní konvence  

Vlastní konvence jsou podporované v EF6 a vyšší. Další informace najdete v tématu [Vlastní konvence Code First](~/ef6/modeling/code-first/conventions/custom.md).
