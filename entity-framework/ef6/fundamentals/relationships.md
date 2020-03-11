---
title: Relace, navigační vlastnosti a cizí klíče – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 892e872e3cb11ea95084cf6d9ab43c8d500bc0de
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419351"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="f1886-102">Relace, navigační vlastnosti a cizí klíče</span><span class="sxs-lookup"><span data-stu-id="f1886-102">Relationships, navigation properties, and foreign keys</span></span>

<span data-ttu-id="f1886-103">Tento článek poskytuje přehled o tom, jak Entity Framework spravuje vztahy mezi entitami.</span><span class="sxs-lookup"><span data-stu-id="f1886-103">This article gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="f1886-104">Poskytuje také pokyny k tomu, jak namapovat a manipulovat s relacemi.</span><span class="sxs-lookup"><span data-stu-id="f1886-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="f1886-105">Vztahy v EF</span><span class="sxs-lookup"><span data-stu-id="f1886-105">Relationships in EF</span></span>

<span data-ttu-id="f1886-106">V relačních databázích jsou relace (označované také jako asociace) mezi tabulkami definovány pomocí cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="f1886-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="f1886-107">Cizí klíč (FK) je sloupec nebo kombinace sloupců, které se používají k vytvoření a vykonání propojení mezi daty ve dvou tabulkách.</span><span class="sxs-lookup"><span data-stu-id="f1886-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="f1886-108">K dispozici jsou všeobecně tři typy vztahů: 1:1, 1: n a m:n.</span><span class="sxs-lookup"><span data-stu-id="f1886-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="f1886-109">V relaci 1: n je cizí klíč definován v tabulce, která představuje mnoho konců relace.</span><span class="sxs-lookup"><span data-stu-id="f1886-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="f1886-110">Relace m:n zahrnuje definování třetí tabulky (označované jako spojení nebo spojovací tabulka), jejíž primární klíč se skládá z cizích klíčů z obou souvisejících tabulek.</span><span class="sxs-lookup"><span data-stu-id="f1886-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="f1886-111">V relaci 1:1 se primární klíč chová navíc jako cizí klíč a neexistuje žádný samostatný sloupec cizího klíče pro žádnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="f1886-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="f1886-112">Následující obrázek znázorňuje dvě tabulky, které se účastní relace 1: n.</span><span class="sxs-lookup"><span data-stu-id="f1886-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="f1886-113">Tabulka **kurzů** je závislá na tabulce, protože obsahuje sloupec **DepartmentID** , který ho propojuje s tabulkou **oddělení** .</span><span class="sxs-lookup"><span data-stu-id="f1886-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Tabulky oddělení a kurzu](~/ef6/media/database2.png)

