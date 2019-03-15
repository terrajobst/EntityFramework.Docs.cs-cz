---
title: Relace, navigačních vlastností a cizí klíče - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 416eb1fb590330ba292a858347e26b83dddc74df
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829197"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="0fd95-102">Relace, navigačních vlastností a cizí klíče</span><span class="sxs-lookup"><span data-stu-id="0fd95-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="0fd95-103">Toto téma obsahuje základní informace o tom, jak Entity Framework spravuje vztahy mezi entitami.</span><span class="sxs-lookup"><span data-stu-id="0fd95-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="0fd95-104">Poskytuje pokyny o tom, jak mapovat a manipulaci s relací.</span><span class="sxs-lookup"><span data-stu-id="0fd95-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="0fd95-105">Vztahy v EF</span><span class="sxs-lookup"><span data-stu-id="0fd95-105">Relationships in EF</span></span>

<span data-ttu-id="0fd95-106">V relačních databázích jsou definovány relace (také nazývané přidružení) mezi tabulkami prostřednictvím cizí klíče.</span><span class="sxs-lookup"><span data-stu-id="0fd95-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="0fd95-107">Cizí klíč (Cizíklíč) je sloupec nebo kombinace sloupců, který se používá k zahájení a vynucují odkaz mezi daty ve dvou tabulkách.</span><span class="sxs-lookup"><span data-stu-id="0fd95-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="0fd95-108">Existují obecně tři typy vztahů: 1: 1, 1 n a many-to-many.</span><span class="sxs-lookup"><span data-stu-id="0fd95-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="0fd95-109">Ve vztahu jednoho k několika je cizí klíč definovaný pro tabulku, která představuje řadu konci vztahu.</span><span class="sxs-lookup"><span data-stu-id="0fd95-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="0fd95-110">Vztah mnoho mnoho zahrnuje definování třetí tabulka (označují se jako tabulku spojení nebo join), jejichž primární klíč se skládá z cizích klíčů z obou související tabulky.</span><span class="sxs-lookup"><span data-stu-id="0fd95-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="0fd95-111">V relaci primární klíč slouží také jako cizí klíč a neexistuje žádný samostatný sloupec cizího klíče pro obě tabulky.</span><span class="sxs-lookup"><span data-stu-id="0fd95-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="0fd95-112">Následující obrázek ukazuje dvě tabulky, které se účastní v vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="0fd95-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="0fd95-113">**Kurzu** tabulky je závislé tabulky, protože obsahuje **DepartmentID** sloupec, který odkazuje na **oddělení** tabulky.</span><span class="sxs-lookup"><span data-stu-id="0fd95-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Oddělení a kurzu tabulky](~/ef6/media/database2.png)

