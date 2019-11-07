---
title: Relace, navigační vlastnosti a cizí klíče – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: cc7160f2d0ab7ac0c6009f820441c88590cacfaf
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655866"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Relace, navigační vlastnosti a cizí klíče
Toto téma poskytuje přehled o tom, jak Entity Framework spravuje vztahy mezi entitami. Poskytuje také pokyny k tomu, jak namapovat a manipulovat s relacemi.

## <a name="relationships-in-ef"></a>Vztahy v EF

V relačních databázích jsou relace (označované také jako asociace) mezi tabulkami definovány pomocí cizích klíčů. Cizí klíč (FK) je sloupec nebo kombinace sloupců, které se používají k vytvoření a vykonání propojení mezi daty ve dvou tabulkách. K dispozici jsou všeobecně tři typy vztahů: 1:1, 1: n a m:n. V relaci 1: n je cizí klíč definován v tabulce, která představuje mnoho konců relace. Relace m:n zahrnuje definování třetí tabulky (označované jako spojení nebo spojovací tabulka), jejíž primární klíč se skládá z cizích klíčů z obou souvisejících tabulek. V relaci 1:1 se primární klíč chová navíc jako cizí klíč a neexistuje žádný samostatný sloupec cizího klíče pro žádnou tabulku.

Následující obrázek znázorňuje dvě tabulky, které se účastní relace 1: n. Tabulka **kurzů** je závislá na tabulce, protože obsahuje sloupec **DepartmentID** , který ho propojuje s tabulkou **oddělení** .

![Tabulky oddělení a kurzu](~/ef6/media/database2.png)

V Entity Framework entita může souviset s jinými entitami prostřednictvím přidružení nebo vztahu. Každá relace obsahuje dva elementy end, které popisují typ entity a násobnost typu (jedna, nulová nebo jedna nebo mnoho) pro dvě entity v dané relaci. Vztah se může řídit referenčním omezením, které popisuje, který objekt end v relaci je role zabezpečení a která je závislá na roli.

Navigační vlastnosti poskytují způsob, jak procházet přidružení mezi dvěma typy entit. Každý objekt může mít navigační vlastnost pro každý vztah, ve kterém se účastní. Navigační vlastnosti umožňují procházet a spravovat relace v obou směrech, vracet buď Referenční objekt (Pokud je násobnost buď jedna, nebo nula, nebo-jedna), nebo kolekci (Pokud je násobnost mnoho). Můžete se také rozhodnout, že máte jednosměrnou navigaci, v takovém případě můžete definovat vlastnost navigace pouze v jednom z typů, které jsou součástí relace, a nikoli v obou.

Doporučuje se zahrnout vlastnosti v modelu, který se mapuje na cizí klíče v databázi. S využitím vlastností cizího klíče můžete vytvořit nebo změnit relaci úpravou hodnoty cizího klíče u závislého objektu. Tento druh asociace se nazývá přidružení cizího klíče. Použití cizích klíčů je ještě důležitější při práci s odpojenými entitami. Pamatujte na to, že při práci s 1-to-1 nebo 1-to-0.. 1 relace neexistují žádné samostatné sloupce cizího klíče, vlastnost Primary key funguje jako cizí klíč a je vždy zahrnutá v modelu.

Pokud se sloupce cizího klíče v modelu nezahrnují, jsou informace o přidružení spravované jako nezávislé objekty. Relace jsou sledovány prostřednictvím odkazů na objekty namísto vlastností cizích klíčů. Tento typ přidružení se nazývá *nezávislé přidružení*. Nejběžnější způsob, jak upravit *nezávislé přidružení* , je upravit vlastnosti navigace, které jsou generovány pro každou entitu, která se účastní přidružení.

V modelu se můžete rozhodnout použít jeden nebo oba typy přidružení. Pokud máte ale k dispozici čistě relaci m:n, která je propojená tabulkou JOIN obsahující pouze cizí klíče, EF použije nezávislé přidružení ke správě takového vztahu m:n.   

