---
title: Specifikace MSL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
caps.latest.revision: 4
ms.openlocfilehash: 7448efc99f9fd9c6cdf930256a26347376fb354c
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912784"
---
# <a name="msl-specification"></a>Specifikace MSL
Mapování specifikačnímu jazyku (MSL) je jazyk založený na formátu XML, který popisuje mapování mezi koncepčního modelu i modelu úložiště aplikace Entity Framework.

V aplikaci Entity Framework je načtena metadata mapování ze souboru .msl (napsaného v MSL) v okamžiku sestavení. Entity Framework používá pro překlad dotazů vůči Koncepční model příkazů specifických pro úložiště mapování metadat za běhu.

Návrhář Entity Framework (EF designeru) ukládá informace o mapování v souboru .edmx v době návrhu. V okamžiku sestavení Návrhář Entity používá informace v souboru .edmx k vytvoření souboru .msl, která je potřeba pro Entity Framework za běhu

Názvy všech koncepční nebo modelu typy úložišť, které jsou odkazovány v MSL musí být určeny podle jejich příslušných oborem názvů. Informace o názvu oboru názvů konceptuálního modelu naleznete v tématu [specifikace CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Informace o názvu oboru názvů modelu úložiště najdete v tématu [specifikace SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Verze MSL jsou rozlišené pomocí obory názvů XML.

| Verze MSL | Namespace XML                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | název urn: schémata-microsoft-com:windows:storage:mapping:CS |
| MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Zástupnému elementu (MSL)

**Alias** prvek v mapování specification language (MSL) je podřízený element Mapping, který se používá k definování aliasy pro koncepční model a úložiště modelu obory názvů. Názvy všech koncepční nebo modelu typy úložišť, které jsou odkazovány v MSL musí být určeny podle jejich příslušných oborem názvů. Informace týkající se názvu oboru názvů konceptuálního modelu naleznete v tématu Element schématu (CSDL). Informace o názvu oboru názvů modelu úložiště najdete v tématu Element schématu (SSDL).

**Alias** element nemůže mít podřízené prvky.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **Alias** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Ano         | Alias pro obor názvů, která je zadána **hodnotu** atribut. |
| **Hodnota**      | Ano         | Obor názvů, pro kterou hodnotu **klíč** element je alias.     |

### <a name="example"></a>Příklad

Následující příklad ukazuje **Alias** element, který definuje alias, `c`, pro typy, které jsou definované v konceptuálním modelu.

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

## <a name="associationend-element-msl"></a>Element AssociationEnd (MSL)

**AssociationEnd** element v mapování specification language (MSL) se používá při funkcí změny typu entity v konceptuálním modelu jsou mapovány na uložené procedury v podkladové databázi. Pokud změna uložené procedury přijímá parametr, jehož hodnota je uložená ve vlastnosti přidružení, **AssociationEnd** element mapuje hodnotu parametru. Další informace naleznete v níže uvedeném příkladu.

Další informace o mapování funkcí změny typů entit k uloženým procedurám, naleznete v tématu ModificationFunctionMapping – Element (MSL) a návod: mapování Entity na uložené procedury.

**AssociationEnd** prvek může mít následujících podřízených elementů:

-   ScalarProperty

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které se vztahují na **AssociationEnd** elementu.

| Název atributu     | Vyžaduje se | Hodnota                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Element AssociationSet** | Ano         | Název přidružení, které je mapován.                                                                                                                                 |
| **z**           | Ano         | Hodnota **FromRole** atribut, který odpovídá přidružení mapovaný navigační vlastnosti. Další informace najdete v tématu Element NavigationProperty (CSDL). |
| **k**             | Ano         | Hodnota **ToRole** atribut, který odpovídá přidružení mapovaný navigační vlastnosti. Další informace najdete v tématu Element NavigationProperty (CSDL).   |

### <a name="example"></a>Příklad

Vezměte v úvahu následující entity typu koncepčního modelu:

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

Zvažte také následující uložené procedury:

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

Pokud chcete mapovat funkci aktualizace `Course` entity pro tuto uloženou proceduru, je nutné zadat hodnotu, která **DepartmentID** parametr. Hodnota pro `DepartmentID` neodpovídá vlastnosti typu entity; je součástí přidružení nezávislé, jehož mapování je znázorněna zde:

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

Následující kód ukazuje **AssociationEnd** element slouží k mapování **DepartmentID** vlastnost **FK\_kurzu\_oddělení** přidružení, které **UpdateCourse** uložené procedury (ke které funkci aktualizace **kurzu** namapované typ entity):

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

## <a name="associationsetmapping-element-msl"></a>Element AssociationSetMapping (MSL)

**AssociationSetMapping** prvek v mapování specification language (MSL) definuje mapování mezi přidružení v konceptuálním modelu a tabulky sloupce v podkladové databázi.

Přidružení v konceptuálním modelu jsou typy, jejichž vlastnosti představují primární a cizí klíče sloupce v podkladové databázi. **AssociationSetMapping** prvek používá dva prvky EndProperty definovat mapování mezi přidružení typu vlastnosti a sloupce v databázi. Podmínky můžete umístit na tato mapování s elementem podmínku. Mapovat na uložené procedury v databázi s elementem ModificationFunctionMapping insert, update a delete funkce pro přidružení. Definujte jen pro čtení mapování mezi přidružení a sloupců tabulky pomocí řetězce Entity SQL v elementu QueryView.

> [!NOTE]
> Pokud pro přidružení v konceptuálním modelu je definována referenční omezení, přidružení nemusí být namapována na žádnou **AssociationSetMapping** elementu. Pokud **AssociationSetMapping** element je k dispozici pro přidružení, které obsahuje referenční omezení, mapování definované v **AssociationSetMapping** prvek bude ignorován. Další informace najdete v elementu ReferentialConstraint – Element (CSDL).

**AssociationSetMapping** prvek může mít následujících podřízených elementů

-   Zobrazení QueryView (nula nebo jedna)
-   EndProperty (nula nebo dva)
-   Podmínka (nula nebo více)
-   ModificationFunctionMapping (nula nebo jedna)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **AssociationSetMapping** elementu.

| Název atributu     | Vyžaduje se | Hodnota                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Jméno**           | Ano         | Název sady přidružení konceptuálního modelu, který je mapován.                      |
| **Název typu**       | Ne          | Název přidružení typu koncepčního modelu, který je mapován kvalifikovaný v oboru názvů. |
| **StoreEntitySet** | Ne          | Název tabulky, která je mapován.                                                 |

### <a name="example"></a>Příklad

Následující příklad ukazuje **AssociationSetMapping** element, ve kterém **FK\_kurzu\_oddělení** přidružení nastavit v konceptuálním modelu je namapována na  **Kurz** tabulky v databázi. Mapování mezi přidružení typu vlastnosti a sloupce tabulky, které jsou určené v podřízených **EndProperty** elementy.

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

## <a name="complexproperty-element-msl"></a>Element ComplexProperty (MSL)

A **ComplexProperty** prvek v mapování specification language (MSL) definuje mapování mezi komplexní typ vlastnosti Koncepční model entity typu tabulky sloupce a v podkladové databázi. Mapování sloupce vlastností jsou uvedeny v podřízených elementů ScalarProperty.

**ComplexType** vlastnost prvek může mít následujících podřízených elementů:

-   ScalarProperty (nula nebo více)
-   **ComplexProperty** (nula nebo více)
-   ComplextTypeMapping (nula nebo více)
-   Podmínka (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které se vztahují na **ComplexProperty** element:

| Název atributu | Vyžaduje se | Hodnota                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název komplexní vlastnost typu entity v konceptuálním modelu, který je mapován. |
| **Název typu**   | Ne          | Název kvalifikovaný v oboru názvů vlastnost typu koncepčního modelu.                              |

### <a name="example"></a>Příklad

Následující příklad je založen na modelu školy. Následující komplexní typ se přidala do koncepčního modelu:

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

**LastName** a **FirstName** vlastnosti **osoba** typ entity byly nahrazeny jednoho komplexní vlastnost **název**:

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

Zobrazí se následující MSL **ComplexProperty** element slouží k mapování **název** vlastnost na sloupce v podkladové databázi:

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

## <a name="complextypemapping-element-msl"></a>Element ComplexTypeMapping (MSL)

**ComplexTypeMapping** prvek v mapování specification language (MSL) je podřízeným prvkem elementu ResultMapping a definuje mapování mezi importované funkce v konceptuálním modelu a uložené procedury v základním databáze, pokud jsou pravdivé následující výroky:

-   Importovaná funkce vrátí koncepční komplexního typu.
-   Názvy sloupců vrácený uloženou proceduru přesně shodovat s názvy vlastností komplexního typu.

Ve výchozím nastavení mapování sloupců vrácený uložené procedury a komplexní typ je založen na názvy sloupců a vlastnosti. Pokud se názvy sloupců přesně shodovat s názvy vlastností, je nutné použít **ComplexTypeMapping** elementu k definování mapování. Příklad výchozí mapování naleznete v tématu FunctionImportMapping – Element (MSL).

**ComplexTypeMapping** prvek může mít následujících podřízených elementů:

-   ScalarProperty (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které se vztahují na **ComplexTypeMapping** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **Název typu**   | Ano         | Název kvalifikovaný v oboru názvů komplexní typ, který je mapován. |

### <a name="example"></a>Příklad

Vezměte v úvahu následující uložené procedury:

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

Zvažte také následující komplexní typ koncepčního modelu:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Chcete-li vytvořit importovaná funkce, který vrátí instance předchozí komplexní typ, vrátí mapování mezi sloupci pomocí uložené procedury a typu entity musí být definován v **ComplexTypeMapping** element:

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

## <a name="condition-element-msl"></a>Podmínka – Element (MSL)

**Podmínku** prvek v mapování specification language (MSL) umístí podmínek mapování mezi Koncepční model a databáze. Mapování, která je definována v rámci uzlu XML je platné, pokud všechny podmínky, jako zadané v podřízených **podmínku** elementy, jsou splněny. V opačném případě mapování je neplatný. Například, pokud prvek MappingFragment obsahuje jeden nebo více **podmínku** podřízených elementů mapování definované v rámci **MappingFragment** uzlu bude platit jenom Pokud všechny podmínky podřízené  **Podmínka** prvky jsou splněny.

Každou podmínku můžete použít buď **název** (název vlastnosti entity konceptuální model, určené **název** atribut), nebo **Názevsloupce** (název sloupce v Zadaná databáze, tak **Názevsloupce** atributu). Když **název** atribut je nastaven, podmínka je porovnávána s hodnotou vlastnosti entity. Když **Názevsloupce** atribut je nastaven, podmínka je porovnávána s hodnotu sloupce. Pouze jeden z **název** nebo **Názevsloupce** atribut se dá nastavit v **podmínku** elementu.

> [!NOTE]
> Když **podmínku** element se používá v rámci elementu FunctionImportMapping, pouze **název** atribut se nedá použít.

**Podmínku** element může být podřízená následující prvky:

-   AssociationSetMapping
-   ComplexProperty
-   Element EntitySetMapping
-   MappingFragment
-   Element EntityTypeMapping

**Podmínku** prvek může mít žádné podřízené prvky.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které se vztahují na **podmínku** element:

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Názevsloupce** | Ne          | Název sloupce tabulky, jehož hodnota se používá k vyhodnocení podmínky.                                                                                                                                                                                                                   |
| **IsNull**     | Ne          | **Hodnota TRUE** nebo **False**. Pokud je hodnota **True** a hodnota sloupce je **null**, nebo pokud je hodnota **False** a hodnota sloupce není **null**, bude podmínka pravdivá . V opačném případě podmínka není splněna. <br/> **IsNull** a **hodnotu** atributy nelze použít ve stejnou dobu. |
| **Hodnota**      | Ne          | Hodnota, pomocí kterého se porovnává hodnotu sloupce. Pokud jsou hodnoty stejné, je podmínka pravdivá. V opačném případě podmínka není splněna. <br/> **IsNull** a **hodnotu** atributy nelze použít ve stejnou dobu.                                                                       |
| **Jméno**       | Ne          | Název vlastnosti koncepčního modelu entity, jehož hodnota se používá k vyhodnocení podmínky. <br/> Tento atribut se nedá použít pokud **podmínku** element se používá v rámci elementu FunctionImportMapping.                                                                           |

### <a name="example"></a>Příklad

Následující příklad ukazuje **podmínku** prvků jako podřízených prvků **MappingFragment** elementy. Když **HireDate** nemá hodnotu null a **EnrollmentDate** je null, je mapovány dat mezi **SchoolModel.Instructor** typ a **PersonID**a **HireDate** sloupce **osoba** tabulky. Když **EnrollmentDate** nemá hodnotu null a **HireDate** je null, je mapovány dat mezi **SchoolModel.Student** typ a **PersonID** a **registrace** sloupce **osoba** tabulky.

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

## <a name="deletefunction-element-msl"></a>Element DeleteFunction (MSL)

**DeleteFunction** prvek v mapování specification language (MSL) mapuje funkci Odstranit typ entity nebo přidružení v konceptuálním modelu na uloženou proceduru v podkladové databázi. Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště. Další informace najdete v tématu funkce – Element (SSDL).

> [!NOTE]
> Pokud nejsou mapovány všechny tři vložení, aktualizace nebo odstranění operace typu entity na uložené procedury nenamapované operace se nezdaří, pokud je proveden za běhu a UpdateException je vyvolána výjimka.

### <a name="deletefunction-applied-to-entitytypemapping"></a>Použít pro mapování EntityTypeMapping DeleteFunction

Při použití elementu mapování EntityTypeMapping **DeleteFunction** prvek mapuje delete – funkce typu entity v konceptuálním modelu na uložené procedury.

**DeleteFunction** prvek může mít následující podřízené prvky při použití **mapování EntityTypeMapping** element:

-   AssociationEnd (nula nebo více)
-   ComplexProperty (nula nebo více)
-   ScarlarProperty (nula nebo více)

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **DeleteFunction** prvku, když se použije k **mapování EntityTypeMapping** elementu.

| Název atributu            | Vyžaduje se | Hodnota                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Název uložené procedury, ke které je mapován funkci Odstranit kvalifikovaný v oboru názvů. Uložená procedura musí být deklarována v rámci modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupní parametr, který vrací počet ovlivněných řádků.                                                                               |

#### <a name="example"></a>Příklad

Následující příklad je založen na modelu školy a ukazuje **DeleteFunction** element mapování funkci Odstranit **osoba** typu entity na **DeletePerson** uloženou proceduru. **DeletePerson** uložené procedury je deklarována v rámci modelu úložiště.

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

### <a name="deletefunction-applied-to-associationsetmapping"></a>U AssociationSetMapping DeleteFunction

Při použití elementu AssociationSetMapping **DeleteFunction** prvek mapuje funkci Odstranit přidružení v konceptuálním modelu na uložené procedury.

**DeleteFunction** prvek může mít následující podřízené prvky při použití **AssociationSetMapping** element:

-   EndProperty

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **DeleteFunction** prvku, když se použije k **AssociationSetMapping** elementu.

| Název atributu            | Vyžaduje se | Hodnota                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Název uložené procedury, ke které je mapován funkci Odstranit kvalifikovaný v oboru názvů. Uložená procedura musí být deklarována v rámci modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupní parametr, který vrací počet ovlivněných řádků.                                                                               |

#### <a name="example"></a>Příklad

Následující příklad je založen na modelu školy a ukazuje **DeleteFunction** element slouží k mapování funkce delete **CourseInstructor** přidružení, které  **DeleteCourseInstructor** uložené procedury. **DeleteCourseInstructor** uložené procedury je deklarována v rámci modelu úložiště.

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

## <a name="endproperty-element-msl"></a>Element EndProperty (MSL)

**EndProperty** prvek v mapování specification language (MSL) definuje mapování mezi skončí nebo úpravy funkce přidružení konceptuálního modelu a databáze. Sloupce vlastností Mapování bylo specifikováno v ScalarProperty podřízený element.

Když **EndProperty** element slouží k definování mapování na konci přidružení konceptuálního modelu, je podřízeným prvkem elementu AssociationSetMapping. Když **EndProperty** element slouží k definování mapování pro funkci úpravy přidružení konceptuálního modelu, je podřízeným prvkem elementu InsertFunction nebo DeleteFunction elementu.

**EndProperty** prvek může mít následujících podřízených elementů:

-   ScalarProperty (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které se vztahují na **EndProperty** element:

| Název atributu | Vyžaduje se | Hodnota                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Název           | Ano         | Název elementu end přidružení, které je mapován. |

### <a name="example"></a>Příklad

Následující příklad ukazuje **AssociationSetMapping** element, ve kterém **FK\_kurzu\_oddělení** přidružení v konceptuálním modelu je namapována na **Kurzu** tabulky v databázi. Mapování mezi přidružení typu vlastnosti a sloupce tabulky, které jsou určené v podřízených **EndProperty** elementy.

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

### <a name="example"></a>Příklad

Následující příklad ukazuje **EndProperty** mapování funkce insert a delete asociace – element (**CourseInstructor**) na uložené procedury v podkladové databázi. Funkce, které jsou mapovány na jsou deklarovány v rámci modelu úložiště.

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

## <a name="entitycontainermapping-element-msl"></a>Element EntityContainerMapping (MSL)

**Mapování EntityContainerMapping** prvek v mapování specification language (MSL) mapuje kontejner entit v konceptuálním modelu na kontejner entit v modelu úložiště. **Mapování EntityContainerMapping** element je podřízeným prvkem elementu mapování.

**Mapování EntityContainerMapping** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Element EntitySetMapping (nula nebo více)
-   AssociationSetMapping (nula nebo více)
-   FunctionImportMapping (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **mapování EntityContainerMapping** elementu.

| Název atributu            | Vyžaduje se | Hodnota                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Ano         | Název kontejneru entity modelu úložiště, které je mapován.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Ano         | Název kontejneru entity konceptuální model, který je mapován.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Ne          | **Hodnota TRUE** nebo **False**. Pokud **False**, vygenerují se aktualizace k dispozici žádná zobrazení. Tento atribut by měl být nastaven **False** Pokud máte mapování jen pro čtení, který by byl neplatný, protože data můžou není operace round-trip úspěšně. <br/> Výchozí hodnota je **True**. |

### <a name="example"></a>Příklad

Následující příklad ukazuje **mapování EntityContainerMapping** element, který se mapuje **SchoolModelEntities** kontejneru (kontejner entit koncepčního modelu)  **SchoolModelStoreContainer** kontejneru (kontejner entity model úložiště):

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

## <a name="entitysetmapping-element-msl"></a>Element EntitySetMapping (MSL)

**Elementu EntitySetMapping** nastaví prvek v mapování specification language (MSL) mapy všechny typy v konceptuálním modelu entity nastavena na entity v modelu úložiště. Je logický kontejner pro sadu entit v konceptuálním modelu instancí entit stejného typu (a odvozené typy). Sadu entit v modelu úložiště představuje tabulku nebo zobrazení v podkladové databázi. Sada entit koncepčního modelu je zadána hodnota **název** atribut **elementu EntitySetMapping** elementu. Mapované na tabulku nebo zobrazení je určená **StoreEntitySet** atribut každý podřízený prvek MappingFragment nebo v **elementu EntitySetMapping** elementu samotného.

**Elementu EntitySetMapping** prvek může mít následujících podřízených elementů:

-   Element EntityTypeMapping (nula nebo více)
-   Zobrazení QueryView (nula nebo jedna)
-   MappingFragment (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **elementu EntitySetMapping** elementu.

| Název atributu           | Vyžaduje se | Hodnota                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**                 | Ano         | Název sady entit konceptuální model, který je mapován.                                                                                                                                                             |
| **TypeName** **1**       | Ne          | Název typu koncepčního modelu entity, které je mapován.                                                                                                                                                            |
| **StoreEntitySet** **1** | Ne          | Název sady entit úložiště modelu, který je mapován na.                                                                                                                                                             |
| **Makecolumnsdistinct nastaveným**  | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli jsou vráceny pouze jedinečné sloupce. <br/> Pokud tento atribut je nastaven na **True**, **GenerateUpdateViews** atribut elementu EntityContainerMapping musí být nastaven na **False**. |

 

**1** **TypeName** a **StoreEntitySet** atributy lze použít místo podřízené prvky element EntityTypeMapping a MappingFragment pro mapování typu jedné entity k jedné tabulky.

### <a name="example"></a>Příklad

Následující příklad ukazuje **elementu EntitySetMapping** element, který mapuje tři typy (základní typ a dvou odvozených typů) v **kurzy** sadu entit v konceptuálním modelu na třech různých tabulkách v podkladová databáze. Tabulky jsou určeny **StoreEntitySet** atributy v každém **MappingFragment** elementu.

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

## <a name="entitytypemapping-element-msl"></a>Element EntityTypeMapping – Element (MSL)

**Mapování EntityTypeMapping** prvek v mapování specification language (MSL) definuje mapování mezi typem entity v konceptuálním modelu a tabulky a zobrazení v podkladové databázi. Informace o koncepčního modelu typy entit a podkladové databázové tabulky nebo zobrazení najdete v tématu Element EntityType (CSDL) a objekt EntitySet. Element (SSDL). Je určená koncepčního modelu typ entity, která je mapovaná **TypeName** atribut **mapování EntityTypeMapping** elementu. Je určená tabulky nebo zobrazení, které je mapován **StoreEntitySet** atribut MappingFragment podřízeného prvku.

ModificationFunctionMapping podřízený element se dá použít k mapování vložení, aktualizace a odstranění funkce typy entit na uložené procedury v databázi.

**Mapování EntityTypeMapping** prvek může mít následujících podřízených elementů:

-   MappingFragment (nula nebo více)
-   ModificationFunctionMapping (nula nebo jedna)
-   ScalarProperty
-   Podmínka

> [!NOTE]
> **MappingFragment** a **ModificationFunctionMapping** prvky nemůže být podřízené prvky **mapování EntityTypeMapping** element ve stejnou dobu.


> [!NOTE]
> **ScalarProperty** a **podmínku** prvků může být pouze podřízené prvky **mapování EntityTypeMapping** prvku, když se používá v rámci elementu FunctionImportMapping.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **mapování EntityTypeMapping** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název typu**   | Ano         | Název typu koncepčního modelu entity, které je mapován kvalifikovaný v oboru názvů. <br/> Pokud je typ abstract nebo odvozeného typu, musí být hodnota `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Příklad

Následující příklad ukazuje element EntitySetMapping se dva podřízené **mapování EntityTypeMapping** elementy. V prvním **mapování EntityTypeMapping** elementu, **SchoolModel.Person** typ entity je namapována na **osoba** tabulky. Ve druhém **mapování EntityTypeMapping** prvek, funkce aktualizace objektu **SchoolModel.Person** typ je mapována na uloženou proceduru **UpdatePerson**, v databázi .

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

### <a name="example"></a>Příklad

Další příklad ukazuje mapování hierarchie typů, ve kterém je kořenový typ abstraktní. Všimněte si použití `IsOfType` syntaxe **TypeName** atributy.

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

## <a name="functionimportmapping-element-msl"></a>Element FunctionImportMapping (MSL)

**FunctionImportMapping** prvek v mapování specification language (MSL) definuje mapování mezi importované funkce v konceptuálním modelu a uložené procedury nebo funkce v podkladové databázi. Importované funkce musí být deklarována v konceptuálním modelu a uložených procedur musí být deklarována v rámci modelu úložiště. Další informace najdete v elementu FunctionImport (CSDL) a funkce – Element (SSDL).

> [!NOTE]
> Ve výchozím nastavení Pokud importované funkce vrátí typ koncepčního modelu entity nebo komplexní typ, pak názvy sloupců vrácený základní uložené procedury musí přesně shodovat s názvy vlastností v typu koncepčního modelu. Pokud se názvy sloupců přesně shodovat s názvy vlastností, mapování musí být definován v elementu ResultMapping.

**FunctionImportMapping** prvek může mít následujících podřízených elementů:

-   ResultMapping (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které se vztahují na **FunctionImportMapping** element:

| Název atributu         | Vyžaduje se | Hodnota                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Ano         | Název funkce importu v konceptuálním modelu, který je mapován.           |
| **FunctionName**       | Ano         | Název funkce v rámci modelu úložiště, které je mapován kvalifikovaný v oboru názvů. |

### <a name="example"></a>Příklad

Následující příklad je založen na modelu školy. Vezměte v úvahu následující funkce v rámci modelu úložiště:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Zvažte také toto importované funkce v konceptuálním modelu:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

Následující příklad show **FunctionImportMapping** element sloužící ke zmapování funkci a funkce import nad sebou:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>Element InsertFunction (MSL)

**InsertFunction** prvek v mapování specification language (MSL) se mapuje na uloženou proceduru v podkladové databázi vložit – funkce typu entity nebo přidružení v konceptuálním modelu. Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště. Další informace najdete v tématu funkce – Element (SSDL).

> [!NOTE]
> Pokud nejsou mapovány všechny tři vložení, aktualizace nebo odstranění operace typu entity na uložené procedury nenamapované operace se nezdaří, pokud je proveden za běhu a UpdateException je vyvolána výjimka.

**InsertFunction** element může být podřízený ModificationFunctionMapping element a používá se pro element EntityTypeMapping element nebo AssociationSetMapping element.

### <a name="insertfunction-applied-to-entitytypemapping"></a>Použít pro mapování EntityTypeMapping InsertFunction

Při použití elementu mapování EntityTypeMapping **InsertFunction** element mapuje na uložené procedury insert – funkce typu entity v konceptuálním modelu.

**InsertFunction** prvek může mít následující podřízené prvky při použití **mapování EntityTypeMapping** element:

-   AssociationEnd (nula nebo více)
-   ComplexProperty (nula nebo více)
-   ResultBinding (nula nebo jedna)
-   ScarlarProperty (nula nebo více)

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **InsertFunction** element při použití **mapování EntityTypeMapping** elementu.

| Název atributu            | Vyžaduje se | Hodnota                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Název uložené procedury, ke které je mapován funkce Vložit kvalifikovaný v oboru názvů. Uložená procedura musí být deklarována v rámci modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupní parametr, který vrací počet ovlivněných řádků.                                                                               |

#### <a name="example"></a>Příklad

Následující příklad je založen na modelu školy a ukazuje **InsertFunction** element slouží k vložení funkce osobu typu entity k mapování **InsertPerson** uložené procedury. **InsertPerson** uložené procedury je deklarována v rámci modelu úložiště.

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
### <a name="insertfunction-applied-to-associationsetmapping"></a>U AssociationSetMapping InsertFunction

Při použití elementu AssociationSetMapping **InsertFunction** prvek mapuje funkce Vložit asociace v konceptuálním modelu na uložené procedury.

**InsertFunction** prvek může mít následující podřízené prvky při použití **AssociationSetMapping** element:

-   EndProperty

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **InsertFunction** prvku, když se použije k **AssociationSetMapping** elementu.

| Název atributu            | Vyžaduje se | Hodnota                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Název uložené procedury, ke které je mapován funkce Vložit kvalifikovaný v oboru názvů. Uložená procedura musí být deklarována v rámci modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupní parametr, který vrací počet ovlivněných řádků.                                                                               |

#### <a name="example"></a>Příklad

Následující příklad je založen na modelu školy a ukazuje **InsertFunction** element slouží k mapování Vložit funkci **CourseInstructor** přidružení, které  **InsertCourseInstructor** uložené procedury. **InsertCourseInstructor** uložené procedury je deklarována v rámci modelu úložiště.

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

## <a name="mapping-element-msl"></a>Element Mapping (MSL)

**Mapování** element v mapování specification language (MSL) obsahuje informace o mapování objektů, které jsou definované v konceptuálním modelu na databázi (jak je popsáno v modelu úložiště). Další informace najdete v tématu Specifikace CSDL a specifikace SSDL.

**Mapování** prvek je kořenovým prvkem pro určení mapování. Obor názvů XML pro mapování specifikace je http://schemas.microsoft.com/ado/2009/11/mapping/cs.

Mapping element může mít následující podřízené prvky (v uvedeném pořadí):

-   Alias (nula nebo více)
-   Mapování EntityContainerMapping (právě jeden)

Koncepční názvy a typy modelu úložiště, které jsou odkazovány v MSL musí být určeny podle jejich příslušných oborem názvů. Informace týkající se názvu oboru názvů konceptuálního modelu naleznete v tématu Element schématu (CSDL). Informace o názvu oboru názvů modelu úložiště najdete v tématu Element schématu (SSDL). Aliasy pro obory názvů, které se používají v MSL lze definovat pomocí prvku Alias.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **mapování** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Místo**      | Ano         | **C S**. To je pevná a nedá se změnit. |

### <a name="example"></a>Příklad

Následující příklad ukazuje **mapování** element, který je založen na součást modelu školy. Další informace o modelu školy najdete v článku rychlý start (Entity Framework):

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

## <a name="mappingfragment-element-msl"></a>Element MappingFragment (MSL)

**MappingFragment** prvek v mapování specification language (MSL) definuje mapování mezi vlastnostmi typu entity koncepčního modelu a tabulky nebo zobrazení v databázi. Informace o koncepčního modelu typy entit a podkladové databázové tabulky nebo zobrazení najdete v tématu Element EntityType (CSDL) a objekt EntitySet. Element (SSDL). **MappingFragment** může být podřízený element elementu element EntityTypeMapping nebo EntitySetMapping element.

**MappingFragment** prvek může mít následujících podřízených elementů:

-   Typ ComplexType (nula nebo více)
-   ScalarProperty (nula nebo více)
-   Podmínka (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **MappingFragment** elementu.

| Název atributu          | Vyžaduje se | Hodnota                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Ano         | Název tabulky nebo zobrazení, které je mapován.                                                                                                                                                                           |
| **Makecolumnsdistinct nastaveným** | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli jsou vráceny pouze jedinečné sloupce. <br/> Pokud tento atribut je nastaven na **True**, **GenerateUpdateViews** atribut elementu EntityContainerMapping musí být nastaven na **False**. |

### <a name="example"></a>Příklad

Následující příklad ukazuje **MappingFragment** jako podřízený element **mapování EntityTypeMapping** elementu. V tomto příkladu vlastnosti **kurzu** typ v konceptuálním modelu jsou mapovány na sloupce **kurzu** tabulky v databázi.

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

### <a name="example"></a>Příklad

Následující příklad ukazuje **MappingFragment** jako podřízený element **elementu EntitySetMapping** elementu. Stejně jako v příkladu výše, vlastnosti **kurzu** typ v konceptuálním modelu jsou mapovány na sloupce **kurzu** tabulky v databázi.

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

## <a name="modificationfunctionmapping-element-msl"></a>Element ModificationFunctionMapping (MSL)

**ModificationFunctionMapping** prvek v mapování specification language (MSL) mapuje vložit, aktualizovat a odstranit funkce entity typu koncepčního modelu pro uložené procedury v podkladové databázi. **ModificationFunctionMapping** element můžete také namapovat insert a odstranit funkce pro přidružení many-to-many v konceptuálním modelu na uložené procedury v podkladové databázi. Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště. Další informace najdete v tématu funkce – Element (SSDL).

> [!NOTE]
> Pokud nejsou mapovány všechny tři vložení, aktualizace nebo odstranění operace typu entity na uložené procedury nenamapované operace se nezdaří, pokud je proveden za běhu a UpdateException je vyvolána výjimka.


> [!NOTE]
> Pokud jsou funkcí změny jedné entity v hierarchii dědičnosti namapovaných na uložené procedury, funkce úprav pro všechny typy v hierarchii musí být namapována na uložené procedury.

**ModificationFunctionMapping** element může být podřízený element EntityTypeMapping element nebo AssociationSetMapping element.

**ModificationFunctionMapping** prvek může mít následujících podřízených elementů:

-   DeleteFunction (nula nebo jedna)
-   InsertFunction (nula nebo jedna)
-   UpdateFunction (nula nebo jedna)

Žádné atributy se vztahují na **ModificationFunctionMapping** elementu.

### <a name="example"></a>Příklad

Následující příklad ukazuje mapování pro sadu entit **lidé** sadu entit v modelu školy. Kromě mapování sloupců pro **osoba** typ entity, mapování vložení, aktualizace a odstranění funkce **osoba** typu jsou uvedeny. Funkce, které jsou mapovány na jsou deklarovány v rámci modelu úložiště.

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

### <a name="example"></a>Příklad

Následující příklad ukazuje sada mapování pro přidružení **CourseInstructor** přidružení v modelu školy. Kromě mapování sloupců pro **CourseInstructor** přidružení, mapování funkce insert a delete **CourseInstructor** přidružení se zobrazí. Funkce, které jsou mapovány na jsou deklarovány v rámci modelu úložiště.

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
 

 

## <a name="queryview-element-msl"></a>Element QueryView (MSL)

**Zobrazení QueryView** prvek v mapování specification language (MSL) definuje jen pro čtení mapování mezi typu entity nebo přidružení v konceptuálním modelu a tabulky v podkladové databázi. Je definována mapování pomocí dotazu Entity SQL, který je vyhodnocován pro model úložiště a express sady výsledků z hlediska entity nebo přidružení v konceptuálním modelu. Vzhledem k tomu dotazů zobrazení jen pro čtení, nelze použít standardní aktualizace příkazy aktualizovat typy, které jsou definovány v zobrazení dotazu. Tyto typy můžete provádět aktualizace pomocí funkcí změny. Další informace najdete v tématu Postupy: mapování funkcí změny na uložené procedury.

> [!NOTE]
> V **zobrazení QueryView** elementu, Entity SQL výrazy, které obsahují **GroupBy**, agregace skupiny nebo vlastnosti navigace nejsou podporovány.

 

**Zobrazení QueryView** element může být podřízený element elementu EntitySetMapping nebo AssociationSetMapping elementu. V případě předchozího zobrazení dotazu definuje mapování určená jen pro čtení pro entitu v konceptuálním modelu. V takovém případě definuje zobrazení dotazu jen pro čtení mapování pro přidružení v konceptuálním modelu.

> [!NOTE]
> Pokud **AssociationSetMapping** prvek je pro přidružení s referenčním omezením **AssociationSetMapping** prvek je ignorován. Další informace najdete v elementu ReferentialConstraint – Element (CSDL).

**Zobrazení QueryView** element nemůže mít žádné podřízené prvky.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **zobrazení QueryView** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Název typu**   | Ne          | Název typu koncepčního modelu, která je mapovaná zobrazení dotazu. |

### <a name="example"></a>Příklad

Následující příklad ukazuje **zobrazení QueryView** jako podřízený element **elementu EntitySetMapping** elementu a definuje mapování zobrazení dotazu pro **oddělení** typ entity Model školy.

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

Vzhledem k tomu, že dotaz vrací pouze podmnožinu členů **oddělení** typu v rámci modelu úložiště **oddělení** typ v modelu školní byla změněna podle tato mapování následujícím způsobem:

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

### <a name="example"></a>Příklad

Další příklad ukazuje **zobrazení QueryView** jako podřízený element **AssociationSetMapping** elementu a definuje mapování určená jen pro čtení pro `FK_Course_Department` přidružení v modelu školy.

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
 
### <a name="comments"></a>Komentáře

Můžete definovat dotaz zobrazení podporují následující scénáře:

-   Definování entity v konceptuálním modelu, který neobsahuje všechny vlastnosti entity v modelu úložiště. To zahrnuje vlastnosti, které nemají výchozí hodnoty a nepodporují **null** hodnoty.
-   Vypočítané sloupce v rámci modelu úložiště mapování na vlastnosti typů entit v konceptuálním modelu.
-   Definujte mapování, kde nejsou podmínek použitých ke oddílu entit v konceptuálním modelu založené na rovnost. Při zadání podmíněné mapování pomocí **podmínku** elementu zadaných podmínek musí být roven zadané hodnotě. Další informace najdete v tématu podmínku – Element (MSL).
-   Stejný sloupec v modelu úložiště přiřadit více typů v konceptuálním modelu.
-   Mapovat více typů do stejné tabulky.
-   Definice asociací v konceptuálním modelu, která nejsou založena na cizí klíče v relační schéma.
-   Pomocí vlastní obchodní logiky můžete nastavit hodnoty vlastností v konceptuálním modelu. Například můžete namapovat řetězcovou hodnotu "T" ve zdroji dat na hodnotu **true**, logická hodnota, v konceptuálním modelu.
-   Definujte Podmíněné filtry pro výsledky dotazu.
-   Vynuťte méně omezení na data v konceptuálním modelu než v modelu úložiště. Například můžete dokonce vytvářet vlastnost v konceptuálním modelu s možnou hodnotou Null i v případě, že sloupec, ke které je mapován nepodporuje **null**hodnoty.

Při definování zobrazení dotazu pro entity, platí následující aspekty:

-   Zobrazení dotazu jsou jen pro čtení. Aktualizace k entitám lze vytvořit pouze pomocí funkcí změny.
-   Při definování typu entity v zobrazení dotazu, musíte také definovat všechny související entity v zobrazení dotazu.
-   Při mapování many-to-many přidružení do entity v modelu úložiště, který představuje odkaz tabulky v relační schéma, je nutné definovat **zobrazení QueryView** prvek **AssociationSetMapping** – element pro tuto tabulku spojení.
-   Zobrazení dotazu musí být definován pro všechny typy v hierarchii typu. Můžete to udělat následujícími způsoby:
-   -   Pomocí jediného **zobrazení QueryView** prvek, který určuje jeden dotaz Entity SQL, která vrátí sjednocení všechny typy entit v hierarchii.
    -   Pomocí jediného **zobrazení QueryView** prvek, který určuje jeden dotaz Entity SQL, které používá operátor velikosti PÍSMEN pro návratový typ konkrétní entitu v hierarchii na základě konkrétní podmínky.
    -   Ještě **zobrazení QueryView** – element pro konkrétní typ v hierarchii. V takovém případě použijte **TypeName** atribut **zobrazení QueryView** elementu zadali typ entity pro každé zobrazení.
-   Při definování zobrazení dotazu nelze zadat **StorageSetName** atribut na **elementu EntitySetMapping** elementu.
-   Při definování zobrazení dotazu **elementu EntitySetMapping**element nemůže obsahovat také **vlastnost** mapování.

## <a name="resultbinding-element-msl"></a>Element ResultBinding (MSL)

**ResultBinding** mapuje hodnoty sloupců, které jsou vráceny pomocí uložené procedury vlastností entity v konceptuálním modelu při funkcí změny typu entity se mapují do uložené prvek v mapování specification language (MSL) postupy v podkladové databázi. Například pokud je vrácena hodnota sloupce identity insert uložené procedury, **ResultBinding** prvek mapuje na vlastnost typu entity v konceptuálním modelu vrácené hodnoty.

**ResultBinding** element může být podřízený InsertFunction element nebo UpdateFunction element.

**ResultBinding** element nemůže mít žádné podřízené prvky.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které se vztahují na **ResultBinding** element:

| Název atributu | Vyžaduje se | Hodnota                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název vlastnosti entity v konceptuálním modelu, který je mapován. |
| **Názevsloupce** | Ano         | Název sloupce mapován.                                          |

### <a name="example"></a>Příklad

Následující příklad je založen na modelu školy a ukazuje **InsertFunction** element slouží k mapování funkce Vložit **osoba** typu entity na **InsertPerson** uloženou proceduru. ( **InsertPerson** úložnou proceduru jsou uvedené níže a je deklarována v rámci modelu úložiště.) A **ResultBinding** element slouží k mapování hodnotu sloupce, který je vrácen uložené procedury (**NewPersonID**) na vlastnost typ entity (**PersonID**).

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

Popisuje následující příkaz jazyka Transact-SQL **InsertPerson** uložené procedury:

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

## <a name="resultmapping-element-msl"></a>Element ResultMapping (MSL)

**ResultMapping** prvek v mapování specification language (MSL) definuje mapování importované funkce v konceptuálním modelu a uložené procedury v podkladové databázi, pokud jsou splněny následující:

-   Importovaná funkce vrátí typ koncepčního modelu entity nebo komplexního typu.
-   Názvy sloupců vrácený uloženou proceduru přesně shodovat s názvy vlastností na typ entity nebo komplexního typu.

Ve výchozím nastavení mapování sloupců vrácený uložené procedury a typu entity nebo komplexní typ je založen na názvy sloupců a vlastnosti. Pokud se názvy sloupců přesně shodovat s názvy vlastností, je nutné použít **ResultMapping** elementu k definování mapování. Příklad výchozí mapování naleznete v tématu FunctionImportMapping – Element (MSL).

**ResultMapping** element je podřízeným prvkem elementu FunctionImportMapping.

**ResultMapping** prvek může mít následujících podřízených elementů:

-   Element EntityTypeMapping (nula nebo více)
-   ComplexTypeMapping

Žádné atributy se vztahují na **ResultMapping** elementu.

### <a name="example"></a>Příklad

Vezměte v úvahu následující uložené procedury:

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

Zvažte také následující entity typu koncepčního modelu:

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

Chcete-li vytvořit importované funkce, který vrátí instance předchozí typ entity, mapování sloupců vrácený uložené procedury a typu entity musí být definován v **ResultMapping** element:

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

## <a name="scalarproperty-element-msl"></a>Element ScalarProperty (MSL)

**ScalarProperty** prvek v mapování specification language (MSL) se mapuje na sloupce tabulky nebo parametr uložené procedury v podkladové databázi vlastnost v typu koncepčního modelu entity, komplexního typu nebo přidružení.

> [!NOTE]
> Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště. Další informace najdete v tématu funkce – Element (SSDL).

**ScalarProperty** element může být podřízená následující prvky:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Jako podřízený objekt **MappingFragment**, **ComplexProperty**, nebo **EndProperty** elementu, **ScalarProperty** element mapy vlastností v konceptuálním modelu na sloupec v databázi. Jako podřízený objekt **InsertFunction**, **UpdateFunction**, nebo **DeleteFunction** elementu, **ScalarProperty** element mapy vlastností v konceptuálním modelu k parametru uložené procedury.

**ScalarProperty** element nemůže mít žádné podřízené prvky.

### <a name="applicable-attributes"></a>Příslušné atributy

Atributy, které se vztahují **ScalarProperty** element se liší v závislosti na roli elementu.

Následující tabulka popisuje atributy, které se dají použít při **ScalarProperty** element slouží k mapování vlastností koncepčního modelu na sloupec v databázi:

| Název atributu | Vyžaduje se | Hodnota                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Jméno**       | Ano         | Název vlastnosti konceptuální model, který je mapován. |
| **Názevsloupce** | Ano         | Název sloupce tabulky, který je mapován.              |

Následující tabulka popisuje atributy, které se vztahují na **ScalarProperty** prvku, když se používá k mapování vlastností koncepčního modelu k parametru uložené procedury:

| Název atributu    | Vyžaduje se | Hodnota                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**          | Ano         | Název vlastnosti konceptuální model, který je mapován.                                                                                 |
| **Název parametru** | Ano         | Název parametru, který je mapován.                                                                                                 |
| **Verze**       | Ne          | **Aktuální** nebo **původní** v závislosti na tom, zda aktuální hodnotu nebo původní hodnotu vlastnosti by měla sloužit pro řízení souběžnosti. |

### <a name="example"></a>Příklad

Následující příklad ukazuje **ScalarProperty** element použít dvěma způsoby:

-   Mapování vlastností **osoba** typu entity na sloupce **osoba**tabulky.
-   Mapování vlastností **osoba** typu entity na parametry **UpdatePerson** uložené procedury. Uložené procedury jsou deklarovány v rámci modelu úložiště.

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

### <a name="example"></a>Příklad

Další příklad ukazuje **ScalarProperty** element sloužící ke zmapování insert a delete funkce přidružení konceptuálního modelu na uložené procedury v databázi. Uložené procedury jsou deklarovány v rámci modelu úložiště.

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

## <a name="updatefunction-element-msl"></a>Element UpdateFunction (MSL)

**UpdateFunction** prvek v mapování specification language (MSL) se mapuje na uloženou proceduru v podkladové databázi aktualizace – funkce typu entity v konceptuálním modelu. Uložené procedury modifikací, které jsou mapovány funkce musí být deklarována v rámci modelu úložiště. Další informace najdete v tématu funkce – Element (SSDL).

> [!NOTE]
>  Pokud nejsou mapovány všechny tři vložení, aktualizace nebo odstranění operace typu entity na uložené procedury nenamapované operace se nezdaří, pokud je proveden za běhu a UpdateException je vyvolána výjimka.

**UpdateFunction** element může být podřízený ModificationFunctionMapping element a použity pro mapování EntityTypeMapping prvek.

**UpdateFunction** prvek může mít následujících podřízených elementů:

-   AssociationEnd (nula nebo více)
-   ComplexProperty (nula nebo více)
-   ResultBinding (nula nebo jedna)
-   ScarlarProperty (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **UpdateFunction** elementu.

| Název atributu            | Vyžaduje se | Hodnota                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Název uložené procedury, ke které je mapován funkce update kvalifikovaný v oboru názvů. Uložená procedura musí být deklarována v rámci modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupní parametr, který vrací počet ovlivněných řádků.                                                                               |

### <a name="example"></a>Příklad

Následující příklad je založen na modelu školy a ukazuje **UpdateFunction** element slouží k mapování funkce aktualizace **osoba** typu entity na **UpdatePerson** uloženou proceduru. **UpdatePerson** uložené procedury je deklarována v rámci modelu úložiště.

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