<span data-ttu-id="f1886-115">V Entity Framework entita může souviset s jinými entitami prostřednictvím přidružení nebo vztahu.</span><span class="sxs-lookup"><span data-stu-id="f1886-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="f1886-116">Každá relace obsahuje dva elementy end, které popisují typ entity a násobnost typu (jedna, nulová nebo jedna nebo mnoho) pro dvě entity v dané relaci.</span><span class="sxs-lookup"><span data-stu-id="f1886-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="f1886-117">Vztah se může řídit referenčním omezením, které popisuje, který objekt end v relaci je role zabezpečení a která je závislá na roli.</span><span class="sxs-lookup"><span data-stu-id="f1886-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="f1886-118">Navigační vlastnosti poskytují způsob, jak procházet přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="f1886-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="f1886-119">Každý objekt může mít navigační vlastnost pro každý vztah, ve kterém se účastní.</span><span class="sxs-lookup"><span data-stu-id="f1886-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="f1886-120">Navigační vlastnosti umožňují procházet a spravovat relace v obou směrech, vracet buď Referenční objekt (Pokud je násobnost buď jedna, nebo nula, nebo-jedna), nebo kolekci (Pokud je násobnost mnoho).</span><span class="sxs-lookup"><span data-stu-id="f1886-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="f1886-121">Můžete se také rozhodnout, že máte jednosměrnou navigaci, v takovém případě můžete definovat vlastnost navigace pouze v jednom z typů, které jsou součástí relace, a nikoli v obou.</span><span class="sxs-lookup"><span data-stu-id="f1886-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="f1886-122">Doporučuje se zahrnout vlastnosti v modelu, který se mapuje na cizí klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="f1886-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="f1886-123">S využitím vlastností cizího klíče můžete vytvořit nebo změnit relaci úpravou hodnoty cizího klíče u závislého objektu.</span><span class="sxs-lookup"><span data-stu-id="f1886-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="f1886-124">Tento druh asociace se nazývá přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="f1886-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="f1886-125">Použití cizích klíčů je ještě důležitější při práci s odpojenými entitami.</span><span class="sxs-lookup"><span data-stu-id="f1886-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="f1886-126">Pamatujte na to, že při práci s 1-to-1 nebo 1-to-0.. 1 relace neexistují žádné samostatné sloupce cizího klíče, vlastnost Primary key funguje jako cizí klíč a je vždy zahrnutá v modelu.</span><span class="sxs-lookup"><span data-stu-id="f1886-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="f1886-127">Pokud se sloupce cizího klíče v modelu nezahrnují, jsou informace o přidružení spravované jako nezávislé objekty.</span><span class="sxs-lookup"><span data-stu-id="f1886-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="f1886-128">Relace jsou sledovány prostřednictvím odkazů na objekty namísto vlastností cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="f1886-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="f1886-129">Tento typ přidružení se nazývá *nezávislé přidružení*.</span><span class="sxs-lookup"><span data-stu-id="f1886-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="f1886-130">Nejběžnější způsob, jak upravit *nezávislé přidružení* , je upravit vlastnosti navigace, které jsou generovány pro každou entitu, která se účastní přidružení.</span><span class="sxs-lookup"><span data-stu-id="f1886-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="f1886-131">V modelu se můžete rozhodnout použít jeden nebo oba typy přidružení.</span><span class="sxs-lookup"><span data-stu-id="f1886-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="f1886-132">Pokud máte ale k dispozici čistě relaci m:n, která je propojená tabulkou JOIN obsahující pouze cizí klíče, EF použije nezávislé přidružení ke správě takového vztahu m:n.</span><span class="sxs-lookup"><span data-stu-id="f1886-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="f1886-133">Následující obrázek znázorňuje koncepční model, který byl vytvořen pomocí Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="f1886-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="f1886-134">Model obsahuje dvě entity, které se účastní relace 1: n.</span><span class="sxs-lookup"><span data-stu-id="f1886-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="f1886-135">Obě entity mají navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f1886-135">Both entities have navigation properties.</span></span> <span data-ttu-id="f1886-136">**Kurz** je závislá entita a má definovanou vlastnost cizího klíče **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="f1886-136">**Course** is the dependent entity and has the **DepartmentID** foreign key property defined.</span></span>