Následující obrázek znázorňuje koncepční model, který byl vytvořen pomocí Entity Framework Designer. Model obsahuje dvě entity, které se účastní relace 1: n. Obě entity mají navigační vlastnosti. **Kurz** je závislá entita a má definovanou vlastnost cizího klíče **DepartmentID** .

![Tabulky oddělení a kurzu s navigačními vlastnostmi](~/ef6/media/relationshipefdesigner.png)

Následující fragment kódu ukazuje stejný model, který byl vytvořen pomocí Code First.

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>Konfigurace nebo mapování vztahů

Zbývající část této stránky popisuje, jak získat přístup k datům a manipulovat s nimi pomocí relací. Informace o nastavení vztahů v modelu najdete na následujících stránkách.

-   Pokud chcete nakonfigurovat vztahy v Code First, přečtěte si téma [datové poznámky](~/ef6/modeling/code-first/data-annotations.md) a [Fluent API – vztahy](~/ef6/modeling/code-first/fluent/relationships.md).
-   Chcete-li nakonfigurovat relace pomocí Entity Framework Designer, přečtěte si téma [relace s návrhářem EF](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Vytváření a úpravy relací

Při *přidružení cizího klíče*při změně vztahu se stav závislého objektu se stavem `EntityState.Unchanged` změní na `EntityState.Modified`. V nezávislém vztahu změna vztahu neaktualizuje stav závislého objektu.

Následující příklady ukazují, jak použít vlastnosti cizího klíče a navigační vlastnosti k přidružení souvisejících objektů. Pomocí přidružení cizího klíče můžete pomocí obou metod změnit, vytvořit nebo upravit relace. U nezávislých přidružení nelze použít vlastnost cizího klíče.

- Přiřazením nové hodnoty k vlastnosti cizího klíče, jak je uvedeno v následujícím příkladu.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- Následující kód odstraní relaci nastavením cizího klíče na **hodnotu null**. Všimněte si, že vlastnost cizího klíče musí mít hodnotu null.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > Pokud je odkaz v přidaném stavu (v tomto příkladu je to objekt kurzu), navigační vlastnost odkazu nebude synchronizována s hodnotami klíče nového objektu, dokud není voláno SaveChanges. K synchronizaci nedochází, protože kontext objektu neobsahuje trvalé klíče pro přidané objekty, dokud nebudou uloženy. Pokud je nutné, aby byly nové objekty plně synchronizovány, jakmile nastavíte relaci, použijte jednu z následujících metod. *

- Přiřazením nového objektu navigační vlastnosti. Následující kód vytvoří relaci mezi kurzem a `department`. Pokud jsou objekty připojeny k tomuto kontextu, `course` je také přidána do kolekce `department.Courses` a odpovídající vlastnost cizího klíče objektu `course` je nastavena na hodnotu klíčové vlastnosti oddělení.  
  ``` csharp
  course.Department = department;
  ```

- Pokud chcete relaci odstranit, nastavte vlastnost navigace na `null`. Pokud pracujete se Entity Framework, který je založen na rozhraní .NET 4,0, je nutné před nastavením na hodnotu null načíst související konec. Příklad:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  Počínaje Entity Framework 5,0, která je založena na rozhraní .NET 4,5, můžete nastavit relaci na hodnotu null bez načtení souvisejícího konce. Pomocí následující metody můžete také nastavit aktuální hodnotu null.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- Odstraněním nebo přidáním objektu v kolekci entit. Například můžete přidat objekt typu `Course` do kolekce `department.Courses`. Tato operace vytvoří vztah mezi konkrétním **kurzem** a konkrétním `department`. Pokud jsou objekty připojeny k kontextu, odkaz na oddělení a vlastnost cizího klíče v objektu **Course** budou nastaveny na příslušné `department`.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- Pomocí metody `ChangeRelationshipState` ke změně stavu zadaného vztahu mezi dvěma objekty entity. Tato metoda se nejčastěji používá při práci s N-vrstvými aplikacemi a *nezávislou asociací* (nedá se použít s přidružením cizího klíče). Chcete-li použít tuto metodu, musíte také vyřadit do `ObjectContext`, jak je znázorněno v následujícím příkladu.  
V následujícím příkladu existuje vztah n:n mezi instruktory a kurzy. Volání metody `ChangeRelationshipState` a předání parametru `EntityState.Added`, umožňuje `SchoolContext`, že mezi dvěma objekty bylo přidáno relaci:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  Všimněte si, že pokud aktualizujete (nestačí jenom přidávat) relaci, musíte po přidání nového vztahu odstranit původní relaci:

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Synchronizace změn mezi cizími klíči a navigačními vlastnostmi

Když změníte vztah objektů připojených k kontextu pomocí jedné z výše popsaných metod, Entity Framework musí udržovat cizí klíče, odkazy a kolekce synchronizované. Entity Framework automaticky spravuje tuto synchronizaci (označovanou také jako oprava relace) pro entity POCO s proxy servery. Další informace najdete v tématu [práce se servery proxy](~/ef6/fundamentals/proxies.md).

Pokud používáte entity POCO bez proxy, je nutné zajistit, aby byla volána metoda **DetectChanges** pro synchronizaci souvisejících objektů v kontextu. Všimněte si, že následující rozhraní API automaticky aktivují volání **DetectChanges** .

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   Provádění dotazu LINQ na `DbSet`

## <a name="loading-related-objects"></a>Načítání souvisejících objektů

V Entity Framework běžně používáte navigační vlastnosti k načtení entit, které souvisejí s vrácenou entitou podle definovaného přidružení. Další informace najdete v tématu [načítání souvisejících objektů](~/ef6/querying/related-data.md).

> [!NOTE]
> Při přidružení cizího klíče při načtení souvisejícího konce závislého objektu se načte související objekt na základě hodnoty cizího klíče závislé na, který je aktuálně v paměti:

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

V nezávislém přidružení je související konec závislého objektu dotazován na základě hodnoty cizího klíče, který je aktuálně v databázi. Pokud se však změnil vztah a vlastnost reference na závislém objektu odkazuje na jiný objekt zabezpečení, který je načten v kontextu objektu, Entity Framework se pokusí vytvořit relaci, která je definována v klientovi.

## <a name="managing-concurrency"></a>Správa souběžnosti

V cizím i nezávislém přidružení jsou kontroly souběžnosti založeny na klíčích entit a dalších vlastnostech entit, které jsou definovány v modelu. Při použití návrháře EF k vytvoření modelu nastavte atribut `ConcurrencyMode` na **pevnou** , aby určoval, že by měla být vlastnost kontrolována pro souběžnost. Při použití Code First k definování modelu použijte anotaci `ConcurrencyCheck` u vlastností, u kterých chcete zkontrolovat souběžnost. Při práci s Code First lze také pomocí anotace `TimeStamp` určit, zda má být vlastnost pro souběžnost kontrolována. V dané třídě může být pouze jedna vlastnost časového razítka. Code First mapuje tuto vlastnost na pole bez hodnoty null v databázi.

Při práci s entitami, které se účastní kontroly souběžnosti a řešení, doporučujeme vždy používat přidružení cizího klíče.

Další informace najdete v tématu [zpracování konfliktů souběžnosti](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Práce s překrývajícími se klíči

Překrývající se klíče jsou složené klíče, kde některé vlastnosti v klíči jsou také součástí jiného klíče v entitě. Nemůžete mít překrývající se klíč v nezávislém přidružení. Chcete-li změnit přidružení cizího klíče, které obsahuje překrývající se klíče, doporučujeme místo použití odkazů na objekty upravovat hodnoty cizích klíčů.
