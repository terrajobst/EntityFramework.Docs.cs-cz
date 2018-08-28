---
title: Relace, navigačních vlastností a cizí klíče - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: c1d48f18a7dd25a6a48537f0de5379f861bf447a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997998"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Relace, navigačních vlastností a cizí klíče
Toto téma obsahuje základní informace o tom, jak Entity Framework spravuje vztahy mezi entitami. Poskytuje pokyny o tom, jak mapovat a manipulaci s relací.

## <a name="relationships-in-ef"></a>Vztahy v EF

V relačních databázích jsou definovány relace (také nazývané přidružení) mezi tabulkami prostřednictvím cizí klíče. Cizí klíč (Cizíklíč) je sloupec nebo kombinace sloupců, který se používá k zahájení a vynucují odkaz mezi daty ve dvou tabulkách. Existují obecně tři typy vztahů: 1: 1, 1 n a many-to-many. Ve vztahu jednoho k několika je cizí klíč definovaný pro tabulku, která představuje řadu konci vztahu. Vztah mnoho mnoho zahrnuje definování třetí tabulka (označují se jako tabulku spojení nebo join), jejichž primární klíč se skládá z cizích klíčů z obou související tabulky. V relaci primární klíč slouží také jako cizí klíč a neexistuje žádný samostatný sloupec cizího klíče pro obě tabulky.

Následující obrázek ukazuje dvě tabulky, které se účastní v vztah jeden mnoho. **Kurzu** tabulky je závislé tabulky, protože obsahuje **DepartmentID** sloupec, který odkazuje na **oddělení** tabulky.

![Databáze 2](~/ef6/media/database2.png)

V Entity Framework entity souviset s jinými entitami, prostřednictvím přidružení nebo vztah. Každý vztah obsahuje dva elementy, které popisují typ entity a násobnosti typu (jedna, nula nebo jedna nebo řada) pro dvě entity, které v této relaci. Relace můžou řídit referenční omezení, která popisuje, jaké end v relaci je hlavní role a který je závislé role.

Vlastnosti navigace poskytují způsob, jak procházet přidružení mezi dvěma typy entit. Každý objekt může mít navigační vlastnost pro každý vztah, ve kterém se podílí. Navigační vlastnosti umožňují přejít a spravovat vztahy v obou směrech, vrací buď odkaz na objekt (Pokud je násobnost buď jeden nebo nula nebo jedna) nebo celé kolekci (Pokud je násobnost mnoho). Také můžete mít jednosměrné navigace, v takovém případě můžete definovat vlastnost navigace na pouze jeden z typů, které se účastní vztahu a ne na obě.

Doporučuje se pro vložení vlastností do modelu, které mapují na cizí klíče v databázi. Pomocí vlastnosti cizího klíče zahrnuty můžete vytvořit nebo změnit úpravou hodnoty cizího klíče na závislý objekt relace. Tento druh přidružení se nazývá přidružení cizího klíče. Použití cizího klíče je ještě více základní, pokud pracujete s odpojené entity. Poznámka:, že pracujete s 1: 1 nebo 1 na 0.. vztahy 1, neexistuje žádný samostatný sloupec cizího klíče, vlastnost primárního klíče funguje jako cizí klíč a je vždy součástí modelu.

Pokud sloupce cizího klíče nejsou součástí modelu, informací o přidružení je spravovat jako nezávislý objekt. Relace jsou sledovány pomocí odkazů na objekty místo vlastnosti cizího klíče. Tento typ přidružení se nazývá *nezávislé přidružení*. Nejběžnější způsob, jak upravit *nezávislé přidružení* je upravit navigační vlastnosti, které jsou generovány pro každou entitu, která se účastní asociace.

Můžete použít jeden nebo oba typy přidružení v modelu. Ale pokud budete mít čistě many-to-many vztah, který je připojen pomocí připojení k tabulce, která obsahuje pouze cizí klíče, použije EF nezávislé přidružení ke správě takové vztah mnoho mnoho.   

Následující obrázek ukazuje koncepční model, který byl vytvořen s Entity Framework Designer. Model obsahuje dvě entity, které jsou součástí vztah jeden mnoho. Oba entity mají navigační vlastnosti. **Kurz** depend entitou a má **DepartmentID** cizí definovanou klíčovou vlastnost.

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

Následující fragment kódu ukazuje stejný model, který byl vytvořen s Code First.

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
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>Konfigurace nebo mapování relací

Zbytek této stránce se zaměřuje na přístup k a manipulaci s daty pomocí relací. Informace o nastavení relace v modelu získáte na následujících stránkách.

