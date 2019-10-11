---
title: Specifikace MSL – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182558"
---
# <a name="msl-specification"></a><span data-ttu-id="e698c-102">Specifikace MSL</span><span class="sxs-lookup"><span data-stu-id="e698c-102">MSL Specification</span></span>
<span data-ttu-id="e698c-103">Specifikace jazyka MSL (Mapping Specification Language) je jazyk založený na jazyce XML, který popisuje mapování mezi koncepčním modelem a modelem úložiště aplikace Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e698c-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="e698c-104">V Entity Framework aplikaci jsou mapování metadat načtena ze souboru. MSL (napsaného v MSL) v čase sestavení.</span><span class="sxs-lookup"><span data-stu-id="e698c-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="e698c-105">Entity Framework používá mapování metadat za běhu k překladu dotazů na koncepční model pro ukládání specifických příkazů.</span><span class="sxs-lookup"><span data-stu-id="e698c-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="e698c-106">Entity Framework Designer (EF Designer) ukládá informace o mapování v souboru EDMX v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="e698c-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="e698c-107">V době sestavování Entity Designer používá informace v souboru. edmx k vytvoření souboru. MSL, který je vyžadován Entity Framework za běhu.</span><span class="sxs-lookup"><span data-stu-id="e698c-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="e698c-108">Názvy všech typů konceptuálního modelu nebo modelů úložiště, které jsou odkazovány v MSL, musí být kvalifikovány svými příslušnými názvy oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="e698c-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="e698c-109">Informace o názvu oboru názvů konceptuálního modelu naleznete v tématu [specifikace CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="e698c-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="e698c-110">Informace o názvu oboru názvů modelu úložiště najdete v tématu [specifikace SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="e698c-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="e698c-111">Verze MSL jsou odlišeny obory názvů XML.</span><span class="sxs-lookup"><span data-stu-id="e698c-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="e698c-112">Verze MSL</span><span class="sxs-lookup"><span data-stu-id="e698c-112">MSL Version</span></span> | <span data-ttu-id="e698c-113">Obor názvů XML</span><span class="sxs-lookup"><span data-stu-id="e698c-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="e698c-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="e698c-114">MSL v1</span></span>      | <span data-ttu-id="e698c-115">urn: schemas-microsoft-com: Windows: Storage: mapování: CS</span><span class="sxs-lookup"><span data-stu-id="e698c-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="e698c-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="e698c-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="e698c-117">MSL V3</span><span class="sxs-lookup"><span data-stu-id="e698c-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="e698c-118">Alias – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-118">Alias Element (MSL)</span></span>

<span data-ttu-id="e698c-119">Element **alias** v jazyce MSL (Mapping Specification Language) je podřízeným prvkem mapování, který se používá k definování aliasů pro koncepční model a obory názvů modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="e698c-120">Názvy všech typů konceptuálního modelu nebo modelů úložiště, které jsou odkazovány v MSL, musí být kvalifikovány svými příslušnými názvy oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="e698c-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="e698c-121">Informace o názvu oboru názvů konceptuálního modelu naleznete v tématu Schema – element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="e698c-122">Informace o názvu oboru názvů modelu úložiště najdete v tématu schéma – element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="e698c-123">Element **alias** nemůže mít podřízené elementy.</span><span class="sxs-lookup"><span data-stu-id="e698c-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-124">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-124">Applicable Attributes</span></span>

<span data-ttu-id="e698c-125">Následující tabulka popisuje atributy, které lze použít na prvek **alias** .</span><span class="sxs-lookup"><span data-stu-id="e698c-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="e698c-126">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-126">Attribute Name</span></span> | <span data-ttu-id="e698c-127">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-127">Is Required</span></span> | <span data-ttu-id="e698c-128">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="e698c-129">**Klíč**</span><span class="sxs-lookup"><span data-stu-id="e698c-129">**Key**</span></span>        | <span data-ttu-id="e698c-130">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-130">Yes</span></span>         | <span data-ttu-id="e698c-131">Alias pro obor názvů, který je určen atributem **Value** .</span><span class="sxs-lookup"><span data-stu-id="e698c-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="e698c-132">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="e698c-132">**Value**</span></span>      | <span data-ttu-id="e698c-133">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-133">Yes</span></span>         | <span data-ttu-id="e698c-134">Obor názvů, pro který je hodnota **klíčového** prvku alias.</span><span class="sxs-lookup"><span data-stu-id="e698c-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="e698c-135">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-135">Example</span></span>

<span data-ttu-id="e698c-136">Následující příklad ukazuje prvek **alias** definující alias `c` pro typy, které jsou definovány v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a><span data-ttu-id="e698c-137">AssociationEnd – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="e698c-138">Element **AssociationEnd** v jazyce MSL (Mapping Specification Language) se používá, pokud jsou funkce úprav typu entity v koncepčním modelu mapovány na uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="e698c-139">Pokud uložená procedura úpravy převezme parametr, jehož hodnota je uložena ve vlastnosti Association, element **AssociationEnd** mapuje hodnotu vlastnosti na parametr.</span><span class="sxs-lookup"><span data-stu-id="e698c-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="e698c-140">Další informace naleznete v níže uvedeném příkladu.</span><span class="sxs-lookup"><span data-stu-id="e698c-140">For more information, see the example below.</span></span>

<span data-ttu-id="e698c-141">Další informace o mapování funkcí úprav typů entit na uložené procedury naleznete v tématu ModificationFunctionMapping element (MSL) a návod: Mapování entity na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e698c-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="e698c-142">Element **AssociationEnd** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="e698c-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-144">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-144">Applicable Attributes</span></span>

<span data-ttu-id="e698c-145">Následující tabulka popisuje atributy, které se vztahují na element **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="e698c-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="e698c-146">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-146">Attribute Name</span></span>     | <span data-ttu-id="e698c-147">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-147">Is Required</span></span> | <span data-ttu-id="e698c-148">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-149">**Vlastností**</span><span class="sxs-lookup"><span data-stu-id="e698c-149">**AssociationSet**</span></span> | <span data-ttu-id="e698c-150">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-150">Yes</span></span>         | <span data-ttu-id="e698c-151">Název asociace, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="e698c-152">**from**</span><span class="sxs-lookup"><span data-stu-id="e698c-152">**From**</span></span>           | <span data-ttu-id="e698c-153">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-153">Yes</span></span>         | <span data-ttu-id="e698c-154">Hodnota atributu **FromRole** vlastnosti navigace, která odpovídá mapovanému přidružení.</span><span class="sxs-lookup"><span data-stu-id="e698c-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="e698c-155">Další informace naleznete v tématu element NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="e698c-156">**To**</span><span class="sxs-lookup"><span data-stu-id="e698c-156">**To**</span></span>             | <span data-ttu-id="e698c-157">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-157">Yes</span></span>         | <span data-ttu-id="e698c-158">Hodnota atributu **ToRole** vlastnosti navigace, která odpovídá mapovanému přidružení.</span><span class="sxs-lookup"><span data-stu-id="e698c-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="e698c-159">Další informace naleznete v tématu element NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="e698c-160">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-160">Example</span></span>

<span data-ttu-id="e698c-161">Vezměte v úvahu následující typ entity koncepčního modelu:</span><span class="sxs-lookup"><span data-stu-id="e698c-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="e698c-162">Zvažte také následující uloženou proceduru:</span><span class="sxs-lookup"><span data-stu-id="e698c-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="e698c-163">Aby bylo možné namapovat funkci Update entity `Course` na tuto uloženou proceduru, je nutné do parametru **DepartmentID** dodat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e698c-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="e698c-164">Hodnota parametru `DepartmentID` neodpovídá vlastnosti typu entity; je obsažen v nezávislém přidružení, jehož mapování je zobrazeno zde:</span><span class="sxs-lookup"><span data-stu-id="e698c-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="e698c-165">Následující kód ukazuje element **AssociationEnd** , který slouží k mapování vlastnosti **DepartmentID** třídy **FK @ no__t-3Course @ no__t-4Department** na uloženou proceduru **UpdateCourse** (do které funkce Update Typ entity **kurzu** je namapován):</span><span class="sxs-lookup"><span data-stu-id="e698c-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="e698c-166">AssociationSetMapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="e698c-167">Element **AssociationSetMapping** v jazyce MSL (Mapping Specification Language) definuje mapování mezi přidružením v koncepčním modelu a sloupcích tabulky v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="e698c-168">Asociace v koncepčním modelu jsou typy, jejichž vlastnosti představují sloupce primárního a cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="e698c-169">Element **AssociationSetMapping** používá dva elementy endproperty lze mapovat k definování mapování mezi vlastnostmi typu přidružení a sloupci v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="e698c-170">Můžete umístit podmínky na tato mapování s prvkem podmínky.</span><span class="sxs-lookup"><span data-stu-id="e698c-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="e698c-171">Namapujte funkce INSERT, Update a DELETE pro přidružení k uloženým procedurám v databázi pomocí elementu ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="e698c-172">Definujte mapování jen pro čtení mezi přidruženími a sloupci tabulky pomocí řetězce Entity SQL v elementu QueryView.</span><span class="sxs-lookup"><span data-stu-id="e698c-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-173">Pokud je pro přidružení v koncepčním modelu definováno referenční omezení, není nutné přidružení namapovat na element **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="e698c-174">Pokud je k dispozici element **AssociationSetMapping** pro asociaci s referenčním omezením, mapování definovaná v elementu **AssociationSetMapping** budou ignorována.</span><span class="sxs-lookup"><span data-stu-id="e698c-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="e698c-175">Další informace naleznete v tématu elementu ReferentialConstraint element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="e698c-176">Element **AssociationSetMapping** může mít následující podřízené elementy.</span><span class="sxs-lookup"><span data-stu-id="e698c-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="e698c-177">QueryView (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="e698c-178">Endproperty lze mapovat (nula nebo 2)</span><span class="sxs-lookup"><span data-stu-id="e698c-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="e698c-179">Podmínka (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="e698c-180">ModificationFunctionMapping (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-181">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-181">Applicable Attributes</span></span>

<span data-ttu-id="e698c-182">Následující tabulka popisuje atributy, které mohou být aplikovány na element **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="e698c-183">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-183">Attribute Name</span></span>     | <span data-ttu-id="e698c-184">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-184">Is Required</span></span> | <span data-ttu-id="e698c-185">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-186">**Název**</span><span class="sxs-lookup"><span data-stu-id="e698c-186">**Name**</span></span>           | <span data-ttu-id="e698c-187">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-187">Yes</span></span>         | <span data-ttu-id="e698c-188">Název namapované sady přidružení koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="e698c-189">**Popisuje**</span><span class="sxs-lookup"><span data-stu-id="e698c-189">**TypeName**</span></span>       | <span data-ttu-id="e698c-190">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-190">No</span></span>          | <span data-ttu-id="e698c-191">Obor názvů kvalifikovaný název typu přidružení koncepčního modelu, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="e698c-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="e698c-192">**StoreEntitySet**</span></span> | <span data-ttu-id="e698c-193">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-193">No</span></span>          | <span data-ttu-id="e698c-194">Název mapované tabulky.</span><span class="sxs-lookup"><span data-stu-id="e698c-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="e698c-195">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-195">Example</span></span>

<span data-ttu-id="e698c-196">Následující příklad ukazuje element **AssociationSetMapping** , ve kterém je přidružení **FK @ no__t-2Course @ no__t-3Department** namapováno v koncepčním modelu na tabulku **kurzů** v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="e698c-197">Mapování mezi vlastnostmi typu přidružení a sloupci tabulky jsou specifikovány v podřízených elementech **endproperty lze mapovat** .</span><span class="sxs-lookup"><span data-stu-id="e698c-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="e698c-198">ComplexProperty – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="e698c-199">Element **ComplexProperty** v jazyce MSL (Mapping Specification Language) definuje mapování mezi vlastností komplexního typu u typu entity koncepčního modelu a sloupců tabulky v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="e698c-200">Mapování sloupců vlastností je zadáno v podřízených elementech ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="e698c-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="e698c-201">Element vlastnosti **complexType** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-202">ScalarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="e698c-203">**ComplexProperty** (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="e698c-204">ComplextTypeMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="e698c-205">Podmínka (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-206">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-206">Applicable Attributes</span></span>

<span data-ttu-id="e698c-207">Následující tabulka popisuje atributy, které se vztahují na element **ComplexProperty** :</span><span class="sxs-lookup"><span data-stu-id="e698c-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="e698c-208">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-208">Attribute Name</span></span> | <span data-ttu-id="e698c-209">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-209">Is Required</span></span> | <span data-ttu-id="e698c-210">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-211">**Název**</span><span class="sxs-lookup"><span data-stu-id="e698c-211">**Name**</span></span>       | <span data-ttu-id="e698c-212">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-212">Yes</span></span>         | <span data-ttu-id="e698c-213">Název komplexní vlastnosti typu entity v koncepčním modelu, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="e698c-214">**Popisuje**</span><span class="sxs-lookup"><span data-stu-id="e698c-214">**TypeName**</span></span>   | <span data-ttu-id="e698c-215">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-215">No</span></span>          | <span data-ttu-id="e698c-216">Obor názvů kvalifikovaný název typu vlastnosti koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="e698c-217">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-217">Example</span></span>

<span data-ttu-id="e698c-218">Následující příklad je založen na školním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-218">The following example is based on the School model.</span></span> <span data-ttu-id="e698c-219">Do koncepčního modelu byl přidán následující komplexní typ:</span><span class="sxs-lookup"><span data-stu-id="e698c-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="e698c-220">Vlastnosti **LastName** a **FirstName** typu entity **osoba** byly nahrazeny jednou komplexní vlastností, **název**:</span><span class="sxs-lookup"><span data-stu-id="e698c-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="e698c-221">Následující MSL ukazuje element **ComplexProperty** použitý k mapování vlastnosti **Name** na sloupce v podkladové databázi:</span><span class="sxs-lookup"><span data-stu-id="e698c-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="e698c-222">ComplexTypeMapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="e698c-223">Element **ComplexTypeMapping** v jazyce MSL (Mapping Specification Language) je podřízeným prvkem prvku ResultMapping a definuje mapování mezi funkcí importovanou v koncepčním modelu a uloženou procedurou v podkladové databázi, když následující jsou splněné:</span><span class="sxs-lookup"><span data-stu-id="e698c-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="e698c-224">Import funkce vrací koncepční komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="e698c-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="e698c-225">Názvy sloupců vrácené uloženou procedurou neodpovídají přesně názvům vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="e698c-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="e698c-226">Ve výchozím nastavení je mapování mezi sloupci vrácenou uloženou procedurou a komplexním typem založeno na názvech sloupců a vlastností.</span><span class="sxs-lookup"><span data-stu-id="e698c-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="e698c-227">Pokud názvy sloupců neodpovídají přesně názvům vlastností, je nutné k definování mapování použít element **ComplexTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="e698c-228">Příklad výchozího mapování naleznete v tématu FunctionImportMapping element (MSL).</span><span class="sxs-lookup"><span data-stu-id="e698c-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="e698c-229">Element **ComplexTypeMapping** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-230">ScalarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-231">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-231">Applicable Attributes</span></span>

<span data-ttu-id="e698c-232">Následující tabulka popisuje atributy, které se vztahují na element **ComplexTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="e698c-233">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-233">Attribute Name</span></span> | <span data-ttu-id="e698c-234">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-234">Is Required</span></span> | <span data-ttu-id="e698c-235">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="e698c-236">**Popisuje**</span><span class="sxs-lookup"><span data-stu-id="e698c-236">**TypeName**</span></span>   | <span data-ttu-id="e698c-237">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-237">Yes</span></span>         | <span data-ttu-id="e698c-238">Obor názvů kvalifikovaný název komplexního typu, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-239">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-239">Example</span></span>

<span data-ttu-id="e698c-240">Vezměte v úvahu následující uložený postup:</span><span class="sxs-lookup"><span data-stu-id="e698c-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="e698c-241">Zvažte také následující komplexní typ koncepčního modelu:</span><span class="sxs-lookup"><span data-stu-id="e698c-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="e698c-242">Aby bylo možné vytvořit import funkce, který vrací instance předchozího komplexního typu, mapování mezi sloupci vráceným uloženou procedurou a typem entity musí být definováno v elementu **ComplexTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="e698c-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="e698c-243">Condition – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-243">Condition Element (MSL)</span></span>

<span data-ttu-id="e698c-244">Element **Condition** v Mapping Specification Language (MSL) ukládá podmínky pro mapování mezi koncepčním modelem a podkladovou databází.</span><span class="sxs-lookup"><span data-stu-id="e698c-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="e698c-245">Mapování, které je definováno v uzlu XML je platné, pokud jsou splněny všechny podmínky, jak je uvedeno v podřízených prvcích **podmínky** .</span><span class="sxs-lookup"><span data-stu-id="e698c-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="e698c-246">Jinak mapování není platné.</span><span class="sxs-lookup"><span data-stu-id="e698c-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="e698c-247">Například pokud prvek MappingFragment obsahuje jeden nebo více podřízených prvků **podmínky** , mapování definované v rámci uzlu **MappingFragment** bude platné pouze v případě, že jsou splněny všechny podmínky prvků podřízené **podmínky** .</span><span class="sxs-lookup"><span data-stu-id="e698c-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="e698c-248">Každá podmínka se může vztahovat buď na **název** (název vlastnosti entity koncepčního modelu, který je určený atributem **Name** ), nebo na vlastnost **ColumnName** (název sloupce v databázi, který určuje atribut **ColumnName** ).</span><span class="sxs-lookup"><span data-stu-id="e698c-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="e698c-249">Pokud je nastaven atribut **Name** , je podmínka zkontrolována na hodnotu vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="e698c-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="e698c-250">Pokud je nastaven atribut **ColumnName** , je podmínka porovnána s hodnotou sloupce.</span><span class="sxs-lookup"><span data-stu-id="e698c-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="e698c-251">V elementu **podmínky** lze zadat pouze jeden z atributů **Name** nebo **ColumnName** .</span><span class="sxs-lookup"><span data-stu-id="e698c-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-252">Pokud je element **Condition** použit v rámci elementu FunctionImportMapping, není platný pouze atribut **Name** .</span><span class="sxs-lookup"><span data-stu-id="e698c-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="e698c-253">Element **Condition** může být podřízeným prvkem následujících prvků:</span><span class="sxs-lookup"><span data-stu-id="e698c-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="e698c-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="e698c-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="e698c-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="e698c-255">ComplexProperty</span></span>
-   <span data-ttu-id="e698c-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="e698c-256">EntitySetMapping</span></span>
-   <span data-ttu-id="e698c-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="e698c-257">MappingFragment</span></span>
-   <span data-ttu-id="e698c-258">Element entitytypemapping</span><span class="sxs-lookup"><span data-stu-id="e698c-258">EntityTypeMapping</span></span>

<span data-ttu-id="e698c-259">Element **Condition** nemůže mít žádné podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="e698c-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-260">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-260">Applicable Attributes</span></span>

<span data-ttu-id="e698c-261">Následující tabulka popisuje atributy, které se vztahují na prvek **podmínky** :</span><span class="sxs-lookup"><span data-stu-id="e698c-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="e698c-262">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-262">Attribute Name</span></span> | <span data-ttu-id="e698c-263">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-263">Is Required</span></span> | <span data-ttu-id="e698c-264">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="e698c-265">**ColumnName**</span></span> | <span data-ttu-id="e698c-266">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-266">No</span></span>          | <span data-ttu-id="e698c-267">Název sloupce tabulky, jehož hodnota se používá k vyhodnocení podmínky.</span><span class="sxs-lookup"><span data-stu-id="e698c-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="e698c-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="e698c-268">**IsNull**</span></span>     | <span data-ttu-id="e698c-269">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-269">No</span></span>          | <span data-ttu-id="e698c-270">**True** nebo **false**.</span><span class="sxs-lookup"><span data-stu-id="e698c-270">**True** or **False**.</span></span> <span data-ttu-id="e698c-271">Pokud je hodnota **true** a hodnota sloupce je **null**, nebo pokud je hodnota **false** a hodnota sloupce není **null**, podmínka je pravdivá.</span><span class="sxs-lookup"><span data-stu-id="e698c-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="e698c-272">V opačném případě je podmínka nepravdivá.</span><span class="sxs-lookup"><span data-stu-id="e698c-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="e698c-273">Atributy **IsNull** a **Value** nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="e698c-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="e698c-274">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="e698c-274">**Value**</span></span>      | <span data-ttu-id="e698c-275">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-275">No</span></span>          | <span data-ttu-id="e698c-276">Hodnota, se kterou je porovnána hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="e698c-276">The value with which the column value is compared.</span></span> <span data-ttu-id="e698c-277">Pokud jsou hodnoty stejné, je podmínka pravdivá.</span><span class="sxs-lookup"><span data-stu-id="e698c-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="e698c-278">V opačném případě je podmínka nepravdivá.</span><span class="sxs-lookup"><span data-stu-id="e698c-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="e698c-279">Atributy **IsNull** a **Value** nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="e698c-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="e698c-280">**Název**</span><span class="sxs-lookup"><span data-stu-id="e698c-280">**Name**</span></span>       | <span data-ttu-id="e698c-281">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-281">No</span></span>          | <span data-ttu-id="e698c-282">Název vlastnosti entity koncepčního modelu, jejíž hodnota se používá k vyhodnocení podmínky.</span><span class="sxs-lookup"><span data-stu-id="e698c-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="e698c-283">Tento atribut nelze použít, je-li element **Condition** použit v rámci elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="e698c-284">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-284">Example</span></span>

<span data-ttu-id="e698c-285">Následující příklad ukazuje prvky **podmínky** jako podřízené objekty **MappingFragment** prvků.</span><span class="sxs-lookup"><span data-stu-id="e698c-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="e698c-286">Pokud **ZaměstnánOd** není null a **EnrollmentDate** má hodnotu null, jsou data mapována mezi sloupci **SchoolModel. Instructor** a **PersonID** a **ZaměstnánOd** tabulky **Person** .</span><span class="sxs-lookup"><span data-stu-id="e698c-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="e698c-287">Pokud **EnrollmentDate** není null a **ZaměstnánOd** má hodnotu null, data jsou namapována mezi **SchoolModel. student** Type a sloupce **PersonID** a **zápis** v tabulce **Person** .</span><span class="sxs-lookup"><span data-stu-id="e698c-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="e698c-288">DeleteFunction – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="e698c-289">Element **DeleteFunction** v Mapping Specification Language (MSL) mapuje funkci Delete typu entity nebo přidružení v koncepčním modelu na uloženou proceduru v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="e698c-290">Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="e698c-291">Další informace naleznete v tématu Function Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-292">Pokud nemapujete všechny tři operace vložení, aktualizace nebo odstranění typu entity na uložené procedury, nemapované operace selžou při spuštění za běhu a vyvolá se UpdateException.</span><span class="sxs-lookup"><span data-stu-id="e698c-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="e698c-293">DeleteFunction se aplikuje na element entitytypemapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="e698c-294">Při použití na element element entitytypemapping mapuje element **DeleteFunction** funkci Delete typu entity v koncepčním modelu na uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="e698c-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="e698c-295">Element **DeleteFunction** může mít při použití na element **element entitytypemapping** následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="e698c-296">AssociationEnd (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="e698c-297">ComplexProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="e698c-298">ScarlarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e698c-299">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-299">Applicable Attributes</span></span>

<span data-ttu-id="e698c-300">Následující tabulka popisuje atributy, které mohou být aplikovány na element **DeleteFunction** při použití na element **element entitytypemapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="e698c-301">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-301">Attribute Name</span></span>            | <span data-ttu-id="e698c-302">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-302">Is Required</span></span> | <span data-ttu-id="e698c-303">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="e698c-304">**FunctionName**</span></span>          | <span data-ttu-id="e698c-305">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-305">Yes</span></span>         | <span data-ttu-id="e698c-306">Obor názvů kvalifikovaný název uložené procedury, ke které je namapována funkce DELETE.</span><span class="sxs-lookup"><span data-stu-id="e698c-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="e698c-307">Uložená procedura musí být deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="e698c-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="e698c-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="e698c-309">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-309">No</span></span>          | <span data-ttu-id="e698c-310">Název výstupního parametru, který vrátí počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="e698c-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="e698c-311">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-311">Example</span></span>

<span data-ttu-id="e698c-312">Následující příklad je založen na školním modelu a ukazuje element **DeleteFunction** , který mapuje funkci Delete typu entity **Person** na uloženou proceduru **DeletePerson** .</span><span class="sxs-lookup"><span data-stu-id="e698c-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="e698c-313">Uložená procedura **DeletePerson** je deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="e698c-314">DeleteFunction se aplikuje na AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="e698c-315">Při použití na element AssociationSetMapping mapuje element **DeleteFunction** funkci Delete přidružení v koncepčním modelu na uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="e698c-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="e698c-316">Element **DeleteFunction** může mít při použití na element **AssociationSetMapping** následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="e698c-317">Endproperty lze mapovat</span><span class="sxs-lookup"><span data-stu-id="e698c-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e698c-318">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-318">Applicable Attributes</span></span>

<span data-ttu-id="e698c-319">Následující tabulka popisuje atributy, které mohou být aplikovány na element **DeleteFunction** při použití na element **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="e698c-320">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-320">Attribute Name</span></span>            | <span data-ttu-id="e698c-321">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-321">Is Required</span></span> | <span data-ttu-id="e698c-322">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="e698c-323">**FunctionName**</span></span>          | <span data-ttu-id="e698c-324">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-324">Yes</span></span>         | <span data-ttu-id="e698c-325">Obor názvů kvalifikovaný název uložené procedury, ke které je namapována funkce DELETE.</span><span class="sxs-lookup"><span data-stu-id="e698c-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="e698c-326">Uložená procedura musí být deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="e698c-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="e698c-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="e698c-328">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-328">No</span></span>          | <span data-ttu-id="e698c-329">Název výstupního parametru, který vrátí počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="e698c-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="e698c-330">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-330">Example</span></span>

<span data-ttu-id="e698c-331">Následující příklad je založen na školním modelu a ukazuje element **DeleteFunction** , který se používá k mapování funkce Delete **CourseInstructor** přidružení k uložené proceduře **DeleteCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="e698c-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="e698c-332">Uložená procedura **DeleteCourseInstructor** je deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="e698c-333">Endproperty lze mapovat – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="e698c-334">Element **endproperty lze mapovat** v jazyce MSL definuje mapování mezi funkcí end nebo úpravou přidružení koncepčního modelu a podkladové databáze.</span><span class="sxs-lookup"><span data-stu-id="e698c-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="e698c-335">Mapování sloupce vlastnost je určeno v podřízeném elementu ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="e698c-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="e698c-336">Když je použit element **endproperty lze mapovat** k definování mapování pro konec přidružení koncepčního modelu, je podřízeným prvkem AssociationSetMapping elementu.</span><span class="sxs-lookup"><span data-stu-id="e698c-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="e698c-337">Pokud je použit element **endproperty lze mapovat** k definování mapování pro funkci úprav v rámci přidružení koncepčního modelu, jedná se o podřízený prvek InsertFunction elementu nebo elementu DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="e698c-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="e698c-338">Element **endproperty lze mapovat** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-339">ScalarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-340">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-340">Applicable Attributes</span></span>

<span data-ttu-id="e698c-341">Následující tabulka popisuje atributy, které se vztahují na element **endproperty lze mapovat** :</span><span class="sxs-lookup"><span data-stu-id="e698c-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="e698c-342">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-342">Attribute Name</span></span> | <span data-ttu-id="e698c-343">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-343">Is Required</span></span> | <span data-ttu-id="e698c-344">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="e698c-345">Name</span><span class="sxs-lookup"><span data-stu-id="e698c-345">Name</span></span>           | <span data-ttu-id="e698c-346">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-346">Yes</span></span>         | <span data-ttu-id="e698c-347">Název elementu Association, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-348">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-348">Example</span></span>

<span data-ttu-id="e698c-349">Následující příklad ukazuje element **AssociationSetMapping** , ve kterém je přidružení **FK @ no__t-2Course @ no__t-3Department** v koncepčním modelu mapováno na tabulku **kurzů** v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="e698c-350">Mapování mezi vlastnostmi typu přidružení a sloupci tabulky jsou specifikovány v podřízených elementech **endproperty lze mapovat** .</span><span class="sxs-lookup"><span data-stu-id="e698c-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="e698c-351">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-351">Example</span></span>

<span data-ttu-id="e698c-352">Následující příklad ukazuje element **endproperty lze mapovat** mapující funkce INSERT a DELETE elementu Association (**CourseInstructor**) na uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="e698c-353">Funkce, které jsou mapovány na, jsou deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="e698c-354">EntityContainerMapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="e698c-355">Element **EntityContainerMapping** ve službě Mapping Specification Language (MSL) mapuje kontejner entit v koncepčním modelu na kontejner entit v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="e698c-356">Element **EntityContainerMapping** je podřízeným prvkem mapování elementu.</span><span class="sxs-lookup"><span data-stu-id="e698c-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="e698c-357">Element **EntityContainerMapping** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="e698c-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e698c-358">EntitySetMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="e698c-359">AssociationSetMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="e698c-360">FunctionImportMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-361">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-361">Applicable Attributes</span></span>

<span data-ttu-id="e698c-362">Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="e698c-363">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-363">Attribute Name</span></span>            | <span data-ttu-id="e698c-364">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-364">Is Required</span></span> | <span data-ttu-id="e698c-365">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="e698c-366">**StorageModelContainer**</span></span> | <span data-ttu-id="e698c-367">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-367">Yes</span></span>         | <span data-ttu-id="e698c-368">Název mapovaného kontejneru entit modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="e698c-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="e698c-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="e698c-370">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-370">Yes</span></span>         | <span data-ttu-id="e698c-371">Název kontejneru entity koncepčního modelu, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="e698c-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="e698c-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="e698c-373">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-373">No</span></span>          | <span data-ttu-id="e698c-374">**True** nebo **false**.</span><span class="sxs-lookup"><span data-stu-id="e698c-374">**True** or **False**.</span></span> <span data-ttu-id="e698c-375">Pokud je **hodnota false**, nejsou vygenerována žádná zobrazení aktualizací.</span><span class="sxs-lookup"><span data-stu-id="e698c-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="e698c-376">Tento atribut by měl být nastaven na **hodnotu false** , pokud máte mapování jen pro čtení, které by bylo neplatné, protože data se nemusí úspěšně odcyklovat.</span><span class="sxs-lookup"><span data-stu-id="e698c-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="e698c-377">Výchozí hodnota je **true (pravda**).</span><span class="sxs-lookup"><span data-stu-id="e698c-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-378">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-378">Example</span></span>

<span data-ttu-id="e698c-379">Následující příklad ukazuje element **EntityContainerMapping** , který mapuje kontejner **SchoolModelEntities** (kontejner entity koncepčního modelu) do kontejneru **SchoolModelStoreContainer** (entita modelu úložiště kontejner):</span><span class="sxs-lookup"><span data-stu-id="e698c-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="e698c-380">EntitySetMapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="e698c-381">Element **EntitySetMapping** v jazyce MSL (Mapping Specification Language) mapuje všechny typy v koncepčním modelu entit sady na sady entit v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="e698c-382">Sada entit v koncepčním modelu je logický kontejner pro instance entit stejného typu (a odvozené typy).</span><span class="sxs-lookup"><span data-stu-id="e698c-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="e698c-383">Sada entit v modelu úložiště představuje tabulku nebo zobrazení v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="e698c-384">Sada entit koncepčního modelu je určena hodnotou atributu **Name** elementu **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="e698c-385">Mapování na tabulku nebo zobrazení je určené atributem **StoreEntitySet** v každém podřízeném elementu MappingFragment nebo samotném elementu **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="e698c-386">Element **EntitySetMapping** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-387">Element entitytypemapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="e698c-388">QueryView (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="e698c-389">MappingFragment (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-390">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-390">Applicable Attributes</span></span>

<span data-ttu-id="e698c-391">Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="e698c-392">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-392">Attribute Name</span></span>           | <span data-ttu-id="e698c-393">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-393">Is Required</span></span> | <span data-ttu-id="e698c-394">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-395">**Název**</span><span class="sxs-lookup"><span data-stu-id="e698c-395">**Name**</span></span>                 | <span data-ttu-id="e698c-396">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-396">Yes</span></span>         | <span data-ttu-id="e698c-397">Název namapované sady entit koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="e698c-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="e698c-398">**TypeName** **1**</span></span>       | <span data-ttu-id="e698c-399">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-399">No</span></span>          | <span data-ttu-id="e698c-400">Název typu entity koncepčního modelu, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="e698c-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="e698c-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="e698c-402">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-402">No</span></span>          | <span data-ttu-id="e698c-403">Název sady entit modelu úložiště, která je mapována na.</span><span class="sxs-lookup"><span data-stu-id="e698c-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="e698c-404">**Atributem makecolumnsdistinct nastaveným**</span><span class="sxs-lookup"><span data-stu-id="e698c-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="e698c-405">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-405">No</span></span>          | <span data-ttu-id="e698c-406">**Hodnota true** nebo **false** v závislosti na tom, zda jsou vráceny pouze odlišné řádky.</span><span class="sxs-lookup"><span data-stu-id="e698c-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="e698c-407">Pokud je tento atribut nastaven na **hodnotu true**, atribut **GenerateUpdateViews** elementu EntityContainerMapping musí být nastaven na **hodnotu false**.</span><span class="sxs-lookup"><span data-stu-id="e698c-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="e698c-408">**1** atributy **TypeName** a **StoreEntitySet** lze použít místo podřízených prvků element entitytypemapping a MappingFragment k namapování jednoho typu entity na jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="e698c-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="e698c-409">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-409">Example</span></span>

<span data-ttu-id="e698c-410">Následující příklad ukazuje element **EntitySetMapping** , který mapuje tři typy (základní typ a dva odvozené typy) v sadě entit **výuky** konceptuálního modelu do tří různých tabulek v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="e698c-411">Tabulky jsou určeny atributem **StoreEntitySet** v každém elementu **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="e698c-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="e698c-412">Element entitytypemapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="e698c-413">Element **element entitytypemapping** v jazyce MSL (Mapping Specification Language) definuje mapování mezi typem entity v koncepčním modelu a tabulkách nebo zobrazeními v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="e698c-414">Informace o typech entit koncepčního modelu a podkladových databázových tabulkách a zobrazeních naleznete v tématu EntityType element (CSDL) a EntitySet element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="e698c-415">Typ entity koncepčního modelu, který je mapován, je určen atributem **TypeName** elementu **element entitytypemapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="e698c-416">Namapovaná tabulka nebo zobrazení je určeno atributem **StoreEntitySet** podřízeného elementu MappingFragment.</span><span class="sxs-lookup"><span data-stu-id="e698c-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="e698c-417">K namapování funkcí pro vložení, aktualizaci nebo odstranění typů entit na uložené procedury v databázi lze použít podřízený element ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="e698c-418">Element **element entitytypemapping** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-419">MappingFragment (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="e698c-420">ModificationFunctionMapping (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="e698c-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="e698c-421">ScalarProperty</span></span>
-   <span data-ttu-id="e698c-422">Podmínka</span><span class="sxs-lookup"><span data-stu-id="e698c-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-423">Prvky **MappingFragment** a **ModificationFunctionMapping** nemohou být podřízenými prvky elementu **element entitytypemapping** současně.</span><span class="sxs-lookup"><span data-stu-id="e698c-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="e698c-424">Prvky **ScalarProperty** a **Condition** mohou být pouze podřízené prvky elementu **element entitytypemapping** , pokud jsou použity v rámci elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-425">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-425">Applicable Attributes</span></span>

<span data-ttu-id="e698c-426">Následující tabulka popisuje atributy, které mohou být aplikovány na element **element entitytypemapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="e698c-427">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-427">Attribute Name</span></span> | <span data-ttu-id="e698c-428">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-428">Is Required</span></span> | <span data-ttu-id="e698c-429">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-430">**Popisuje**</span><span class="sxs-lookup"><span data-stu-id="e698c-430">**TypeName**</span></span>   | <span data-ttu-id="e698c-431">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-431">Yes</span></span>         | <span data-ttu-id="e698c-432">Obor názvů kvalifikovaný název typu entity koncepčního modelu, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="e698c-433">Pokud je typ abstraktní nebo odvozený typ, musí být hodnota `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="e698c-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-434">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-434">Example</span></span>

<span data-ttu-id="e698c-435">Následující příklad ukazuje element EntitySetMapping se dvěma podřízenými elementy **element entitytypemapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="e698c-436">V prvním elementu **element entitytypemapping** je jako typ entity **SchoolModel. person** namapována tabulka **Person (osoba** ).</span><span class="sxs-lookup"><span data-stu-id="e698c-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="e698c-437">Ve druhém elementu **element entitytypemapping** je funkce aktualizace typu **SchoolModel. person** namapována na uloženou proceduru **UpdatePerson**v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="e698c-438">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-438">Example</span></span>

<span data-ttu-id="e698c-439">Následující příklad ukazuje mapování hierarchie typů, ve kterém je kořenový typ abstraktní.</span><span class="sxs-lookup"><span data-stu-id="e698c-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="e698c-440">Všimněte si použití syntaxe `IsOfType` pro atributy **TypeName** .</span><span class="sxs-lookup"><span data-stu-id="e698c-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="e698c-441">FunctionImportMapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="e698c-442">Element **FunctionImportMapping** v jazyce MSL definuje mapování mezi funkcí importovanou v koncepčním modelu a uloženou procedurou nebo funkcí v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="e698c-443">Importy funkcí musí být deklarovány v koncepčním modelu a uložené procedury musí být deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="e698c-444">Další informace naleznete v tématu element FunctionImport (CSDL) a element Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-445">Ve výchozím nastavení, pokud import funkce vrátí typ entity koncepčního modelu nebo komplexní typ, pak názvy sloupců vrácené základní uloženou procedurou musí přesně odpovídat názvům vlastností typu koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="e698c-446">Pokud názvy sloupců neodpovídají přesně názvům vlastností, mapování musí být definováno v elementu ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="e698c-447">Element **FunctionImportMapping** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-448">ResultMapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-449">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-449">Applicable Attributes</span></span>

<span data-ttu-id="e698c-450">Následující tabulka popisuje atributy, které se vztahují na element **FunctionImportMapping** :</span><span class="sxs-lookup"><span data-stu-id="e698c-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="e698c-451">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-451">Attribute Name</span></span>         | <span data-ttu-id="e698c-452">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-452">Is Required</span></span> | <span data-ttu-id="e698c-453">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-454">**Názevfunkceimportu**</span><span class="sxs-lookup"><span data-stu-id="e698c-454">**FunctionImportName**</span></span> | <span data-ttu-id="e698c-455">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-455">Yes</span></span>         | <span data-ttu-id="e698c-456">Název importované funkce v koncepčním modelu, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="e698c-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="e698c-457">**FunctionName**</span></span>       | <span data-ttu-id="e698c-458">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-458">Yes</span></span>         | <span data-ttu-id="e698c-459">Kvalifikovaný název oboru názvů funkce v modelu úložiště, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-460">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-460">Example</span></span>

<span data-ttu-id="e698c-461">Následující příklad je založen na školním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-461">The following example is based on the School model.</span></span> <span data-ttu-id="e698c-462">V modelu úložiště zvažte následující funkci:</span><span class="sxs-lookup"><span data-stu-id="e698c-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="e698c-463">Zvažte také, že je tato funkce importovaná v koncepčním modelu:</span><span class="sxs-lookup"><span data-stu-id="e698c-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="e698c-464">Následující příklad ukazuje element **FunctionImportMapping** použitý k namapování funkce a importu funkcí výše na sebe:</span><span class="sxs-lookup"><span data-stu-id="e698c-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="e698c-465">InsertFunction – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="e698c-466">Element **InsertFunction** v Mapping Specification Language (MSL) mapuje funkci INSERT typu entity nebo přidružení v koncepčním modelu na uloženou proceduru v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="e698c-467">Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="e698c-468">Další informace naleznete v tématu Function Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-469">Pokud nemapujete všechny tři operace vložení, aktualizace nebo odstranění typu entity na uložené procedury, nemapované operace selžou při spuštění za běhu a vyvolá se UpdateException.</span><span class="sxs-lookup"><span data-stu-id="e698c-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="e698c-470">Element **InsertFunction** může být podřízeným prvkem prvku ModificationFunctionMapping a použit pro element element entitytypemapping nebo prvek AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="e698c-471">InsertFunction se aplikuje na element entitytypemapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="e698c-472">Při použití na element element entitytypemapping mapuje element **InsertFunction** funkci INSERT typu entity v koncepčním modelu na uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="e698c-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="e698c-473">Element **InsertFunction** může mít při použití na element **element entitytypemapping** následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="e698c-474">AssociationEnd (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="e698c-475">ComplexProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="e698c-476">ResultBinding (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="e698c-477">ScarlarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e698c-478">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-478">Applicable Attributes</span></span>

<span data-ttu-id="e698c-479">Následující tabulka popisuje atributy, které mohou být aplikovány na element **InsertFunction** při použití na element **element entitytypemapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="e698c-480">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-480">Attribute Name</span></span>            | <span data-ttu-id="e698c-481">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-481">Is Required</span></span> | <span data-ttu-id="e698c-482">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="e698c-483">**FunctionName**</span></span>          | <span data-ttu-id="e698c-484">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-484">Yes</span></span>         | <span data-ttu-id="e698c-485">Obor názvů kvalifikovaný název uložené procedury, ke které je namapována funkce INSERT.</span><span class="sxs-lookup"><span data-stu-id="e698c-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="e698c-486">Uložená procedura musí být deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="e698c-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="e698c-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="e698c-488">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-488">No</span></span>          | <span data-ttu-id="e698c-489">Název výstupního parametru, který vrátí počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="e698c-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="e698c-490">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-490">Example</span></span>

<span data-ttu-id="e698c-491">Následující příklad je založen na školním modelu a ukazuje element **InsertFunction** , který se používá k mapování funkce INSERT typu entity Person do uložené procedury **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="e698c-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="e698c-492">Uložená procedura **InsertPerson** je deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="e698c-493">InsertFunction se aplikuje na AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="e698c-494">Při použití na element AssociationSetMapping mapuje element **InsertFunction** funkci INSERT přidružení v koncepčním modelu na uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="e698c-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="e698c-495">Element **InsertFunction** může mít při použití na element **AssociationSetMapping** následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="e698c-496">Endproperty lze mapovat</span><span class="sxs-lookup"><span data-stu-id="e698c-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="e698c-497">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-497">Applicable Attributes</span></span>

<span data-ttu-id="e698c-498">Následující tabulka popisuje atributy, které mohou být aplikovány na element **InsertFunction** při použití na element **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="e698c-499">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-499">Attribute Name</span></span>            | <span data-ttu-id="e698c-500">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-500">Is Required</span></span> | <span data-ttu-id="e698c-501">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="e698c-502">**FunctionName**</span></span>          | <span data-ttu-id="e698c-503">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-503">Yes</span></span>         | <span data-ttu-id="e698c-504">Obor názvů kvalifikovaný název uložené procedury, ke které je namapována funkce INSERT.</span><span class="sxs-lookup"><span data-stu-id="e698c-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="e698c-505">Uložená procedura musí být deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="e698c-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="e698c-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="e698c-507">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-507">No</span></span>          | <span data-ttu-id="e698c-508">Název výstupního parametru, který vrátí počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="e698c-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="e698c-509">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-509">Example</span></span>

<span data-ttu-id="e698c-510">Následující příklad je založen na školním modelu a ukazuje element **InsertFunction** , který se používá k mapování funkce vložení **CourseInstructor** přidružení k uložené proceduře **InsertCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="e698c-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="e698c-511">Uložená procedura **InsertCourseInstructor** je deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="e698c-512">Mapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="e698c-513">Element **Mapping** v Mapping Specification Language (MSL) obsahuje informace pro mapování objektů, které jsou definovány v koncepčním modelu, do databáze (jak je popsáno v modelu úložiště).</span><span class="sxs-lookup"><span data-stu-id="e698c-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="e698c-514">Další informace najdete v tématu Specifikace CSDL a SSDL.</span><span class="sxs-lookup"><span data-stu-id="e698c-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="e698c-515">Element **Mapping** je kořenovým prvkem specifikace mapování.</span><span class="sxs-lookup"><span data-stu-id="e698c-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="e698c-516">Obor názvů XML pro specifikace mapování je https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="e698c-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="e698c-517">Element Mapping může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="e698c-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="e698c-518">Alias (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="e698c-519">EntityContainerMapping (právě jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="e698c-520">Názvy koncepčních a typů modelů úložiště, které jsou odkazovány v MSL, musí být kvalifikovány svými příslušnými názvy oborů názvů.</span><span class="sxs-lookup"><span data-stu-id="e698c-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="e698c-521">Informace o názvu oboru názvů konceptuálního modelu naleznete v tématu Schema – element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="e698c-522">Informace o názvu oboru názvů modelu úložiště najdete v tématu schéma – element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="e698c-523">Aliasy pro obory názvů, které jsou používány v MSL, lze definovat pomocí elementu alias.</span><span class="sxs-lookup"><span data-stu-id="e698c-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-524">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-524">Applicable Attributes</span></span>

<span data-ttu-id="e698c-525">Následující tabulka popisuje atributy, které lze použít na element **Mapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="e698c-526">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-526">Attribute Name</span></span> | <span data-ttu-id="e698c-527">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-527">Is Required</span></span> | <span data-ttu-id="e698c-528">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="e698c-529">**Space**</span><span class="sxs-lookup"><span data-stu-id="e698c-529">**Space**</span></span>      | <span data-ttu-id="e698c-530">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-530">Yes</span></span>         | <span data-ttu-id="e698c-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="e698c-531">**C-S**.</span></span> <span data-ttu-id="e698c-532">Toto je pevná hodnota a nelze ji změnit.</span><span class="sxs-lookup"><span data-stu-id="e698c-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-533">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-533">Example</span></span>

<span data-ttu-id="e698c-534">Následující příklad ukazuje prvek **mapování** , který je založen na rámci školního modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="e698c-535">Další informace o školním modelu najdete v tématu rychlý Start (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="e698c-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="e698c-536">MappingFragment – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="e698c-537">Element **MappingFragment** v jazyce MSL (Mapping Specification Language) definuje mapování mezi vlastnostmi typu entity koncepčního modelu a tabulkou nebo zobrazením v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="e698c-538">Informace o typech entit koncepčního modelu a podkladových databázových tabulkách a zobrazeních naleznete v tématu EntityType element (CSDL) a EntitySet element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="e698c-539">**MappingFragment** může být podřízeným prvkem elementu element entitytypemapping nebo elementu EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="e698c-540">Element **MappingFragment** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-541">ComplexType (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="e698c-542">ScalarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="e698c-543">Podmínka (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-544">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-544">Applicable Attributes</span></span>

<span data-ttu-id="e698c-545">Následující tabulka popisuje atributy, které mohou být aplikovány na element **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="e698c-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="e698c-546">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-546">Attribute Name</span></span>          | <span data-ttu-id="e698c-547">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-547">Is Required</span></span> | <span data-ttu-id="e698c-548">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="e698c-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="e698c-550">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-550">Yes</span></span>         | <span data-ttu-id="e698c-551">Název tabulky nebo zobrazení, které je mapováno.</span><span class="sxs-lookup"><span data-stu-id="e698c-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="e698c-552">**Atributem makecolumnsdistinct nastaveným**</span><span class="sxs-lookup"><span data-stu-id="e698c-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="e698c-553">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-553">No</span></span>          | <span data-ttu-id="e698c-554">**Hodnota true** nebo **false** v závislosti na tom, zda jsou vráceny pouze odlišné řádky.</span><span class="sxs-lookup"><span data-stu-id="e698c-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="e698c-555">Pokud je tento atribut nastaven na **hodnotu true**, atribut **GenerateUpdateViews** elementu EntityContainerMapping musí být nastaven na **hodnotu false**.</span><span class="sxs-lookup"><span data-stu-id="e698c-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-556">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-556">Example</span></span>

<span data-ttu-id="e698c-557">Následující příklad ukazuje element **MappingFragment** jako podřízený prvek **element entitytypemapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="e698c-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="e698c-558">V tomto příkladu jsou vlastnosti typu **kurzu** v koncepčním modelu mapovány na sloupce tabulky **kurzů** v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="e698c-559">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-559">Example</span></span>

<span data-ttu-id="e698c-560">Následující příklad ukazuje element **MappingFragment** jako podřízený prvek **EntitySetMapping** elementu.</span><span class="sxs-lookup"><span data-stu-id="e698c-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="e698c-561">Stejně jako v předchozím příkladu jsou vlastnosti typu **kurzu** v koncepčním modelu mapovány na sloupce tabulky **Course** v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="e698c-562">ModificationFunctionMapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="e698c-563">Element **ModificationFunctionMapping** ve službě Mapping Specification Language (MSL) mapuje funkce vložení, aktualizace a odstranění typu entity koncepčního modelu na uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="e698c-564">Element **ModificationFunctionMapping** může také mapovat funkce INSERT a DELETE pro asociace m:n v koncepčním modelu na uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="e698c-565">Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="e698c-566">Další informace naleznete v tématu Function Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-567">Pokud nemapujete všechny tři operace vložení, aktualizace nebo odstranění typu entity na uložené procedury, nemapované operace selžou při spuštění za běhu a vyvolá se UpdateException.</span><span class="sxs-lookup"><span data-stu-id="e698c-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="e698c-568">Pokud jsou funkce úprav pro jednu entitu v hierarchii dědičnosti mapovány na uložené procedury, musí být funkce úprav pro všechny typy v hierarchii namapovány na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e698c-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="e698c-569">Element **ModificationFunctionMapping** může být podřízeným prvkem elementu element entitytypemapping nebo elementu AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="e698c-570">Element **ModificationFunctionMapping** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-571">DeleteFunction (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="e698c-572">InsertFunction (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="e698c-573">UpdateFunction (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="e698c-574">Pro element **ModificationFunctionMapping** se nevztahují žádné atributy.</span><span class="sxs-lookup"><span data-stu-id="e698c-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="e698c-575">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-575">Example</span></span>

<span data-ttu-id="e698c-576">Následující příklad ukazuje mapování sady entit pro entitu **lidé** nastavenou ve škole modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="e698c-577">Vedle mapování sloupce pro typ entity **osoba** se zobrazí mapování funkcí pro vložení, aktualizaci a odstranění typu **osoba** .</span><span class="sxs-lookup"><span data-stu-id="e698c-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="e698c-578">Funkce, které jsou mapovány na, jsou deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="e698c-579">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-579">Example</span></span>

<span data-ttu-id="e698c-580">Následující příklad ukazuje mapování sady přidružení pro přidružení **CourseInstructor** nastavené ve škole modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="e698c-581">Kromě mapování sloupce pro přidružení **CourseInstructor** se zobrazí mapování funkcí INSERT a DELETE v přidružení **CourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="e698c-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="e698c-582">Funkce, které jsou mapovány na, jsou deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="e698c-583">QueryView – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="e698c-584">Element **QueryView** v jazyce MSL (Mapping Specification Language) definuje mapování jen pro čtení mezi typem entity nebo přidružením v koncepčním modelu a tabulkou v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="e698c-585">Mapování je definováno s Entity SQL dotazem, který je vyhodnocován na základě modelu úložiště a vyjadřuje sadu výsledků dotazu v souvislosti s entitou nebo přidružením v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="e698c-586">Vzhledem k tomu, že zobrazení dotazů jsou jen pro čtení, nemůžete použít standardní příkazy aktualizace k aktualizaci typů, které jsou definovány zobrazeními dotazů.</span><span class="sxs-lookup"><span data-stu-id="e698c-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="e698c-587">Můžete provádět aktualizace těchto typů pomocí funkcí úprav.</span><span class="sxs-lookup"><span data-stu-id="e698c-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="e698c-588">Další informace naleznete v tématu How to: Namapujte funkce úprav na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e698c-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-589">V elementu **QueryView** se nepodporuje Entity SQL výrazy, které obsahují **GroupBy**, agregace skupin nebo navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e698c-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="e698c-590">Element **QueryView** může být podřízeným prvkem elementu EntitySetMapping nebo elementu AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="e698c-591">V bývalém případě zobrazení dotazu definuje mapování pro entitu v koncepčním modelu jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e698c-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="e698c-592">V druhém případě zobrazení dotazu definuje mapování jen pro čtení pro přidružení v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-593">Pokud je element **AssociationSetMapping** pro přidružení s referenčním omezením, element **AssociationSetMapping** se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="e698c-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="e698c-594">Další informace naleznete v tématu elementu ReferentialConstraint element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="e698c-595">Element **QueryView** nemůže mít žádné podřízené elementy.</span><span class="sxs-lookup"><span data-stu-id="e698c-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-596">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-596">Applicable Attributes</span></span>

<span data-ttu-id="e698c-597">Následující tabulka popisuje atributy, které mohou být aplikovány na element **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="e698c-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="e698c-598">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-598">Attribute Name</span></span> | <span data-ttu-id="e698c-599">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-599">Is Required</span></span> | <span data-ttu-id="e698c-600">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-601">**Popisuje**</span><span class="sxs-lookup"><span data-stu-id="e698c-601">**TypeName**</span></span>   | <span data-ttu-id="e698c-602">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-602">No</span></span>          | <span data-ttu-id="e698c-603">Název typu koncepčního modelu, který je namapován zobrazením dotazu.</span><span class="sxs-lookup"><span data-stu-id="e698c-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-604">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-604">Example</span></span>

<span data-ttu-id="e698c-605">Následující příklad ukazuje element **QueryView** jako podřízený prvek **EntitySetMapping** a definuje mapování zobrazení dotazu pro typ entity **oddělení** v modelu školy.</span><span class="sxs-lookup"><span data-stu-id="e698c-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="e698c-606">Vzhledem k tomu, že dotaz vrací pouze podmnožinu členů typu **oddělení** v modelu úložiště, typ **oddělení** ve školním modelu byl změněn na základě tohoto mapování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e698c-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="e698c-607">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-607">Example</span></span>

<span data-ttu-id="e698c-608">Následující příklad ukazuje element **QueryView** jako podřízenou položku prvku **AssociationSetMapping** a definuje mapování jen pro čtení pro asociaci `FK_Course_Department` v modelu školy.</span><span class="sxs-lookup"><span data-stu-id="e698c-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="e698c-609">Komentáře</span><span class="sxs-lookup"><span data-stu-id="e698c-609">Comments</span></span>

<span data-ttu-id="e698c-610">Můžete definovat zobrazení dotazů a povolit následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="e698c-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="e698c-611">Definujte entitu v koncepčním modelu, která neobsahuje všechny vlastnosti entity v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="e698c-612">To zahrnuje vlastnosti, které nemají výchozí hodnoty a nepodporují hodnoty **null** .</span><span class="sxs-lookup"><span data-stu-id="e698c-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="e698c-613">Namapujte počítané sloupce v modelu úložiště na vlastnosti typů entit v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="e698c-614">Definujte mapování, kde podmínky používané pro rozdělení entit v koncepčním modelu nejsou založené na rovnosti.</span><span class="sxs-lookup"><span data-stu-id="e698c-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="e698c-615">Když zadáte podmíněné mapování pomocí elementu **Condition** , dodaná podmínka se musí rovnat zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="e698c-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="e698c-616">Další informace naleznete v tématu Condition element (MSL).</span><span class="sxs-lookup"><span data-stu-id="e698c-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="e698c-617">Namapujte stejný sloupec v modelu úložiště na více typů v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="e698c-618">Namapujte více typů na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="e698c-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="e698c-619">Definujte asociace v koncepčním modelu, které nejsou založené na cizích klíčích v relačním schématu.</span><span class="sxs-lookup"><span data-stu-id="e698c-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="e698c-620">Použijte vlastní obchodní logiku k nastavení hodnoty vlastností v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="e698c-621">Například můžete namapovat hodnotu řetězce "T" ve zdroji dat na hodnotu **true**, logická hodnota v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="e698c-622">Definujte podmíněné filtry pro výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="e698c-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="e698c-623">Vyvynuťte méně omezení pro data v koncepčním modelu než v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="e698c-624">Můžete například vytvořit vlastnost v koncepčním modelu s možnou hodnotou null i v případě, že sloupec, ke kterému je namapován, nepodporuje hodnoty **null**.</span><span class="sxs-lookup"><span data-stu-id="e698c-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="e698c-625">Při definování zobrazení dotazu pro entity platí následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="e698c-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="e698c-626">Zobrazení dotazu jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e698c-626">Query views are read-only.</span></span> <span data-ttu-id="e698c-627">Aktualizace entit můžete provádět pouze pomocí funkcí úprav.</span><span class="sxs-lookup"><span data-stu-id="e698c-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="e698c-628">Při definování typu entity pomocí zobrazení dotazu je nutné definovat také všechny související entity pomocí zobrazení dotazu.</span><span class="sxs-lookup"><span data-stu-id="e698c-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="e698c-629">Pokud namapujete přidružení m:n k entitě v modelu úložiště, která představuje tabulku odkazů v relačním schématu, je nutné definovat element **QueryView** v elementu **AssociationSetMapping** pro tuto tabulku odkazů.</span><span class="sxs-lookup"><span data-stu-id="e698c-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="e698c-630">Zobrazení dotazu musí být definována pro všechny typy v hierarchii typu.</span><span class="sxs-lookup"><span data-stu-id="e698c-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="e698c-631">Můžete to udělat následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="e698c-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="e698c-632">Pomocí jednoho prvku **QueryView** , který určuje jeden Entity SQL dotaz, který vrací sjednocení všech typů entit v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="e698c-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="e698c-633">Pomocí jednoho prvku **QueryView** , který určuje jeden Entity SQL dotaz, který používá operátor Case k vrácení konkrétního typu entity v hierarchii založeného na konkrétní podmínce.</span><span class="sxs-lookup"><span data-stu-id="e698c-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="e698c-634">S dalším prvkem **QueryView** pro konkrétní typ v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="e698c-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="e698c-635">V takovém případě použijte atribut **TypeName** elementu **QueryView** k určení typu entity pro každé zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e698c-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="e698c-636">Pokud je definováno zobrazení dotazu, nelze zadat atribut **StorageSetName** elementu **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="e698c-637">Pokud je definováno zobrazení dotazu, element **EntitySetMapping**nemůže také obsahovat mapování **vlastností** .</span><span class="sxs-lookup"><span data-stu-id="e698c-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="e698c-638">ResultBinding – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="e698c-639">Element **ResultBinding** v Mapping Specification Language (MSL) mapuje hodnoty sloupce, které jsou vraceny uloženými procedurami do vlastností entity v koncepčním modelu, pokud jsou funkce změny typu entity mapovány na uložené procedury v podkladová databáze.</span><span class="sxs-lookup"><span data-stu-id="e698c-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="e698c-640">Například pokud je hodnota sloupce identity vrácena uloženou procedurou INSERT, element **ResultBinding** mapuje vrácenou hodnotu na vlastnost typu entity v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="e698c-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="e698c-641">Element **ResultBinding** může být podřízeným prvkem elementu InsertFunction nebo elementu UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="e698c-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="e698c-642">Element **ResultBinding** nemůže mít žádné podřízené elementy.</span><span class="sxs-lookup"><span data-stu-id="e698c-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-643">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-643">Applicable Attributes</span></span>

<span data-ttu-id="e698c-644">Následující tabulka popisuje atributy, které se vztahují na element **ResultBinding** :</span><span class="sxs-lookup"><span data-stu-id="e698c-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="e698c-645">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-645">Attribute Name</span></span> | <span data-ttu-id="e698c-646">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-646">Is Required</span></span> | <span data-ttu-id="e698c-647">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-648">**Název**</span><span class="sxs-lookup"><span data-stu-id="e698c-648">**Name**</span></span>       | <span data-ttu-id="e698c-649">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-649">Yes</span></span>         | <span data-ttu-id="e698c-650">Název vlastnosti entity v koncepčním modelu, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="e698c-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="e698c-651">**ColumnName**</span></span> | <span data-ttu-id="e698c-652">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-652">Yes</span></span>         | <span data-ttu-id="e698c-653">Název mapovaného sloupce</span><span class="sxs-lookup"><span data-stu-id="e698c-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="e698c-654">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-654">Example</span></span>

<span data-ttu-id="e698c-655">Následující příklad je založen na školním modelu a ukazuje element **InsertFunction** , který slouží k mapování funkce INSERT typu entity **Person** na uloženou proceduru **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="e698c-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="e698c-656">(Uloženou proceduru **InsertPerson** se zobrazuje níže a deklaruje se v modelu úložiště.) Element **ResultBinding** se používá k mapování hodnoty sloupce, který je vrácený uloženou procedurou (**NewPersonID**) na vlastnost typu entity (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="e698c-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="e698c-657">Následující příkaz Transact-SQL popisuje uloženou proceduru **InsertPerson** :</span><span class="sxs-lookup"><span data-stu-id="e698c-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="e698c-658">ResultMapping – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="e698c-659">Element **ResultMapping** v mapování specifikace jazyka MSL definuje mapování mezi funkcí importovanou v koncepčním modelu a uloženou procedurou v podkladové databázi, pokud jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="e698c-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="e698c-660">Import funkce vrací typ entity koncepčního modelu nebo komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="e698c-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="e698c-661">Názvy sloupců vrácené uloženou procedurou neodpovídají přesně názvům vlastností typu entity nebo komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="e698c-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="e698c-662">Ve výchozím nastavení je mapování mezi sloupci vrácenou uloženou procedurou a typem entity nebo komplexním typem založeno na názvech sloupců a vlastností.</span><span class="sxs-lookup"><span data-stu-id="e698c-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="e698c-663">Pokud názvy sloupců neodpovídají přesně názvům vlastností, je nutné k definování mapování použít element **ResultMapping** .</span><span class="sxs-lookup"><span data-stu-id="e698c-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="e698c-664">Příklad výchozího mapování naleznete v tématu FunctionImportMapping element (MSL).</span><span class="sxs-lookup"><span data-stu-id="e698c-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="e698c-665">Element **ResultMapping** je podřízeným prvkem elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="e698c-666">Element **ResultMapping** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-667">Element entitytypemapping (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="e698c-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="e698c-668">ComplexTypeMapping</span></span>

<span data-ttu-id="e698c-669">Pro element **ResultMapping** se nevztahují žádné atributy.</span><span class="sxs-lookup"><span data-stu-id="e698c-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="e698c-670">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-670">Example</span></span>

<span data-ttu-id="e698c-671">Vezměte v úvahu následující uložený postup:</span><span class="sxs-lookup"><span data-stu-id="e698c-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="e698c-672">Zvažte také následující typ entity koncepčního modelu:</span><span class="sxs-lookup"><span data-stu-id="e698c-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="e698c-673">Aby bylo možné vytvořit import funkce, který vrací instance předchozí entity typu, mapování mezi sloupci vrácenou uloženou procedurou a typem entity musí být definováno v elementu **ResultMapping** :</span><span class="sxs-lookup"><span data-stu-id="e698c-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="e698c-674">ScalarProperty – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="e698c-675">Element **ScalarProperty** v Mapping Specification Language (MSL) mapuje vlastnost u typu entity koncepčního modelu, komplexního typu nebo přidružení k sloupci tabulky nebo parametru uložené procedury v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="e698c-676">Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="e698c-677">Další informace naleznete v tématu Function Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="e698c-678">Element **ScalarProperty** může být podřízeným prvkem následujících prvků:</span><span class="sxs-lookup"><span data-stu-id="e698c-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="e698c-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="e698c-679">MappingFragment</span></span>
-   <span data-ttu-id="e698c-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="e698c-680">InsertFunction</span></span>
-   <span data-ttu-id="e698c-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="e698c-681">UpdateFunction</span></span>
-   <span data-ttu-id="e698c-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="e698c-682">DeleteFunction</span></span>
-   <span data-ttu-id="e698c-683">Endproperty lze mapovat</span><span class="sxs-lookup"><span data-stu-id="e698c-683">EndProperty</span></span>
-   <span data-ttu-id="e698c-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="e698c-684">ComplexProperty</span></span>
-   <span data-ttu-id="e698c-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="e698c-685">ResultMapping</span></span>

<span data-ttu-id="e698c-686">Jako podřízený element **MappingFragment**, **ComplexProperty**nebo **endproperty lze mapovat** prvek **ScalarProperty** mapuje vlastnost v koncepčním modelu na sloupec v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="e698c-687">Jako podřízený element **InsertFunction**, **UpdateFunction**nebo **DeleteFunction** prvek **ScalarProperty** mapuje vlastnost v koncepčním modelu na parametr uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="e698c-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="e698c-688">Element **ScalarProperty** nemůže mít žádné podřízené elementy.</span><span class="sxs-lookup"><span data-stu-id="e698c-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-689">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-689">Applicable Attributes</span></span>

<span data-ttu-id="e698c-690">Atributy, které se vztahují na element **ScalarProperty** , se liší v závislosti na roli elementu.</span><span class="sxs-lookup"><span data-stu-id="e698c-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="e698c-691">Následující tabulka popisuje atributy, které jsou použity při použití prvku **ScalarProperty** k namapování vlastnosti koncepčního modelu na sloupec v databázi:</span><span class="sxs-lookup"><span data-stu-id="e698c-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="e698c-692">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-692">Attribute Name</span></span> | <span data-ttu-id="e698c-693">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-693">Is Required</span></span> | <span data-ttu-id="e698c-694">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="e698c-695">**Název**</span><span class="sxs-lookup"><span data-stu-id="e698c-695">**Name**</span></span>       | <span data-ttu-id="e698c-696">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-696">Yes</span></span>         | <span data-ttu-id="e698c-697">Název vlastnosti konceptuálního modelu, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="e698c-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="e698c-698">**ColumnName**</span></span> | <span data-ttu-id="e698c-699">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-699">Yes</span></span>         | <span data-ttu-id="e698c-700">Název sloupce tabulky, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="e698c-701">Následující tabulka popisuje atributy, které se vztahují na element **ScalarProperty** při použití k namapování vlastnosti koncepčního modelu na parametr uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="e698c-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="e698c-702">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-702">Attribute Name</span></span>    | <span data-ttu-id="e698c-703">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-703">Is Required</span></span> | <span data-ttu-id="e698c-704">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-705">**Název**</span><span class="sxs-lookup"><span data-stu-id="e698c-705">**Name**</span></span>          | <span data-ttu-id="e698c-706">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-706">Yes</span></span>         | <span data-ttu-id="e698c-707">Název vlastnosti konceptuálního modelu, který je mapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="e698c-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="e698c-708">**ParameterName**</span></span> | <span data-ttu-id="e698c-709">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-709">Yes</span></span>         | <span data-ttu-id="e698c-710">Název parametru, který je namapován.</span><span class="sxs-lookup"><span data-stu-id="e698c-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="e698c-711">**Verze**</span><span class="sxs-lookup"><span data-stu-id="e698c-711">**Version**</span></span>       | <span data-ttu-id="e698c-712">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-712">No</span></span>          | <span data-ttu-id="e698c-713">**Aktuální** nebo **původní** v závislosti na tom, zda má být aktuální hodnota nebo původní hodnota vlastnosti použita pro kontrolu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="e698c-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="e698c-714">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-714">Example</span></span>

<span data-ttu-id="e698c-715">Následující příklad ukazuje element **ScalarProperty** použitý dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="e698c-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="e698c-716">Pro mapování vlastností typu entity **Person** na sloupce tabulky **Person**.</span><span class="sxs-lookup"><span data-stu-id="e698c-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="e698c-717">Pro mapování vlastností typu entity **Person** na parametry uložené procedury **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="e698c-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="e698c-718">Uložené procedury jsou deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="e698c-719">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-719">Example</span></span>

<span data-ttu-id="e698c-720">Následující příklad ukazuje element **ScalarProperty** , který slouží k mapování funkcí INSERT a DELETE přidružení koncepčního modelu k uloženým procedurám v databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="e698c-721">Uložené procedury jsou deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="e698c-722">UpdateFunction – element (MSL)</span><span class="sxs-lookup"><span data-stu-id="e698c-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="e698c-723">Element **UpdateFunction** ve službě Mapping Specification Language (MSL) mapuje funkci Update typu entity v koncepčním modelu na uloženou proceduru v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="e698c-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="e698c-724">Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="e698c-725">Další informace naleznete v tématu Function Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="e698c-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="e698c-726">Pokud nemapujete všechny tři operace vložení, aktualizace nebo odstranění typu entity na uložené procedury, nemapované operace selžou při spuštění za běhu a vyvolá se UpdateException.</span><span class="sxs-lookup"><span data-stu-id="e698c-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="e698c-727">Element **UpdateFunction** může být podřízeným prvkem prvku ModificationFunctionMapping a použit pro element element entitytypemapping.</span><span class="sxs-lookup"><span data-stu-id="e698c-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="e698c-728">Element **UpdateFunction** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="e698c-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="e698c-729">AssociationEnd (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="e698c-730">ComplexProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="e698c-731">ResultBinding (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="e698c-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="e698c-732">ScarlarProperty (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="e698c-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="e698c-733">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="e698c-733">Applicable Attributes</span></span>

<span data-ttu-id="e698c-734">Následující tabulka popisuje atributy, které mohou být aplikovány na element **UpdateFunction** .</span><span class="sxs-lookup"><span data-stu-id="e698c-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="e698c-735">Název atributu</span><span class="sxs-lookup"><span data-stu-id="e698c-735">Attribute Name</span></span>            | <span data-ttu-id="e698c-736">Je povinné</span><span class="sxs-lookup"><span data-stu-id="e698c-736">Is Required</span></span> | <span data-ttu-id="e698c-737">Value</span><span class="sxs-lookup"><span data-stu-id="e698c-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="e698c-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="e698c-738">**FunctionName**</span></span>          | <span data-ttu-id="e698c-739">Ano</span><span class="sxs-lookup"><span data-stu-id="e698c-739">Yes</span></span>         | <span data-ttu-id="e698c-740">Obor názvů kvalifikovaný název uložené procedury, na kterou je namapovaná funkce Update.</span><span class="sxs-lookup"><span data-stu-id="e698c-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="e698c-741">Uložená procedura musí být deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="e698c-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="e698c-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="e698c-743">Ne</span><span class="sxs-lookup"><span data-stu-id="e698c-743">No</span></span>          | <span data-ttu-id="e698c-744">Název výstupního parametru, který vrátí počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="e698c-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="e698c-745">Příklad</span><span class="sxs-lookup"><span data-stu-id="e698c-745">Example</span></span>

<span data-ttu-id="e698c-746">Následující příklad je založen na školním modelu a ukazuje element **UpdateFunction** , který se používá k namapování funkce Update typu entity **Person** na uloženou proceduru **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="e698c-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="e698c-747">Uložená procedura **UpdatePerson** je deklarována v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e698c-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
