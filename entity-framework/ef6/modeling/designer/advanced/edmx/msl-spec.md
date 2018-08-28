---
title: Specifikace MSL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 77dc7072c70b104188cd23974f32308960daebb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996028"
---
# <a name="msl-specification"></a><span data-ttu-id="aaba2-102">Specifikace MSL</span><span class="sxs-lookup"><span data-stu-id="aaba2-102">MSL Specification</span></span>
<span data-ttu-id="aaba2-103">Mapování specifikačnímu jazyku (MSL) je jazyk založený na formátu XML, který popisuje mapování mezi koncepčního modelu i modelu úložiště aplikace Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aaba2-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="aaba2-104">V aplikaci Entity Framework je načtena metadata mapování ze souboru .msl (napsaného v MSL) v okamžiku sestavení.</span><span class="sxs-lookup"><span data-stu-id="aaba2-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="aaba2-105">Entity Framework používá pro překlad dotazů vůči Koncepční model příkazů specifických pro úložiště mapování metadat za běhu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="aaba2-106">Návrhář Entity Framework (EF designeru) ukládá informace o mapování v souboru .edmx v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="aaba2-107">V okamžiku sestavení Návrhář Entity používá informace v souboru .edmx k vytvoření souboru .msl, která je potřeba pro Entity Framework za běhu</span><span class="sxs-lookup"><span data-stu-id="aaba2-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="aaba2-108">Názvy všech koncepční nebo modelu typy úložišť, které jsou odkazovány v MSL musí být určeny podle jejich příslušných oborem názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="aaba2-109">Informace o názvu oboru názvů konceptuálního modelu naleznete v tématu [specifikace CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="aaba2-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="aaba2-110">Informace o názvu oboru názvů modelu úložiště najdete v tématu [specifikace SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="aaba2-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="aaba2-111">Verze MSL jsou rozlišené pomocí obory názvů XML.</span><span class="sxs-lookup"><span data-stu-id="aaba2-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="aaba2-112">Verze MSL</span><span class="sxs-lookup"><span data-stu-id="aaba2-112">MSL Version</span></span> | <span data-ttu-id="aaba2-113">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="aaba2-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="aaba2-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="aaba2-114">MSL v1</span></span>      | <span data-ttu-id="aaba2-115">název urn: schémata-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="aaba2-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="aaba2-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="aaba2-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="aaba2-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="aaba2-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="aaba2-118">Zástupnému elementu (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-118">Alias Element (MSL)</span></span>

<span data-ttu-id="aaba2-119">**Alias** prvek v mapování specification language (MSL) je podřízený element Mapping, který se používá k definování aliasy pro koncepční model a úložiště modelu obory názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="aaba2-120">Názvy všech koncepční nebo modelu typy úložišť, které jsou odkazovány v MSL musí být určeny podle jejich příslušných oborem názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="aaba2-121">Informace týkající se názvu oboru názvů konceptuálního modelu naleznete v tématu Element schématu (CSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="aaba2-122">Informace o názvu oboru názvů modelu úložiště najdete v tématu Element schématu (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="aaba2-123">**Alias** element nemůže mít podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-124">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-124">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-125">Následující tabulka popisuje atributy, které mohou být použity **Alias** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="aaba2-126">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-126">Attribute Name</span></span> | <span data-ttu-id="aaba2-127">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-127">Is Required</span></span> | <span data-ttu-id="aaba2-128">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="aaba2-129">**Key**</span></span>        | <span data-ttu-id="aaba2-130">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-130">Yes</span></span>         | <span data-ttu-id="aaba2-131">Alias pro obor názvů, která je zadána **hodnotu** atribut.</span><span class="sxs-lookup"><span data-stu-id="aaba2-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="aaba2-132">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="aaba2-132">**Value**</span></span>      | <span data-ttu-id="aaba2-133">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-133">Yes</span></span>         | <span data-ttu-id="aaba2-134">Obor názvů, pro kterou hodnotu **klíč** element je alias.</span><span class="sxs-lookup"><span data-stu-id="aaba2-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="aaba2-135">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-135">Example</span></span>

<span data-ttu-id="aaba2-136">Následující příklad ukazuje **Alias** element, který definuje alias, `c`, pro typy, které jsou definované v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a><span data-ttu-id="aaba2-137">Element AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="aaba2-138">**AssociationEnd** element v mapování specification language (MSL) se používá při funkcí změny typu entity v konceptuálním modelu jsou mapovány na uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="aaba2-139">Pokud změna uložené procedury přijímá parametr, jehož hodnota je uložená ve vlastnosti přidružení, **AssociationEnd** element mapuje hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="aaba2-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="aaba2-140">Další informace naleznete v níže uvedeném příkladu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-140">For more information, see the example below.</span></span>

<span data-ttu-id="aaba2-141">Další informace o mapování funkcí změny typů entit k uloženým procedurám, naleznete v tématu ModificationFunctionMapping – Element (MSL) a návod: mapování Entity na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="aaba2-142">**AssociationEnd** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="aaba2-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-144">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-144">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-145">Následující tabulka popisuje atributy, které se vztahují na **AssociationEnd** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="aaba2-146">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-146">Attribute Name</span></span>     | <span data-ttu-id="aaba2-147">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-147">Is Required</span></span> | <span data-ttu-id="aaba2-148">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-149">**Element AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="aaba2-149">**AssociationSet**</span></span> | <span data-ttu-id="aaba2-150">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-150">Yes</span></span>         | <span data-ttu-id="aaba2-151">Název přidružení, které je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="aaba2-152">**z**</span><span class="sxs-lookup"><span data-stu-id="aaba2-152">**From**</span></span>           | <span data-ttu-id="aaba2-153">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-153">Yes</span></span>         | <span data-ttu-id="aaba2-154">Hodnota **FromRole** atribut, který odpovídá přidružení mapovaný navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="aaba2-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="aaba2-155">Další informace najdete v tématu Element NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="aaba2-156">**k**</span><span class="sxs-lookup"><span data-stu-id="aaba2-156">**To**</span></span>             | <span data-ttu-id="aaba2-157">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-157">Yes</span></span>         | <span data-ttu-id="aaba2-158">Hodnota **ToRole** atribut, který odpovídá přidružení mapovaný navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="aaba2-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="aaba2-159">Další informace najdete v tématu Element NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="aaba2-160">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-160">Example</span></span>

<span data-ttu-id="aaba2-161">Vezměte v úvahu následující entity typu koncepčního modelu:</span><span class="sxs-lookup"><span data-stu-id="aaba2-161">Consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

<span data-ttu-id="aaba2-162">Zvažte také následující uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="aaba2-162">Also consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

<span data-ttu-id="aaba2-163">Pokud chcete mapovat funkci aktualizace `Course` entity pro tuto uloženou proceduru, je nutné zadat hodnotu, která **DepartmentID** parametr.</span><span class="sxs-lookup"><span data-stu-id="aaba2-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="aaba2-164">Hodnota pro `DepartmentID` neodpovídá vlastnosti typu entity; je součástí přidružení nezávislé, jehož mapování je znázorněna zde:</span><span class="sxs-lookup"><span data-stu-id="aaba2-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

<span data-ttu-id="aaba2-165">Následující kód ukazuje **AssociationEnd** element slouží k mapování **DepartmentID** vlastnost **FK\_kurzu\_oddělení** přidružení, které **UpdateCourse** uložené procedury (ke které funkci aktualizace **kurzu** namapované typ entity):</span><span class="sxs-lookup"><span data-stu-id="aaba2-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="aaba2-166">Element AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-167">**AssociationSetMapping** prvek v mapování specification language (MSL) definuje mapování mezi přidružení v konceptuálním modelu a tabulky sloupce v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="aaba2-168">Přidružení v konceptuálním modelu jsou typy, jejichž vlastnosti představují primární a cizí klíče sloupce v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="aaba2-169">**AssociationSetMapping** prvek používá dva prvky EndProperty definovat mapování mezi přidružení typu vlastnosti a sloupce v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="aaba2-170">Podmínky můžete umístit na tato mapování s elementem podmínku.</span><span class="sxs-lookup"><span data-stu-id="aaba2-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="aaba2-171">Mapovat na uložené procedury v databázi s elementem ModificationFunctionMapping insert, update a delete funkce pro přidružení.</span><span class="sxs-lookup"><span data-stu-id="aaba2-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="aaba2-172">Definujte jen pro čtení mapování mezi přidružení a sloupců tabulky pomocí řetězce Entity SQL v elementu QueryView.</span><span class="sxs-lookup"><span data-stu-id="aaba2-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-173">Pokud pro přidružení v konceptuálním modelu je definována referenční omezení, přidružení nemusí být namapována na žádnou **AssociationSetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="aaba2-174">Pokud **AssociationSetMapping** element je k dispozici pro přidružení, které obsahuje referenční omezení, mapování definované v **AssociationSetMapping** prvek bude ignorován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="aaba2-175">Další informace najdete v elementu ReferentialConstraint – Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="aaba2-176">**AssociationSetMapping** prvek může mít následujících podřízených elementů</span><span class="sxs-lookup"><span data-stu-id="aaba2-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="aaba2-177">Zobrazení QueryView (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="aaba2-178">EndProperty (nula nebo dva)</span><span class="sxs-lookup"><span data-stu-id="aaba2-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="aaba2-179">Podmínka (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="aaba2-180">ModificationFunctionMapping (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-181">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-181">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-182">Následující tabulka popisuje atributy, které mohou být použity **AssociationSetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="aaba2-183">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-183">Attribute Name</span></span>     | <span data-ttu-id="aaba2-184">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-184">Is Required</span></span> | <span data-ttu-id="aaba2-185">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-186">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="aaba2-186">**Name**</span></span>           | <span data-ttu-id="aaba2-187">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-187">Yes</span></span>         | <span data-ttu-id="aaba2-188">Název sady přidružení konceptuálního modelu, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="aaba2-189">**Název typu**</span><span class="sxs-lookup"><span data-stu-id="aaba2-189">**TypeName**</span></span>       | <span data-ttu-id="aaba2-190">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-190">No</span></span>          | <span data-ttu-id="aaba2-191">Název přidružení typu koncepčního modelu, který je mapován kvalifikovaný v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="aaba2-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="aaba2-192">**StoreEntitySet**</span></span> | <span data-ttu-id="aaba2-193">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-193">No</span></span>          | <span data-ttu-id="aaba2-194">Název tabulky, která je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="aaba2-195">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-195">Example</span></span>

<span data-ttu-id="aaba2-196">Následující příklad ukazuje **AssociationSetMapping** element, ve kterém **FK\_kurzu\_oddělení** přidružení nastavit v konceptuálním modelu je namapována na  **Kurz** tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="aaba2-197">Mapování mezi přidružení typu vlastnosti a sloupce tabulky, které jsou určené v podřízených **EndProperty** elementy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
``` 

## <a name="complexproperty-element-msl"></a><span data-ttu-id="aaba2-198">Element ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="aaba2-199">A **ComplexProperty** prvek v mapování specification language (MSL) definuje mapování mezi komplexní typ vlastnosti Koncepční model entity typu tabulky sloupce a v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="aaba2-200">Mapování sloupce vlastností jsou uvedeny v podřízených elementů ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="aaba2-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="aaba2-201">**ComplexType** vlastnost prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-202">ScalarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="aaba2-203">**ComplexProperty** (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="aaba2-204">ComplextTypeMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="aaba2-205">Podmínka (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-206">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-206">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-207">Následující tabulka popisuje atributy, které se vztahují na **ComplexProperty** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="aaba2-208">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-208">Attribute Name</span></span> | <span data-ttu-id="aaba2-209">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-209">Is Required</span></span> | <span data-ttu-id="aaba2-210">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-211">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="aaba2-211">**Name**</span></span>       | <span data-ttu-id="aaba2-212">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-212">Yes</span></span>         | <span data-ttu-id="aaba2-213">Název komplexní vlastnost typu entity v konceptuálním modelu, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="aaba2-214">**Název typu**</span><span class="sxs-lookup"><span data-stu-id="aaba2-214">**TypeName**</span></span>   | <span data-ttu-id="aaba2-215">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-215">No</span></span>          | <span data-ttu-id="aaba2-216">Název kvalifikovaný v oboru názvů vlastnost typu koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="aaba2-217">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-217">Example</span></span>

<span data-ttu-id="aaba2-218">Následující příklad je založen na modelu školy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-218">The following example is based on the School model.</span></span> <span data-ttu-id="aaba2-219">Následující komplexní typ se přidala do koncepčního modelu:</span><span class="sxs-lookup"><span data-stu-id="aaba2-219">The following complex type has been added to the conceptual model:</span></span>

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

<span data-ttu-id="aaba2-220">**LastName** a **FirstName** vlastnosti **osoba** typ entity byly nahrazeny jednoho komplexní vlastnost **název**:</span><span class="sxs-lookup"><span data-stu-id="aaba2-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

<span data-ttu-id="aaba2-221">Zobrazí se následující MSL **ComplexProperty** element slouží k mapování **název** vlastnost na sloupce v podkladové databázi:</span><span class="sxs-lookup"><span data-stu-id="aaba2-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="aaba2-222">Element ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-223">**ComplexTypeMapping** prvek v mapování specification language (MSL) je podřízeným prvkem elementu ResultMapping a definuje mapování mezi importované funkce v konceptuálním modelu a uložené procedury v základním databáze, pokud jsou pravdivé následující výroky:</span><span class="sxs-lookup"><span data-stu-id="aaba2-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="aaba2-224">Importovaná funkce vrátí koncepční komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="aaba2-225">Názvy sloupců vrácený uloženou proceduru přesně shodovat s názvy vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="aaba2-226">Ve výchozím nastavení mapování sloupců vrácený uložené procedury a komplexní typ je založen na názvy sloupců a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="aaba2-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="aaba2-227">Pokud se názvy sloupců přesně shodovat s názvy vlastností, je nutné použít **ComplexTypeMapping** elementu k definování mapování.</span><span class="sxs-lookup"><span data-stu-id="aaba2-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="aaba2-228">Příklad výchozí mapování naleznete v tématu FunctionImportMapping – Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="aaba2-229">**ComplexTypeMapping** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-230">ScalarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-231">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-231">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-232">Následující tabulka popisuje atributy, které se vztahují na **ComplexTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="aaba2-233">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-233">Attribute Name</span></span> | <span data-ttu-id="aaba2-234">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-234">Is Required</span></span> | <span data-ttu-id="aaba2-235">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="aaba2-236">**Název typu**</span><span class="sxs-lookup"><span data-stu-id="aaba2-236">**TypeName**</span></span>   | <span data-ttu-id="aaba2-237">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-237">Yes</span></span>         | <span data-ttu-id="aaba2-238">Název kvalifikovaný v oboru názvů komplexní typ, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-239">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-239">Example</span></span>

<span data-ttu-id="aaba2-240">Vezměte v úvahu následující uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="aaba2-240">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="aaba2-241">Zvažte také následující komplexní typ koncepčního modelu:</span><span class="sxs-lookup"><span data-stu-id="aaba2-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="aaba2-242">Chcete-li vytvořit importovaná funkce, který vrátí instance předchozí komplexní typ, vrátí mapování mezi sloupci pomocí uložené procedury a typu entity musí být definován v **ComplexTypeMapping** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a><span data-ttu-id="aaba2-243">Podmínka – Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-243">Condition Element (MSL)</span></span>

<span data-ttu-id="aaba2-244">**Podmínku** prvek v mapování specification language (MSL) umístí podmínek mapování mezi Koncepční model a databáze.</span><span class="sxs-lookup"><span data-stu-id="aaba2-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="aaba2-245">Mapování, která je definována v rámci uzlu XML je platné, pokud všechny podmínky, jako zadané v podřízených **podmínku** elementy, jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="aaba2-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="aaba2-246">V opačném případě mapování je neplatný.</span><span class="sxs-lookup"><span data-stu-id="aaba2-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="aaba2-247">Například, pokud prvek MappingFragment obsahuje jeden nebo více **podmínku** podřízených elementů mapování definované v rámci **MappingFragment** uzlu bude platit jenom Pokud všechny podmínky podřízené  **Podmínka** prvky jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="aaba2-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="aaba2-248">Každou podmínku můžete použít buď **název** (název vlastnosti entity konceptuální model, určené **název** atribut), nebo **Názevsloupce** (název sloupce v Zadaná databáze, tak **Názevsloupce** atributu).</span><span class="sxs-lookup"><span data-stu-id="aaba2-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="aaba2-249">Když **název** atribut je nastaven, podmínka je porovnávána s hodnotou vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="aaba2-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="aaba2-250">Když **Názevsloupce** atribut je nastaven, podmínka je porovnávána s hodnotu sloupce.</span><span class="sxs-lookup"><span data-stu-id="aaba2-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="aaba2-251">Pouze jeden z **název** nebo **Názevsloupce** atribut se dá nastavit v **podmínku** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-252">Když **podmínku** element se používá v rámci elementu FunctionImportMapping, pouze **název** atribut se nedá použít.</span><span class="sxs-lookup"><span data-stu-id="aaba2-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="aaba2-253">**Podmínku** element může být podřízená následující prvky:</span><span class="sxs-lookup"><span data-stu-id="aaba2-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="aaba2-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="aaba2-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="aaba2-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="aaba2-255">ComplexProperty</span></span>
-   <span data-ttu-id="aaba2-256">Element EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="aaba2-256">EntitySetMapping</span></span>
-   <span data-ttu-id="aaba2-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="aaba2-257">MappingFragment</span></span>
-   <span data-ttu-id="aaba2-258">Element EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="aaba2-258">EntityTypeMapping</span></span>

<span data-ttu-id="aaba2-259">**Podmínku** prvek může mít žádné podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-260">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-260">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-261">Následující tabulka popisuje atributy, které se vztahují na **podmínku** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="aaba2-262">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-262">Attribute Name</span></span> | <span data-ttu-id="aaba2-263">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-263">Is Required</span></span> | <span data-ttu-id="aaba2-264">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-265">**Názevsloupce**</span><span class="sxs-lookup"><span data-stu-id="aaba2-265">**ColumnName**</span></span> | <span data-ttu-id="aaba2-266">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-266">No</span></span>          | <span data-ttu-id="aaba2-267">Název sloupce tabulky, jehož hodnota se používá k vyhodnocení podmínky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="aaba2-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="aaba2-268">**IsNull**</span></span>     | <span data-ttu-id="aaba2-269">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-269">No</span></span>          | <span data-ttu-id="aaba2-270">**Hodnota TRUE** nebo **False**.</span><span class="sxs-lookup"><span data-stu-id="aaba2-270">**True** or **False**.</span></span> <span data-ttu-id="aaba2-271">Pokud je hodnota **True** a hodnota sloupce je **null**, nebo pokud je hodnota **False** a hodnota sloupce není **null**, bude podmínka pravdivá .</span><span class="sxs-lookup"><span data-stu-id="aaba2-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="aaba2-272">V opačném případě podmínka není splněna.</span><span class="sxs-lookup"><span data-stu-id="aaba2-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="aaba2-273">**IsNull** a **hodnotu** atributy nelze použít ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="aaba2-274">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="aaba2-274">**Value**</span></span>      | <span data-ttu-id="aaba2-275">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-275">No</span></span>          | <span data-ttu-id="aaba2-276">Hodnota, pomocí kterého se porovnává hodnotu sloupce.</span><span class="sxs-lookup"><span data-stu-id="aaba2-276">The value with which the column value is compared.</span></span> <span data-ttu-id="aaba2-277">Pokud jsou hodnoty stejné, je podmínka pravdivá.</span><span class="sxs-lookup"><span data-stu-id="aaba2-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="aaba2-278">V opačném případě podmínka není splněna.</span><span class="sxs-lookup"><span data-stu-id="aaba2-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="aaba2-279">**IsNull** a **hodnotu** atributy nelze použít ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="aaba2-280">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="aaba2-280">**Name**</span></span>       | <span data-ttu-id="aaba2-281">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-281">No</span></span>          | <span data-ttu-id="aaba2-282">Název vlastnosti koncepčního modelu entity, jehož hodnota se používá k vyhodnocení podmínky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="aaba2-283">Tento atribut se nedá použít pokud **podmínku** element se používá v rámci elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="aaba2-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="aaba2-284">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-284">Example</span></span>

<span data-ttu-id="aaba2-285">Následující příklad ukazuje **podmínku** prvků jako podřízených prvků **MappingFragment** elementy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="aaba2-286">Když **HireDate** nemá hodnotu null a **EnrollmentDate** je null, je mapovány dat mezi **SchoolModel.Instructor** typ a **PersonID**a **HireDate** sloupce **osoba** tabulky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="aaba2-287">Když **EnrollmentDate** nemá hodnotu null a **HireDate** je null, je mapovány dat mezi **SchoolModel.Student** typ a **PersonID** a **registrace** sloupce **osoba** tabulky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a><span data-ttu-id="aaba2-288">Element DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="aaba2-289">**DeleteFunction** prvek v mapování specification language (MSL) mapuje funkci Odstranit typ entity nebo přidružení v konceptuálním modelu na uloženou proceduru v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="aaba2-290">Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="aaba2-291">Další informace najdete v tématu funkce – Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-292">Pokud nejsou mapovány všechny tři vložení, aktualizace nebo odstranění operace typu entity na uložené procedury nenamapované operace se nezdaří, pokud je proveden za běhu a UpdateException je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="aaba2-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="aaba2-293">Použít pro mapování EntityTypeMapping DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="aaba2-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="aaba2-294">Při použití elementu mapování EntityTypeMapping **DeleteFunction** prvek mapuje delete – funkce typu entity v konceptuálním modelu na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="aaba2-295">**DeleteFunction** prvek může mít následující podřízené prvky při použití **mapování EntityTypeMapping** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="aaba2-296">AssociationEnd (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="aaba2-297">ComplexProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="aaba2-298">ScarlarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-299">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-299">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-300">Následující tabulka popisuje atributy, které mohou být použity **DeleteFunction** prvku, když se použije k **mapování EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="aaba2-301">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-301">Attribute Name</span></span>            | <span data-ttu-id="aaba2-302">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-302">Is Required</span></span> | <span data-ttu-id="aaba2-303">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-304">**functionName**</span><span class="sxs-lookup"><span data-stu-id="aaba2-304">**FunctionName**</span></span>          | <span data-ttu-id="aaba2-305">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-305">Yes</span></span>         | <span data-ttu-id="aaba2-306">Název uložené procedury, ke které je mapován funkci Odstranit kvalifikovaný v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="aaba2-307">Uložená procedura musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="aaba2-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="aaba2-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="aaba2-309">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-309">No</span></span>          | <span data-ttu-id="aaba2-310">Název výstupní parametr, který vrací počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="aaba2-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="aaba2-311">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-311">Example</span></span>

<span data-ttu-id="aaba2-312">Následující příklad je založen na modelu školy a ukazuje **DeleteFunction** element mapování funkci Odstranit **osoba** typu entity na **DeletePerson** uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="aaba2-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="aaba2-313">**DeletePerson** uložené procedury je deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="aaba2-314">U AssociationSetMapping DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="aaba2-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="aaba2-315">Při použití elementu AssociationSetMapping **DeleteFunction** prvek mapuje funkci Odstranit přidružení v konceptuálním modelu na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="aaba2-316">**DeleteFunction** prvek může mít následující podřízené prvky při použití **AssociationSetMapping** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="aaba2-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="aaba2-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-318">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-318">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-319">Následující tabulka popisuje atributy, které mohou být použity **DeleteFunction** prvku, když se použije k **AssociationSetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="aaba2-320">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-320">Attribute Name</span></span>            | <span data-ttu-id="aaba2-321">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-321">Is Required</span></span> | <span data-ttu-id="aaba2-322">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-323">**functionName**</span><span class="sxs-lookup"><span data-stu-id="aaba2-323">**FunctionName**</span></span>          | <span data-ttu-id="aaba2-324">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-324">Yes</span></span>         | <span data-ttu-id="aaba2-325">Název uložené procedury, ke které je mapován funkci Odstranit kvalifikovaný v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="aaba2-326">Uložená procedura musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="aaba2-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="aaba2-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="aaba2-328">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-328">No</span></span>          | <span data-ttu-id="aaba2-329">Název výstupní parametr, který vrací počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="aaba2-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="aaba2-330">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-330">Example</span></span>

<span data-ttu-id="aaba2-331">Následující příklad je založen na modelu školy a ukazuje **DeleteFunction** element slouží k mapování funkce delete **CourseInstructor** přidružení, které  **DeleteCourseInstructor** uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="aaba2-332">**DeleteCourseInstructor** uložené procedury je deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a><span data-ttu-id="aaba2-333">Element EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="aaba2-334">**EndProperty** prvek v mapování specification language (MSL) definuje mapování mezi skončí nebo úpravy funkce přidružení konceptuálního modelu a databáze.</span><span class="sxs-lookup"><span data-stu-id="aaba2-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="aaba2-335">Sloupce vlastností Mapování bylo specifikováno v ScalarProperty podřízený element.</span><span class="sxs-lookup"><span data-stu-id="aaba2-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="aaba2-336">Když **EndProperty** element slouží k definování mapování na konci přidružení konceptuálního modelu, je podřízeným prvkem elementu AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="aaba2-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="aaba2-337">Když **EndProperty** element slouží k definování mapování pro funkci úpravy přidružení konceptuálního modelu, je podřízeným prvkem elementu InsertFunction nebo DeleteFunction elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="aaba2-338">**EndProperty** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-339">ScalarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-340">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-340">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-341">Následující tabulka popisuje atributy, které se vztahují na **EndProperty** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="aaba2-342">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-342">Attribute Name</span></span> | <span data-ttu-id="aaba2-343">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-343">Is Required</span></span> | <span data-ttu-id="aaba2-344">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="aaba2-345">Název</span><span class="sxs-lookup"><span data-stu-id="aaba2-345">Name</span></span>           | <span data-ttu-id="aaba2-346">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-346">Yes</span></span>         | <span data-ttu-id="aaba2-347">Název elementu end přidružení, které je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-348">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-348">Example</span></span>

<span data-ttu-id="aaba2-349">Následující příklad ukazuje **AssociationSetMapping** element, ve kterém **FK\_kurzu\_oddělení** přidružení v konceptuálním modelu je namapována na **Kurzu** tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="aaba2-350">Mapování mezi přidružení typu vlastnosti a sloupce tabulky, které jsou určené v podřízených **EndProperty** elementy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a><span data-ttu-id="aaba2-351">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-351">Example</span></span>

<span data-ttu-id="aaba2-352">Následující příklad ukazuje **EndProperty** mapování funkce insert a delete asociace – element (**CourseInstructor**) na uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="aaba2-353">Funkce, které jsou mapovány na jsou deklarovány v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-353">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="aaba2-354">Element EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-355">**Mapování EntityContainerMapping** prvek v mapování specification language (MSL) mapuje kontejner entit v konceptuálním modelu na kontejner entit v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="aaba2-356">**Mapování EntityContainerMapping** element je podřízeným prvkem elementu mapování.</span><span class="sxs-lookup"><span data-stu-id="aaba2-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="aaba2-357">**Mapování EntityContainerMapping** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="aaba2-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="aaba2-358">Element EntitySetMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="aaba2-359">AssociationSetMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="aaba2-360">FunctionImportMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-361">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-361">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-362">Následující tabulka popisuje atributy, které mohou být použity **mapování EntityContainerMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="aaba2-363">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-363">Attribute Name</span></span>            | <span data-ttu-id="aaba2-364">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-364">Is Required</span></span> | <span data-ttu-id="aaba2-365">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="aaba2-366">**StorageModelContainer**</span></span> | <span data-ttu-id="aaba2-367">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-367">Yes</span></span>         | <span data-ttu-id="aaba2-368">Název kontejneru entity modelu úložiště, které je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="aaba2-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="aaba2-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="aaba2-370">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-370">Yes</span></span>         | <span data-ttu-id="aaba2-371">Název kontejneru entity konceptuální model, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="aaba2-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="aaba2-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="aaba2-373">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-373">No</span></span>          | <span data-ttu-id="aaba2-374">**Hodnota TRUE** nebo **False**.</span><span class="sxs-lookup"><span data-stu-id="aaba2-374">**True** or **False**.</span></span> <span data-ttu-id="aaba2-375">Pokud **False**, vygenerují se aktualizace k dispozici žádná zobrazení.</span><span class="sxs-lookup"><span data-stu-id="aaba2-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="aaba2-376">Tento atribut by měl být nastaven **False** Pokud máte mapování jen pro čtení, který by byl neplatný, protože data můžou není operace round-trip úspěšně.</span><span class="sxs-lookup"><span data-stu-id="aaba2-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="aaba2-377">Výchozí hodnota je **True**.</span><span class="sxs-lookup"><span data-stu-id="aaba2-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-378">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-378">Example</span></span>

<span data-ttu-id="aaba2-379">Následující příklad ukazuje **mapování EntityContainerMapping** element, který se mapuje **SchoolModelEntities** kontejneru (kontejner entit koncepčního modelu)  **SchoolModelStoreContainer** kontejneru (kontejner entity model úložiště):</span><span class="sxs-lookup"><span data-stu-id="aaba2-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="aaba2-380">Element EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-381">**Elementu EntitySetMapping** nastaví prvek v mapování specification language (MSL) mapy všechny typy v konceptuálním modelu entity nastavena na entity v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="aaba2-382">Je logický kontejner pro sadu entit v konceptuálním modelu instancí entit stejného typu (a odvozené typy).</span><span class="sxs-lookup"><span data-stu-id="aaba2-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="aaba2-383">Sadu entit v modelu úložiště představuje tabulku nebo zobrazení v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="aaba2-384">Sada entit koncepčního modelu je zadána hodnota **název** atribut **elementu EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="aaba2-385">Mapované na tabulku nebo zobrazení je určená **StoreEntitySet** atribut každý podřízený prvek MappingFragment nebo v **elementu EntitySetMapping** elementu samotného.</span><span class="sxs-lookup"><span data-stu-id="aaba2-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="aaba2-386">**Elementu EntitySetMapping** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-387">Element EntityTypeMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="aaba2-388">Zobrazení QueryView (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="aaba2-389">MappingFragment (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-390">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-390">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-391">Následující tabulka popisuje atributy, které mohou být použity **elementu EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="aaba2-392">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-392">Attribute Name</span></span>           | <span data-ttu-id="aaba2-393">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-393">Is Required</span></span> | <span data-ttu-id="aaba2-394">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-395">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="aaba2-395">**Name**</span></span>                 | <span data-ttu-id="aaba2-396">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-396">Yes</span></span>         | <span data-ttu-id="aaba2-397">Název sady entit konceptuální model, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="aaba2-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="aaba2-398">**TypeName** **1**</span></span>       | <span data-ttu-id="aaba2-399">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-399">No</span></span>          | <span data-ttu-id="aaba2-400">Název typu koncepčního modelu entity, které je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="aaba2-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="aaba2-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="aaba2-402">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-402">No</span></span>          | <span data-ttu-id="aaba2-403">Název sady entit úložiště modelu, který je mapován na.</span><span class="sxs-lookup"><span data-stu-id="aaba2-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="aaba2-404">**Makecolumnsdistinct nastaveným**</span><span class="sxs-lookup"><span data-stu-id="aaba2-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="aaba2-405">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-405">No</span></span>          | <span data-ttu-id="aaba2-406">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli jsou vráceny pouze jedinečné sloupce.</span><span class="sxs-lookup"><span data-stu-id="aaba2-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="aaba2-407">Pokud tento atribut je nastaven na **True**, **GenerateUpdateViews** atribut elementu EntityContainerMapping musí být nastaven na **False**.</span><span class="sxs-lookup"><span data-stu-id="aaba2-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="aaba2-408">**1** **TypeName** a **StoreEntitySet** atributy lze použít místo podřízené prvky element EntityTypeMapping a MappingFragment pro mapování typu jedné entity k jedné tabulky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="aaba2-409">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-409">Example</span></span>

<span data-ttu-id="aaba2-410">Následující příklad ukazuje **elementu EntitySetMapping** element, který mapuje tři typy (základní typ a dvou odvozených typů) v **kurzy** sadu entit v konceptuálním modelu na třech různých tabulkách v podkladová databáze.</span><span class="sxs-lookup"><span data-stu-id="aaba2-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="aaba2-411">Tabulky jsou určeny **StoreEntitySet** atributy v každém **MappingFragment** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="aaba2-412">Element EntityTypeMapping – Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-413">**Mapování EntityTypeMapping** prvek v mapování specification language (MSL) definuje mapování mezi typem entity v konceptuálním modelu a tabulky a zobrazení v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="aaba2-414">Informace o koncepčního modelu typy entit a podkladové databázové tabulky nebo zobrazení najdete v tématu Element EntityType (CSDL) a objekt EntitySet. Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="aaba2-415">Je určená koncepčního modelu typ entity, která je mapovaná **TypeName** atribut **mapování EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="aaba2-416">Je určená tabulky nebo zobrazení, které je mapován **StoreEntitySet** atribut MappingFragment podřízeného prvku.</span><span class="sxs-lookup"><span data-stu-id="aaba2-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="aaba2-417">ModificationFunctionMapping podřízený element se dá použít k mapování vložení, aktualizace a odstranění funkce typy entit na uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="aaba2-418">**Mapování EntityTypeMapping** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-419">MappingFragment (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="aaba2-420">ModificationFunctionMapping (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="aaba2-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="aaba2-421">ScalarProperty</span></span>
-   <span data-ttu-id="aaba2-422">Podmínka</span><span class="sxs-lookup"><span data-stu-id="aaba2-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-423">**MappingFragment** a **ModificationFunctionMapping** prvky nemůže být podřízené prvky **mapování EntityTypeMapping** element ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="aaba2-424">**ScalarProperty** a **podmínku** prvků může být pouze podřízené prvky **mapování EntityTypeMapping** prvku, když se používá v rámci elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="aaba2-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-425">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-425">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-426">Následující tabulka popisuje atributy, které mohou být použity **mapování EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="aaba2-427">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-427">Attribute Name</span></span> | <span data-ttu-id="aaba2-428">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-428">Is Required</span></span> | <span data-ttu-id="aaba2-429">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-430">**Název typu**</span><span class="sxs-lookup"><span data-stu-id="aaba2-430">**TypeName**</span></span>   | <span data-ttu-id="aaba2-431">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-431">Yes</span></span>         | <span data-ttu-id="aaba2-432">Název typu koncepčního modelu entity, které je mapován kvalifikovaný v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="aaba2-433">Pokud je typ abstract nebo odvozeného typu, musí být hodnota `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="aaba2-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-434">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-434">Example</span></span>

<span data-ttu-id="aaba2-435">Následující příklad ukazuje element EntitySetMapping se dva podřízené **mapování EntityTypeMapping** elementy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="aaba2-436">V prvním **mapování EntityTypeMapping** elementu, **SchoolModel.Person** typ entity je namapována na **osoba** tabulky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="aaba2-437">Ve druhém **mapování EntityTypeMapping** prvek, funkce aktualizace objektu **SchoolModel.Person** typ je mapována na uloženou proceduru **UpdatePerson**, v databázi .</span><span class="sxs-lookup"><span data-stu-id="aaba2-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="aaba2-438">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-438">Example</span></span>

<span data-ttu-id="aaba2-439">Další příklad ukazuje mapování hierarchie typů, ve kterém je kořenový typ abstraktní.</span><span class="sxs-lookup"><span data-stu-id="aaba2-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="aaba2-440">Všimněte si použití `IsOfType` syntaxe **TypeName** atributy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="aaba2-441">Element FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-442">**FunctionImportMapping** prvek v mapování specification language (MSL) definuje mapování mezi importované funkce v konceptuálním modelu a uložené procedury nebo funkce v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="aaba2-443">Importované funkce musí být deklarována v konceptuálním modelu a uložených procedur musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="aaba2-444">Další informace najdete v elementu FunctionImport (CSDL) a funkce – Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-445">Ve výchozím nastavení Pokud importované funkce vrátí typ koncepčního modelu entity nebo komplexní typ, pak názvy sloupců vrácený základní uložené procedury musí přesně shodovat s názvy vlastností v typu koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="aaba2-446">Pokud se názvy sloupců přesně shodovat s názvy vlastností, mapování musí být definován v elementu ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="aaba2-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="aaba2-447">**FunctionImportMapping** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-448">ResultMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-449">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-449">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-450">Následující tabulka popisuje atributy, které se vztahují na **FunctionImportMapping** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="aaba2-451">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-451">Attribute Name</span></span>         | <span data-ttu-id="aaba2-452">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-452">Is Required</span></span> | <span data-ttu-id="aaba2-453">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="aaba2-454">**FunctionImportName**</span></span> | <span data-ttu-id="aaba2-455">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-455">Yes</span></span>         | <span data-ttu-id="aaba2-456">Název funkce importu v konceptuálním modelu, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="aaba2-457">**functionName**</span><span class="sxs-lookup"><span data-stu-id="aaba2-457">**FunctionName**</span></span>       | <span data-ttu-id="aaba2-458">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-458">Yes</span></span>         | <span data-ttu-id="aaba2-459">Název funkce v rámci modelu úložiště, které je mapován kvalifikovaný v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-460">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-460">Example</span></span>

<span data-ttu-id="aaba2-461">Následující příklad je založen na modelu školy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-461">The following example is based on the School model.</span></span> <span data-ttu-id="aaba2-462">Vezměte v úvahu následující funkce v rámci modelu úložiště:</span><span class="sxs-lookup"><span data-stu-id="aaba2-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="aaba2-463">Zvažte také toto importované funkce v konceptuálním modelu:</span><span class="sxs-lookup"><span data-stu-id="aaba2-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="aaba2-464">Následující příklad show **FunctionImportMapping** element sloužící ke zmapování funkci a funkce import nad sebou:</span><span class="sxs-lookup"><span data-stu-id="aaba2-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="aaba2-465">Element InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="aaba2-466">**InsertFunction** prvek v mapování specification language (MSL) se mapuje na uloženou proceduru v podkladové databázi vložit – funkce typu entity nebo přidružení v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="aaba2-467">Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="aaba2-468">Další informace najdete v tématu funkce – Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-469">Pokud nejsou mapovány všechny tři vložení, aktualizace nebo odstranění operace typu entity na uložené procedury nenamapované operace se nezdaří, pokud je proveden za běhu a UpdateException je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="aaba2-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="aaba2-470">**InsertFunction** element může být podřízený ModificationFunctionMapping element a používá se pro element EntityTypeMapping element nebo AssociationSetMapping element.</span><span class="sxs-lookup"><span data-stu-id="aaba2-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="aaba2-471">Použít pro mapování EntityTypeMapping InsertFunction</span><span class="sxs-lookup"><span data-stu-id="aaba2-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="aaba2-472">Při použití elementu mapování EntityTypeMapping **InsertFunction** element mapuje na uložené procedury insert – funkce typu entity v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="aaba2-473">**InsertFunction** prvek může mít následující podřízené prvky při použití **mapování EntityTypeMapping** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="aaba2-474">AssociationEnd (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="aaba2-475">ComplexProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="aaba2-476">ResultBinding (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="aaba2-477">ScarlarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-478">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-478">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-479">Následující tabulka popisuje atributy, které mohou být použity **InsertFunction** element při použití **mapování EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="aaba2-480">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-480">Attribute Name</span></span>            | <span data-ttu-id="aaba2-481">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-481">Is Required</span></span> | <span data-ttu-id="aaba2-482">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-483">**functionName**</span><span class="sxs-lookup"><span data-stu-id="aaba2-483">**FunctionName**</span></span>          | <span data-ttu-id="aaba2-484">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-484">Yes</span></span>         | <span data-ttu-id="aaba2-485">Název uložené procedury, ke které je mapován funkce Vložit kvalifikovaný v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="aaba2-486">Uložená procedura musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="aaba2-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="aaba2-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="aaba2-488">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-488">No</span></span>          | <span data-ttu-id="aaba2-489">Název výstupní parametr, který vrací počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="aaba2-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="aaba2-490">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-490">Example</span></span>

<span data-ttu-id="aaba2-491">Následující příklad je založen na modelu školy a ukazuje **InsertFunction** element slouží k vložení funkce osobu typu entity k mapování **InsertPerson** uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="aaba2-492">**InsertPerson** uložené procedury je deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="aaba2-493">U AssociationSetMapping InsertFunction</span><span class="sxs-lookup"><span data-stu-id="aaba2-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="aaba2-494">Při použití elementu AssociationSetMapping **InsertFunction** prvek mapuje funkce Vložit asociace v konceptuálním modelu na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="aaba2-495">**InsertFunction** prvek může mít následující podřízené prvky při použití **AssociationSetMapping** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="aaba2-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="aaba2-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-497">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-497">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-498">Následující tabulka popisuje atributy, které mohou být použity **InsertFunction** prvku, když se použije k **AssociationSetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="aaba2-499">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-499">Attribute Name</span></span>            | <span data-ttu-id="aaba2-500">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-500">Is Required</span></span> | <span data-ttu-id="aaba2-501">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-502">**functionName**</span><span class="sxs-lookup"><span data-stu-id="aaba2-502">**FunctionName**</span></span>          | <span data-ttu-id="aaba2-503">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-503">Yes</span></span>         | <span data-ttu-id="aaba2-504">Název uložené procedury, ke které je mapován funkce Vložit kvalifikovaný v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="aaba2-505">Uložená procedura musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="aaba2-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="aaba2-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="aaba2-507">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-507">No</span></span>          | <span data-ttu-id="aaba2-508">Název výstupní parametr, který vrací počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="aaba2-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="aaba2-509">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-509">Example</span></span>

<span data-ttu-id="aaba2-510">Následující příklad je založen na modelu školy a ukazuje **InsertFunction** element slouží k mapování Vložit funkci **CourseInstructor** přidružení, které  **InsertCourseInstructor** uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="aaba2-511">**InsertCourseInstructor** uložené procedury je deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a><span data-ttu-id="aaba2-512">Element Mapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-513">**Mapování** element v mapování specification language (MSL) obsahuje informace o mapování objektů, které jsou definované v konceptuálním modelu na databázi (jak je popsáno v modelu úložiště).</span><span class="sxs-lookup"><span data-stu-id="aaba2-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="aaba2-514">Další informace najdete v tématu Specifikace CSDL a specifikace SSDL.</span><span class="sxs-lookup"><span data-stu-id="aaba2-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="aaba2-515">**Mapování** prvek je kořenovým prvkem pro určení mapování.</span><span class="sxs-lookup"><span data-stu-id="aaba2-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="aaba2-516">Obor názvů XML pro mapování specifikace je http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="aaba2-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="aaba2-517">Mapping element může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="aaba2-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="aaba2-518">Alias (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="aaba2-519">Mapování EntityContainerMapping (právě jeden)</span><span class="sxs-lookup"><span data-stu-id="aaba2-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="aaba2-520">Koncepční názvy a typy modelu úložiště, které jsou odkazovány v MSL musí být určeny podle jejich příslušných oborem názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="aaba2-521">Informace týkající se názvu oboru názvů konceptuálního modelu naleznete v tématu Element schématu (CSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="aaba2-522">Informace o názvu oboru názvů modelu úložiště najdete v tématu Element schématu (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="aaba2-523">Aliasy pro obory názvů, které se používají v MSL lze definovat pomocí prvku Alias.</span><span class="sxs-lookup"><span data-stu-id="aaba2-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-524">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-524">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-525">Následující tabulka popisuje atributy, které mohou být použity **mapování** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="aaba2-526">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-526">Attribute Name</span></span> | <span data-ttu-id="aaba2-527">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-527">Is Required</span></span> | <span data-ttu-id="aaba2-528">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="aaba2-529">**místo**</span><span class="sxs-lookup"><span data-stu-id="aaba2-529">**Space**</span></span>      | <span data-ttu-id="aaba2-530">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-530">Yes</span></span>         | <span data-ttu-id="aaba2-531">**C S**.</span><span class="sxs-lookup"><span data-stu-id="aaba2-531">**C-S**.</span></span> <span data-ttu-id="aaba2-532">To je pevná a nedá se změnit.</span><span class="sxs-lookup"><span data-stu-id="aaba2-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-533">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-533">Example</span></span>

<span data-ttu-id="aaba2-534">Následující příklad ukazuje **mapování** element, který je založen na součást modelu školy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="aaba2-535">Další informace o modelu školy najdete v článku rychlý start (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="aaba2-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="aaba2-536">Element MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="aaba2-537">**MappingFragment** prvek v mapování specification language (MSL) definuje mapování mezi vlastnostmi typu entity koncepčního modelu a tabulky nebo zobrazení v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="aaba2-538">Informace o koncepčního modelu typy entit a podkladové databázové tabulky nebo zobrazení najdete v tématu Element EntityType (CSDL) a objekt EntitySet. Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="aaba2-539">**MappingFragment** může být podřízený element elementu element EntityTypeMapping nebo EntitySetMapping element.</span><span class="sxs-lookup"><span data-stu-id="aaba2-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="aaba2-540">**MappingFragment** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-541">Typ ComplexType (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="aaba2-542">ScalarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="aaba2-543">Podmínka (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-544">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-544">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-545">Následující tabulka popisuje atributy, které mohou být použity **MappingFragment** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="aaba2-546">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-546">Attribute Name</span></span>          | <span data-ttu-id="aaba2-547">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-547">Is Required</span></span> | <span data-ttu-id="aaba2-548">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="aaba2-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="aaba2-550">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-550">Yes</span></span>         | <span data-ttu-id="aaba2-551">Název tabulky nebo zobrazení, které je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="aaba2-552">**Makecolumnsdistinct nastaveným**</span><span class="sxs-lookup"><span data-stu-id="aaba2-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="aaba2-553">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-553">No</span></span>          | <span data-ttu-id="aaba2-554">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli jsou vráceny pouze jedinečné sloupce.</span><span class="sxs-lookup"><span data-stu-id="aaba2-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="aaba2-555">Pokud tento atribut je nastaven na **True**, **GenerateUpdateViews** atribut elementu EntityContainerMapping musí být nastaven na **False**.</span><span class="sxs-lookup"><span data-stu-id="aaba2-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-556">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-556">Example</span></span>

<span data-ttu-id="aaba2-557">Následující příklad ukazuje **MappingFragment** jako podřízený element **mapování EntityTypeMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="aaba2-558">V tomto příkladu vlastnosti **kurzu** typ v konceptuálním modelu jsou mapovány na sloupce **kurzu** tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="aaba2-559">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-559">Example</span></span>

<span data-ttu-id="aaba2-560">Následující příklad ukazuje **MappingFragment** jako podřízený element **elementu EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="aaba2-561">Stejně jako v příkladu výše, vlastnosti **kurzu** typ v konceptuálním modelu jsou mapovány na sloupce **kurzu** tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="aaba2-562">Element ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-563">**ModificationFunctionMapping** prvek v mapování specification language (MSL) mapuje vložit, aktualizovat a odstranit funkce entity typu koncepčního modelu pro uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="aaba2-564">**ModificationFunctionMapping** element můžete také namapovat insert a odstranit funkce pro přidružení many-to-many v konceptuálním modelu na uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="aaba2-565">Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="aaba2-566">Další informace najdete v tématu funkce – Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-567">Pokud nejsou mapovány všechny tři vložení, aktualizace nebo odstranění operace typu entity na uložené procedury nenamapované operace se nezdaří, pokud je proveden za běhu a UpdateException je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="aaba2-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="aaba2-568">Pokud jsou funkcí změny jedné entity v hierarchii dědičnosti namapovaných na uložené procedury, funkce úprav pro všechny typy v hierarchii musí být namapována na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="aaba2-569">**ModificationFunctionMapping** element může být podřízený element EntityTypeMapping element nebo AssociationSetMapping element.</span><span class="sxs-lookup"><span data-stu-id="aaba2-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="aaba2-570">**ModificationFunctionMapping** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-571">DeleteFunction (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="aaba2-572">InsertFunction (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="aaba2-573">UpdateFunction (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="aaba2-574">Žádné atributy se vztahují na **ModificationFunctionMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="aaba2-575">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-575">Example</span></span>

<span data-ttu-id="aaba2-576">Následující příklad ukazuje mapování pro sadu entit **lidé** sadu entit v modelu školy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="aaba2-577">Kromě mapování sloupců pro **osoba** typ entity, mapování vložení, aktualizace a odstranění funkce **osoba** typu jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="aaba2-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="aaba2-578">Funkce, které jsou mapovány na jsou deklarovány v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-578">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="aaba2-579">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-579">Example</span></span>

<span data-ttu-id="aaba2-580">Následující příklad ukazuje sada mapování pro přidružení **CourseInstructor** přidružení v modelu školy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="aaba2-581">Kromě mapování sloupců pro **CourseInstructor** přidružení, mapování funkce insert a delete **CourseInstructor** přidružení se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="aaba2-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="aaba2-582">Funkce, které jsou mapovány na jsou deklarovány v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-582">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="aaba2-583">Element QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="aaba2-584">**Zobrazení QueryView** prvek v mapování specification language (MSL) definuje jen pro čtení mapování mezi typu entity nebo přidružení v konceptuálním modelu a tabulky v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="aaba2-585">Je definována mapování pomocí dotazu Entity SQL, který je vyhodnocován pro model úložiště a express sady výsledků z hlediska entity nebo přidružení v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="aaba2-586">Vzhledem k tomu dotazů zobrazení jen pro čtení, nelze použít standardní aktualizace příkazy aktualizovat typy, které jsou definovány v zobrazení dotazu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="aaba2-587">Tyto typy můžete provádět aktualizace pomocí funkcí změny.</span><span class="sxs-lookup"><span data-stu-id="aaba2-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="aaba2-588">Další informace najdete v tématu Postupy: mapování funkcí změny na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-589">V **zobrazení QueryView** elementu, Entity SQL výrazy, které obsahují **GroupBy**, agregace skupiny nebo vlastnosti navigace nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="aaba2-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="aaba2-590">**Zobrazení QueryView** element může být podřízený element elementu EntitySetMapping nebo AssociationSetMapping elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="aaba2-591">V případě předchozího zobrazení dotazu definuje mapování určená jen pro čtení pro entitu v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="aaba2-592">V takovém případě definuje zobrazení dotazu jen pro čtení mapování pro přidružení v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-593">Pokud **AssociationSetMapping** prvek je pro přidružení s referenčním omezením **AssociationSetMapping** prvek je ignorován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="aaba2-594">Další informace najdete v elementu ReferentialConstraint – Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="aaba2-595">**Zobrazení QueryView** element nemůže mít žádné podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-596">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-596">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-597">Následující tabulka popisuje atributy, které mohou být použity **zobrazení QueryView** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="aaba2-598">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-598">Attribute Name</span></span> | <span data-ttu-id="aaba2-599">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-599">Is Required</span></span> | <span data-ttu-id="aaba2-600">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-601">**Název typu**</span><span class="sxs-lookup"><span data-stu-id="aaba2-601">**TypeName**</span></span>   | <span data-ttu-id="aaba2-602">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-602">No</span></span>          | <span data-ttu-id="aaba2-603">Název typu koncepčního modelu, která je mapovaná zobrazení dotazu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-604">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-604">Example</span></span>

<span data-ttu-id="aaba2-605">Následující příklad ukazuje **zobrazení QueryView** jako podřízený element **elementu EntitySetMapping** elementu a definuje mapování zobrazení dotazu pro **oddělení** typ entity Model školy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

<span data-ttu-id="aaba2-606">Vzhledem k tomu, že dotaz vrací pouze podmnožinu členů **oddělení** typu v rámci modelu úložiště **oddělení** typ v modelu školní byla změněna podle tato mapování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="aaba2-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a><span data-ttu-id="aaba2-607">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-607">Example</span></span>

<span data-ttu-id="aaba2-608">Další příklad ukazuje **zobrazení QueryView** jako podřízený element **AssociationSetMapping** elementu a definuje mapování určená jen pro čtení pro `FK_Course_Department` přidružení v modelu školy.</span><span class="sxs-lookup"><span data-stu-id="aaba2-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a><span data-ttu-id="aaba2-609">Komentáře</span><span class="sxs-lookup"><span data-stu-id="aaba2-609">Comments</span></span>

<span data-ttu-id="aaba2-610">Můžete definovat dotaz zobrazení podporují následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="aaba2-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="aaba2-611">Definování entity v konceptuálním modelu, který neobsahuje všechny vlastnosti entity v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="aaba2-612">To zahrnuje vlastnosti, které nemají výchozí hodnoty a nepodporují **null** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="aaba2-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="aaba2-613">Vypočítané sloupce v rámci modelu úložiště mapování na vlastnosti typů entit v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="aaba2-614">Definujte mapování, kde nejsou podmínek použitých ke oddílu entit v konceptuálním modelu založené na rovnost.</span><span class="sxs-lookup"><span data-stu-id="aaba2-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="aaba2-615">Při zadání podmíněné mapování pomocí **podmínku** elementu zadaných podmínek musí být roven zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="aaba2-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="aaba2-616">Další informace najdete v tématu podmínku – Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="aaba2-617">Stejný sloupec v modelu úložiště přiřadit více typů v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="aaba2-618">Mapovat více typů do stejné tabulky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="aaba2-619">Definice asociací v konceptuálním modelu, která nejsou založena na cizí klíče v relační schéma.</span><span class="sxs-lookup"><span data-stu-id="aaba2-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="aaba2-620">Pomocí vlastní obchodní logiky můžete nastavit hodnoty vlastností v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="aaba2-621">Například můžete namapovat řetězcovou hodnotu "T" ve zdroji dat na hodnotu **true**, logická hodnota, v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="aaba2-622">Definujte Podmíněné filtry pro výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="aaba2-623">Vynuťte méně omezení na data v konceptuálním modelu než v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="aaba2-624">Například můžete dokonce vytvářet vlastnost v konceptuálním modelu s možnou hodnotou Null i v případě, že sloupec, ke které je mapován nepodporuje **null**hodnoty.</span><span class="sxs-lookup"><span data-stu-id="aaba2-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="aaba2-625">Při definování zobrazení dotazu pro entity, platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="aaba2-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="aaba2-626">Zobrazení dotazu jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="aaba2-626">Query views are read-only.</span></span> <span data-ttu-id="aaba2-627">Aktualizace k entitám lze vytvořit pouze pomocí funkcí změny.</span><span class="sxs-lookup"><span data-stu-id="aaba2-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="aaba2-628">Při definování typu entity v zobrazení dotazu, musíte také definovat všechny související entity v zobrazení dotazu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="aaba2-629">Při mapování many-to-many přidružení do entity v modelu úložiště, který představuje odkaz tabulky v relační schéma, je nutné definovat **zobrazení QueryView** prvek **AssociationSetMapping** – element pro tuto tabulku spojení.</span><span class="sxs-lookup"><span data-stu-id="aaba2-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="aaba2-630">Zobrazení dotazu musí být definován pro všechny typy v hierarchii typu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="aaba2-631">Můžete to udělat následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="aaba2-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="aaba2-632">Pomocí jediného **zobrazení QueryView** prvek, který určuje jeden dotaz Entity SQL, která vrátí sjednocení všechny typy entit v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="aaba2-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="aaba2-633">Pomocí jediného **zobrazení QueryView** prvek, který určuje jeden dotaz Entity SQL, které používá operátor velikosti PÍSMEN pro návratový typ konkrétní entitu v hierarchii na základě konkrétní podmínky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="aaba2-634">Ještě **zobrazení QueryView** – element pro konkrétní typ v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="aaba2-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="aaba2-635">V takovém případě použijte **TypeName** atribut **zobrazení QueryView** elementu zadali typ entity pro každé zobrazení.</span><span class="sxs-lookup"><span data-stu-id="aaba2-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="aaba2-636">Při definování zobrazení dotazu nelze zadat **StorageSetName** atribut na **elementu EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="aaba2-637">Při definování zobrazení dotazu **elementu EntitySetMapping**element nemůže obsahovat také **vlastnost** mapování.</span><span class="sxs-lookup"><span data-stu-id="aaba2-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="aaba2-638">Element ResultBinding (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="aaba2-639">**ResultBinding** mapuje hodnoty sloupců, které jsou vráceny pomocí uložené procedury vlastností entity v konceptuálním modelu při funkcí změny typu entity se mapují do uložené prvek v mapování specification language (MSL) postupy v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="aaba2-640">Například pokud je vrácena hodnota sloupce identity insert uložené procedury, **ResultBinding** prvek mapuje na vlastnost typu entity v konceptuálním modelu vrácené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="aaba2-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="aaba2-641">**ResultBinding** element může být podřízený InsertFunction element nebo UpdateFunction element.</span><span class="sxs-lookup"><span data-stu-id="aaba2-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="aaba2-642">**ResultBinding** element nemůže mít žádné podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-643">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-643">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-644">Následující tabulka popisuje atributy, které se vztahují na **ResultBinding** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="aaba2-645">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-645">Attribute Name</span></span> | <span data-ttu-id="aaba2-646">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-646">Is Required</span></span> | <span data-ttu-id="aaba2-647">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-648">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="aaba2-648">**Name**</span></span>       | <span data-ttu-id="aaba2-649">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-649">Yes</span></span>         | <span data-ttu-id="aaba2-650">Název vlastnosti entity v konceptuálním modelu, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="aaba2-651">**Názevsloupce**</span><span class="sxs-lookup"><span data-stu-id="aaba2-651">**ColumnName**</span></span> | <span data-ttu-id="aaba2-652">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-652">Yes</span></span>         | <span data-ttu-id="aaba2-653">Název sloupce mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="aaba2-654">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-654">Example</span></span>

<span data-ttu-id="aaba2-655">Následující příklad je založen na modelu školy a ukazuje **InsertFunction** element slouží k mapování funkce Vložit **osoba** typu entity na **InsertPerson** uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="aaba2-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="aaba2-656">( **InsertPerson** úložnou proceduru jsou uvedené níže a je deklarována v rámci modelu úložiště.) A **ResultBinding** element slouží k mapování hodnotu sloupce, který je vrácen uložené procedury (**NewPersonID**) na vlastnost typ entity (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="aaba2-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

<span data-ttu-id="aaba2-657">Popisuje následující příkaz jazyka Transact-SQL **InsertPerson** uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="aaba2-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a><span data-ttu-id="aaba2-658">Element ResultMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="aaba2-659">**ResultMapping** prvek v mapování specification language (MSL) definuje mapování importované funkce v konceptuálním modelu a uložené procedury v podkladové databázi, pokud jsou splněny následující:</span><span class="sxs-lookup"><span data-stu-id="aaba2-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="aaba2-660">Importovaná funkce vrátí typ koncepčního modelu entity nebo komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="aaba2-661">Názvy sloupců vrácený uloženou proceduru přesně shodovat s názvy vlastností na typ entity nebo komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="aaba2-662">Ve výchozím nastavení mapování sloupců vrácený uložené procedury a typu entity nebo komplexní typ je založen na názvy sloupců a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="aaba2-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="aaba2-663">Pokud se názvy sloupců přesně shodovat s názvy vlastností, je nutné použít **ResultMapping** elementu k definování mapování.</span><span class="sxs-lookup"><span data-stu-id="aaba2-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="aaba2-664">Příklad výchozí mapování naleznete v tématu FunctionImportMapping – Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="aaba2-665">**ResultMapping** element je podřízeným prvkem elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="aaba2-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="aaba2-666">**ResultMapping** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-667">Element EntityTypeMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="aaba2-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="aaba2-668">ComplexTypeMapping</span></span>

<span data-ttu-id="aaba2-669">Žádné atributy se vztahují na **ResultMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="aaba2-670">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-670">Example</span></span>

<span data-ttu-id="aaba2-671">Vezměte v úvahu následující uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="aaba2-671">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="aaba2-672">Zvažte také následující entity typu koncepčního modelu:</span><span class="sxs-lookup"><span data-stu-id="aaba2-672">Also consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

<span data-ttu-id="aaba2-673">Chcete-li vytvořit importované funkce, který vrátí instance předchozí typ entity, mapování sloupců vrácený uložené procedury a typu entity musí být definován v **ResultMapping** element:</span><span class="sxs-lookup"><span data-stu-id="aaba2-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="aaba2-674">Element ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="aaba2-675">**ScalarProperty** prvek v mapování specification language (MSL) se mapuje na sloupce tabulky nebo parametr uložené procedury v podkladové databázi vlastnost v typu koncepčního modelu entity, komplexního typu nebo přidružení.</span><span class="sxs-lookup"><span data-stu-id="aaba2-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="aaba2-676">Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="aaba2-677">Další informace najdete v tématu funkce – Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="aaba2-678">**ScalarProperty** element může být podřízená následující prvky:</span><span class="sxs-lookup"><span data-stu-id="aaba2-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="aaba2-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="aaba2-679">MappingFragment</span></span>
-   <span data-ttu-id="aaba2-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="aaba2-680">InsertFunction</span></span>
-   <span data-ttu-id="aaba2-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="aaba2-681">UpdateFunction</span></span>
-   <span data-ttu-id="aaba2-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="aaba2-682">DeleteFunction</span></span>
-   <span data-ttu-id="aaba2-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="aaba2-683">EndProperty</span></span>
-   <span data-ttu-id="aaba2-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="aaba2-684">ComplexProperty</span></span>
-   <span data-ttu-id="aaba2-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="aaba2-685">ResultMapping</span></span>

<span data-ttu-id="aaba2-686">Jako podřízený objekt **MappingFragment**, **ComplexProperty**, nebo **EndProperty** elementu, **ScalarProperty** element mapy vlastností v konceptuálním modelu na sloupec v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="aaba2-687">Jako podřízený objekt **InsertFunction**, **UpdateFunction**, nebo **DeleteFunction** elementu, **ScalarProperty** element mapy vlastností v konceptuálním modelu k parametru uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="aaba2-688">**ScalarProperty** element nemůže mít žádné podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-689">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-689">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-690">Atributy, které se vztahují **ScalarProperty** element se liší v závislosti na roli elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="aaba2-691">Následující tabulka popisuje atributy, které se dají použít při **ScalarProperty** element slouží k mapování vlastností koncepčního modelu na sloupec v databázi:</span><span class="sxs-lookup"><span data-stu-id="aaba2-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="aaba2-692">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-692">Attribute Name</span></span> | <span data-ttu-id="aaba2-693">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-693">Is Required</span></span> | <span data-ttu-id="aaba2-694">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="aaba2-695">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="aaba2-695">**Name**</span></span>       | <span data-ttu-id="aaba2-696">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-696">Yes</span></span>         | <span data-ttu-id="aaba2-697">Název vlastnosti konceptuální model, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="aaba2-698">**Názevsloupce**</span><span class="sxs-lookup"><span data-stu-id="aaba2-698">**ColumnName**</span></span> | <span data-ttu-id="aaba2-699">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-699">Yes</span></span>         | <span data-ttu-id="aaba2-700">Název sloupce tabulky, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="aaba2-701">Následující tabulka popisuje atributy, které se vztahují na **ScalarProperty** prvku, když se používá k mapování vlastností koncepčního modelu k parametru uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="aaba2-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="aaba2-702">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-702">Attribute Name</span></span>    | <span data-ttu-id="aaba2-703">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-703">Is Required</span></span> | <span data-ttu-id="aaba2-704">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-705">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="aaba2-705">**Name**</span></span>          | <span data-ttu-id="aaba2-706">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-706">Yes</span></span>         | <span data-ttu-id="aaba2-707">Název vlastnosti konceptuální model, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="aaba2-708">**Název parametru**</span><span class="sxs-lookup"><span data-stu-id="aaba2-708">**ParameterName**</span></span> | <span data-ttu-id="aaba2-709">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-709">Yes</span></span>         | <span data-ttu-id="aaba2-710">Název parametru, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="aaba2-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="aaba2-711">**Verze**</span><span class="sxs-lookup"><span data-stu-id="aaba2-711">**Version**</span></span>       | <span data-ttu-id="aaba2-712">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-712">No</span></span>          | <span data-ttu-id="aaba2-713">**Aktuální** nebo **původní** v závislosti na tom, zda aktuální hodnotu nebo původní hodnotu vlastnosti by měla sloužit pro řízení souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="aaba2-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="aaba2-714">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-714">Example</span></span>

<span data-ttu-id="aaba2-715">Následující příklad ukazuje **ScalarProperty** element použít dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="aaba2-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="aaba2-716">Mapování vlastností **osoba** typu entity na sloupce **osoba**tabulky.</span><span class="sxs-lookup"><span data-stu-id="aaba2-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="aaba2-717">Mapování vlastností **osoba** typu entity na parametry **UpdatePerson** uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="aaba2-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="aaba2-718">Uložené procedury jsou deklarovány v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-718">The stored procedures are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="aaba2-719">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-719">Example</span></span>

<span data-ttu-id="aaba2-720">Další příklad ukazuje **ScalarProperty** element sloužící ke zmapování insert a delete funkce přidružení konceptuálního modelu na uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="aaba2-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="aaba2-721">Uložené procedury jsou deklarovány v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-721">The stored procedures are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a><span data-ttu-id="aaba2-722">Element UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="aaba2-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="aaba2-723">**UpdateFunction** prvek v mapování specification language (MSL) se mapuje na uloženou proceduru v podkladové databázi aktualizace – funkce typu entity v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="aaba2-724">Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="aaba2-725">Další informace najdete v tématu funkce – Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="aaba2-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="aaba2-726">Pokud nejsou mapovány všechny tři vložení, aktualizace nebo odstranění operace typu entity na uložené procedury nenamapované operace se nezdaří, pokud je proveden za běhu a UpdateException je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="aaba2-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="aaba2-727">**UpdateFunction** element může být podřízený ModificationFunctionMapping element a použity pro mapování EntityTypeMapping prvek.</span><span class="sxs-lookup"><span data-stu-id="aaba2-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="aaba2-728">**UpdateFunction** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="aaba2-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="aaba2-729">AssociationEnd (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="aaba2-730">ComplexProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="aaba2-731">ResultBinding (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="aaba2-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="aaba2-732">ScarlarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="aaba2-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="aaba2-733">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="aaba2-733">Applicable Attributes</span></span>

<span data-ttu-id="aaba2-734">Následující tabulka popisuje atributy, které mohou být použity **UpdateFunction** elementu.</span><span class="sxs-lookup"><span data-stu-id="aaba2-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="aaba2-735">Název atributu</span><span class="sxs-lookup"><span data-stu-id="aaba2-735">Attribute Name</span></span>            | <span data-ttu-id="aaba2-736">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="aaba2-736">Is Required</span></span> | <span data-ttu-id="aaba2-737">Hodnota</span><span class="sxs-lookup"><span data-stu-id="aaba2-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="aaba2-738">**functionName**</span><span class="sxs-lookup"><span data-stu-id="aaba2-738">**FunctionName**</span></span>          | <span data-ttu-id="aaba2-739">Ano</span><span class="sxs-lookup"><span data-stu-id="aaba2-739">Yes</span></span>         | <span data-ttu-id="aaba2-740">Název uložené procedury, ke které je mapován funkce update kvalifikovaný v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="aaba2-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="aaba2-741">Uložená procedura musí být deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="aaba2-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="aaba2-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="aaba2-743">Ne</span><span class="sxs-lookup"><span data-stu-id="aaba2-743">No</span></span>          | <span data-ttu-id="aaba2-744">Název výstupní parametr, který vrací počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="aaba2-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="aaba2-745">Příklad</span><span class="sxs-lookup"><span data-stu-id="aaba2-745">Example</span></span>

<span data-ttu-id="aaba2-746">Následující příklad je založen na modelu školy a ukazuje **UpdateFunction** element slouží k mapování funkce aktualizace **osoba** typu entity na **UpdatePerson** uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="aaba2-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="aaba2-747">**UpdatePerson** uložené procedury je deklarována v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaba2-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
