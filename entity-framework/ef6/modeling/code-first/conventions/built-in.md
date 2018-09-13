---
title: První vytváření – EF6 kódu
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490997"
---
# <a name="code-first-conventions"></a>První konvence kódu
Code First umožňuje popisuje model s použitím tříd jazyka C# nebo Visual Basic .NET. Základní obrazec modelu je zjišťován pomocí konvencí. Konvence jsou sady pravidel, která se používají pro automatickou konfiguraci konceptuálního modelu podle definice tříd při práci se službou Code First. Konvence jsou definovány v oboru názvů System.Data.Entity.ModelConfiguration.Conventions.  

Model lze dále konfigurovat pomocí anotacemi dat nebo rozhraní fluent API. Prioritu konfiguraci prostřednictvím rozhraní fluent API následovat anotacemi dat a vytváření názvů. Další informace najdete v části [anotacemi dat](~/ef6/modeling/code-first/data-annotations.md), [rozhraní Fluent API – vztahy](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API – typy a vlastnosti](~/ef6/modeling/code-first/fluent/types-and-properties.md) a [Fluent API s využitím VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Podrobný seznam Code First konvence je k dispozici v [dokumentace k rozhraní API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Toto téma obsahuje základní informace o konvencích použitých ji Code First.  

## <a name="type-discovery"></a>Typ zjišťování  

Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény). Kromě definování třídy, je také potřeba nechat **DbContext** vědět, jaké typy, které chcete zahrnout do modelu. K tomuto účelu můžete definovat, která je odvozena z třídy kontextu **DbContext** a zveřejňuje **DbSet** vlastnosti pro typy, které mají být součástí modelu. Kód nejprve bude obsahovat tyto typy a také se o přijetí změn v odkazovaných typů, i v případě, že odkazované typy jsou definovány v jiném sestavení.  

Pokud vaše typy účastnit v hierarchii dědičnosti, stačí definovat **DbSet** vlastnost pro základní třídu a odvozené typy budou přidány automaticky, pokud jsou ve stejném sestavení jako základní třídy.  

V následujícím příkladu je zaznamenána pouze jedna **DbSet** vlastnosti definované v **SchoolEntities** třídy (**oddělení**). Tato vlastnost kód nejprve používá ke zjišťování a získává všechny odkazované typy.  

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

Pokud chcete vyloučit z modelu typ, použijte **NotMapped** atribut nebo **DbModelBuilder.Ignore** rozhraní fluent API.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Primární klíče konvence  

Kód nejprve odvodí, že vlastnost je primární klíč, pokud je vlastnost třídy s názvem "ID" (nerozlišuje velikost písmen) nebo název třídy následovaný "ID". Pokud je číselného typu vlastnost primárního klíče nebo identifikátor GUID bude nakonfigurován jako sloupec identity.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Vztah konvence  

Navigační vlastnosti v Entity Framework, poskytují způsob, jak procházet vztah mezi dvěma typy entit. Každý objekt může mít navigační vlastnost pro každý vztah, ve kterém se podílí. Navigační vlastnosti umožňují přejít a spravovat vztahy v obou směrech, vrací buď odkaz na objekt (Pokud je násobnost buď jeden nebo nula nebo jedna) nebo celé kolekci (Pokud je násobnost mnoho). Kód nejprve odvodí relace založené na navigační vlastnosti definované ve vašich typů.  

Kromě vlastnosti navigace doporučujeme vám, že složku zahrnujete vlastnosti cizího klíče na typy, které představují závislé objekty. Cizí klíč pro daný vztah představuje jakoukoli vlastnost se stejným typem dat jako hlavní vlastnost primárního klíče a s názvem, který následuje jedna z následujících formátů: "\<název navigační vlastnosti\>\<instančního objektu Název vlastnost primárního klíče\>","\<název instančního objektu třídy\>\<vlastnost primárního klíče název\>", nebo"\<název instančního objektu vlastnost primárního klíče\>". Pokud se nenajde více shod prioritu v uvedeném pořadí. Detekce cizích klíčů se nerozlišují malá a velká písmena. Pokud vlastnost cizího klíče se zjistí, Code First odvodí násobnost relací podle možnosti použití hodnoty Null cizího klíče. Pokud je vlastnost s možnou hodnotou Null vztah je registrované jako volitelné. předávala relace podle potřeby.  

Pokud není cizí klíč na závislé entity s možnou hodnotou Null, pak Code First nastaví kaskádové odstranění relace. Pokud cizí klíč na závislé entita může mít hodnotu Null, Code First nenastaví kaskádové odstranění na relaci a při odstranění objektu zabezpečení cizí klíč se nastaví na hodnotu null. Násobnost a kaskádové odstranění chování detekoval konvence lze přepsat pomocí rozhraní fluent API.  

V následujícím příkladu navigačních vlastností a cizí klíče slouží k definování vztahů mezi třídami oddělení a kurzu.  

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
> Pokud máte více vztahů mezi stejné typy (Předpokládejme například, že definujete **osoba** a **knihy** třídy, kde **osoba** třída obsahuje  **ReviewedBooks** a **AuthoredBooks** navigačních vlastností a **knihy** třída obsahuje **Autor** a  **Kontrolor** navigační vlastnosti) budete muset ručně konfigurovat vztahy pomocí anotacemi dat nebo rozhraní fluent API. Další informace najdete v tématu [anotacemi dat – vztahy](~/ef6/modeling/code-first/data-annotations.md) a [rozhraní Fluent API – vztahy](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Konvence komplexní typy  

Při Code First umožňuje zjistit primární klíč nelze odvodit, kde žádný primární klíč je zaregistrované prostřednictvím anotací dat nebo rozhraní fluent API definice třídy, pak typ automaticky zaregistrována jako komplexního typu. Komplexní typ detekce také vyžaduje, typ nebude mít vlastnosti, které odkazují na typy entit a neodkazuje na jiný typ vlastnost kolekce. Daný následující definice tříd Code First by odvodit, který **podrobnosti** je komplexní typ, protože nemá primární klíč.  

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

## <a name="connection-string-convention"></a>Připojovací řetězec konvence  

Další informace o konvence, DbContext používá ke zjišťování připojení pro použití podívejte [připojení a modely](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Odebrání konvence  

Můžete odebrat všechny konvence definovaný v oboru názvů System.Data.Entity.ModelConfiguration.Conventions. Následující příklad odebere **PluralizingTableNameConvention**.  

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

Vlastní konvence jsou podporovány v EF6 a vyšší. Další informace najdete v části [první konvence kódu vlastní](~/ef6/modeling/code-first/conventions/custom.md).