![Tabulky oddělení a kurzu s navigačními vlastnostmi](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="f1886-138">Následující fragment kódu ukazuje stejný model, který byl vytvořen pomocí Code First.</span><span class="sxs-lookup"><span data-stu-id="f1886-138">The following code snippet shows the same model that was created with Code First.</span></span>

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

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="f1886-139">Konfigurace nebo mapování vztahů</span><span class="sxs-lookup"><span data-stu-id="f1886-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="f1886-140">Zbývající část této stránky popisuje, jak získat přístup k datům a manipulovat s nimi pomocí relací.</span><span class="sxs-lookup"><span data-stu-id="f1886-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="f1886-141">Informace o nastavení vztahů v modelu najdete na následujících stránkách.</span><span class="sxs-lookup"><span data-stu-id="f1886-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="f1886-142">Pokud chcete nakonfigurovat vztahy v Code First, přečtěte si téma [datové poznámky](~/ef6/modeling/code-first/data-annotations.md) a [Fluent API – vztahy](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="f1886-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="f1886-143">Chcete-li nakonfigurovat relace pomocí Entity Framework Designer, přečtěte si téma [relace s návrhářem EF](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="f1886-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="f1886-144">Vytváření a úpravy relací</span><span class="sxs-lookup"><span data-stu-id="f1886-144">Creating and modifying relationships</span></span>

<span data-ttu-id="f1886-145">Při *přidružení cizího klíče*při změně vztahu se stav závislého objektu se stavem `EntityState.Unchanged` změní na `EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="f1886-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an `EntityState.Unchanged` state changes to `EntityState.Modified`.</span></span> <span data-ttu-id="f1886-146">V nezávislém vztahu změna vztahu neaktualizuje stav závislého objektu.</span><span class="sxs-lookup"><span data-stu-id="f1886-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="f1886-147">Následující příklady ukazují, jak použít vlastnosti cizího klíče a navigační vlastnosti k přidružení souvisejících objektů.</span><span class="sxs-lookup"><span data-stu-id="f1886-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="f1886-148">Pomocí přidružení cizího klíče můžete pomocí obou metod změnit, vytvořit nebo upravit relace.</span><span class="sxs-lookup"><span data-stu-id="f1886-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="f1886-149">U nezávislých přidružení nelze použít vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="f1886-149">With independent associations, you cannot use the foreign key property.</span></span>

- <span data-ttu-id="f1886-150">Přiřazením nové hodnoty k vlastnosti cizího klíče, jak je uvedeno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="f1886-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- <span data-ttu-id="f1886-151">Následující kód odstraní relaci nastavením cizího klíče na **hodnotu null**.</span><span class="sxs-lookup"><span data-stu-id="f1886-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="f1886-152">Všimněte si, že vlastnost cizího klíče musí mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="f1886-152">Note, that the foreign key property must be nullable.</span></span>  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > <span data-ttu-id="f1886-153">Pokud je odkaz v přidaném stavu (v tomto příkladu je to objekt kurzu), navigační vlastnost odkazu nebude synchronizována s hodnotami klíče nového objektu, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f1886-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="f1886-154">K synchronizaci nedochází, protože kontext objektu neobsahuje trvalé klíče pro přidané objekty, dokud nebudou uloženy.</span><span class="sxs-lookup"><span data-stu-id="f1886-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="f1886-155">Pokud je nutné, aby byly nové objekty plně synchronizovány, jakmile nastavíte relaci, použijte jednu z následujících metod. \*</span><span class="sxs-lookup"><span data-stu-id="f1886-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

- <span data-ttu-id="f1886-156">Přiřazením nového objektu navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f1886-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="f1886-157">Následující kód vytvoří relaci mezi kurzem a `department`.</span><span class="sxs-lookup"><span data-stu-id="f1886-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="f1886-158">Pokud jsou objekty připojeny k tomuto kontextu, `course` je také přidána do kolekce `department.Courses` a odpovídající vlastnost cizího klíče objektu `course` je nastavena na hodnotu klíčové vlastnosti oddělení.</span><span class="sxs-lookup"><span data-stu-id="f1886-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
  ``` csharp
  course.Department = department;
  ```

- <span data-ttu-id="f1886-159">Pokud chcete relaci odstranit, nastavte vlastnost navigace na `null`.</span><span class="sxs-lookup"><span data-stu-id="f1886-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="f1886-160">Pokud pracujete se Entity Framework, který je založen na rozhraní .NET 4,0, je nutné před nastavením na hodnotu null načíst související konec.</span><span class="sxs-lookup"><span data-stu-id="f1886-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="f1886-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f1886-161">For example:</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  <span data-ttu-id="f1886-162">Počínaje Entity Framework 5,0, která je založena na rozhraní .NET 4,5, můžete nastavit relaci na hodnotu null bez načtení souvisejícího konce.</span><span class="sxs-lookup"><span data-stu-id="f1886-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="f1886-163">Pomocí následující metody můžete také nastavit aktuální hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="f1886-163">You can also set the current value to null using the following method.</span></span>   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- <span data-ttu-id="f1886-164">Odstraněním nebo přidáním objektu v kolekci entit.</span><span class="sxs-lookup"><span data-stu-id="f1886-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="f1886-165">Například můžete přidat objekt typu `Course` do kolekce `department.Courses`.</span><span class="sxs-lookup"><span data-stu-id="f1886-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="f1886-166">Tato operace vytvoří vztah mezi konkrétním **kurzem** a konkrétním `department`.</span><span class="sxs-lookup"><span data-stu-id="f1886-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="f1886-167">Pokud jsou objekty připojeny k kontextu, odkaz na oddělení a vlastnost cizího klíče v objektu **Course** budou nastaveny na příslušné `department`.</span><span class="sxs-lookup"><span data-stu-id="f1886-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- <span data-ttu-id="f1886-168">Pomocí metody `ChangeRelationshipState` ke změně stavu zadaného vztahu mezi dvěma objekty entity.</span><span class="sxs-lookup"><span data-stu-id="f1886-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="f1886-169">Tato metoda se nejčastěji používá při práci s N-vrstvými aplikacemi a *nezávislou asociací* (nedá se použít s přidružením cizího klíče).</span><span class="sxs-lookup"><span data-stu-id="f1886-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="f1886-170">Chcete-li použít tuto metodu, musíte také vyřadit do `ObjectContext`, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="f1886-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="f1886-171">V následujícím příkladu existuje vztah n:n mezi instruktory a kurzy.</span><span class="sxs-lookup"><span data-stu-id="f1886-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="f1886-172">Volání metody `ChangeRelationshipState` a předání parametru `EntityState.Added`, umožňuje `SchoolContext`, že mezi dvěma objekty bylo přidáno relaci:</span><span class="sxs-lookup"><span data-stu-id="f1886-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  <span data-ttu-id="f1886-173">Všimněte si, že pokud aktualizujete (nestačí jenom přidávat) relaci, musíte po přidání nového vztahu odstranit původní relaci:</span><span class="sxs-lookup"><span data-stu-id="f1886-173">Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:</span></span>

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="f1886-174">Synchronizace změn mezi cizími klíči a navigačními vlastnostmi</span><span class="sxs-lookup"><span data-stu-id="f1886-174">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="f1886-175">Když změníte vztah objektů připojených k kontextu pomocí jedné z výše popsaných metod, Entity Framework musí udržovat cizí klíče, odkazy a kolekce synchronizované. Entity Framework automaticky spravuje tuto synchronizaci (označovanou také jako oprava relace) pro entity POCO s proxy servery.</span><span class="sxs-lookup"><span data-stu-id="f1886-175">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="f1886-176">Další informace najdete v tématu [práce se servery proxy](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="f1886-176">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="f1886-177">Pokud používáte entity POCO bez proxy, je nutné zajistit, aby byla volána metoda **DetectChanges** pro synchronizaci souvisejících objektů v kontextu.</span><span class="sxs-lookup"><span data-stu-id="f1886-177">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="f1886-178">Všimněte si, že následující rozhraní API automaticky aktivují volání **DetectChanges** .</span><span class="sxs-lookup"><span data-stu-id="f1886-178">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

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
-   <span data-ttu-id="f1886-179">Provádění dotazu LINQ na `DbSet`</span><span class="sxs-lookup"><span data-stu-id="f1886-179">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="f1886-180">Načítání souvisejících objektů</span><span class="sxs-lookup"><span data-stu-id="f1886-180">Loading related objects</span></span>

<span data-ttu-id="f1886-181">V Entity Framework běžně používáte navigační vlastnosti k načtení entit, které souvisejí s vrácenou entitou podle definovaného přidružení.</span><span class="sxs-lookup"><span data-stu-id="f1886-181">In Entity Framework you commonly use navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="f1886-182">Další informace najdete v tématu [načítání souvisejících objektů](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="f1886-182">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f1886-183">Při přidružení cizího klíče při načtení souvisejícího konce závislého objektu se načte související objekt na základě hodnoty cizího klíče závislé na, který je aktuálně v paměti:</span><span class="sxs-lookup"><span data-stu-id="f1886-183">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="f1886-184">V nezávislém přidružení je související konec závislého objektu dotazován na základě hodnoty cizího klíče, který je aktuálně v databázi.</span><span class="sxs-lookup"><span data-stu-id="f1886-184">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="f1886-185">Pokud se však změnil vztah a vlastnost reference na závislém objektu odkazuje na jiný objekt zabezpečení, který je načten v kontextu objektu, Entity Framework se pokusí vytvořit relaci, která je definována v klientovi.</span><span class="sxs-lookup"><span data-stu-id="f1886-185">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="f1886-186">Správa souběžnosti</span><span class="sxs-lookup"><span data-stu-id="f1886-186">Managing concurrency</span></span>

<span data-ttu-id="f1886-187">V cizím i nezávislém přidružení jsou kontroly souběžnosti založeny na klíčích entit a dalších vlastnostech entit, které jsou definovány v modelu.</span><span class="sxs-lookup"><span data-stu-id="f1886-187">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="f1886-188">Při použití návrháře EF k vytvoření modelu nastavte atribut `ConcurrencyMode` na **pevnou** , aby určoval, že by měla být vlastnost kontrolována pro souběžnost.</span><span class="sxs-lookup"><span data-stu-id="f1886-188">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="f1886-189">Při použití Code First k definování modelu použijte anotaci `ConcurrencyCheck` u vlastností, u kterých chcete zkontrolovat souběžnost.</span><span class="sxs-lookup"><span data-stu-id="f1886-189">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="f1886-190">Při práci s Code First lze také pomocí anotace `TimeStamp` určit, zda má být vlastnost pro souběžnost kontrolována.</span><span class="sxs-lookup"><span data-stu-id="f1886-190">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="f1886-191">V dané třídě může být pouze jedna vlastnost časového razítka.</span><span class="sxs-lookup"><span data-stu-id="f1886-191">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="f1886-192">Code First mapuje tuto vlastnost na pole bez hodnoty null v databázi.</span><span class="sxs-lookup"><span data-stu-id="f1886-192">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="f1886-193">Při práci s entitami, které se účastní kontroly souběžnosti a řešení, doporučujeme vždy používat přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="f1886-193">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="f1886-194">Další informace najdete v tématu [zpracování konfliktů souběžnosti](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="f1886-194">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="f1886-195">Práce s překrývajícími se klíči</span><span class="sxs-lookup"><span data-stu-id="f1886-195">Working with overlapping Keys</span></span>

<span data-ttu-id="f1886-196">Překrývající se klíče jsou složené klíče, kde některé vlastnosti v klíči jsou také součástí jiného klíče v entitě.</span><span class="sxs-lookup"><span data-stu-id="f1886-196">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="f1886-197">Nemůžete mít překrývající se klíč v nezávislém přidružení.</span><span class="sxs-lookup"><span data-stu-id="f1886-197">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="f1886-198">Chcete-li změnit přidružení cizího klíče, které obsahuje překrývající se klíče, doporučujeme místo použití odkazů na objekty upravovat hodnoty cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="f1886-198">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