<span data-ttu-id="0fd95-115">V Entity Framework entity souviset s jinými entitami, prostřednictvím přidružení nebo vztah.</span><span class="sxs-lookup"><span data-stu-id="0fd95-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="0fd95-116">Každý vztah obsahuje dva elementy, které popisují typ entity a násobnosti typu (jedna, nula nebo jedna nebo řada) pro dvě entity, které v této relaci.</span><span class="sxs-lookup"><span data-stu-id="0fd95-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="0fd95-117">Relace můžou řídit referenční omezení, která popisuje, jaké end v relaci je hlavní role a který je závislé role.</span><span class="sxs-lookup"><span data-stu-id="0fd95-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="0fd95-118">Vlastnosti navigace poskytují způsob, jak procházet přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="0fd95-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="0fd95-119">Každý objekt může mít navigační vlastnost pro každý vztah, ve kterém se podílí.</span><span class="sxs-lookup"><span data-stu-id="0fd95-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="0fd95-120">Navigační vlastnosti umožňují přejít a spravovat vztahy v obou směrech, vrací buď odkaz na objekt (Pokud je násobnost buď jeden nebo nula nebo jedna) nebo celé kolekci (Pokud je násobnost mnoho).</span><span class="sxs-lookup"><span data-stu-id="0fd95-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="0fd95-121">Také můžete mít jednosměrné navigace, v takovém případě můžete definovat vlastnost navigace na pouze jeden z typů, které se účastní vztahu a ne na obě.</span><span class="sxs-lookup"><span data-stu-id="0fd95-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="0fd95-122">Doporučuje se pro vložení vlastností do modelu, které mapují na cizí klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="0fd95-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="0fd95-123">Pomocí vlastnosti cizího klíče zahrnuty můžete vytvořit nebo změnit úpravou hodnoty cizího klíče na závislý objekt relace.</span><span class="sxs-lookup"><span data-stu-id="0fd95-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="0fd95-124">Tento druh přidružení se nazývá přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="0fd95-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="0fd95-125">Použití cizího klíče je ještě více základní, pokud pracujete s odpojené entity.</span><span class="sxs-lookup"><span data-stu-id="0fd95-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="0fd95-126">Poznámka:, že pracujete s 1: 1 nebo 1 na 0.. vztahy 1, neexistuje žádný samostatný sloupec cizího klíče, vlastnost primárního klíče funguje jako cizí klíč a je vždy součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="0fd95-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="0fd95-127">Pokud sloupce cizího klíče nejsou součástí modelu, informací o přidružení je spravovat jako nezávislý objekt.</span><span class="sxs-lookup"><span data-stu-id="0fd95-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="0fd95-128">Relace jsou sledovány pomocí odkazů na objekty místo vlastnosti cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="0fd95-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="0fd95-129">Tento typ přidružení se nazývá *nezávislé přidružení*.</span><span class="sxs-lookup"><span data-stu-id="0fd95-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="0fd95-130">Nejběžnější způsob, jak upravit *nezávislé přidružení* je upravit navigační vlastnosti, které jsou generovány pro každou entitu, která se účastní asociace.</span><span class="sxs-lookup"><span data-stu-id="0fd95-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="0fd95-131">Můžete použít jeden nebo oba typy přidružení v modelu.</span><span class="sxs-lookup"><span data-stu-id="0fd95-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="0fd95-132">Ale pokud budete mít čistě many-to-many vztah, který je připojen pomocí připojení k tabulce, která obsahuje pouze cizí klíče, použije EF nezávislé přidružení ke správě takové vztah mnoho mnoho.</span><span class="sxs-lookup"><span data-stu-id="0fd95-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="0fd95-133">Následující obrázek ukazuje koncepční model, který byl vytvořen s Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="0fd95-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="0fd95-134">Model obsahuje dvě entity, které jsou součástí vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="0fd95-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="0fd95-135">Oba entity mají navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0fd95-135">Both entities have navigation properties.</span></span> <span data-ttu-id="0fd95-136">**Kurz** depend entitou a má **DepartmentID** cizí definovanou klíčovou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0fd95-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![Oddělení a kurzu tabulky s navigační vlastnosti](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="0fd95-138">Následující fragment kódu ukazuje stejný model, který byl vytvořen s Code First.</span><span class="sxs-lookup"><span data-stu-id="0fd95-138">The following code snippet shows the same model that was created with Code First.</span></span>

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

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="0fd95-139">Konfigurace nebo mapování relací</span><span class="sxs-lookup"><span data-stu-id="0fd95-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="0fd95-140">Zbytek této stránce se zaměřuje na přístup k a manipulaci s daty pomocí relací.</span><span class="sxs-lookup"><span data-stu-id="0fd95-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="0fd95-141">Informace o nastavení relace v modelu získáte na následujících stránkách.</span><span class="sxs-lookup"><span data-stu-id="0fd95-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="0fd95-142">Konfiguraci relace v Code First najdete v tématu [anotacemi dat](~/ef6/modeling/code-first/data-annotations.md) a [rozhraní Fluent API – vztahy](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="0fd95-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="0fd95-143">Konfigurace relace pomocí Entity Framework Designer, naleznete v tématu [vztahy s EF designeru](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="0fd95-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="0fd95-144">Vytvoření a úprava relací</span><span class="sxs-lookup"><span data-stu-id="0fd95-144">Creating and modifying relationships</span></span>

<span data-ttu-id="0fd95-145">V *přidružení cizího klíče*, když změníte vztah stavu závislý objekt s `EntityState.Unchanged` stav se změní na `EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="0fd95-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an `EntityState.Unchanged` state changes to `EntityState.Modified`.</span></span> <span data-ttu-id="0fd95-146">Ve vztahu k nezávislé změny vztahů neaktualizuje stav objektu závislý.</span><span class="sxs-lookup"><span data-stu-id="0fd95-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="0fd95-147">Následující příklady ukazují, jak pomocí vlastnosti cizího klíče a navigačních vlastností můžete přidružit souvisejících objektů.</span><span class="sxs-lookup"><span data-stu-id="0fd95-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="0fd95-148">Přidružení cizího klíče můžete pomocí některé z metod můžete změnit, vytvořit nebo upravit relace.</span><span class="sxs-lookup"><span data-stu-id="0fd95-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="0fd95-149">S nezávislé přidružení nelze použít vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="0fd95-149">With independent associations, you cannot use the foreign key property.</span></span>

- <span data-ttu-id="0fd95-150">Přiřazením novou hodnotu pro vlastnost cizího klíče, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0fd95-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- <span data-ttu-id="0fd95-151">Následující kód odebere relace nastavením cizího klíče na **null**.</span><span class="sxs-lookup"><span data-stu-id="0fd95-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="0fd95-152">Všimněte si, že vlastnost cizího klíče musí být s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="0fd95-152">Note, that the foreign key property must be nullable.</span></span>  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > <span data-ttu-id="0fd95-153">Pokud odkaz je ve stavu přidáno (v tomto příkladu kurzu objektu), odkaz na vlastnost navigace se nebudou synchronizovat s klíčovými hodnotami nový objekt dokud je volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="0fd95-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="0fd95-154">Synchronizace nebude fungovat, protože kontext objektu neobsahuje trvalé klíče pro přidání objektů, dokud se uloží.</span><span class="sxs-lookup"><span data-stu-id="0fd95-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="0fd95-155">Pokud potřebujete nové objekty, které jsou plně synchronizována v co nejdříve nastavit relace, použijte jednu z následujících methods.\*</span><span class="sxs-lookup"><span data-stu-id="0fd95-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

- <span data-ttu-id="0fd95-156">Po přiřazení nového objektu pro navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0fd95-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="0fd95-157">Následující kód vytvoří vztah mezi kurz a `department`.</span><span class="sxs-lookup"><span data-stu-id="0fd95-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="0fd95-158">Pokud objekty jsou připojené ke kontextu, `course` je taky přidaný ke `department.Courses` kolekce a odpovídající cizího klíče na vlastnost `course` objektu je nastavena na hodnotu klíče oddělení.</span><span class="sxs-lookup"><span data-stu-id="0fd95-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
  ``` csharp
  course.Department = department;
  ```

- <span data-ttu-id="0fd95-159">Pokud chcete odstranit vztah, nastavte vlastnost navigace na `null`.</span><span class="sxs-lookup"><span data-stu-id="0fd95-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="0fd95-160">Pokud pracujete se sadou Entity Framework, která je založena na rozhraní .NET 4.0, element end musí být načteny, než se nastaví na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="0fd95-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="0fd95-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="0fd95-161">For example:</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  <span data-ttu-id="0fd95-162">Od verze Entity Framework 5.0, založené na rozhraní .NET 4.5, můžete nastavit vztah na hodnotu null bez načtení element end.</span><span class="sxs-lookup"><span data-stu-id="0fd95-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="0fd95-163">Můžete také nastavit aktuální hodnotu na hodnotu null, následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0fd95-163">You can also set the current value to null using the following method.</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- <span data-ttu-id="0fd95-164">Odstraněním nebo přidávání objektů v kolekci entit.</span><span class="sxs-lookup"><span data-stu-id="0fd95-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="0fd95-165">Například můžete přidat objekt typu `Course` k `department.Courses` kolekce.</span><span class="sxs-lookup"><span data-stu-id="0fd95-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="0fd95-166">Tato operace vytvoří vztah mezi konkrétní **kurzu** a konkrétní `department`.</span><span class="sxs-lookup"><span data-stu-id="0fd95-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="0fd95-167">Pokud objekty jsou připojené ke kontextu, odkaz na oddělení a vlastnost cizího klíče na **kurzu** objektu se nastaví na příslušné `department`.</span><span class="sxs-lookup"><span data-stu-id="0fd95-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- <span data-ttu-id="0fd95-168">S použitím `ChangeRelationshipState` metodu, která změní stav zadaného vztah mezi dva objekty entity.</span><span class="sxs-lookup"><span data-stu-id="0fd95-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="0fd95-169">Tato metoda se nejčastěji používá při práci s N-vrstvé aplikace a *nezávislé přidružení* (jej nelze použít s přidružení cizího klíče).</span><span class="sxs-lookup"><span data-stu-id="0fd95-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="0fd95-170">Také, aby používali tuto metodu je třeba vyřadit dolů na `ObjectContext`, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="0fd95-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="0fd95-171">V následujícím příkladu je many-to-many vztah mezi Instruktoři a kurzy.</span><span class="sxs-lookup"><span data-stu-id="0fd95-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="0fd95-172">Volání `ChangeRelationshipState` metoda a předávání `EntityState.Added` parametr, umožňuje `SchoolContext` vědět, že byla přidána vztahu mezi dvěma objekty:</span><span class="sxs-lookup"><span data-stu-id="0fd95-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  <span data-ttu-id="0fd95-173">Všimněte si, že pokud aktualizujete (nikoli pouze přidáním) vztahu, musíte odstranit staré relace po přidání nové:</span><span class="sxs-lookup"><span data-stu-id="0fd95-173">Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:</span></span>

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="0fd95-174">Synchronizace změn mezi cizí klíče a vlastnosti navigace</span><span class="sxs-lookup"><span data-stu-id="0fd95-174">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="0fd95-175">Při změně vztah mezi objekty pomocí jedné z metod popsaných výše připojit ke kontextu Entity Framework je potřeba udržovat synchronizované cizí klíče, odkazy a kolekce. Entity Framework automaticky spravuje této synchronizace (označované také jako relace opravit) pro POCO entity s proxy servery.</span><span class="sxs-lookup"><span data-stu-id="0fd95-175">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="0fd95-176">Další informace najdete v tématu [práce s proxy](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="0fd95-176">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="0fd95-177">Pokud používáte entity POCO bez proxy servery, je třeba Ujistěte se, že **metoda DetectChanges** metoda je volána k synchronizaci souvisejících objektů, které v kontextu.</span><span class="sxs-lookup"><span data-stu-id="0fd95-177">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="0fd95-178">Všimněte si, který automaticky aktivuje následující rozhraní API **metoda DetectChanges** volání.</span><span class="sxs-lookup"><span data-stu-id="0fd95-178">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

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
-   <span data-ttu-id="0fd95-179">Provádění LINQ dotazovat `DbSet`</span><span class="sxs-lookup"><span data-stu-id="0fd95-179">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="0fd95-180">Načítají se související objekty</span><span class="sxs-lookup"><span data-stu-id="0fd95-180">Loading related objects</span></span>

<span data-ttu-id="0fd95-181">V Entity Framework, které nejčastěji používáte pro načtení entity, které se vztahují k vrácenou entitu definovanou v přidružení použijte navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0fd95-181">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="0fd95-182">Další informace najdete v tématu [načítání související objekty](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="0fd95-182">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0fd95-183">V přidružení cizího klíče při načtení související konec závislý objekt objekt v relaci se načtou podle hodnoty cizího klíče, který je aktuálně v paměti závislé:</span><span class="sxs-lookup"><span data-stu-id="0fd95-183">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="0fd95-184">V nezávislých přidružení je dotazován související konec závislý objekt podle hodnoty cizího klíče, který je aktuálně v databázi.</span><span class="sxs-lookup"><span data-stu-id="0fd95-184">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="0fd95-185">Nicméně pokud relace byla změněna a odkaz na vlastnost závislý objekt odkazuje na jiný objekt, který je načten v kontextu objektu, Entity Framework se pokusíme vytvořit relaci jako je definován na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="0fd95-185">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="0fd95-186">Správa souběžnosti</span><span class="sxs-lookup"><span data-stu-id="0fd95-186">Managing concurrency</span></span>

<span data-ttu-id="0fd95-187">V cizím klíči a nezávislé přidružení kontrolách souběžnosti se na základě klíče entit a jiné vlastnosti entity, které jsou definovány v modelu.</span><span class="sxs-lookup"><span data-stu-id="0fd95-187">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="0fd95-188">Při použití k vytvoření modelu EF designeru, nastavte `ConcurrencyMode` atribut **oprava** k určení, že vlastnost by měla sloužit k souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="0fd95-188">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="0fd95-189">Při použití Code First definovat model, použijte `ConcurrencyCheck` Poznámka u vlastnosti, které mají být vráceny pro souběžnost.</span><span class="sxs-lookup"><span data-stu-id="0fd95-189">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="0fd95-190">Při práci se službou Code First také můžete použít `TimeStamp` poznámky k určení, že vlastnost by měla sloužit k souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="0fd95-190">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="0fd95-191">Může mít pouze jednu vlastnost časového razítka v dané třídě.</span><span class="sxs-lookup"><span data-stu-id="0fd95-191">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="0fd95-192">Tato vlastnost mapy kódu nejprve neumožňující hodnotu pole v databázi.</span><span class="sxs-lookup"><span data-stu-id="0fd95-192">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="0fd95-193">Doporučujeme vám, že vždy používáte přidružení cizího klíče při práci s entitami, které jsou součástí Kontrola souběžnosti a řešení.</span><span class="sxs-lookup"><span data-stu-id="0fd95-193">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="0fd95-194">Další informace najdete v tématu [zpracování konfliktů souběžnosti](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="0fd95-194">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="0fd95-195">Práce s překrývajícími se klíči</span><span class="sxs-lookup"><span data-stu-id="0fd95-195">Working with overlapping Keys</span></span>

<span data-ttu-id="0fd95-196">Překrývající se klíče jsou složené klíče, kde některé vlastnosti v klíči jsou taky součástí jiný klíč v dané entitě.</span><span class="sxs-lookup"><span data-stu-id="0fd95-196">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="0fd95-197">Překrývající se klíč nemůže mít v nezávislých přidružení.</span><span class="sxs-lookup"><span data-stu-id="0fd95-197">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="0fd95-198">Chcete-li změnit přidružení cizího klíče, který obsahuje překrývajícími se klíči, doporučujeme proto upravovat hodnoty cizího klíče místo použití odkazy na objekty.</span><span class="sxs-lookup"><span data-stu-id="0fd95-198">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