-   Konfiguraci relace v Code First najdete v tématu [anotacemi dat](~/ef6/modeling/code-first/data-annotations.md) a [rozhraní Fluent API – vztahy](~/ef6/modeling/code-first/fluent/relationships.md).
-   Konfigurace relace pomocí Entity Framework Designer, naleznete v tématu [vztahy s EF designeru](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Vytvoření a úprava relací

V *přidružení cizího klíče*, když změníte relace stavu pomocí EntityState.Unchanged stav se změní na EntityState.Modified závislý objekt. Ve vztahu k nezávislé změny vztahů neaktualizuje stav objektu závislý.

Následující příklady ukazují, jak pomocí vlastnosti cizího klíče a navigačních vlastností můžete přidružit souvisejících objektů. Přidružení cizího klíče můžete pomocí některé z metod můžete změnit, vytvořit nebo upravit relace. S nezávislé přidružení nelze použít vlastnost cizího klíče.

-   Přiřazením novou hodnotu pro vlastnost cizího klíče, jako v následujícím příkladu.  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   Následující kód odebere relace nastavením cizího klíče na **null**. Všimněte si, že vlastnost cizího klíče musí být s možnou hodnotou Null.  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > Pokud odkaz je ve stavu přidáno (v tomto příkladu kurzu objektu), odkaz na vlastnost navigace se nebudou synchronizovat s klíčovými hodnotami nový objekt dokud je volána metoda SaveChanges. Synchronizace nebude fungovat, protože kontext objektu neobsahuje trvalé klíče pro přidání objektů, dokud se uloží. Pokud potřebujete nové objekty, které jsou plně synchronizována v co nejdříve nastavit relace, použijte jednu z následujících methods.*

-   Po přiřazení nového objektu pro navigační vlastnost. Následující kód vytvoří vztah mezi kurz a `department`. Pokud objekty jsou připojené ke kontextu, `course` je taky přidaný ke `department.Courses` kolekce a odpovídající cizího klíče na vlastnost `course` objektu je nastavena na hodnotu klíče oddělení.  
    ``` csharp
    course.Department = department;
    ```

 -   Pokud chcete odstranit vztah, nastavte vlastnost navigace na `null`. Pokud pracujete se sadou Entity Framework, která je založena na rozhraní .NET 4.0, element end musí být načteny, než se nastaví na hodnotu null. Příklad:  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    Od verze Entity Framework 5.0, založené na rozhraní .NET 4.5, můžete nastavit vztah na hodnotu null bez načtení element end. Můžete také nastavit aktuální hodnotu na hodnotu null, následujícím způsobem.  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   Odstraněním nebo přidávání objektů v kolekci entit. Například můžete přidat objekt typu `Course` k `department.Courses` kolekce. Tato operace vytvoří vztah mezi konkrétní **kurzu** a konkrétní `department`. Pokud objekty jsou připojené ke kontextu, odkaz na oddělení a vlastnost cizího klíče na **kurzu** objektu se nastaví na příslušné `department`.  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- S použitím `ChangeRelationshipState` metodu, která změní stav zadaného vztah mezi dva objekty entity. Tato metoda se nejčastěji používá při práci s N-vrstvé aplikace a *nezávislé přidružení* (jej nelze použít s přidružení cizího klíče). Také, aby používali tuto metodu je třeba vyřadit dolů na `ObjectContext`, jak je znázorněno v následujícím příkladu.  
V následujícím příkladu je many-to-many vztah mezi Instruktoři a kurzy. Volání `ChangeRelationshipState` metoda a předávání `EntityState.Added` parametr, umožňuje `SchoolContext` vědět, že byla přidána vztahu mezi dvěma objekty:

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Synchronizace změn mezi cizí klíče a vlastnosti navigace

Při změně vztah mezi objekty pomocí jedné z metod popsaných výše připojit ke kontextu Entity Framework je potřeba udržovat synchronizované cizí klíče, odkazy a kolekce. Entity Framework automaticky spravuje této synchronizace (označované také jako relace opravit) pro POCO entity s proxy servery. Další informace najdete v tématu [práce s proxy](~/ef6/fundamentals/proxies.md).

Pokud používáte entity POCO bez proxy servery, je třeba Ujistěte se, že **metoda DetectChanges** metoda je volána k synchronizaci souvisejících objektů, které v kontextu. Všimněte si, který automaticky aktivuje následující rozhraní API **metoda DetectChanges** volání.

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
-   Provádění LINQ dotazovat `DbSet`

## <a name="loading-related-objects"></a>Načítají se související objekty

V Entity Framework, které nejčastěji používáte pro načtení entity, které se vztahují k vrácenou entitu definovanou v přidružení použijte navigační vlastnosti. Další informace najdete v tématu [načítání související objekty](~/ef6/querying/related-data.md).

> [!NOTE]
> V přidružení cizího klíče při načtení související konec závislý objekt objekt v relaci se načtou podle hodnoty cizího klíče, který je aktuálně v paměti závislé:

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

V nezávislých přidružení je dotazován související konec závislý objekt podle hodnoty cizího klíče, který je aktuálně v databázi. Nicméně pokud relace byla změněna a odkaz na vlastnost závislý objekt odkazuje na jiný objekt, který je načten v kontextu objektu, Entity Framework se pokusíme vytvořit relaci jako je definován na straně klienta.

## <a name="managing-concurrency"></a>Správa souběžnosti

V cizím klíči a nezávislé přidružení kontrolách souběžnosti se na základě klíče entit a jiné vlastnosti entity, které jsou definovány v modelu. Při použití k vytvoření modelu EF designeru, nastavte `ConcurrencyMode` atribut **oprava** k určení, že vlastnost by měla sloužit k souběžnosti. Při použití Code First definovat model, použijte `ConcurrencyCheck` Poznámka u vlastnosti, které mají být vráceny pro souběžnost. Při práci se službou Code First také můžete použít `TimeStamp` poznámky k určení, že vlastnost by měla sloužit k souběžnosti. Může mít pouze jednu vlastnost časového razítka v dané třídě. Tato vlastnost mapy kódu nejprve neumožňující hodnotu pole v databázi.

Doporučujeme vám, že vždy používáte přidružení cizího klíče při práci s entitami, které jsou součástí Kontrola souběžnosti a řešení.

Další informace najdete v tématu [zpracování konfliktů souběžnosti](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Práce s překrývajícími se klíči

Překrývající se klíče jsou složené klíče, kde některé vlastnosti v klíči jsou taky součástí jiný klíč v dané entitě. Překrývající se klíč nemůže mít v nezávislých přidružení. Chcete-li změnit přidružení cizího klíče, který obsahuje překrývajícími se klíči, doporučujeme proto upravovat hodnoty cizího klíče místo použití odkazy na objekty.
